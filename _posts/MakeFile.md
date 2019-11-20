
# Table of Contents

1.  [Makefile文件格式](#org4d0ab52)
    1.  [概述](#org709d9ec)
    2.  [目标](#org887ce2b)
    3.  [前置条件](#orgd2e3449)
    4.  [命令](#orgd48d9e0)
2.  [Makefile文件的语法](#org873400e)
    1.  [注释](#orgf2b9c87)
    2.  [回声](#orgf60b2b8)
    3.  [通配符](#org732e2e2)
    4.  [模式匹配](#org2f98b24)
    5.  [变量和赋值符](#org1606cbc)
    6.  [内置变量](#orgd52cb36)
    7.  [自动变量](#org659669e)
    8.  [判断和循环](#org1931269)
    9.  [函数](#org6d68976)
3.  [Makefile的实例](#orgcb10135)



<a id="org4d0ab52"></a>

# Makefile文件格式


<a id="org709d9ec"></a>

## 概述

Makefile由一系列规则组成，每条规则的形式如下。

    <target> : <prerequisite>
    [tab] <command>

\*目标\*是必须的，不可省略。\*命令\*和\*前置命令\*是可选的，但是两者必有其一。
每条规则就明确两件事：<sub>构建目标的前置条件是什么</sub>，以及如何构建\_。下面就详细讲解，
每条规则的这三个组成部分。


<a id="org887ce2b"></a>

## 目标

一个目标就构成一条规则。可以是文件名，也可以是多个文件名。除了文件名，目标还可以是某个操作的名字，
这称为“伪目标”（phony target）

    clean:
          rm *.o

上述代码的目标是clean，它不是文件名，而是一个操作的名字，作用删除对象文件。
但当前目录存在clean这个文件，它就不会执行了，因为文件已经存在。
为了这种情况，可以明确声明clean是“伪目标”，写法如下。

    .PHONY: clean
    clean:
         rm *.o

像PHONY这种内置的目标明还有不少，可以查看[手册](https://www.gnu.org/software/make/manual/html_node/Special-Targets.html#Special-Targets)。
如果没有指定目标的话，默认执行第一个目标

    $ make


<a id="orgd2e3449"></a>

## 前置条件

前置条件通常是一组文件名，之前用空格隔开，它指定了“目标”是否需要重新构建的判断标准，
只要有一个前置文件不存在，或者更新过，“目标”就需要重新构建。

    result.txt: source.txt
        cp source.txt result.txt

必须存在 source.txt才能正常make，否则必须生成source。

    source.txt:
        echo "this is the source" > source.txt

上面代码中，source.txt后面没有前置条件，就意味着它跟其他文件都无关，只要这个文件还不存在，每次调用make source.txt，它都会生成。
如果需要生成多个文件，往往采用下面的写法。

    source: file1 file2 file3

执行make source命令后，就会一次性生成 file1，file2，file3 三个文件。

    $ make source


<a id="orgd48d9e0"></a>

## 命令

命令（conmmand）表示如何更新目标文件，由一行或者多行的shell命令组成。它是构建“目标”的具体命令。
每行命令前必须有一个\*tab\*键，如果想换成其他键可以使用内置变量 **.RECIPEPREFIX** 声明。

    .RECIPEPREFIX = >
    all:
    > echo Hello, world

需要注意的是，\*每行命令\*在一个单独的shell中执行。这些 <span class="underline">Shell之间没有继承关系</span> 。

    var-lost:
        export foo=bar
        echo "foo=[$$foo]"

上面代码执行后（make var-lost），取不到foo的值。因为两行命令在两个不同的进程执行。一个解决办法是将两行命令写在一行，中间用分号分隔。

    var-kept:
        export foo=bar; echo "foo=[$$foo]"

另一个解决办法是在换行符前加反斜杠转义。

    var-kept:
        export foo=bar; \
        echo "foo=[$$foo]"

最后一个办法就是加上\*.ONESHELL:\*命令。

    .ONESHELL:
    var-kept:
        export foo=bar;
        echo "foo=[$$foo]"


<a id="org873400e"></a>

# Makefile文件的语法


<a id="orgf2b9c87"></a>

## 注释

注释使用\*#\*。


<a id="orgf60b2b8"></a>

## 回声

正常情况下，make会打印每条命令，然后再执行，这就叫做回声（echoing）。
在命令的前面加上@，就可以关闭回声。
由于在构建过程中，需要了解当前在执行哪条命令，所以通常只在注释和纯显示的echo命令前面加上@。


<a id="org732e2e2"></a>

## 通配符


<a id="org2f98b24"></a>

## 模式匹配

Make命令允许对文件名，进行类似正则运算的匹配，主要用到的匹配符是%。
比如，假定当前目录下有 f1.c 和 f2.c 两个源码文件，需要将它们编译为对应的对象文件。

    %.o: %.c

等同于

    f1.o: f1.c
    f2.o: f2.c


<a id="org1606cbc"></a>

## 变量和赋值符

Makefile允许使用等号自定义变量。

    txt = Hello World
    test:
        @echo $(txt)

调用时，变量需要放在 $( ) 之中
调用Shell变量，需要在美元符号前，再加一个美元符号，这是因为Make命令会对美元符号转义。

    test:
        @echo $$HOME

    echo $HOME

有时，变量的值可能指向另一个变量。

    v1 = $(v2)

上面代码中，变量 v1 的值是另一个变量 v2。这时会产生一个问题，v1 的值到底在定义时扩展（静态扩展），还是在运行时扩展（动态扩展）？如果 v2 的值是动态的，这两种扩展方式的结果可能会差异很大。
为了解决类似问题，Makefile一共提供了四个赋值运算符 （=、:=、？=、+=）,它们的区别请看[stackoverflow](http://stackoverflow.com/questions/448910/makefile-variable-assignment)

    variable = value
    # 在执行时扩展，允许递归扩展。
    
    variable := value
    # 在定义时扩展。
    
    variable ?= value
    # 只有在该变量为空时才设置值。
    
    variable += value
    # 将值追加到变量的尾端。


<a id="orgd52cb36"></a>

## 内置变量

make命令提供了一系列的内置变量，比如，\((cc)指向当前使用的编译器，\)(make) 指向当前使用的make工具。详细内置变量查看清单，见[手册](https://www.gnu.org/software/make/manual/html_node/Implicit-Variables.html)

    output:
        $(CC) -o output input.c


<a id="org659669e"></a>

## 自动变量

Make命令还提供一些自动变量，它们的值与当前规则有关。主要有以下几个。

-   **$@:** $@指代当前目标，就是Make命令当前构建的那个目标。比如，make foo的 $@ 就指代foo。

    a.txt b.txt:
        touch $@

等同于下面的写法：

    a.txt:
        touch a.txt
    b.txt:
        touch b.txt

-   **$<:** $< 指代第一个前置条件。比如，规则为 t: p1 p2，那么$< 就指代p1。

    a.txt: b.txt c.txt
        cp $< $@

等同于下面的写法：

    a.txt: b.txt c.txt
        cp b.txt a.txt

-   **$?:** 指代比目标更新的所有前置条件，之间用空格分隔。比如，规则为 t: p1 p2，其中 p2 的时间戳比 t 新，$?就指代p2。
-   **$^:** $^ 指代所有前置条件，之间以空格分隔。比如，规则为 t: p1 p2，那么 $^ 就指代 p1 p2 。
-   **$\*:** $\* 指代匹配符 % 匹配的部分， 比如% 匹配 f1.txt 中的f1 ，$\* 就表示 f1。
-   **$(@D) 和 $(@F):** $(@D) 和 $(@F) 分别指向 $@ 的目录名和文件名。比如，\(@是 src/input.c，那么\)(@D) 的值为 src ，$(@F) 的值为 input.c
-   **$(<D) 和 $(<F):** $(<D) 和 $(<F) 分别指向 $< 的目录名和文件名。

所有的自动变量清单，请看[手册](https://www.gnu.org/software/make/manual/html_node/Automatic-Variables.html)，下面是一个列子：

    dest/%.txt: src/%.txt
        @[ -d dest ] || mkdir dest
        cp $< $@


<a id="org1931269"></a>

## 判断和循环

Makefile使用 Bash 语法，完成判断和循环。

    ifeq ($(CC),gcc)
      libs=$(libs_for_gcc)
    else
      libs=$(normal_libs)
    endif

上面代码判断当前编译器是否 gcc ，然后指定不同的库文件。

    LIST = one two three
    all:
        for i in $(LIST); do \
            echo $$i; \
        done
    # 等同于
    all:
        for i in one two three; do \
            echo $i; \
        done


<a id="org6d68976"></a>

## 函数

Makefile还可以使用函数，格式如下。

    $(function arguments)
    # 或者
    ${function arguments}

Makefile提供了许多[内置函数](https://www.gnu.org/software/make/manual/html_node/Functions.html)，可供调用。下面是几个常用的内置函数。

-   **shell 函数:** shell 函数用来执行 shell 命令

    srcfiles := $(shell echo src/{00..99}.txt)

-   **wildcard函数:** wildcard 函数用来在 Makefile 中，替换 Bash 的通配符。

    srcfiles := $(wildcard src/*.txt)

-   **subst 函数:** subst 函数用来文本替换，格式如下。

    $(subst from,to,text)

下面的例子将字符串"feet on the street"替换成"fEEt on the strEEt"。

    $(subst ee,EE,feet on the street)

下面是一个稍微复杂的例子。

    comma:= ,
    empty:=
    # space变量用两个空变量作为标识符，当中是一个空格
    space:= $(empty) $(empty)
    foo:= a b c
    bar:= $(subst $(space),$(comma),$(foo))
    # bar is now `a,b,c'.

-   **替换后缀名:** 替换后缀名函数的写法是：变量名 + 冒号 + 后缀名替换规则。它实际上patsubst函数的一种简写形式。

    min: $(OUTPUT:.js=.min.js)

上面代码的意思是，将变量OUTPUT中的后缀名 .js 全部替换成 .min.js 。


<a id="orgcb10135"></a>

# Makefile的实例

-   执行多个目标

    .PHONY: cleanall cleanobj cleandiff
    
    cleanall : cleanobj cleandiff
            rm program
    
    cleanobj :
            rm *.o
    
    cleandiff :
            rm *.diff

上面代码可以调用不同目标，删除不同后缀名的文件，也可以调用一个目标（cleanall），删除所有指定类型的文件。

-   编译C语言项目

    edit : main.o kbd.o command.o display.o
        cc -o edit main.o kbd.o command.o display.o
    main.o : main.c defs.h
        cc -c main.c
    kbd.o : kbd.c defs.h command.h
        cc -c kbd.c
    command.o : command.c defs.h command.h
        cc -c command.c
    display.o : display.c defs.h
        cc -c display.c
    clean :
         rm edit main.o kbd.o command.o display.o
    .PHONY: edit clean

