<h1 align="center">Codeforces 1726 题解</h1>

[比赛传送门](https://codeforces.com/contest/1726)

## A

### 题意

给定一个长度为 $n$ 数组 $a$，你可以选择一个区间 $[l,r]$，并选择一个位置将这段序列断开，把左面的部分接到右面，求 $a_n-a_1$ 的最大值。

### 题解

存在三种情况：

1. 选取区间为 $[1,n]$，答案为 $\max\limits_{i=1}^{n-1}(a_i-a_{i+1})$。
2. 选取区间为 $[2,n]$，答案为 $(\max\limits_{i=2}^{n}a_i)-a_1$。
3. 选取区间为 $[1,n-1]$，答案为 $a_n-(\min\limits_{i=1}^{n-1}a_i)$。

取三者最大值即可。

### 代码

```c++
// Problem: A. Mainak and Array
// Contest: Codeforces - Codeforces Round #819 (Div. 1 + Div. 2) and Grimoire of Code Annual Contest 2022
// URL: https://codeforces.com/contest/1726/problem/A
// Memory Limit: 256 MB
// Time Limit: 1000 ms
// 
// Powered by CP Editor (https://cpeditor.org)

#include<bits/stdc++.h>
#define miny(x,y) x=min(x,y)
#define maxy(x,y) x=max(x,y)
using namespace std;
namespace Std{
void read(int &x){
	x=0;
	int y=1;
	char c=getchar();
	while(c<'0'||c>'9'){
		if(c=='-')y=-1;
		c=getchar();
	}
	while(c>='0'&&c<='9'){
		x=(x<<1)+(x<<3)+(c&15);
		c=getchar();
	}
	x*=y;
}
int t,n,a[2010];
void main(){
	read(t);
	while(t--){
		read(n);
		int mx=0,mn=1000000000;
		for(int i=1;i<=n;++i){
			read(a[i]);
			maxy(mx,a[i]);
			miny(mn,a[i]);
		}
		int ans=0;
		for(int i=2;i<=n;++i)maxy(ans,a[i-1]-a[i]);
		if(a[n]==mx||a[1]==mn)ans=mx-mn;
		ans=max(ans,max(mx-a[1],a[n]-mn));
		printf("%d\n",ans);
	}
}
}  // namespace Std
int main(){
	Std::main();
	return 0;
}
```

## B

### 题意

一个长度为 $n$ 的正整数数组 $a$，定义一个数组 $p$，$p_i$ 表示原数组中所有严格小于 $a_i$ 的数的异或和，当且仅当 $\forall 1\le i\le n$，$p_i=0$ 时，称数组 $a$ 是有趣的。给定 $n$ 和 $m$，要求构造一个长度为 $n$ 数组 $a$，使得 $a$ 中元素和为 $m$，且 $a$ 是有趣的，要求判无解。

### 题解

首先，这题需要一个结论，$\forall i\le 1\le n$，如果 $a_i$ 不是数组中的最大值，则数组中元素 $a_i$ 的个数为偶数。

证明：~~其实是显然的，~~比 $a_i$ 小的数异或和为 $0$，假设存在一个比 $a_i$ 大的数组中的最小值 $a_j$，为了满足 $p_j=0$，$a_i$ 的个数必须是偶数。

开始分类讨论：

1. $m<n$：无解
2. $n$ 是奇数：构造 $\forall 1\le i<n$，$a_i=1$，$a_n=m-n+1$。
3. $n$ 是偶数，$m$ 是奇数：由于所有不是最大值的数的个数为偶数，则最大值的个数也需要是偶数，假设我们让最大值是 $x$，有 $2\times y$ 个最大值，则我们相当于要继续去构造一个长度为 $n-2\times y$，元素和为 $m-x\times y\times 2$ 的数组，发现这两个数依然分别是偶数和奇数，这个问题会被递归下去，而最终 $n=0$ 时，$m$ 依然是奇数，所以 $m\ne 0$ ，所以无解。
4. $n$ 是偶数，$m$ 是偶数：构造 $\forall 1\le i<n-1$，$a_i=1$，$a_{n-1}=a_n=\frac{m-n+2}{2}$。

### 代码

```c++
// Problem: B. Mainak and Interesting Sequence
// Contest: Codeforces - Codeforces Round #819 (Div. 1 + Div. 2) and Grimoire of Code Annual Contest 2022
// URL: https://codeforces.com/contest/1726/problem/B
// Memory Limit: 256 MB
// Time Limit: 1000 ms
// 
// Powered by CP Editor (https://cpeditor.org)

#include<bits/stdc++.h>
using namespace std;
namespace Std{
void read(int &x){
	x=0;
	int y=1;
	char c=getchar();
	while(c<'0'||c>'9'){
		if(c=='-')y=-1;
		c=getchar();
	}
	while(c>='0'&&c<='9'){
		x=(x<<1)+(x<<3)+(c&15);
		c=getchar();
	}
	x*=y;
}
int t,n,m;
void main(){
	read(t);
	while(t--){
		read(n);read(m);
		if(m<n)puts("No");
		else{
			if(n&1){
			puts("Yes");for(int i=1;i<n;++i)printf("1 ");
			printf("%d\n",m-n+1);}else{
				if(m&1)puts("No");else{
					puts("Yes");
					for(int i=1;i<n-1;++i)printf("1 ");
					printf("%d %d\n",(m-n+2)/2,(m-n+2)/2);
				}
			}
		}
	}
}
}  // namespace Std
int main(){
	Std::main();
	return 0;
}
```

## C

### 题意

给定一个长度为 $2\times n$ 的平衡括号序列，以及一个图，$\forall 1\le l<r\le 2\times n$，如果这个括号序列的 $[l,r]$ 子串是平衡的，则图上节点 $l$ 和节点 $r$ 有连边，否则没有。求图的连通块个数。

### 题解

我们考虑每一条边的 $l$ 和 $r$，$l$ 一定是左括号，$r$ 一定是右括号，所以我们只处理右括号即可得到所有图上边。并且我们发现，两个平衡序列拼起来一定是平衡序列。既然整个序列是平衡的，每一个右括号 $r$ 向左一定有至少一个平衡点，我们考虑找到离他最近的一个平衡点 $l$。如果 $l-1$ 这个位置是右括号，正如上面所说，它的左面也有平衡点，所以相当于两个平衡括号序列拼在了一起，当前连通块与之前的连通块连在了一起，就没有新的连通块产生。但如果 $l-1$ 是左括号，向左没有平衡点，$r$ 和 $l$ 会形成单独的连通块，连通块个数 $+1$。

### 代码

```c++
// Problem: C. Jatayu's Balanced Bracket Sequence
// Contest: Codeforces - Codeforces Round #819 (Div. 1 + Div. 2) and Grimoire of Code Annual Contest 2022
// URL: https://codeforces.com/contest/1726/problem/C
// Memory Limit: 256 MB
// Time Limit: 2000 ms
// 
// Powered by CP Editor (https://cpeditor.org)

#include<bits/stdc++.h>
using namespace std;
namespace Std{
void read(int &x){
	x=0;
	int y=1;
	char c=getchar();
	while(c<'0'||c>'9'){
		if(c=='-')y=-1;
		c=getchar();
	}
	while(c>='0'&&c<='9'){
		x=(x<<1)+(x<<3)+(c&15);
		c=getchar();
	}
	x*=y;
}
int t,n,lst[400010];
char s[400010];
void main(){
	read(t);
	while(t--){
		read(n);
		scanf("%s",s+1);
		int sum=0,ans=0;
		for(int i=1;i<=2*n;++i){
			sum+=((s[i]=='(')?(1):(-1));
			if(s[i]==')'){
				int tmp=lst[sum];
				if(s[tmp]=='('||(!tmp))++ans;
			}else lst[sum-1]=i-1;
		}
		printf("%d\n",ans);
		for(int i=1;i<=2*n;++i){
			sum+=((s[i]=='(')?(1):(-1));
			lst[sum]=0;
		}
	}
}
}  // namespace Std
int main(){
	Std::main();
	return 0;
}
```

## D

### 题意

给定一个 $n$ 个点，$m$ 条边的连通图，$n-1\le m\le n+2$，要求把边染成红蓝两种颜色，设删除蓝色的边之后连通块个数为 $c_1$，删除红边为 $c_2$，求一种方案使 $c_1+c_2$ 最小。

### 题解

我们考虑一条边的最优贡献是将连通块的个数减少 $1$，这种情况的前提是连接的两个顶点原来不连通，即连完没有环。那么我们可以先染一棵树，然后会剩下三条非树边，让这三条非树边无环。我们随便找一棵生成树，如果剩下的三条边成环了，我们直接把其中一条边删掉，设这条边为 $(u,v)$，则可以从树上 $u$ 到 $v$ 的路径上任选一条边，这样剩余三条边一定无环。当然如果觉得这样麻烦，你可以选择：一但成环，就将边随机打乱重跑，直到不成环，几乎是卡不掉的。

### 代码

```c++
// Problem: D. Edge Split
// Contest: Codeforces - Codeforces Round #819 (Div. 1 + Div. 2) and Grimoire of Code Annual Contest 2022
// URL: https://codeforces.com/contest/1726/problem/D
// Memory Limit: 256 MB
// Time Limit: 2000 ms
// 
// Powered by CP Editor (https://cpeditor.org)

#include<bits/stdc++.h>
using namespace std;
namespace Std{
void read(int &x){
	x=0;
	int y=1;
	char c=getchar();
	while(c<'0'||c>'9'){
		if(c=='-')y=-1;
		c=getchar();
	}
	while(c>='0'&&c<='9'){
		x=(x<<1)+(x<<3)+(c&15);
		c=getchar();
	}
	x*=y;
}
int t,n,m,f[1000010];
int find(int x){
	return f[x]==x?x:f[x]=find(f[x]);
}
struct node{
	int l,r,id;
	node(){}
	node(int _l,int _r,int _id){
		l=_l,r=_r,id=_id;
	}
}k[1000010];
int sta[1000010],col[1000010],top;
bool work(void){
	for(int i=1;i<=n;++i)f[i]=i;
	top=0;
	for(int i=1;i<=m;++i){
		int fx=find(k[i].l),fy=find(k[i].r);
		if(fx==fy){
			sta[++top]=i;
		}else f[fx]=fy;
	}
	for(int i=1;i<=n;++i)f[i]=i;
	for(int i=1;i<=top;++i){
		int fx=find(k[sta[i]].l),fy=find(k[sta[i]].r);
		if(fx==fy)return 0;
		f[fx]=fy;
	}
	return 1;
}
void main(){
	read(t);
	while(t--){
		read(n);read(m);
		int u,v;
		for(int i=1;i<=n;++i)f[i]=i;
		for(int i=1;i<=m;++i){
			read(u);read(v);
			k[i]=node(u,v,i);
		}
		while(!work())random_shuffle(k+1,k+1+m);
		int now=1;
		for(int i=1;i<=m;++i){
			if(now<=top&&i==sta[now])col[k[i].id]=1,++now;
			else col[k[i].id]=0;
		}
		for(int i=1;i<=m;++i)printf("%d",col[i]);
		puts("");
	}
}
}  // namespace Std
int main(){
	Std::main();
	return 0;
}
```

## E

### 题意

一个长度为 $n$ 的排列 $p$，他的逆排列 $p^{-1}$ 满足 $p_{p_i^{-1}}=i$，求有多少个长度为 $n$ 的排列 $p$，满足 $\forall i\le 1\le n$，$|p_i-p_i^{-1}|\le 1$。

### 题解

经过手画发现，最终 $p$ 只有一下四种置换环：

1. $a_i=i$。
2. $a_i=j$，$a_j=i$。
3. $a_i=j$，$a_{i+1}=j-1$，$a_{j-1}=i$，$a_j=i+1$。
4. $a_i=j$，$a_{i+1}=j+1$，$a_j=i+1$，$a_{j+1}=i$。

如果只考虑一元环和二元环，可以直接预处理。设 $f_i$ 表示长度为 $i$ 的只有一元环和二元环的排列的个数，则 $f_i=f_{i-2}*(i-1)+f_{i-1}$。

之后考虑四元环的处理，我们枚举有 $i$ 个四元环，则需要 $2\times i$ 组相邻数对，方案数为 $\tbinom{n-2\times i}{2\times i}$，我们发现，四元环的两个相邻数对总有一组是大小反转的，所以需要选出一半是反转的，反转的与未反转的任意顺序连接，则总方案数为 $\sum\limits_{i=0}^{\lfloor \frac{n}{4}\rfloor}\tbinom{n-2\times i}{2\times i}\times \tbinom{2\times i}{i}\times (i!)\times f_{n-4\times i}$。

### 代码

```c++
// Problem: E. Almost Perfect
// Contest: Codeforces - Codeforces Round #819 (Div. 1 + Div. 2) and Grimoire of Code Annual Contest 2022
// URL: https://codeforces.com/contest/1726/problem/E
// Memory Limit: 256 MB
// Time Limit: 3000 ms
// 
// Powered by CP Editor (https://cpeditor.org)

#include<bits/stdc++.h>
#define int long long
#define mod 998244353
using namespace std;
namespace Std{
void read(int &x){
	x=0;
	int y=1;
	char c=getchar();
	while(c<'0'||c>'9'){
		if(c=='-')y=-1;
		c=getchar();
	}
	while(c>='0'&&c<='9'){
		x=(x<<1)+(x<<3)+(c&15);
		c=getchar();
	}
	x*=y;
}
int qp(int x,int y){
	int cmp=1;
	while(y){
		if(y&1)(cmp*=x)%=mod;
		(x*=x)%=mod;
		y>>=1;
	}
	return cmp;
}
int fac[300010],ifac[300010],f[300010],n,t;
int C(int x,int y){
	if(y>x)return 0;
	return fac[x]*ifac[y]%mod*ifac[x-y]%mod;
}
void main(){
	fac[0]=1;
	for(int i=1;i<=300000;++i)fac[i]=(fac[i-1]*i)%mod;
	ifac[300000]=qp(fac[300000],mod-2);
	for(int i=299999;~i;--i)ifac[i]=(ifac[i+1]*(i+1))%mod;
	f[0]=f[1]=1;
	for(int i=2;i<=300000;++i)f[i]=(f[i-2]*(i-1)+f[i-1])%mod;
	read(t);
	while(t--){
		read(n);
		int ans=0;
		for(int i=0;i*4<=n;++i){
			ans+=C(n-2*i,2*i)*C(2*i,i)%mod*fac[i]%mod*f[n-4*i]%mod;
		}
		printf("%lld\n",ans%mod);
	}
}
}  // namespace Std
#undef int
int main(){
	Std::main();
	return 0;
}
```

