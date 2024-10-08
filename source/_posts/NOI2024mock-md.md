---
title: NOI2024 前模拟赛
date: 2024-05-07 20:56:19
tags:
	- 校内
	- 模拟赛
categories: 学习笔记
---
# 练功
练内功。
## 5.7

$13/100+0/100+3/3$

怎么有人文件都不会写了。

### T1

#### 题面

求方程 $Ax^2+Bxy+Cy^2+Dx+Ey\equiv 0 \pmod P$ 的 $M$ 个根，保证 $D>0$。

多测，$\sum M\leq 5\times 10^5$，$P>>M$。

#### Sol
考虑一个观察：限制非常的宽，如果我们随机给这些方程加一些限制，然后尝试解出这个方程（可能无解或重复），期望 $O(M)$ 次就可以得到 $M$ 个本质不同的解。这一步需要一定的胆子和数学直觉。

一种做法是枚举 $x$，解关于 $y$ 的二次方程，但是需要算二次剩余。

还有一种做法是钦定 $y=kx$，然后枚举这个 $k$ 解关于 $x$ 的二次方程，注意到 $x=0$ 是这个方程的一个根，则另一个根可以直接通过韦达定理计算，无需解二次剩余。

这种做法的几何意义是枚举一条斜率为整数的直线，交这个椭圆一定能得到一个有理点。


### T2

#### 题面

记 $[n]={0,1,2,\ldots,n}$。

求：

$$\sum_{r=0}^{n-1}\bigg(\sum_{S\sube [nm-1]}\bigg[\bigg(\sum_{x\in S}x\bigg)\equiv r\pmod n\bigg]\bigg)^2$$

多测，$T\neq 50$，$1\leq n,m\leq 10^{12}$。

#### Sol

看到平方，先组合意义一下。

相当于选两个集合使得他们元素总和模 $n$ 相等，也就是两个集合元素总和作差模 $n$ 余 $0$，注意到我们是在模意义下计算作差，因此可以变成选两个集合，总和模 $n$ 余 $0$。

这相当于是 $0\sim n-1$，每个数字出现了 $2m$ 次的背包，求最后总和模 $n$ 余 $0$ 的方案数。

也就是求：

$$[x^0]\prod_{i=0}^{n-1}(1+x^i)^{2m} \bmod (x^n-1)$$

直接考虑单位根反演：

$$\dfrac{1}{n}\sum_{i=0}^{n-1}\prod_{j=0}^{n-1}(1+\omega_{n}^{ij})^{2m}$$

考虑 $ij\bmod n$ 的序列，发现只与 $\gcd(i,n)$ 有关。

那么可以枚举这个 $\gcd$，得到：

$$\dfrac{1}{n}\sum_{d|n} \varphi(d)\prod_{i=0}^{d-1}(1+\omega_d^{i})^{2m\frac{n}{d}}$$

考虑计算 $\prod_{i=0}^{n-1}(1+\omega_{n}^i)$。

这个式子有结论：其值为 $1+(-1)^{n-1}$。

到这里就可以直接枚举约数直接算了，复杂度为 $\mathcal{O}(T(\sqrt{n}+d(n)\log(n)))$。

使用 Pollard-Rho 分解质因数配合计算 $2$ 的光速幂可以做到 $\mathcal{O}(\sqrt{P}+T(n^{\frac{1}{4}}+d(n)))$，但是没有必要。

如何算 $\prod_{i=0}^{n-1}(1+\omega_{n}^i)$？

注意到我们有：

$$x^n-1=\prod_{i=0}^{n-1}(x-\omega_{n}^{i})$$

直接代入 $x=-1$ 就可以得到结论。

**看到题中（尤其是数数题）里有取模相关一定要考虑单位根啊啊啊啊啊啊啊啊。**

还有一种手法是推导原多项式 DFT 后的结果，与这种方法并无多大差别。

### T3

#### 题面

Luogu P7729

#### Sol

太困难了。

具体地想法是维护出第一问的答案，发现一定是若干个连通块不动，剩下一个连通块变化一条边，然后找出最小环可以维护第一问，第二问不会。

最小环 $\mathcal{O}(nm)$ 可以建出 BFS 树然后考虑最多只有一条边不在树上。


