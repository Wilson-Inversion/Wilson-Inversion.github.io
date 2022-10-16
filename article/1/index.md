<h1 align="center">莫比乌斯反演，杜教筛</h1>

## 积性函数

**定义**：若 $a$ 与 $b$ 互质，且 $f(ab)=f(a)f(b)$，则 $f(x)$ 是积性函数。

## 狄利克雷卷积

$$
\begin{equation}
(f*g)(n)=\sum\limits_{d|n}f(d)g(\frac{n}{d})
\end{equation}
$$

**性质**：

1. 若 $f$ 和 $g$ 都是积性函数，则 $f*g$ 是积性函数。

* 证明：设 $a$ 与 $b$ 互质，$t=f*g$，$t(a)=\sum\limits_{d|a}f(d)g(\frac{a}{d})$，$t(b)=\sum\limits_{d|b}f(d)g(\frac{b}{d})$，$t(ab)=\sum\limits_{d|ab}f(d)g(\frac{ab}{d})$，则：

$$
\begin{equation}
\begin{aligned}
t(a)t(b)
&=\sum\limits_{d_1|a}f(d_1)g(\frac{a}{d_1})\sum\limits_{d_2|b}f(d_2)g(\frac{b}{d_2})\\ 
&=\sum\limits_{d_1|a}\sum\limits_{d_2|b}f(d_1)g(\frac{a}{d_1})f(d_2)g(\frac{b}{d_2})\\
&=\sum\limits_{d_1|a}\sum\limits_{d_2|b}f(d_1d_2)g(\frac{ab}{d_1d_2})\\
&=\sum\limits_{d|ab}f(d)g(\frac{ab}{d})\\
&=t(ab)
\end{aligned}
\end{equation}
$$

2. 交换律：$f*g=g*f$。

3. 结合律：$(f*g)*h=f*(g*h)$。

* 证明：

  $$
  \begin{equation}
  \begin{aligned}
  ((f*g)*h)(n)
  &=\sum\limits_{z|n}(f*g)(z)h(\frac{n}{z})\\
  &=\sum\limits_{z|n}\sum\limits_{x|z}f(x)g(\frac{z}{x})h(\frac{n}{z})\\
  &=\sum\limits_{xyz=n}f(x)g(y)h(z)
  \end{aligned}
  \end{equation}
  $$

  后者同理，命题得证。

**常用函数**：
$$
\begin{equation}
\varepsilon(n)=[n=1]
\end{equation}
$$

$$
\begin{equation}
\sigma_k(n)=\sum\limits_{d|n}d^k
\end{equation}
$$

$$
\begin{equation}
\text{Id}_k(n)=n^k
\end{equation}
$$

$$
\begin{equation}
\varphi(n)=\sum\limits_{i=1}^{n}[gcd(i,n)=1]
\end{equation}
$$

## 莫比乌斯函数

