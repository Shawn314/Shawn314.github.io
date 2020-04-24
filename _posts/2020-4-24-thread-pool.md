---
layout:     post
title:      "线程池c++实现"
date:       Fri Apr 24 17:28:41 CST 2020
author:     "Shawn"
tags:
    - Emacs Orgmode table tutorial
---

-------------------------------------------------------------------------------
<!-- markdown-toc start - Don't edit this section. Run M-x markdown-toc-refresh-toc -->
**Table of Contents**

- [介绍](#介绍)
- [线程池组成模块](#线程池组成模块)
    - [任务队列](#任务队列)
    - [线程类](#线程类)
    - [线程池](#线程池)
- [具体实现](#具体实现)

<!-- markdown-toc end -->

# 介绍 #

> 工作中使用线程池的地方还是挺多的，为了能够高效利用并发以及 CPU 核数，通过线程池
> 的方式可以并发的处理不同的任务，提高处理效率。为此，在网上总结了一套万能的模板。

# 线程池组成模块 #

## 任务队列 ##

| 方法/成员   | 说明                 |
|-------------|----------------------|
| nextTask    | 获取队列中下一个任务 |
| addTask     | 往队列中加任务       |
| queue<Task> | 任务队列             |
| lock        | 队列操作时需要上锁   |
| cond        | 条件变量             |

## 线程类 ##

| 方法/成员 | 说明         |
|-----------|--------------|
| start     | 启动线程     |
| join      | 等待线程结束 |


##  线程池 ##

| 方法/成员  | 说明             |
|------------|------------------|
| addTask    | 添加任务         |
| thread_num | 线程池中线程数量 |
| finish     | 结束接口         |


# 具体实现 #

有了以上的思路，具体实现参考了[Thread pool implementation using pthread](Thread pool implementation using pthreads)

``` c++
#include <stdio.h>
#include <queue>
#include <unistd.h>
#include <pthread.h>
#include <malloc.h>
#include <stdlib.h>
#include <assert.h>

// Reusable thread class
class Thread
{
public:
    Thread()
    {
        state = EState_None;
        handle = 0;
    }

    virtual ~Thread()
    {
        assert(state != EState_Started);
    }

    void start()
    {
        assert(state == EState_None);
        // in case of thread create error I usually FatalExit...
        if (pthread_create(&handle, NULL, threadProc, this))
            abort();
        state = EState_Started;
    }

    void join()
    {
        // A started thread must be joined exactly once!
        // This requirement could be eliminated with an alternative implementation but it isn't needed.
        assert(state == EState_Started);
        pthread_join(handle, NULL);
        state = EState_Joined;
    }

protected:
    virtual void run() = 0;

private:
    static void* threadProc(void* param)
    {
        Thread* thread = reinterpret_cast<Thread*>(param);
        thread->run();
        return NULL;
    }

private:
    enum EState
    {
        EState_None,
        EState_Started,
        EState_Joined
    };

    EState state;
    pthread_t handle;
};


// Base task for Tasks
// run() should be overloaded and expensive calculations done there
// showTask() is for debugging and can be deleted if not used
class Task {
public:
    Task() {}
    virtual ~Task() {}
    virtual void run()=0;
    virtual void showTask()=0;
};


// Wrapper around std::queue with some mutex protection
class WorkQueue
{
public:
    WorkQueue() {
        pthread_mutex_init(&qmtx,0);

        // wcond is a condition variable that's signaled
        // when new work arrives
        pthread_cond_init(&wcond, 0);
    }

    ~WorkQueue() {
        // Cleanup pthreads
        pthread_mutex_destroy(&qmtx);
        pthread_cond_destroy(&wcond);
    }

    // Retrieves the next task from the queue
    Task *nextTask() {
        // The return value
        Task *nt = 0;

        // Lock the queue mutex
        pthread_mutex_lock(&qmtx);

        while (tasks.empty())
            pthread_cond_wait(&wcond, &qmtx);

        nt = tasks.front();
        tasks.pop();

        // Unlock the mutex and return
        pthread_mutex_unlock(&qmtx);
        return nt;
    }
    // Add a task
    void addTask(Task *nt) {
        // Lock the queue
        pthread_mutex_lock(&qmtx);
        // Add the task
        tasks.push(nt);
        // signal there's new work
        pthread_cond_signal(&wcond);
        // Unlock the mutex
        pthread_mutex_unlock(&qmtx);
    }

private:
    std::queue<Task*> tasks;
    pthread_mutex_t qmtx;
    pthread_cond_t wcond;
};

// Thanks to the reusable thread class implementing threads is
// simple and free of pthread api usage.
class PoolWorkerThread : public Thread
{
public:
    PoolWorkerThread(WorkQueue& _work_queue) : work_queue(_work_queue) {}
protected:
    virtual void run()
    {
        while (Task* task = work_queue.nextTask())
            task->run();
    }
private:
    WorkQueue& work_queue;
};

class ThreadPool {
public:
    // Allocate a thread pool and set them to work trying to get tasks
    ThreadPool(int n) {
        printf("Creating a thread pool with %d threads\n", n);
        for (int i=0; i<n; ++i)
        {
            PoolWorkerThread* pt = new PoolWorkerThread(workQueue);
            threads.push_back(pt);
            threads.back()->start();
        }
    }

    // Wait for the threads to finish, then delete them
    ~ThreadPool() {
        finish();
    }

    // Add a task
    void addTask(Task *nt) {
        workQueue.addTask(nt);
    }

    // Asking the threads to finish, waiting for the task
    // queue to be consumed and then returning.
    void finish() {
        for (size_t i=0,e=threads.size(); i<e; ++i)
            workQueue.addTask(NULL);
        for (size_t i=0,e=threads.size(); i<e; ++i)
        {
            threads[i]->join();
            delete threads[i];
        }
        threads.clear();
    }

private:
    std::vector<PoolWorkerThread*> threads;
    WorkQueue workQueue;
};

// stdout is a shared resource, so protected it with a mutex
static pthread_mutex_t console_mutex = PTHREAD_MUTEX_INITIALIZER;

// Debugging function
void showTask(int n) {
    pthread_mutex_lock(&console_mutex);
    pthread_mutex_unlock(&console_mutex);
}

// Task to compute fibonacci numbers
// It's more efficient to use an iterative algorithm, but
// the recursive algorithm takes longer and is more interesting
// than sleeping for X seconds to show parrallelism
class FibTask : public Task {
public:
    FibTask(int n) : Task(), _n(n) {}
    ~FibTask() {
        // Debug prints
        pthread_mutex_lock(&console_mutex);
        printf("tid(%d) - fibd(%d) being deleted\n", (int)pthread_self(), (int)_n);
        pthread_mutex_unlock(&console_mutex);
    }
    virtual void run() {
        // Note: it's important that this isn't contained in the console mutex lock
        long long val = innerFib(_n);
        // Show results
        pthread_mutex_lock(&console_mutex);
        printf("Fibd %d = %lld\n",(int)_n, val);
        pthread_mutex_unlock(&console_mutex);


        // The following won't work in parrallel:
        //   pthread_mutex_lock(&console_mutex);
        //   printf("Fibd %d = %lld\n",_n, innerFib(_n));
        //   pthread_mutex_unlock(&console_mutex);

        // this thread pool implementation doesn't delete
        // the tasks so we perform the cleanup here
        delete this;
    }
    virtual void showTask() {
        // More debug printing
        pthread_mutex_lock(&console_mutex);
        printf("thread %d computing fibonacci %d\n", (int)pthread_self(), (int)_n);
        pthread_mutex_unlock(&console_mutex);
    }
private:
    // Slow computation of fibonacci sequence
    // To make things interesting, and perhaps imporove load balancing, these
    // inner computations could be added to the task queue
    // Ideally set a lower limit on when that's done
    // (i.e. don't create a task for fib(2)) because thread overhead makes it
    // not worth it
    long long innerFib(long long n) {
        if (n<=1) { return 1; }
        return innerFib(n-1) + innerFib(n-2);
    }
    long long _n;
};

int main(int argc, char *argv[])
{
    // Create a thread pool
    ThreadPool *tp = new ThreadPool(10);

    // Create work for it
    for (int i=0;i<100; ++i) {
        int rv = rand() % 40 + 1;
        showTask(rv);
        tp->addTask(new FibTask(rv));
    }
    delete tp;

    printf("\n\n\n\n\nDone with all work!\n");
}

```