## 5.11

$100/100+5/100+12/52$

越打越唐，T1 T2 都是挺顺的题。

现在思维不知道怎么容易寸止了。

### T1

#### 题面

称一个正整数序列是好的，当且仅当所有真前缀和与真后缀和两两不同。

给定 $n$ 和 $n$ 个限制 $l_i,r_i$。计算有多少长度为 $n$ 的序列，满足每个数 $a_i$ 在 $[l_i,r_i]$ 之间且是好的。

$1\leq n\leq 50,1\leq l_i\leq r_1\leq 2000$。

#### Sol

首先我们只需要判断任意不交的前后缀。

因为数字都是正整数，所以前缀和序列和后缀和序列都单调。

考虑一个 check，将这两个序列归并起来，这样只需要比较归并后的序列相邻元素。

由此设 dp 状态，设 $f_{l,r,sl,sr}$ 表示填完 $l$ 前缀，前缀和为 $sl$，填完了 $r$ 后缀，后缀和为 $sr$。要求不能有 $sl=sr$。如果 $sl<sr$ 那就先填前缀，否则先填后缀（哪边小动哪边，符合归并的流程）状态数 $\mathcal{O}(n^4V^2)$。

注意到我们只需要知道 $sl$ 和 $sr$ 的大小关系，那就记录只记录差值 $dt=sl-sr$，根据刚才的填数过程显然有 $|dt|\leq \max r_i$，这样状态数就只有 $\mathcal{O}(n^2V)$ 了，使用前缀和来进行转移，复杂度 $\mathcal{O}(n^2V)$。 

### T2

#### 题面

称一个长为 $n$ 的正整数序列 $p$ 是坏的，当且仅当：

$$\forall 1<i\leq n, |p_i-p_{i-1}|\leq k$$

给出一个长为 $n$ 的正整数序列 $a$，以及三个数 $m,l,k$，满足 $1\leq a_i\leq m$。

你需要找出一个最长的序列 $b$，满足：

1. $1\leq b_i\leq m$。
2. $a$ 是 $b$ 的前缀。
3. $b$ 中不存在长度超过 $l$ 的坏子序列。

$1\leq n\leq 2\times 10^5,1\leq m,l,k\leq 10^9$。

#### Sol

考虑坏子序列的 dp，设 $f_v$ 表示以数字 $v$ 结尾的最长坏子序列，那么每次加入一个数字的转移就是 $f_v=\max\limits_{i=v-k}^{v+k}f_i+1$，我们可以上个线段树预处理出只考虑 $a$ 时的 $f$ 数组。

接下来放新的数字的时候考虑一个贪心，每次找出一个 $x$，使得放上这个 $x$ 后的 $f_x$ 最小，且这个 $f_x$ 尽可能靠边。

这个贪心显然正确性没有问题，直接做的话可以得到一个复杂度为 $\mathcal{O}(ans\times m\log m)$ 的做法，其中 $ans$ 为 $\mathcal{O}(kl)$ 级别。

这个东西复杂度有点大，尤其是还有一个枚举 $x$ 是什么，考虑把 dp 从刷表改成填表，即设 $g_i$ 表示我目前假装我要放在 $i$ 位置，那么新的 $f_i$ 应该是多少。然后每次放完一个数字后，$g$ 的维护就是一段区间取 $\max$。这样子可以做到复杂度为 $\mathcal{O}(ans\log m)$。

考虑上面这个过程实际上干了什么，在初始做完 $a$ 数组的 dp 后，$g$ 内应该是有不超过 $n$ 个连续段。每次找出权值最小的连续段，设其长度为 $L$，操作 $\lceil\frac{L}{K}\rceil$ 次后这个连续段的权值加一。

那么就可以逐连续段考虑，每次让它和两边更小的合并起来算贡献，最后只剩下一个连续段的时候直接算答案（退化为了 $n=0$ 的情况）显然枚举的连续段个数是 $\mathcal{O}(n)$ 的。

总复杂度 $\mathcal{O}(n\log m)$，如果离散化就是 $\mathcal{O}(n\log n)$。

### T3

#### 题面

给出 $n,k$，计算：

$$\sum_{i=1}^{n}\operatorname{lcm}(i,i+1,i+2,\ldots,i+k)$$