**定义**：
$$
\mu(n)=
\left\{\begin{array}{l}
1 &n=1 \\ 
(-1)^{k} &n=a_1\times a_2\times……\times a_k,a_i\in prime \\ 
0 &otherwise
\end{array}\right.
$$

**常用卷积**：

1. $$
   \begin{equation}
   f*\varepsilon=f
   \end{equation}
   $$
   
2. 
   $$
   \begin{equation}
   \mu*\text{Id}_0=\varepsilon(n)
   \end{equation}
   $$

* 证明：
  
  $n=1$ 时明显成立，$n\ne 1$ 时，设 $n=\prod\limits_{i=1}^{k}p_i^{c_i}$，$n'=\prod\limits_{i=1}^{k}p_i$，则：
  $$
  \begin{equation}
  \sum\limits_{d|n}\mu(d)=\sum\limits_{d|n'}\mu(d)=\sum\limits_{i=0}^{k}\binom{k}{i}(-1)^i=(1+(-1))^k
  \end{equation}
  $$
  
  当 $k\ne 0$ 时，$(1+(-1))^k$ 恒等于 $1$。
  
  即 $\mu*\text{Id}_0=\varepsilon(n)$。
  
3. 
   $$
   \begin{equation}
   \varphi*\text{Id}_0=\text{Id}_1
   \end{equation}
   $$
   
* 证明：仅考虑实际意义，$\sum\limits_{d|n}\varphi(\frac{n}{d})$ 的实际含义为所有 $\le n$ 的数中与 $n$ 的 $\gcd$ 为 $d$ 的数的数量，而枚举的 $d$ 包含了 $\gcd$ 的所有情况，故结论成立。

4. 
   $$
   \begin{equation}
   \mu*\text{Id}_1=\varphi
   \end{equation}
   $$

* 证明：
   $$
   \begin{equation}
   \begin{aligned}
   \mu*\text{Id}_1
   &=\mu*\text{Id}_0*\varphi\\
   &=\varepsilon*\varphi\\
   &=\varphi
   \end{aligned}
   \end{equation}
   $$

## 莫比乌斯反演

如果 $f(n)=\sum\limits_{d|n}g(d)$，则 $g(n)=\sum\limits_{d|n}\mu(d)f(\frac{n}{d})$。

* 证明：
  $$
  f=g*\text{Id}_0\\
  f*\mu=(g*\text{Id}_0)*\mu\\
  g=f*\mu
  $$

### 例题

#### [$\color{black}\text{P4450 双亲数}$](https://www.luogu.com.cn/problem/P4450)

题意：求 $\sum\limits_{i=1}^{A}\sum\limits_{j=1}^{B}[\gcd(i,j)=D]$，$1\le A,B\le 10^6$，$1\le D\le \min(A,B)$。

$$
\begin{equation}
\begin{aligned}
\sum\limits_{i=1}^{A}\sum\limits_{j=1}^{B}[\gcd(i,j)=D]
&=\sum\limits_{i=1}^{\lfloor\frac{A}{D}\rfloor}\sum\limits_{j=1}^{\lfloor\frac{B}{D}\rfloor}[\gcd(i,j)=1]\\
&=\sum\limits_{i=1}^{\lfloor\frac{A}{D}\rfloor}\sum\limits_{j=1}^{\lfloor\frac{B}{D}\rfloor}\sum\limits_{e|\gcd(i,j)}\mu(e)\\
&=\sum\limits_{e=1}^{\min(\lfloor\frac{A}{D}\rfloor,\lfloor\frac{B}{D}\rfloor)}\mu(e)\lfloor\frac{A}{De}\rfloor\lfloor\frac{B}{De}\rfloor
\end{aligned}
\end{equation}
$$

#### [$\color{black}\text{P1390 公约数的和}$](https://www.luogu.com.cn/problem/P1390)

题意：求 $\sum\limits_{i=1}^{n}\sum\limits_{j=i+1}^{n}\gcd(i,j)$，$2\le n\le 2\times10^6$。

$$
\begin{equation}
\begin{aligned}
\sum\limits_{i=1}^{n}\sum\limits_{j=i+1}^{n}\gcd(i,j)
&=\frac{(\sum\limits_{i=1}^{n}\sum\limits_{j=1}^{n}\gcd(i,j))-(\sum\limits_{i=1}^{n}i)}{2}\\
&=\frac{(\sum\limits_{d=1}^{n}\sum\limits_{i=1}^{\lfloor\frac{n}{d}\rfloor}\sum\limits_{j=1}^{\lfloor\frac{n}{d}\rfloor}[\gcd(i,j)=1])-(\sum\limits_{i=1}^{n}i)}{2}\\
&=\frac{(\sum\limits_{d=1}^{n}d\sum\limits_{i=1}^{\lfloor\frac{n}{d}\rfloor}\sum\limits_{j=1}^{\lfloor\frac{n}{d}\rfloor}[\gcd(i,j)=1])-(\sum\limits_{i=1}^{n}i)}{2}\\
&=\frac{(\sum\limits_{d=1}^{n}d\sum\limits_{e=1}^{\lfloor\frac{n}{d}\rfloor}\mu(e)(\lfloor\frac{n}{de}\rfloor)^2)-(\sum\limits_{i=1}^{n}i)}{2}\\
&=\frac{(\sum\limits_{T=1}^{n}\sum\limits_{d|T}d\mu(\frac{T}{d})(\lfloor\frac{n}{T}\rfloor)^2)-(\sum\limits_{i=1}^{n}i)}{2}\\
&=\frac{(\sum\limits_{T=1}^{n}\varphi(T)(\lfloor\frac{n}{T}\rfloor)^2)-(\sum\limits_{i=1}^{n}i)}{2}
\end{aligned}
\end{equation}
$$

#### [$\color{black}\text{P2257 YY的GCD}$](https://www.luogu.com.cn/problem/P2257)

题意：多测，给定 $n,m$，求 $1≤x≤n$，$1≤y≤m$ 且 $gcd⁡(x,y)$ 为质数的 $(x,y)$ 有多少对。$1\le T\le 10^4$，$1\le n,m\le 10^6$。
$$
\begin{equation}
\begin{aligned}
\sum\limits_{k=2,k\in prime}^{n}\sum\limits_{i=1}^{n}\sum\limits_{j=1}^{m}[\gcd(i,j)=k]
&=\sum\limits_{k=2,k\in prime}^{n}\sum\limits_{i=1}^{\lfloor\frac{n}{k}\rfloor}\sum\limits_{j=1}^{\lfloor\frac{m}{k}\rfloor}[\gcd(i,j)=1]\\
&=\sum\limits_{k=2,k\in prime}^{n}\sum\limits_{i=1}^{\lfloor\frac{n}{k}\rfloor}\sum\limits_{j=1}^{\lfloor\frac{m}{k}\rfloor}\sum\limits_{e|\gcd(i,j)}\mu(e)\\
&=\sum\limits_{k=2,k\in prime}\sum\limits_{e=1}^{\lfloor\frac{n}{k}\rfloor}\mu(e)\lfloor\frac{n}{ke}\rfloor \lfloor\frac{m}{ke}\rfloor\\
&=\sum\limits_{T=1}^{n}\lfloor\frac{n}{T}\rfloor \lfloor\frac{m}{T}\rfloor\sum\limits_{k|T,k\in prime}\mu(\frac{T}{k})
\end{aligned}
\end{equation}
$$
后面的部分可以通过外层枚举 $k$，内层枚举 $\frac{T}{k}$ 来预处理。

#### [$\color{black}\text{P1829 [国家集训队]Crash的数字表格}$](https://www.luogu.com.cn/problem/P1829)

题意：给定 $n,m$，求 $\sum\limits_{i=1}^{n}\sum\limits_{j=1}^{m}\text{lcm}(i,j)$，$1\le n,m\le10^7$。
$$
\begin{equation}
\begin{aligned}
\sum\limits_{i=1}^{n}\sum\limits_{j=1}^{m}\text{lcm}(i,j)
&=\sum\limits_{i=1}^{n}\sum\limits_{j=1}^{m}\frac{ij}{\gcd(i,j)}\\
&=\sum\limits_{i=1}^{n}\sum\limits_{j=1}^{m}\frac{i}{\gcd(i,j)}\frac{j}{\gcd(i,j)}\gcd(i,j)\\
&=\sum\limits_{d=1}^{n}d\sum\limits_{i=1}^{\lfloor\frac{n}{d}\rfloor}\sum\limits_{j=1}^{\lfloor\frac{m}{d}\rfloor}ij[\gcd(i,j)=1]\\
&=\sum\limits_{d=1}^{n}d\sum\limits_{e=1}^{\lfloor\frac{n}{d}\rfloor}\mu(e)e^2\frac{\lfloor\frac{n}{de}\rfloor(\lfloor\frac{n}{de}\rfloor+1)}{2}\frac{\lfloor\frac{m}{de}\rfloor(\lfloor\frac{m}{de}\rfloor+1)}{2}\\
\end{aligned}
\end{equation}
$$
前后分别数论分块即可。

#### [$\color{black}\text{P3327 [SDOI2015]约数个数和}$](https://www.luogu.com.cn/problem/P3327)

题意：多测，求 $\sum\limits_{i=1}^{n}\sum\limits_{j=1}^{m}d(ij)$，$d(x)$ 表示 $x$ 的约数个数（$\sigma_1(x)$）。$1\le T,n,m\le5\times10^4$

结论：$d(ij)=\sum\limits_{k|i}\sum\limits_{l|j}[\gcd(k,l)=1]$。

证明：我们钦定 $ij$ 的一个约数 $k$，考虑 $k$ 的一个质因子 $p$，在 $i$ 中有 $p_i$ 个，$j$ 中 $p_j$ 个，$k$ 中 $p_k$ 个，分两种情况：

1. $p_k\le p_i$，我们让 $k$ 中的所有 $p$ 这个因子全部从 $i$ 中选。
2. $p_k>p_i$，这种情况我们可以钦定，不从 $i$ 中选，仅从 $j$ 中选择 $p_k-p_i$ 个。

以上情况均满足上式，且包含了所有情况，故命题得证。
$$
\begin{equation}
\begin{aligned}
\sum\limits_{i=1}^{n}\sum\limits_{j=1}^{m}d(ij)
&=\sum\limits_{i=1}^{n}\sum\limits_{j=1}^{m}\sum\limits_{k|i}\sum\limits_{l|j}[\gcd(k,l)=1]\\
&=\sum\limits_{i=1}^{n}\sum\limits_{j=1}^{m}[\gcd(i,j)=1]\lfloor\frac{n}{i}\rfloor\lfloor\frac{m}{j}\rfloor\\
&=\sum\limits_{i=1}^{n}\sum\limits_{j=1}^{m}\sum\limits_{e|\gcd(i,j)}\mu(e)\lfloor\frac{n}{i}\rfloor\lfloor\frac{m}{j}\rfloor\\
&=\sum\limits_{e=1}^{n}\mu(e)\sum\limits_{i=1}^{\lfloor\frac{n}{e}\rfloor}\sum\limits_{j=1}^{\lfloor\frac{m}{e}\rfloor}\lfloor\frac{n}{ie}\rfloor\lfloor\frac{m}{je}\rfloor
\end{aligned}
\end{equation}
$$
设 $f(x)=\sum\limits_{i=1}^{x}\lfloor\frac{x}{i}\rfloor$，原式变为 $\sum\limits_{e=1}^{n}\mu(e)f(\lfloor\frac{n}{e}\rfloor)f(\lfloor\frac{m}{e}\rfloor)$，$f(x)$ 可以 $O(n\sqrt{n})$ 预处理，然后整除分块即可。

#### [$\color{black}\text{P3172 [CQOI2015]选数}$](https://www.luogu.com.cn/problem/P3172)

题意：给定 $l,r$，有 $n$ 个数，每个数的值可以取 $[l,r]$ 之间的任何数（可重），求这些数的最大公约数恰好为 $k$ 的情况数。$1\le n,k\le10^9$，$1\le l\le r\le 10^9$，$r-l\le 10^5$。

我们可以 $l\gets\lfloor\frac{l-1}{k}\rfloor$，$r\gets\lfloor\frac{r}{k}\rfloor$，转化为每个数的取值为 $[l+1,r]$，求最大公约数是 $1$ 的方案数。
$$
\begin{equation}
\begin{aligned}
\sum\limits_{i_1=l+1}^{r}\sum\limits_{i_2=l+1}^{r}...\sum\limits_{i_n=l+1}^{r}[\gcd(i)=1]
&=\sum\limits_{i_1=l+1}^{r}\sum\limits_{i_2=l+1}^{r}...\sum\limits_{i_n=l+1}^{r}\sum\limits_{e|\gcd(i)}\mu(e)\\
&=\sum\limits_{e=1}^{r}\mu(e)(\lfloor\frac{r}{e}\rfloor-\lfloor\frac{l}{e}\rfloor)^n
\end{aligned}
\end{equation}
$$
看起来像杜教筛，杜教筛是这题的一种解法，但也可以不用。

我们考虑 $\gcd$ 的值的范围：

1. 如果所有数都一样，$\gcd$ 是它本身。
2. 其他情况，根据辗转相减原理， $\gcd$ 的范围为 $[1,r-l]$。

所以我们分成两部分来算，所有数都相等的部分如果 $l=0$ 则有一次贡献，否则没有贡献。

存在不同的数，枚举 $\mu$ 的范围为 $[1,r-l]$ ，这样复杂度就是对的。

#### [$\color{black}\text{P4449 于神之怒加强版}$](https://www.luogu.com.cn/problem/P4449)

题意：多测，同一数据点的每组数据的 $k$ 都相等，求 $\sum\limits_{i=1}^{n}\sum\limits_{j=1}^{m}\gcd(i,j)^k$，$1\le T\le2\times10^3$，$1\le n,m,k\le5\times10^6$。
$$
\begin{equation}
\begin{aligned}
\sum\limits_{i=1}^{n}\sum\limits_{j=1}^{m}\gcd(i,j)^k
&=\sum\limits_{d=1}^{n}d^k\sum\limits_{i=1}^{\lfloor\frac{n}{d}\rfloor}\sum\limits_{j=1}^{\lfloor\frac{m}{d}\rfloor}[\gcd(i,j)=1]\\
&=\sum\limits_{d=1}^{n}d^k\sum\limits_{e=1}^{\lfloor\frac{n}{d}\rfloor}\mu(e)\lfloor\frac{n}{de}\rfloor\lfloor\frac{m}{de}\rfloor\\
&=\sum\limits_{T=1}^{n}\lfloor\frac{n}{T}\rfloor\lfloor\frac{m}{T}\rfloor\sum\limits_{d|T}d^k\mu(\frac{T}{d})
\end{aligned}
\end{equation}
$$

#### [$\color{black}\text{P3312 [SDOI2014]数表}$](https://www.luogu.com.cn/problem/P3312)

题意：多测，求 $\sum\limits_{i=1}^{n}\sum\limits_{j=1}^{m}\sigma_1(\gcd(i,j))[\sigma_1(\gcd(i,j))\le a]$。$1\le T\le2\times10^4$，$1\le n,m\le10^5$，$|a|\le 10^9$。

先考虑没有 $a$ 的限制：
$$
\begin{equation}
\begin{aligned}
\sum\limits_{i=1}^{n}\sum\limits_{j=1}^{m}\sigma_1(\gcd(i,j))
&=\sum\limits_{d=1}^{n}\sigma_1(d)\sum\limits_{i=1}^{\lfloor\frac{n}{d}\rfloor}\sum\limits_{j=1}^{\lfloor\frac{m}{d}\rfloor}[\gcd(i,j)=1]\\
&=\sum\limits_{d=1}^{n}\sigma_1(d)\sum\limits_{e=1}^{\lfloor\frac{n}{d}\rfloor}\mu(e)\lfloor\frac{n}{de}\rfloor \lfloor\frac{m}{de}\rfloor\\
&=\sum\limits_{T=1}^{n}\sum\limits_{d|T}\sigma_1(d)\mu(\frac{T}{d})\lfloor\frac{n}{T}\rfloor \lfloor\frac{m}{T}\rfloor
\end{aligned}
\end{equation}
$$
有 $a$ 的限制，但如果各询问 $a$ 是单调的，这个问题就变成了单点修改区间查询问题，所以把询问离线并按照 $a$ 排序，考虑 $a$ 增大时，也会有一些新的 $\sigma_1(d)$ 会对所有 $d$ 的倍数产生贡献，可以在树状数组上进行 $\frac{n}{d}$ 次单点修改，再进行 $\sqrt{n}$ 次区间查询，总复杂度为 $O(n\ln(n)\log(n)+T\sqrt{n}\log(n))$。

#### [$\color{black}\text{P6222 「P6156 简单题」加强版}$](https://www.luogu.com.cn/problem/P6222)

题意：多测，同一数据点的每组数据的 $k$ 都相等，求 $\sum\limits_{i=1}^{n}\sum\limits_{j=1}^{n}(i+j)^k\gcd(i,j)\mu^2(\gcd(i,j))$，$1\le n\le10^7$，$1\le T\le2\times10^4$，$1\le k\le2^{31}$。
$$
\begin{equation}
\begin{aligned}
\sum\limits_{i=1}^{n}\sum\limits_{j=1}^{n}(i+j)^k\gcd(i,j)\mu^2(\gcd(i,j))
&=\sum\limits_{d=1}^{n}d\mu^2(d)d^k\sum\limits_{i=1}^{\lfloor\frac{n}{d}\rfloor}\sum\limits_{j=1}^{\lfloor\frac{n}{d}\rfloor}[\gcd(i,j)=1](i+j)^k\\
&=\sum\limits_{d=1}^{n}d\mu^2(d)d^k\sum\limits_{i=1}^{\lfloor\frac{n}{d}\rfloor}\sum\limits_{j=1}^{\lfloor\frac{n}{d}\rfloor}(i+j)^k\sum\limits_{e|\gcd(i,j)}\mu(e)\\
&=\sum\limits_{d=1}^{n}d\mu^2(d)d^k\sum\limits_{e=1}^{\lfloor\frac{n}{d}\rfloor}\mu(e)e^k\sum\limits_{i=1}^{\lfloor\frac{n}{de}\rfloor}\sum\limits_{j=1}^{\lfloor\frac{n}{de}\rfloor}(i+j)^k\\
&=\sum\limits_{T=1}^{n}T^k\sum\limits_{d|T}d\mu^2(d)\mu(\frac{T}{d})\sum\limits_{i=1}^{\lfloor\frac{n}{T}\rfloor}\sum\limits_{j=1}^{\lfloor\frac{n}{T}\rfloor}(i+j)^k\\
\end{aligned}
\end{equation}
$$
设 $S_1(x)=\sum\limits_{i=1}^{x}i^k$ ，$S_2(x)=\sum\limits_{i=1}^{x}S_1(i)$，$g(x)=\sum\limits_{i=1}^{x}\sum\limits_{j=1}^{x}(i+j)^k$。
$$
\begin{equation}
\begin{aligned}
g(x)
&=\sum\limits_{i=1}^{x}\sum\limits_{j=1}^{x}(i+j)^k\\
&=\sum\limits_{i=1}^{x}\sum_{j=i+1}^{i+x}j^k\\
&=\sum\limits_{i=1}^{x}S_1(i+x)-S_1(i)\\
&=\sum\limits_{i=x+1}^{2\times x}S_1(i)-\sum\limits_{i=1}^{x}S_1(i)\\
&=S_2(2\times x)-2S_2(x)
\end{aligned}
\end{equation}
$$
$S_2$ 可以预处理，现在复杂度在于求 $f(x)=\sum\limits_{d|x}d\mu^2(d)\mu(\frac{x}{d})$，由于是两个积性函数卷起来，$f(x)$ 是积性函数，考虑 $f(x)$ 的处理方法。

1. $f(1)=1$。
2. $x$ 有不止一个质因子，则 $f(x)=f(d^k)\times f(\frac{x}{d^k})$，其中 $d$ 是最小质因子，$k$ 是其个数。
3. $x$ 只有一个质因子，令 $x=d^k$：
   * $k=1$：$f(x)=x-1$。
   * $k=2$：$f(x)=-d$。
   * $k\ge 3$：考虑到无论怎么分，$\mu(d)$ 和 $\mu(\frac{n}{d})$ 至少有一个是平方因子，所以结果是 $0$。

所以可以记录每个数的最小质因子的个数来更新。

## 杜教筛

$f(x)$ 是积性函数，求 $f(x)$ 的前缀和。

设 $f(x)$ 的前缀和是 $S(x)$，我们钦定一个积性函数 $g(x)$，接下来表示一下 $f*g$ 的前缀和：
$$
\begin{equation}
\begin{aligned}
\sum\limits_{i=1}^{n}(f*g)(i)
&=\sum\limits_{i=1}^{n}\sum\limits_{j|i}g(j)f(\frac{i}{j})\\
&=\sum\limits_{j=1}^{n}g(j)\sum\limits_{i=1}^{\lfloor\frac{n}{j}\rfloor}f(i)\\
&=\sum\limits_{i=1}^{n}g(i)S(\lfloor\frac{n}{i}\rfloor)
\end{aligned}
\end{equation}
$$
现在要求 $S(n)$，如果我们知道 $g(1)$，可以把这个问题转化为求 $g(1)S(n)$：
$$
\begin{equation}
\begin{aligned}
g(1)S(n)
&=\sum\limits_{i=1}^{n}g(i)S(\lfloor\frac{n}{i}\rfloor)-\sum\limits_{i=2}^{n}g(i)S(\lfloor\frac{n}{i}\rfloor)\\
&=\sum\limits_{i=1}^{n}(f*g)(n)-\sum_{i=2}^{n}g(i)S(\lfloor\frac{n}{i}\rfloor)
\end{aligned}
\end{equation}
$$
如果我们能够快速求出 $f*g$ 的前缀和和 $g$ 的前缀和，第二部分使用整除分块递归处理，并记忆化，可以实现 $O(n^{\frac{3}{4}})$ 求解；如果能够预处理出 $S$ 的前 $n^{\frac{2}{3}}$，复杂度可以降低为 $O(n^{\frac{2}{3}})$。

### 例题

#### 求 $\sum\limits_{i=1}^{n}\varphi(i)$

$$
\varphi*\text{1}=id
$$

#### 求 $\sum\limits_{i=1}^{n}\mu(i)$

$$
\mu*\text{1}=\varepsilon
$$

#### [$\color{black}\text{P3768 简单的数学题}$](https://www.luogu.com.cn/problem/P3768)

题意：求 $\sum\limits_{i=1}^{n}\sum\limits_{j=1}^{n}ij\gcd(i,j)$，$n\le10^{10}$。
$$
\begin{equation}
\begin{aligned}
\sum\limits_{i=1}^{n}\sum\limits_{j=1}^{n}ij\gcd(i,j)
&=\sum\limits_{d=1}^{n}d^3\sum\limits_{i=1}^{\lfloor\frac{n}{d}\rfloor}\sum\limits_{j=1}^{\lfloor\frac{n}{d}\rfloor}ij[\gcd(i,j)=1]\\
&=\sum\limits_{d=1}^{n}d^3\sum\limits_{e=1}^{\lfloor\frac{n}{d}\rfloor}\mu(e)e^2\sum\limits_{i=1}^{\lfloor\frac{n}{de}\rfloor}\sum\limits_{j=1}^{\lfloor\frac{n}{de}\rfloor}ij\\
&=\sum\limits_{T=1}^{n}T^2\sum\limits_{d|T}d\mu(\frac{T}{d})\sum\limits_{i=1}^{\lfloor\frac{n}{T}\rfloor}\sum\limits_{j=1}^{\lfloor\frac{n}{T}\rfloor}ij\\
&=\sum\limits_{T=1}^{n}T^2\varphi(T)\sum\limits_{i=1}^{\lfloor\frac{n}{T}\rfloor}\sum\limits_{j=1}^{\lfloor\frac{n}{T}\rfloor}ij
\end{aligned}
\end{equation}
$$
后面可以整除分块，关键复杂度在于前面，设 $f(x)=x^2\varphi(x)$，$g(x)=x^2$。
$$
\begin{equation}
\begin{aligned}
(f*g)(x)
&=\sum\limits_{d|x}d^2\varphi(d)(\frac{x}{d})^2\\
&=x^2\sum\limits_{d|x}\varphi(d)\\
&=x^3
\end{aligned}
\end{equation}
$$
$\sum\limits_{i=1}^{n}i^3=(\sum\limits_{i=1}^{n}i)^2$，这样就可以快速求出 $g$ 和 $f*g$ 的前缀和了。
