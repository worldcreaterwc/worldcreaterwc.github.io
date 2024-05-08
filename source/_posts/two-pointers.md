---
title: 尺取相关
date: 2024-03-20 20:15:29
categories: 学习笔记
tags: 
	- 数据结构
	- 双指针
---

我有练功症。

# 问题描述

对于序列 $a$ 和每个 $r$，计算一个最大的 $l$ 使得 $w(l,r)=1$，记作 $f(r)=l$。

其中 $w(l,r)$ 代表 $[l,r]$ 内某种信息合并，并且不存在 $l\leq l'\leq r'\leq r$，$w(l,r)=0,w(l',r')=1$，即满足单调性。

## 标准版：
$w(l,r)$ 支持加入一个元素，删除一个元素，以及快速合并。

例如区间和，哈希值等信息。

例如

双指针，初始 $l=0,r=1$ 每次将 $r\gets r+1$ 并计算新的 $w(l,r)$，然后不断让 $l\gets l+1$ 直到 $w(l+1,r)=0$ 为止，此时 $f(r)=l$。

这里 $l,r$ 维护了一个队列。

$l,r$ 都只会增加，因此复杂度 $\mathcal{O}(n)$。

## 加强版：

$w(l,r)$ 支持加入一个元素，撤销一次上一次加入，不支持删除，依然可以快速合并。

例如区间 $\gcd$ 等信息。

不考虑拿线段树等结构硬维护 $w(l,r)$，不优美。

还是考虑双指针，但是 $l,r$ 不能像前面那样显示地维护队列，考虑使用双栈模拟队列，在 $l,r$ 中找一个分界点，设其为 $p$，那么 $[l,p]$ 和 $(p,r]$ 就是两个栈，合并这两个栈的信息就可以得到 $[l,r]$。

当 $r\gets r+1$ 时，显然可以直接加入。

当 $l\gets l+1$ 时，可以视作 $[l,p]$ 内这个栈撤销了一个元素。直接弹出。

$[l,p]$ 弹空后，再 $l\gets l+1$ 会爆。考虑直接暴力重构。令 $p=r$，然后到序枚举 $r\sim l$ 加到 新的 $[l,p]$ 栈中。新的 $(p,r]$ 为空栈。

$l,r$ 还是只会增加，重构时每个元素只会被扫到一次，因此复杂度还是 $\mathcal{O}(n)$。

## 二次加强版

$w(l,r)$ 支持加入一个元素，撤销一次上一次加入，不支持删除，不能快速合并。

朴素双指针我们甚至没有办法找到一个快速维护 $l\gets l+1$ 的方法。

换个角度思考，对于某个位置 $x$，如果有 $f(r)\leq x\leq r$，我们称 $r$ 包含了 $x$，显然所有包含 $x$ 的位置是一段以 $x$ 为左端点的区间，我们记 $p_x=r$ 为最大的 $r$ 包含 $x$。

如果我们知道了 $p_x$ 的值我们可以使用线段树分治的方法维护出信息，很遗憾我们并不知道它的信息。

一个观察时 $p_x$ 显然也是单调不降的，考虑对 $p_x$ 双指针。



维护一个 $w_r$，表示 $r$ ，表示枚举 $r$，找出所有没有被访问过的 $l$ 使得 $p_l\geq r$，然后就可以令 $ $。