#### Sol

老题新放是吧/qd

考虑设 $\dfrac{x^{\overline{k+1}}}{\operatorname{lcm}(x,x+1,x+2,\ldots,x+k)}=C$。

对于 $C$ 值相等的 $x$ 我们显然可以放在一起插值计算前缀和，最后把所有 $n$ 以内不同 $C$ 值的答案加起来。

这里有一个结论，设 $K=\operatorname{lcm}(1,2,3,\ldots,k)$，则所有 $x$ 模 $K$ 同余的位置 $C$ 相等。

那么直接枚举 $x$ 模 $K$ 的余数硬插。复杂度 $\mathcal{O}(K\mathrm{poly}(k))$。

## 5.22

$0/100+60/100+0/100=60$

我是睡觉高手。

### T1

#### 题面

给定一张网格图，其中某些节点有障碍，你每次只能向右向下走，要从 $(0,0)$ 走到 $(n,m)$，你需要选出数量最大的路径集合，使得集合中不存在两条路径，其中一条穿过了另一条。

这里两个路径不同，当且仅当存在一个障碍，一条路径经过了这个障碍的上方，另一条经过了下方。

$1\leq n,m\leq 2000$。

#### Sol

考虑原先障碍构成的连通块形状比较奇怪不好分析，所以我们先把不能从起点到达的点，或者不能到达终点的点也视作障碍，这样就舒服多了。

然后单独长在边缘上的连通块没用，把它们扔掉，接下来有结论：答案等于剩下的连通块数加一。

貌似是直接调整能够得到这个结论？

### T2

#### 题面

老鼠进洞，但是每个位置有多个老鼠，每个洞也可以进多个老鼠。

$n\leq 5\times 10^5$，时限 3s。

#### Sol

你注意到朴素老鼠进洞是开两个堆不断弹弹弹，把弹出去的东西扔到另一个堆里，那你就把堆换成一个平衡树加速弹弹弹，写了一下发现过了。

题解分析这玩意是因为每次取出堆扔到另一个堆的过程是中，合并的部分不会有交，但是感觉不是特别有道理。

另一种做法是考虑 dp，设 $f_i,j$ 表示当前在当前位置，前面匹配掉的老鼠减去洞的数量为 $j$ 是的答案，这个东西肯定是凸的，所以来个 slope trick 优化转移。

std 是怎么考虑的呢？注意到原问题本身是一个线性规划问题，考虑对其进行对偶，**对偶的时候尽可能少的保留变量，尽可能对着原问题对偶，不要去对偶费用流图，这样会很麻烦。**

对偶后的问题可以模拟费用流解决，你先别急，让我想想怎么对偶。

**启示：对于线性规划性问题，对偶是一个很好的解决办法。**

### T3

#### 题面

给出一个字符串集合 $S$，大小为 $n$，每个串长度不超过 $m$，问你从一个串 $T$ 开始，每次随机以 $p_i$ 的概率加入字符 $i$，期望多少步可以得到 $S$ 中的某个串。

$n\leq 100,n\times m\leq 10^4$。

#### Sol

暴力做法就是考虑建出 AC 自动机，然后高斯消元，复杂度是 $\mathcal{O}((nm)^3)$ 的。

发现解方程的过程不可避免，那么考虑怎么减少未知数的数量。

**AC 自动机的一个显然的性质：所有转移边要么指向自己在 trie 上的儿子，要么指向比自己深度低的节点。**

观察方程：$E(x)=1+\sum\limits_{i\in \Sigma} p_iE(\delta_{x,i})$，如果我们已经知道了所有深度比 $x$ 低的节点的表示，且 $x$ 在 trie 上只有一个儿子，那么我们可以直接得出这个儿子的表示。

如果 $x$ 在 trie 上有多于一个儿子怎么办？随便选一个儿子，对另外的儿子设位置变量来表示出我选的儿子。

用 bfs 的方法一层一层的把表示推出来，最后利用终点答案为 $0$ 列出方程。

这样做法本质上是对 trie 进行实链剖分，那么变量就是虚儿子的数量（也就是实链的数量），而实链数量为叶子数量，这个只有 $n$ 个，因此再做高斯消元复杂度就对了。

