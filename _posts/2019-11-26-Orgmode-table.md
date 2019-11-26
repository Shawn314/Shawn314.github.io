---
layout:     post
title:      "Emacs orgmode tables学习"
date:       Tue Nov 26 20:48:10 CST 2019
author:     "Shawn"
tags:
    - Emacs Orgmode table tutorial
---
# 目录

1.  [表格创建](#org9fa9026)
    1.  [输入维数建表格](#org85b57bd)
    2.  [文本转换表格](#org9183a65)
2.  [行和列](#org7d0772f)
    1.  [行列移动](#org087eb79)
    2.  [行列添加](#org8625b41)
    3.  [排序](#org4fb47e6)
3.  [单元格引用](#org6304e5e)
    1.  [单元格范围](#org08db8da)
4.  [表格公式](#org884db85)
    1.  [简单公式](#org3ca77a1)
    2.  [数学函数](#orgb252fc3)
    3.  [总结向量语法](#org576305d)
    4.  [可选模式](#orgd550614)



<a id="org9fa9026"></a>

# 表格创建<sup><a id="fnr.1" class="footref" href="#fn.1">1</a></sup>

在org-mode中创建表格有很多方式：


<a id="org85b57bd"></a>

## 输入维数建表格

输入C-c \|，emacs 会提示输入维数，输入之后表格就会建立

<table border="2" cellspacing="0" cellpadding="6" rules="groups" frame="hsides">


<colgroup>
<col  class="org-left" />

<col  class="org-left" />

<col  class="org-left" />

<col  class="org-right" />

<col  class="org-left" />
</colgroup>
<thead>
<tr>
<th scope="col" class="org-left">Date</th>
<th scope="col" class="org-left">Category 1</th>
<th scope="col" class="org-left">Category 2</th>
<th scope="col" class="org-right">Numeric</th>
<th scope="col" class="org-left">&#xa0;</th>
</tr>
</thead>

<tbody>
<tr>
<td class="org-left"><span class="timestamp-wrapper"><span class="timestamp">&lt;2013-01-14 Mon&gt;</span></span></td>
<td class="org-left">A</td>
<td class="org-left">C</td>
<td class="org-right">10.0</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left"><span class="timestamp-wrapper"><span class="timestamp">&lt;2013-01-14 Mon&gt;</span></span></td>
<td class="org-left">B</td>
<td class="org-left">C</td>
<td class="org-right">15.0</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left"><span class="timestamp-wrapper"><span class="timestamp">&lt;2013-01-15 Tue&gt;</span></span></td>
<td class="org-left">A</td>
<td class="org-left">A</td>
<td class="org-right">12.5</td>
<td class="org-left">&#xa0;</td>
</tr>
</tbody>
</table>


<a id="org9183a65"></a>

## 文本转换表格

选中文本，使用快捷键C-c \|，将文本转化成一个表格

<table border="2" cellspacing="0" cellpadding="6" rules="groups" frame="hsides">


<colgroup>
<col  class="org-left" />

<col  class="org-left" />

<col  class="org-left" />

<col  class="org-right" />
</colgroup>
<tbody>
<tr>
<td class="org-left">Date</td>
<td class="org-left">Category1</td>
<td class="org-left">Category2</td>
<td class="org-right">Numeric</td>
</tr>


<tr>
<td class="org-left"><span class="timestamp-wrapper"><span class="timestamp">&lt;2013-01-14 Mon&gt;</span></span></td>
<td class="org-left">A</td>
<td class="org-left">C</td>
<td class="org-right">10.0</td>
</tr>


<tr>
<td class="org-left"><span class="timestamp-wrapper"><span class="timestamp">&lt;2013-01-15 Tue&gt;</span></span></td>
<td class="org-left">B</td>
<td class="org-left">C</td>
<td class="org-right">15.0</td>
</tr>


<tr>
<td class="org-left"><span class="timestamp-wrapper"><span class="timestamp">&lt;2013-01-16 Wed&gt;</span></span></td>
<td class="org-left">A</td>
<td class="org-left">A</td>
<td class="org-right">12.5</td>
</tr>
</tbody>
</table>

最后在第一行之后放入一道横线，快捷键 **C-c -**

<table border="2" cellspacing="0" cellpadding="6" rules="groups" frame="hsides">


<colgroup>
<col  class="org-left" />

<col  class="org-left" />

<col  class="org-left" />

<col  class="org-right" />
</colgroup>
<thead>
<tr>
<th scope="col" class="org-left">Date</th>
<th scope="col" class="org-left">Category1</th>
<th scope="col" class="org-left">Category2</th>
<th scope="col" class="org-right">Numeric</th>
</tr>
</thead>

<tbody>
<tr>
<td class="org-left"><span class="timestamp-wrapper"><span class="timestamp">&lt;2013-01-14 Mon&gt;</span></span></td>
<td class="org-left">A</td>
<td class="org-left">C</td>
<td class="org-right">10.0</td>
</tr>


<tr>
<td class="org-left"><span class="timestamp-wrapper"><span class="timestamp">&lt;2013-01-16 Wed&gt;</span></span></td>
<td class="org-left">A</td>
<td class="org-left">A</td>
<td class="org-right">12.5</td>
</tr>


<tr>
<td class="org-left"><span class="timestamp-wrapper"><span class="timestamp">&lt;2013-01-15 Tue&gt;</span></span></td>
<td class="org-left">B</td>
<td class="org-left">C</td>
<td class="org-right">15.0</td>
</tr>
</tbody>
</table>


<a id="org7d0772f"></a>

# 行和列


<a id="org087eb79"></a>

## 行列移动

可以使用M-up and M-down来移动行和列


<a id="org8625b41"></a>

## 行列添加

可以使用M-S-up and M-S-down来移动行和列


<a id="org4fb47e6"></a>

## 排序

C-c ^ 


<a id="org6304e5e"></a>

# 单元格引用

单元格引用格式为： **@row$column** ，可以使用 **C-c }** 开启行数列数。
获取当前光标所在的单元格行列，输入 **C-c ?**

-   absolute (e.g. @1 is row 1)
-   relative to the current position (e.g. @+1 is one row down or @-2 is two rows above)
-   immutable - relative to the first and last row (e.g. @< is the first row, @> is the last row and @>>> is the third last row)
-   relative to the hlines (e.g. @+I is the first row after the first hline, @-III is the first row before the third hline and @II+2 is the second row after the second hline.

<table border="2" cellspacing="0" cellpadding="6" rules="groups" frame="hsides">


<colgroup>
<col  class="org-left" />

<col  class="org-left" />
</colgroup>
<thead>
<tr>
<th scope="col" class="org-left">Example</th>
<th scope="col" class="org-left">Description</th>
</tr>
</thead>

<tbody>
<tr>
<td class="org-left">@3$2</td>
<td class="org-left">Row 3, column 2. Same as C3</td>
</tr>


<tr>
<td class="org-left">@3</td>
<td class="org-left">Row 2, current column.</td>
</tr>


<tr>
<td class="org-left">$2</td>
<td class="org-left">Current row, column 2. Same as C&</td>
</tr>


<tr>
<td class="org-left">@-2$-3</td>
<td class="org-left">Two rows up and three columns left of the current cell</td>
</tr>


<tr>
<td class="org-left">@>$<<</td>
<td class="org-left">The last row, second last column</td>
</tr>


<tr>
<td class="org-left">@+II$2</td>
<td class="org-left">The first row after the second hline, second column</td>
</tr>
</tbody>
</table>


<a id="org08db8da"></a>

## 单元格范围

可以用 *..* 表达，例如@3..@8。


<a id="org884db85"></a>

# 表格公式


<a id="org3ca77a1"></a>

## 简单公式

加入公式可以有以下几种方式：

1.  在一个单元格内输入:=，然后输入公式，例如$2\*2

<table border="2" cellspacing="0" cellpadding="6" rules="groups" frame="hsides">


<colgroup>
<col  class="org-left" />

<col  class="org-right" />

<col  class="org-left" />

<col  class="org-left" />

<col  class="org-left" />
</colgroup>
<thead>
<tr>
<th scope="col" class="org-left">Date</th>
<th scope="col" class="org-right">Numeric</th>
<th scope="col" class="org-left">Category 1</th>
<th scope="col" class="org-left">Category 2</th>
<th scope="col" class="org-left">&#xa0;</th>
</tr>
</thead>

<tbody>
<tr>
<td class="org-left"><span class="timestamp-wrapper"><span class="timestamp">&lt;2013-01-14 Mon&gt;</span></span></td>
<td class="org-right">15.0</td>
<td class="org-left">B</td>
<td class="org-left">C</td>
<td class="org-left">:=$2\*2</td>
</tr>


<tr>
<td class="org-left"><span class="timestamp-wrapper"><span class="timestamp">&lt;2013-01-14 Mon&gt;</span></span></td>
<td class="org-right">10.0</td>
<td class="org-left">A</td>
<td class="org-left">C</td>
<td class="org-left">&#xa0;</td>
</tr>


<tr>
<td class="org-left"><span class="timestamp-wrapper"><span class="timestamp">&lt;2013-01-15 Tue&gt;</span></span></td>
<td class="org-right">12.5</td>
<td class="org-left">A</td>
<td class="org-left">A</td>
<td class="org-left">&#xa0;</td>
</tr>
</tbody>
</table>

然后输入C-c C-c完成当前单元格的计算。

1.  第二种是加入语法：#+TBLFM

<table border="2" cellspacing="0" cellpadding="6" rules="groups" frame="hsides">


<colgroup>
<col  class="org-left" />

<col  class="org-right" />

<col  class="org-left" />

<col  class="org-left" />

<col  class="org-right" />
</colgroup>
<thead>
<tr>
<th scope="col" class="org-left">Date</th>
<th scope="col" class="org-right">Numeric</th>
<th scope="col" class="org-left">Category 1</th>
<th scope="col" class="org-left">Category 2</th>
<th scope="col" class="org-right">&#xa0;</th>
</tr>
</thead>

<tbody>
<tr>
<td class="org-left"><span class="timestamp-wrapper"><span class="timestamp">&lt;2013-01-14 Mon&gt;</span></span></td>
<td class="org-right">15.0</td>
<td class="org-left">B</td>
<td class="org-left">C</td>
<td class="org-right">45.</td>
</tr>


<tr>
<td class="org-left"><span class="timestamp-wrapper"><span class="timestamp">&lt;2013-01-14 Mon&gt;</span></span></td>
<td class="org-right">10.0</td>
<td class="org-left">A</td>
<td class="org-left">C</td>
<td class="org-right">30.</td>
</tr>


<tr>
<td class="org-left"><span class="timestamp-wrapper"><span class="timestamp">&lt;2013-01-15 Tue&gt;</span></span></td>
<td class="org-right">12.5</td>
<td class="org-left">A</td>
<td class="org-left">A</td>
<td class="org-right">37.5</td>
</tr>
</tbody>
</table>

C-c \*: 在一个单元格复制语法，
C-u C-c \*: 复制语法到每一行


<a id="orgb252fc3"></a>

## 数学函数

<table border="2" cellspacing="0" cellpadding="6" rules="groups" frame="hsides">


<colgroup>
<col  class="org-left" />

<col  class="org-left" />
</colgroup>
<thead>
<tr>
<th scope="col" class="org-left">Function</th>
<th scope="col" class="org-left">Description</th>
</tr>
</thead>

<tbody>
<tr>
<td class="org-left">exp(c)</td>
<td class="org-left">exponential</td>
</tr>


<tr>
<td class="org-left">log(c)</td>
<td class="org-left">natural logarithm</td>
</tr>


<tr>
<td class="org-left">log10(c)</td>
<td class="org-left">log base 10</td>
</tr>


<tr>
<td class="org-left">sqrt(c)</td>
<td class="org-left">square-root</td>
</tr>


<tr>
<td class="org-left">vmin(v)</td>
<td class="org-left">minimum of vector (non blanks)</td>
</tr>


<tr>
<td class="org-left">vmax(v)</td>
<td class="org-left">maximum of vector (non blanks)</td>
</tr>


<tr>
<td class="org-left">vmean(v)</td>
<td class="org-left">mean of vector (non blanks)</td>
</tr>


<tr>
<td class="org-left">vmedian(v)</td>
<td class="org-left">median of vector (non blanks)</td>
</tr>


<tr>
<td class="org-left">vsdev(v)</td>
<td class="org-left">standard deviation of vector (non blanks)</td>
</tr>


<tr>
<td class="org-left">vvar(v)</td>
<td class="org-left">variance of vector (non blanks)</td>
</tr>


<tr>
<td class="org-left">vcov(v1, v2)</td>
<td class="org-left">covariance of two vector (non blanks)</td>
</tr>


<tr>
<td class="org-left">vcor(v1, v2)</td>
<td class="org-left">correlation of two vector (non blanks)</td>
</tr>
</tbody>
</table>


<a id="org576305d"></a>

## 总结向量语法

Orgmode表可以利用calc中的一些函数。例如，添加一个列总和。首先在最后一行添加一行

<table border="2" cellspacing="0" cellpadding="6" rules="groups" frame="hsides">


<colgroup>
<col  class="org-left" />

<col  class="org-right" />

<col  class="org-left" />

<col  class="org-left" />

<col  class="org-right" />
</colgroup>
<thead>
<tr>
<th scope="col" class="org-left">Date</th>
<th scope="col" class="org-right">Numeric</th>
<th scope="col" class="org-left">Category 1</th>
<th scope="col" class="org-left">Category 2</th>
<th scope="col" class="org-right">&#xa0;</th>
</tr>
</thead>

<tbody>
<tr>
<td class="org-left"><span class="timestamp-wrapper"><span class="timestamp">&lt;2013-01-14 Mon&gt;</span></span></td>
<td class="org-right">15.0</td>
<td class="org-left">B</td>
<td class="org-left">C</td>
<td class="org-right">30.</td>
</tr>


<tr>
<td class="org-left"><span class="timestamp-wrapper"><span class="timestamp">&lt;2013-01-14 Mon&gt;</span></span></td>
<td class="org-right">10.0</td>
<td class="org-left">A</td>
<td class="org-left">C</td>
<td class="org-right">&#xa0;</td>
</tr>


<tr>
<td class="org-left"><span class="timestamp-wrapper"><span class="timestamp">&lt;2013-01-15 Tue&gt;</span></span></td>
<td class="org-right">12.5</td>
<td class="org-left">A</td>
<td class="org-left">A</td>
<td class="org-right">&#xa0;</td>
</tr>
</tbody>

<tbody>
<tr>
<td class="org-left">&#xa0;</td>
<td class="org-right">&#xa0;</td>
<td class="org-left">&#xa0;</td>
<td class="org-left">&#xa0;</td>
<td class="org-right">&#xa0;</td>
</tr>
</tbody>
</table>

在@5$2中输入:=vsum(@2..@4)，然后按 C-c C-c。

<table border="2" cellspacing="0" cellpadding="6" rules="groups" frame="hsides">


<colgroup>
<col  class="org-left" />

<col  class="org-right" />

<col  class="org-left" />

<col  class="org-left" />

<col  class="org-right" />
</colgroup>
<thead>
<tr>
<th scope="col" class="org-left">Date</th>
<th scope="col" class="org-right">Numeric</th>
<th scope="col" class="org-left">Category 1</th>
<th scope="col" class="org-left">Category 2</th>
<th scope="col" class="org-right">&#xa0;</th>
</tr>
</thead>

<tbody>
<tr>
<td class="org-left"><span class="timestamp-wrapper"><span class="timestamp">&lt;2013-01-14 Mon&gt;</span></span></td>
<td class="org-right">15</td>
<td class="org-left">B</td>
<td class="org-left">C</td>
<td class="org-right">30.</td>
</tr>


<tr>
<td class="org-left"><span class="timestamp-wrapper"><span class="timestamp">&lt;2013-01-14 Mon&gt;</span></span></td>
<td class="org-right">10</td>
<td class="org-left">A</td>
<td class="org-left">C</td>
<td class="org-right">&#xa0;</td>
</tr>


<tr>
<td class="org-left"><span class="timestamp-wrapper"><span class="timestamp">&lt;2013-01-15 Tue&gt;</span></span></td>
<td class="org-right">12</td>
<td class="org-left">A</td>
<td class="org-left">A</td>
<td class="org-right">&#xa0;</td>
</tr>
</tbody>

<tbody>
<tr>
<td class="org-left">&#xa0;</td>
<td class="org-right">:=vsum(@2..@4)</td>
<td class="org-left">&#xa0;</td>
<td class="org-left">&#xa0;</td>
<td class="org-right">&#xa0;</td>
</tr>
</tbody>
</table>

函数vsum()表示向量和，因此得到：

<table border="2" cellspacing="0" cellpadding="6" rules="groups" frame="hsides">


<colgroup>
<col  class="org-left" />

<col  class="org-right" />

<col  class="org-left" />

<col  class="org-left" />

<col  class="org-right" />
</colgroup>
<thead>
<tr>
<th scope="col" class="org-left">Date</th>
<th scope="col" class="org-right">Numeric</th>
<th scope="col" class="org-left">Category 1</th>
<th scope="col" class="org-left">Category 2</th>
<th scope="col" class="org-right">&#xa0;</th>
</tr>
</thead>

<tbody>
<tr>
<td class="org-left"><span class="timestamp-wrapper"><span class="timestamp">&lt;2013-01-14 Mon&gt;</span></span></td>
<td class="org-right">15.0</td>
<td class="org-left">B</td>
<td class="org-left">C</td>
<td class="org-right">30.</td>
</tr>


<tr>
<td class="org-left"><span class="timestamp-wrapper"><span class="timestamp">&lt;2013-01-14 Mon&gt;</span></span></td>
<td class="org-right">10.0</td>
<td class="org-left">A</td>
<td class="org-left">C</td>
<td class="org-right">&#xa0;</td>
</tr>


<tr>
<td class="org-left"><span class="timestamp-wrapper"><span class="timestamp">&lt;2013-01-15 Tue&gt;</span></span></td>
<td class="org-right">12.5</td>
<td class="org-left">A</td>
<td class="org-left">A</td>
<td class="org-right">&#xa0;</td>
</tr>
</tbody>

<tbody>
<tr>
<td class="org-left">&#xa0;</td>
<td class="org-right">37.5</td>
<td class="org-left">&#xa0;</td>
<td class="org-left">&#xa0;</td>
<td class="org-right">&#xa0;</td>
</tr>
</tbody>
</table>


<a id="orgd550614"></a>

## 可选模式

在calc函数后可加入以下可选模式，来定制化结果。

<table border="2" cellspacing="0" cellpadding="6" rules="groups" frame="hsides">


<colgroup>
<col  class="org-left" />

<col  class="org-left" />

<col  class="org-left" />
</colgroup>
<thead>
<tr>
<th scope="col" class="org-left">Mode option</th>
<th scope="col" class="org-left">Example</th>
<th scope="col" class="org-left">Description</th>
</tr>
</thead>

<tbody>
<tr>
<td class="org-left">p</td>
<td class="org-left">;p10</td>
<td class="org-left">set calculation precision to 10 decimal places</td>
</tr>


<tr>
<td class="org-left">n,s,e,f</td>
<td class="org-left">;s3</td>
<td class="org-left">normal, scientific, engineering or fixed formatting. Example indicates scientific formatting with 3-1=2 decimal places.</td>
</tr>


<tr>
<td class="org-left">D,R</td>
<td class="org-left">;D</td>
<td class="org-left">degrees or radians</td>
</tr>


<tr>
<td class="org-left">F,S</td>
<td class="org-left">;F</td>
<td class="org-left">fraction (ratio) and symbolic (standard) format.</td>
</tr>


<tr>
<td class="org-left">N</td>
<td class="org-left">;N</td>
<td class="org-left">interpret all fields as numbers, making non-numerics 0 values</td>
</tr>


<tr>
<td class="org-left">E</td>
<td class="org-left">;E</td>
<td class="org-left">keep empty cells in the range</td>
</tr>


<tr>
<td class="org-left">L</td>
<td class="org-left">;L</td>
<td class="org-left">treat the values as literal</td>
</tr>
</tbody>
</table>


# Footnotes

<sup><a id="fn.1" href="#fnr.1">1</a></sup> [Emacs orgmode tables](http://www.flutterbys.com.au/stats/tut/tut16.1.html).
