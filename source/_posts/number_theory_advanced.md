---
title: 蒟蒻的进阶数论
date: 2021-12-16 21:56:08
tags:
  - 数学
categories: 学习笔记
author: Jocker_CW
mathjax: true
---

# $\color{#9F00FF}{\text{进阶数论}}$

### 没错 ~~终于到~~ 进阶数论了。

## 目录：

>   1. $Min\_25$筛

>   2. $BSGS$和$exBSGS$

>   3. 原根和阶

>   4. 进阶$BSGS$

### $\color{#F00F9F}{1.Min\_25\text{筛}}$:

#### **简介：** 一种可以在亚线性时间内解决积性函数或完全积性函数的前缀和问题的算法。

#### **关于名字：** 顾名思义，就是 ~~张$Min$在$25$岁发明的算法~~ 一个叫做$Min\_25$的神犇发明的算法。

#### **积性函数：** $\forall x,y,\gcd (x,y)=1.  \ f(x\times y)=f(x)\times f(y)$。

#### **完全积性函数：** $\forall x,y.  \ f(x\times y)=f(x)\times f(y)$

#### **算法复杂度:** 约为$\Theta(\frac{n^{\frac{3}{4}}}{log_2 n})$或$\Theta(n^{1-\epsilon})$

#### **定义：**
$p$，质数

$p_k$，第$k$小的质数，特别的，令$p_0=1$

$LPF(x)$，$x$的最小质因子

#### 该算法适合应用的场景：$F(p)$可以快速计算或是低次的多项式，$F(p^c)$可以快速计算。

#### **正文开始**

目标:$\sum\limits_{i=1}^{m}F(i)$

对于积性函数$F(x)$，我们设函数$f(x)$在$x$是质数时和$F(x)$相等。

我们设$g(n,x)=\sum\limits_{i=1}^{n}{[i\in p\  or\ LPF(i)>p_x]\times f(i)}$

~~先不要问他有什么用，一会你们就知道了~~

考虑递推一下？

$Hint1:$我们考虑从$g(n,x-1)$推过来。

$Hint2:$哪些东西要减去？没有贡献的？

首先我们来看，当$p_x^2> n$时，$g(n,x)=g(n,x-1)$。显然没有多余的贡献要减去，因为如果有的话，那么这个被减去的数字可以被表示为$a\times p_x$，如果说$a<p_x$显然是没有贡献的，因为这个时候$a$可以被质因数分解成若干个小于$p_x$的质数，这些数字会在计算$g(n,x-1)$的时候或之前就被减掉了。而如果$a\geq p_x$的话，就与我们$p_x^2>n$的条件矛盾了。

然后再看$p_x^2\leq n$的情况。这个时候我们要减去些什么？显然，那些是$p_x$的非$1$倍数的数字都得消失。因为我们已经设得$f(x)$是完全积性函数，所以我们可以提出来一个$f(p_x)$，以使得剩下的数字中的最大质因子大于$p_k$，同时我们在这里又不小心多扫掉了一些质数。于是我们要把他们搞回来。于是关于这种情况的递推式子就出来了$g(n,x)=g(n,x-1)-f(p_x)\times(g(\lfloor \frac{n}{p_x}\rfloor,x-1)-\sum\limits_{i=1}^{x-1}f(p_i))$

最后我们得到了：

$g(n,x)=
\begin{cases}
g(n,x-1)\ \ p_x^2> n\\
g(n,x-1)-f(p_x)\times(g(\lfloor \frac{n}{p_x}\rfloor,x-1)-\sum\limits_{i=1}^{x-1}f(p_i))\ \ p_x^2\leq n
\end{cases}$

好了，如果你看到这里就开始晕了，~~这边建议您$fangchi$~~

恭喜您看到这里已经成功地看懂了我们真正需要的递推式的一个要用到的条件。

现在我们构造一个新的递推。

考虑$S(n,x)$表示$\sum\limits_{i=1}^{x}[LPF(i)>p_x]\times F(i)$

来吧是时候继续递推了。

$Hint1:$考虑一下质合分解？

$Hint2:$合数部分考虑一下枚举最小质因数和幂次？

首先我们将它分为质数和合数两部分。

$|P|_m$表示$p_k\leq \sqrt m$的最大的$k$

质数部分：$g(n,|p|_n)-\sum\limits_{i=1}^j f(i)$

合数部分：$\sum\limits_{k>j}\sum\limits_{e=1}^{p_k^e\leq n}F(p_k^e)\times(S(\lfloor \frac{n}{p_x^e}\rfloor,k)+[e>1])$

蛤？你问为什么合数部分还有加一？

因为在我们的$S$并没有计算$1$的部分，$e=1$的时候前面枚举质数的时候已经算到了，而其他的情况需要这个 $1$。