总复杂度 $\mathcal{O}(n^3+n^2m)$。

好题，利用了 AC 自动机的性质去下降高斯消元的未知数个数（叶子个数少以及可以 bfs 一层一层推算）。

## 5.28

验题场。

### T1

#### 题面

给定一个长为 $n$ 的序列 $a$，每次可以选择一组 $i,j$，满足 $a_i\neq a_j$，然后令 $a_i,a_j$ 都变为 $a_i\mathrm a_j$。

在 $20n$ 次操作内排序。

#### Sol

$n=2^k$ 有一个可以变为全等的做法，就是直接分治。

一般情况，可以先把 $a$ 变为回文，然后操作前 $2^k$ 和 后 $2^k$，其中 $k$ 是最大的满足 $2^k\leq n$ 的自然数，最后只有两种数，如果不是有序的，因为 $a$ 回文，所以可以把前面的操作反过来做，这样就有序了。

但是这样操作次数很多，非常不牛！

考虑直接从排序入手，我们思考怎么交换两个数。

发现直接交换 $a,b$ 两个数是困难的，但是交换 $aa,bb$ 两个段是好做的，那么就先把 $a$ 变成相邻相等的，然后直接选择排序即可，如果 $n$ 是奇数加点特判就好了。

### T2

#### 题面

给出 $n,m$。

定义一个长为 $n$，值域在 $[0,2^m)$ 的序列 $a$ 的权值为：

$$\oplus_{i=1}^{n-1}a_i\times a_i+1 \pmod 2^m$$

其中 $\oplus$ 为异或操作。

求所有长为 $n$，值域在 $[0,2^m)$ 的 $a$ 的权值和。

$1\leq n,m\leq 2\times 10^5$。

#### Sol

肯定先拆位，在 $\bmod 2^(i+1)$ 的意义下考虑 $i$ 这一位下多少情况下是 $1$。

可以 dp，需要记录上一个数的 lowbit 来规避乘法进位带来的影响，复杂度大概可以做到 $\mathcal{O}(nm^3)$。

你发现 dp 不太有前途，考虑用一些记数方法。

**我们考虑利用双射法，将一个 $i$ 位 $0$ 的序列射到一个 $i$ 位 $1$ 的序列上**。

找出所有偶数位置，如果全是 $0$，那么显然不会有贡献。

否则找出第一个非零位置 $p$，设其 lowbit 为 $2^j$，将 $a_{p-1}$ 异或上 $2^{i-j}$。

可以发现，这样可以把 $i$ 位上 $01$ 取反（因为这里取了 $a_{p}$ 的 lowbit，即使有进位也只能进的更高，不会有影响）。而且一个序列在做一次可以变回原序列。

这样我们就把序列利用双射划分成两种，每种数量相等，因此我们可以直接计算有贡献的序列数量，快速幂即可。

Ad-hoc，尝试将它变的有迹可循：

1. 拆位，这一步是简单的。
2. 处理相邻位的一种常用方法：奇偶位置分开考虑。
3. 双射的应用（不经典）：找到两类元素之间的双射来证明两类元素个数相等。
4. 如何双射？可以找一些“代表量”，常见的方法是找最小的。

### T3

#### 题面

P7922

#### Sol

考虑 $m=1$ 怎么做。

相当于要计算若干个长度为 $k$ 的区间 $\min\/ \max$ 的前缀和。

考虑扫描线，每次向右边加入一个元素 $r$，一个区间 $[l,r]$ 对后面某个点 $t$ 可以贡献到 $f_{t}{t-r~t_l}$，这大概是一个直角 

made buxiangxiele,zhijie tie shang https://caijimk.netlify.app/post/luogup7922-sol

## 5.31

zhzhmns,tnl

$15/100+16/?+5/5=36/?$

### T1

#### 题面

有一张二分图，现在对着这个二分图建一张网络流图：保留左部点连向右部点的边，流量为 $1$，源点向每个左部点连一条流量为对应节点度数减一的边（保证每个左部点度数至少为一），每个右部点向汇点连一条流量为一的边。

跑这个图的最大流，要求最后源点的出边都满流，求最后残量网络的方案数，以及构造一个残量网络。

#### Sol

