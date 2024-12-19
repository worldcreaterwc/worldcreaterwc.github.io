---
title: PN 筛学习笔记
date: 2024-06-11 20:31:02
categories: 学习笔记
tags:
	- 数论
	- 筛法
---

# PN 筛 \& Min 25 筛学习笔记

前情提要：有狗狗（迫真）模拟赛里放这个东西的板子（？）题，这下只能被迫学习了。

## Powerful Number

定义一个数是 Powerful Number，当且仅当其的所有质因子次幂都至少为二，简称 PN。

光是这个不够，还有一个关键结论：所有 PN 都可以写成 $a^2b^3$ 的形式。

这是简单的，考虑对于任意一个不小于等于二的数都可以写成 $2x+3y$ 的形式，然后配一起就可以了。

有这个可以证明什么？

考虑粗略估计 $n$ 以内所有 PN 的数量，也就是：
$$\sum_{i=2}^{\sqrt{n}} \big\lfloor \sqrt[3]{\frac{n}{i^2}}\big\rfloor$$

把求和符号换成积分可以得到数量级是 $\mathcal{O}(\sqrt{n})$ 的。

（说实话我也不清楚这里常数有多少，感觉这实在有点太粗略了，但是下界又刚好是 $\sqrt{n}$ 的，很奇妙）

这样可以通过线筛处理 $\sqrt{n}$ 内的质数，然后爆搜质因数指数得到 $n$ 以内所有 PN，复杂度是 $\mathcal{O}(\sqrt{n})$。

这得到了什么？

如果我们有一个积性函数 $f$，满足 $\forall p\in \mathbb{P},f(p)=0$，则 $f$ 只在 PN 处有值，那么我们就可以爆搜在 $\mathcal{O}(\sqrt{n})$ 的复杂度内得到 $n$ 以内 $f$ 函数的所有点值。

## Powerful Number 筛

求积性函数 $f$ 的前缀和。

我们构造一个积性函数 $f'$，只需要满足 $\forall p\in \mathbb{P},f'(p)=f(p)$，剩下的点值是任意的。

然后找出一个积性函数 $g$，满足 $f'*g=f$。

注意到对于 $\forall{p}\in \mathbb P$，我们有 $f(p)=f'(p)g(1)+f'(1)g(p)$，且 $f(p)=f'(p)=f'(p)g(1),f'(1)=1$，这可以解出 $g(p)=0$，这个结论可以导出 $g$ 只在 PN 有值。

然后剩下就是简单的了：

$$\sum_{i=1}^{n}f(i)=\sum_{xy\leq n}g(x)f'(y)$$

枚举所有 PN 位置的 $x$，现在只要算 $f'$ 在 $\bigg\lfloor\dfrac{n}{x}\bigg\rfloor$ 处的前缀和。

如果 $f'$ 是简单的（例如 $\mathrm{id}$）这种可以 $\mathcal{O}(1)$ 计算的话，那么总复杂度是 $\mathcal{O}(\sqrt{n})$。

否则可能需要对 $f'$ 用杜教筛（其实这里 $f'$ 的限制非常宽，可以构造出完全积性函数，所以应该也很容易构造出杜教筛函数，但是真的吗），由于询问的 $f'$ 前缀和位置，恰好是杜教筛 $f'(n)$ 前缀和时所有第一层会枚举到的位置，因此这里的复杂度等于杜教筛复杂度，简单的情况下总复杂度为 $\mathcal{O}(n^{\frac{2}{3}})$。

还没完，这里 $g$ 怎么算？

如果能够直接蒙出定义式是最好的，否则考虑利用 $f(p^k)=\sum_{i=0}^kf'(p^i)g(p^{k-i})$ 来递推，将 $k$ 从小到大枚举算出答案。

## Min 25 筛

核心思想是将求和位置拆成素数，合数和 $1$ 分别求和。

要求 $f$ 在质数处的表达式能够快速求前缀和（可以分部计算）（通常是多项式函数），质数次幂处可以快速求值。

考虑计算 $f$ 的前缀和时，先把 $f$ 所有质数处的值求和，对于合数我们枚举它的最小质因子（小于根号）和最小质因子的次数，利用积性函数的性质可以把最小质因子项提出来，然后剩下部分又可以看作一次求前缀和。

我们记 $p_k$ 表示第 $k$ 个质数（可以认为 $p_0=1$），然后记 $F(n,k)$ 表示现在需要计算 $(1,n]$ 中，所有最小质因子大于 $p_k$ 的位置的 $f$ 和。

可以得到转移式：

$$F(n,k)=G(n)-G(p_k)+\sum_{i>k}\sum_{j>0} f(p_i^j)\times \bigg(F(\big\lfloor\dfrac{n}{p_i^j}\big\rfloor,i)+[j>1]\bigg)$$

$G(n)$ 表示 $f$ 小于等于 $n$ 的位置的质数点值和，这里可以拆成多个部分加起来。