好滴我们分析一下复杂度。。。。。计算$g(1-n,|p|_n)$啊这一步就已经$\Theta(n)$了？

别急，我们现在发现，我们在递推的时候每一次调用的$n$是什么？$\lfloor \frac{n}{k}\rfloor$对吧？

这玩意取值多么？

一共就$2\sqrt n$种好吧？

下标再离散化一下，直接解决内存和时间问题！

所以答案是什么？？你在问什么？显然是和$S(m,0)$有关系，因为$S(n,x)$不计算$F(1)$，最后再加上就好了！

上代码。这是$LG$上的模板题

#### [$\color{#0E1D69}{\text {P5325 【模板】Min\_25筛} }$](https://www.luogu.com.cn/problem/P5325)

```cpp
#include <bits/stdc++.h>
using namespace std;
inlind re(int &x){
    x=0;
    bool f=0;
    int w=getchar();
    while(w<'0'||w>'9'){if(w=='-')f=1;w=getchar();}
    while(w<='9'&&w>='0'){
        x=(x<<3)+(x<<1)+w-'0';
        w=getchar();
    }
    if(f) x=-x;
}
inlind wr(int x){
    if(x<0){
        putchar('-');
        x=-x;
    }
    if(x>9)wr(x/10);
    putchar(x%10+'0');
}
const int inv6=166666668;
const int M=1e9+7;
int n;
int p[200300];
bool pr[200300];
int tot;
int sqr;
int sum1c[200300];
int sum2c[203000];
int ids[200300],idb[200300];
int g2k[200300],g1k[200300];//gk=g2k-g1k;
int val[200300];
int cnt;
inlind o(){
    for(int i=2;i<=sqr;i++){
        if(!pr[i]) p[++tot]=i;
        for(int j=1;j<=tot&&i*p[j]<=sqr;j++){
            pr[i*p[j]]=1;
            if(i%p[j]==0) break;
        }
    }
    for(int i=1;i<=tot;i++)sum1c[i]=(sum1c[i-1]+p[i])%M,sum2c[i]=(sum2c[i-1]+p[i]*p[i]%M)%M;

}
inlint calc1c(int x){
    x%=M;
    return x*(x+1)/2%M;
}
inlint calc2c(int x){
    x%=M;
    return x*(x+1)%M*(2*x%M+1)%M*inv6%M;
}
inlint gid(int x){
    if(x<=sqr) return ids[x];
    else return idb[n/x];
}
inlint calcans(int m,int k){
    if(p[k]>m)return 0;
    int ans=( (g2k[gid(m)]-g1k[gid(m)]+M)%M -(sum2c[k]-sum1c[k]+M)%M+2*M)%M;
    for(int i=k+1;i<=tot&&p[i]*p[i]<=m;i++){
        for(int op=p[i],j=1;op<=m;op*=p[i],j++){
            int opM=op%M;
            ans+=opM*(opM-1+M)%M*(calcans(m/op,i)+(bool)(j>1))%M;
            ans%=M;
        }
    }
    return ans;
}
signed main(){

    re(n);
    sqr=sqrt(n);
    o();

    for(int l=1,r;l<=n;l=r+1){
        r=n/(n/l);
        val[++cnt]=n/l;

        g1k[cnt]=calc1c(val[cnt])-1;
        g2k[cnt]=calc2c(val[cnt])-1;
        if(val[cnt]<=sqr)ids[val[cnt]]=cnt;
        else idb[n/val[cnt]]=cnt;
	}

    for(int i=1;i<=tot;i++)
        for(int j=1;j<=cnt&&p[i]*p[i]<=val[j];j++)
            g1k[j]=(g1k[j]-  p[i] * ((g1k[ gid(val[j]/p[i]) ]-sum1c[i-1])%M) %M+M)%M,
            g2k[j]=(g2k[j]- p[i]*p[i]%M * ((g2k[ gid(val[j]/p[i]) ]-sum2c[i-1]) %M)%M+M)%M;
    wr((calcans(n,0)+1)%M);
    return 0;
}

```

------一条华丽的分割线-------

如果分割线以上的内容您看懂了，那么您在这条分割线以下的部分应该是可以乱杀了。

### $\color{#FF000F}{2. BSGS\ \&\ exBSGS}:$

$BSGS$，全称$Baby\ Step\ Giant\ Step$

是用于求解高次同余方程的这样一个东西。

**高次同余方程：** 形如$a^x\equiv b \pmod p$

要求是保证$\gcd(a,p)=1$，否则就要上$exBSGS$。

这个知识点也是非常的好懂，

**~~只要你不像笔者一样犯调用函数不写函数名这种错误：（你听说过$opt=(b,sqp)$么）~~**

在你学会之后就可以愉快地切掉一道蓝题。