考虑对每个连通块使用霍尔定理，发现这要求每个连通块边数小于等于点数，说明每个连通块要么是树，要么是基环树。

然后其实就可以硬来了，这里更好的做法是，把边按照是否满流进行定向，相当于每个左部点入度为一，右部点入度至多为一。

如果是树，当钦定某个右部点没有入度时，就可以一下子知道整个连通块的答案，并且此时其它右部点的入度都为一，所以树的答案是右部点个数。

对于基环树，我们给环上钦定边一个顺序，然后可以知道其它点的答案，所以基环树答案是 $2$。

对每个连通块做一遍，答案乘起来即可。

### T2

#### 题面

P7952

#### Sol

因为每个叶子被操作一次就没了，相当于每个点只能操作深度次，后面就没用了。

考虑长链剖分，那么操作相当于每条链 shift 一下，然后链头贡献到父亲上去，暴力做复杂度是每个长链长度加起来乘上平衡树操作，复杂度是对的。

### T3

P8457

#### Sol

魏老师太强了。

## 6.1

$100/100+40/?+20/?=160/?$

### T1

#### 题面

AT_dwacon5th_prelims_d

#### Sol

发现 $(x\pmod d,y\pmod d)$ 相等的点可以划分成一个等价类，然后它们可以变化为任意形状。

那么就把等价类划分出来，然后计算它们变成什么形状（要么是正方形要么是边长差一的长方形）。

考虑把所有等价类都在左下角对齐，即我钦定一个点，然后所有等价类都必须要在这个点的右上方，可以发现这个点也可以对 $d$ 取模。

然后二分答案，算某个等价类能对应到多少个左下角然后求交，这个可以二维前缀和计算。

这样是 $\mathcal{O}(d^2\log d)$。

可以做到 $\mathcal{O}(d^2)$，做法是反过来，我先枚举左下角然后去判定所有等价类能不能都放的下，优化就是考虑一个 trick：不二分了，在当前点判定当前答案合不合法，合法就令答案减一，这样答案只会减 $d$ 次，复杂度就是 $\mathcal{O}(d^2)$。

### T2

#### 题面

AT_dwacon6th_prelims_e

#### Sol

发现直接 dp，要么要记序列被填了哪些，要么要记段被用了哪些，两个记其中一个都是指数级的。

**所以我们钦定段按照从大到小的顺序进行放，这样不仅可以保证我不用记填了哪些东西，还可以保证我每次放的时候，不会完整覆盖之前的段，只会合并两个段。**

发现还是要记序列连续段的信息，**我们考虑不记录连续段的绝对位置，只记录连续段的相对位置，在转移时再枚举两个连续段的距离。**

然后 dp，记 $f_{i,j,k}$ 表示已经放了 $i$ 个段，形成了 $j$ 个连续段，总共覆盖了 $k$ 个点。

枚举这个段是：

1. 被之前段包含
2. 新开一个段
3. 从某个段延伸出去（枚举变长多少）
4. 合并某个段（枚举段和段中间的距离）

直接转移复杂度是 $\mathcal{O}(n^2x^2)$ 的，可以通过保留有效的进行转移，复杂度是 $\mathcal{O}(nx^2)$，或者前缀和差分优化作到 $\mathcal{O}(n^2x)$。

这个 trick 貌似有个名字叫做“线头 dp”，核心思想是，我把 dp 需要记录的东西简化到转移的时候再进行枚举。

还有一种做法是容斥。

考虑钦定若干个点不能经过，剩下随便放的方案数，乘上容斥系数加起来。

发现每个段的方案数只跟比它长的连续段数和连续段长总和有关。

所以我们设 $f_{i,j,k}$ 表示我只放长度大于等于 $i$ 的段，总共放了 $j$ 段，放了的段长度总和为 $k$，直接转移即可。

由于有 $ij\leq k$，所以复杂度可以证明是 $\mathcal{O}(x^3)$ 的。

### T3

#### 题面

AT_arc175_e

#### Sol

大构造狗都不写。

## 6.7

$100/100+25/100+2/?=127/?$

请出题人造一点强的数据。

### T1

#### 题面

CF1149D

#### Sol

发现对于一个只有 $a$ 边的图，任意一条路径都在最小生成树上。