转移的意义相当于先把**大于** $p_k$ 的质数算出来，然后枚举后面的某个质数作为某个数的最小质因子，把这一项提出来后算最小质因子大于 $p_k$ 的前缀和。

注意这里 $j>1$ 的意义：因为算 $F(n,k)$ 是不包括 $f(1)$ 的取值，因此在 $j>1$ 是 $p_i^j$ 也是一个合数，需要被算进去，$j=1$ 时 $p_i$ 是个质数已经在前面被算过了。

最后 $F(n,0)+f(1)$ 即为答案。

假如我们可以 $O(1)$ 得到 $G$ 的某个点值，那么可以证明直接按照这个递推式暴力算的复杂度是 $O(n^{1-\epsilon})$，但是常数非常小，而且不需要记忆化，记忆化反而有可能变慢。可以通过优化做到 $O\bigg(\dfrac{n^{\frac{3}{4}}}{\log n}\bigg)$，但是效率上不一定会变快很多。

考虑把 $G(n)$ 搞出来。

我们可以构造一个**完全积性函数 $f'$**，满足 $f'(p)=f(p)$，记 $G'(n,k)$ 表示在 $(1,n]$ 中所有最小质因子**大于** $p_k$ 的合数以及所有质数的点值和，那么显然有 $G(n)=G'(n,+\inf)$。

考虑按照 $k$ 一层一层递推。

如果 $p_k^2>n$ 那么显然 $G'(n,k)=G'(n,k-1)$，因为显然不存在一个合数最小质因子大于 $p_k$。

否则提出一个 $f'(k)$，那么就有，$G'(n,k)=G'(n,k-1)-f'(k)(G'(\frac{n}{p_k},k-1)-G'(p_k-1,k-1))$

最后那一项是因为 $G'$ 的定义中是包括算质数的，那么即使提出一项 $f'(k)$ 出来可能也会有小于 $p_k$ 的质数在里面混进去要减掉。

这里因为 $p_k^2\leq n$，因此 $G'(p_k-1,k-1)$ 可以线性筛预处理。

因为 $k$ 只需要算到 $p_k^2>n$ 为止，而且 $G$ 和 $G'$ 所需要的点值，要么是满足 $p_k^2<n$ 的质数，要么是通过 $n$ 不断下取整得到的，前者可以线性筛直接处理，后者根据经典结论可以得到所有点值都可以通过 $n$ 一步下取整得到，积分分析一下复杂度可以得到计算 $G'$ 的复杂度为 $O\bigg(\dfrac{n^{\frac{3}{4}}}{\log n}\bigg)$。 

现在问题是 $G'(n,0)$ 如何计算？

如果 $f'$ 本身可以快速前缀和的话，我使用 PN 筛可以得到不劣于 Min 25 筛的复杂度。

不过 Min 25 筛最大的优势在于，因为计算 $G$ 的目的只需要知道他们在质数位置上的点值和，这是相对独立的，因此我们可以把 $f$ 在质数上的位置拆成若干个可以快速前缀和 $f'$ 的求和，这样灵活性就好不少。

还有一个很奇妙的是，$G$ 本身也是有特殊意义的，如果 $f'=1$ 的话你发现这解决了素数个数前缀和问题。

## Luogu P5325 【模板】Min_25 筛

定义积性函数 $f(x)$，且 $f(p ^ k) = p ^ k(p ^ k - 1)$（$p$ 是一个质数），求

$$\sum_{i = 1} ^ n f(i)$$

对 $10 ^ 9 + 7$ 取模。

Min 25 的话，注意到质数处的取值相当于 $f(p)=p^2-p$，那么 $f'$ 拆成一个 $p^2$ 和一个 $p$ 的完全积性函数，算两个 $G$ 然后作差一下。

还可以 PN，注意到质数处的取值相当于 $f'(p)=\mathrm{id}(p)\varphi(p)$，这东西卷个 $\mathrm{id}$ 就变成了 $\mathrm{id}_2$，说明可以杜教筛算 $f'$ 前缀和，然后就可以 PN 筛了。

这里还有一个类似的结论是：

若 $f(p)=\mathrm{id}_{k}(p)\varphi(p)$，则 $f(p)*\mathrm{id}_{k}=\mathrm{id}_{k+1}$

或者说如果是 $\mathrm{id}$ 点积上一个函数，都可以卷上一个相同次数的 $\mathrm{id}$ 点积某个函数，这两个抵消一下就是相当于提出一项 $n^k$，然后剩下的再卷积。

比如 $f(p)=\mathrm{id}_k\mu(p),g(p)=\mathrm{id}_{k+1}(p)$，那么 $f*g=\mathrm{id}_{k}$。

进一步地发现这相当于是点积的一个特殊分配律：

$$(f\cdot g)*(f \cdot h)=f \cdot (g*h)$$

要求 $f$ 完全积性，$g,h$ 没有要求。

这对于找杜教筛函数可能比较有用。