回到正题，求解$x$，使得对于给定的$a,b$，$a^x\equiv b\pmod p $成立。

接下来就省略$\pmod p$了，~~敲着费劲~~

令$t=\lceil\sqrt{p}\rceil$
我们设$x=At-B$，其中$A,B\leq t$

我们便有$a^{At-B}\equiv b$

稍做变换得到$a^{At}\equiv ba^b$

我们暴力枚举$B$，结果哈希或$map$存起来。

然后我们再暴力枚举$A$，直接查询是否在枚举$B$的时候出现过。

因为$A,B\leq t$，所以复杂度是$\Theta(\sqrt p)$的。

接下来是 $exBSGS$，简单来说，这个$exBSGS$和普通$BSGS$的主要区别就是他能够处理$a,p$不互质的情况。

给定$a,b,p$，不保证$\gcd(a,p)=1$，求最小的非负数$x$使得$a^x\equiv b\pmod p$

直接搞肯定不行，我们考虑去除因子。

我们称一开始的$b,p$分别为$b_1,p_1$，

记$d_i=\gcd(a,p_i)$。

记$tot_i=\frac{a}{d_i}$

特殊的，记$tot_0=1$

我们先把式子写出来

$a^x\equiv b_1\pmod {p_1}$

我们考虑去除$a,p_1$的公因子。

令$b_2=\frac{b_1}{d_1},p_2=\frac{p_1}{d_1}$


此时如果$d_1\nmid b_1$，则无解。

如果$d_1 |b_1$，那么我们就得到了一个新柿子$\frac{a}{d_1}a^{x-1}\equiv tot_1 a^{x-1}\equiv b_2 \pmod {p_2}$

当然，现在的$p_2$不一定和$a$互质，我们只要一直重复这个操作，迟早能够令他们互质。

此时我们已经处理完毕，现在我们的$p_t$已经成功地和$a$互质。

则此时有$tot_{t-1}a^{x-t+1}\equiv b_t\pmod {p_t}$，易证$tot_{t-1}$与$p_t$互质，我们用逆元把他移动到同余式右侧，得到$a^{x-t+1}\equiv b_tinv_{p_t}(tot_{t-1})\pmod {p_t}$。

现在我们直接使用普通版$BSGS$求解就行了。

$LG$上的模板题
#### [$\color{#3498DB}{\text {P3846 【模板】BSGS} }$](https://www.luogu.com.cn/problem/P3846)

#### [$\color{#9D3DCF}{\text {P4195 【模板】扩展 BSGS/exBSGS} }$](https://www.luogu.com.cn/problem/P4195)

注：以下代码是$\color{#9D3DCF}{\text {P4195 } }$的，普通$BSGS$直接把那个叫$BSGS$的函数单独弄一下就能过$\color{#3498DB}{\text {P3846}
}$


已省略主函数等杂项。

```cpp
inlint gcd(int a,int b){
    if(!b) return a;
    return gcd(b,a%b);
}
inlint  exgcd(int a,int b,int& x,int& y){
    if(b==0){x=1;y=0;return a;}
    int ans=exgcd(b,a % b,x,y);
    int t=x;
    x=y;
    y=t-(a/b)*y;
    return ans;
}
inlint qx(int a,int x,int p){
    return a*x%p;
}
inlint qp(int a,int x,int p){
    int res=a,ans=1;
    while(x){
        if(x&1){
            ans=qx(ans,res,p);
            ans%=p;
        }
        res=qx(res,res,p);
        res%=p;
        x>>=1;
    }
    return ans;
}
inlint BSGS(int a,int b,int p){
    if(p==1||b==1) return 0;
    if(a%p==b%p) return 1;
    if(a%p==0) return  -1;
    int sqp=sqrt(p-1)+1;
    int k=b;
    int opt=qp(a,sqp,p);
    map<int,int>ma;
    for(int i=0;i<=sqp;i++,k=qx(a,k,p),k%=p)
        ma[k]=i;

    int okt=opt;
    for(int i=1;i<=sqp;i++,okt=qx(okt,opt,p),okt%=p)
        if(ma.find(okt)!=ma.end())
            return (qx(i,sqp,p)-ma[okt]+p)%p;
    return -1;

}

inlint inv(int a,int b){
    int x,y;
    exgcd(a,b,x,y);
    return (x%b+b)%b;
}
inlint exBSGS(int a,int b,int p){
    if(p==1||b==1) return 0;
    if(a%p==b%p) return 1;
       if(a%p==0) return  -1;

    int na=0;
    int oa=a,ob=b,op=p;
    int tot=1;
    while (gcd(a,p)-1){int d=gcd(a,p);
       /// cout<<b<<" "<<p<<endl;

        na++;
        if(b%d!=0) return -1;
        b/=d;
        p/=d;

        tot*=a/d;
        tot%=p;
        a%=p;
        if(tot==b) return na;

    }
    b%=p;
    b*=inv(tot,p);
    b%=p;
    int res=BSGS(a,b,p);
    if(res==-1) return -1;
    return res+na;
}

```



