<h1 align="center">莫比乌斯反演</h1>

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

**性质**：
$$
\begin{equation}
\sum\limits_{d|n}\mu(d)=\varepsilon(n)
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

## 莫比乌斯反演