所以先把 $a$ 边连通块缩起来，这样最小生成树可以看作 $a$ 连通块用 $b$ 边连成一棵树。

因此一条路径合法当且仅当不经过重复的 $a$ 连通块。

直接状压跑最短路，复杂度是 $O(2^nm)$ 的。

**关键性质：最优方案中，不需要考虑大小小于等于三的连通块。**

原因是，这样如果重复经过一个连通块，一定不如在自己连通块内部走来的优。

所以只需要状压记录大小不小于四的连通块，复杂度可以做到 $\mathcal{O}(2^{\frac{n}{4}}m)$。

由于大小等于四的连通块，走非法方案变劣的情况很少，对这个东西分讨还可以做到 $\mathcal{O}(2^{\frac{n}{5}}\mathrm{poly}(n,m))$。

只有两种边权的最短路技巧：双队列模拟堆，即如果用 $a$ 边转移扔到第一个队列，否则扔到第二个队列，每次去队头中距离较小的进行转移。

[事实上，本题还可以随机。](https://www.luogu.com.cn/article/ezslgda6)

### T2

#### 题面

ICPC WF 2022 R

#### Sol

先缩边双。

然后单个边双是个环是好做的，相当于判两个序列是不是循环同构，kmp 即可。

假如边双是环套环，咋做啊，可以肯定的是做法必然是找到一个判定方法，而不是尝试去移动序列，因为这种跟图中环有关的东西很容易就 NP-hard 起步了。

大胆猜测只要起始集合和最终集合相等就合法，发现不对，可以被两个共一个点的三元环 hack。

那么需要一点更强的条件。

**这里需要考虑序列（尤其是排列）变换有一个指标：逆序对奇偶性** 这在变换是形如交换或者循环位移时非常有用。

对于一个奇环，转一下会发现序列奇偶性不变，因此图中只有奇环时不能生成全部排列。

对于一个偶环，转一下会改变序列奇偶性，猜测这样可以生成全部排列。

事实上对于偶环套别的环，可以构造使得出现交换两个的操作，所以偶环只需要比较集合相等。

对于奇环，也可以构造出所有逆序对相等的集合，

那么就是先判有没有偶环，然后再 check 逆序对。

注意到图没有偶环一定是一个仙人掌，先把不是仙人掌（可以判每个点双是不是都是简单环）判掉，剩下都是简单环随便判判就好了。

### T3

P8478

#### Sol

## 6.11

$1/100+25/25+5/25=1/150$

怎么有人喜欢把题解写两页写不明白的题放 T2 的。

### T1

#### 题面

AT_agc045_d

#### Sol

考虑策略一定是我先尝试选一个，选到自环就爆了，否则可以一下点亮整个置换环。

这里选法是什么呢？注意到排列是在所有排列中均匀随机的，因此最好的策略就是没有策略：从左往右选。

那么选到自环就爆蛋了，假如第一个自环在 $t$ 处，那么要求 $A$ 后面的数都必须处在一个最小编号小于 $t$ 的置换环中。

枚举 $t$，由于没办法直接钦定 $t$ 是第一个自环，所以容斥一下，钦定前面有 $k$ 个自环。

接下来排列有三段： $t$ 前面第一段里面的数是任意的，$A$ 后面第二段里面的是必须连到 $t$ 前面，$t$ 后面 $A$ 前面第三段也任意。

一个一个放，第一段乱放，第二段必须连前面不能自环，第三段乱放，可以 $\mathcal{O}(1)$ 计算。

总复杂度 $\mtahcal{O}(n+A^2)$。

### T2

#### 题面

AT_wtf22_day1_e

这种题给人做的？？？？

### T3

#### 题面

P9157

#### Sol

$n=1$ 时可以 PN。

## 6.19

$100/100+100/100+0/100=200/300$

有点抽象。

### T1

#### 题面

CF859F

#### Sol

考虑霍尔定理，相当于要求每个左部点子集的出边和节点和大于等于 $m$ 和这个左部点加起来的最小值。

运用一些经典手法，可以把限制从子集变成区间（考虑调整，不是一个区间可以裂成若干不是比较弱的独立区间限制）

那么相当于是对于某个区间有一个和大于等于某个数的限制。

从左往右