### $\color{#0FF0F0}{3.\text{原根和阶}}:$
然后我们来学习一下原根和阶，因为后面进阶$BSGS$要用。

（绝大部分证明来源于$OIwiki$）

**阶：** 先抛开群论不谈，在同余式中如果存在，对于$a^x\equiv 1 \pmod m$，最小的正整数$x$被称为$a$模$m$的阶，记做$\delta_m (a)$。存在阶的充要条件是$\gcd(a,m)=1$。

**阶的一些性质：**

----------

注：证明中的同余式除去特殊说明外全部都是$\pmod m$

性质一：对于$\forall 1\leq i\not=j\leq \delta_m (a)$，有$a^i\not \equiv a^j \pmod m$

证明如下：

若存在$i\not =j$使得$a^i \equiv a^j $



则$a^{|i-j|}\equiv 1$

而$|i-j|< \delta_m (a)$

与定义矛盾。

------


性质二：如果$a^n \equiv 1$，则有$\delta_m(a)|n$

证明：我们设$n=q\times\delta_m(a)+r$

如果$r>0$，则有$a^r\equiv a^r\times a^{q\times \delta_m(a)} \equiv a^n \equiv 1$

与定义矛盾。

--------
性质三：

如果$\gcd(a,m)=\gcd(b,m)=1$

则$\delta_m(ab)=\delta_m(a)\times\delta_m(b)$

的充要条件是

$\gcd(\delta_m(a),\delta(b))=1$

证明：

必要性：

由定义得$(ab)^{lcm(\delta_m(a),\delta_m(b))}\equiv 1$

根据性质一，得$\delta_m(ab)|lcm(\delta_m(a),\delta_m(b))$

$\because \delta_m(ab)=\delta_m(a)\times\delta_m(b)$

$\therefore \delta_m(a)\times\delta_m(b)|lcm(\delta_m(a),\delta_m(b))$

$\therefore
lcm(\delta_m(a),\delta_m(b))=\delta_m(a)\times\delta_m(b)$

$\therefore
\gcd(\delta_m(a),\delta(b))=1$

充分性：

$\because
(ab)^{\delta_m(ab)\times \delta_m(b)}\equiv a^{\delta_m(ab)\times \delta_m(b)}\equiv1
$

$\therefore
\delta_m(a)|\delta_m(ab)\times \delta_m(b)
$

又$\because
\gcd(\delta_m(a),\delta(b))=1
$

$\therefore
\delta_m(a)|\delta_m(ab)
$

同理可证
$
\delta_m(b)|\delta_m(ab)
$

$\because
\gcd(\delta_m(a),\delta(b))=1$

$\therefore
\delta_m(a)\times \delta_m(b)|\delta_m(ab)
$

又$\because (ab)^{\delta_m(a)\times \delta_m(b)}\equiv a^{\delta_m(a)\times \delta_m(b)}b^{\delta_m(a)\times \delta_m(b)}\equiv 1$

$\therefore
\delta_m(ab)|\delta_m(a)\times \delta_m(b)
$

$\therefore
\delta_m(ab)=\delta_m(a)\times \delta_m(b)
$

-----
性质四：读者自证吧，敲起来太累了。

$\forall k\in N$
有$\delta_m(a^k)=\frac{\delta_m(a)}{\gcd(\delta_m(a),k)}$

-------
**关于原根:**

定义：若存在$g$使得$\delta_m(g)=\varphi(m)$，则称$g$是$m$的一个原根。

-----

$原根判定定理$

$g$是$m$的原根的充要条件是

对于$\forall{p}\in P \&p|\varphi(m)$，都有$g^{\frac{\varphi(m)}{p}} \not \equiv 1$

----

原根性质如下：

一个数字若存在原根，则最小原根的大小不超过$n^{0.25}$。~~别问为什么，我也不知道~~

----
一个数字如果存在原根，那么他的所有原根都可以表示为其最小原根的指数幂形式。

$m$如果有原根，则他的原根个数为$\varphi(\varphi(m))$。

我们令某个原根是$g$，根据阶的性质四，$\delta_m(g^k)=\frac{\delta_m(g)}{\gcd(\delta_m(g),k)}$。

然后显然命题成立。

-----
**原根存在定理**：仅有$2,4,p^a,2p^a$有原根，其中$p$是奇素数。

$2,4$显然




笔者参考资料：

[$OIwiki$](https://oi-wiki.org/)

[$Min\_25$ /  by $zhoukangyang$](https://www.luogu.com.cn/blog/173660/solution-p5325)