---
title: 蒟蒻浅谈扩展卢卡斯
date: 2021-12-04 20:51:36
tags:
  - 数学
categories: 学习笔记
author: Mr_Stranger_CW
mathjax: true
---

## 扩展卢卡斯$(exLucas)$
#### 一个除了名字以外和卢卡斯定理没有任何关系的东西。

##### 对比一下$Lucas$和$exLucas$:

###### $Lucas:$

适用于组合数取模 ~~废话~~

要求模数是质数

时间复杂度$O(plog_pn)$	

###### $exLucas:$

适用于组合数取模 ~~又是废话~~

对模数没有要求

时间复杂度：

设模数有$c$个质因子，质因子$p_i$的次数为$k_i$。

总复杂度大约是$O(c\times p_i^{k_i}\times \log_{p_i} n\times log_2 \ p_i^{k_i})$

因为$n$往往很大，不好预处理逆元。

### 前置知识

$CRT$中国剩余定理

乘法逆元

$exEuclid$扩展欧几里得定理

以上不会戳[这里](https://www.luogu.com.cn/blog/Mr-Stranger-CW-AWA/ztzl-1)

#### 直接讲$exLucas$：

解决的问题：给定$n,m,q$，求$C_n^m \bmod q$

首先，因为模数非质数的限制 ~~以及先人的智慧~~ 我们可以很快的想到使用$CRT$分解原模数，变成质数的次幂形式进行求解。

我们设$x$为答案,即$C_n^m \bmod q$。

现在我们得到了一个同余方程组

$
\begin{cases} 
x \equiv a_1 \pmod {p_1^{k_1}}\\
x \equiv a_2 \pmod {p_2^{k_2}}\\
x \equiv a_3 \pmod {p_3^{k_3}}\\
\ \ \ \ \ \ \ \ \ \  {\cdots}\\
x \equiv a_{c} \pmod {p_{c}^{k_c}}
\end{cases}
$

其中 $q=\prod_{i=1}^{i=c}{p_i^{k_i}}$。

现在问题转变为如何求出$a_i$，即$C_n^m \bmod p_i^{k_i}$

先把式子写出来

要求 :

$\bmod \ \ p_i^{k_i}:$

$a_i\equiv \frac{n!}{m!(n-m)!}$

我们发现原来用逆元去求取答案的方式不能实现，因为可能有$p_i$的倍数使得不存在逆元

于是我们考虑提出这里面所有的$p_i$的倍数，使得逆元可做。

得到：

$a_i\equiv \frac{\frac{n!}{p_i^x}}{\frac{m!}{p_i^y}\frac{(n-m)!}{p_i^z}}\times p_i^{x-y-z}$

现在同余式右边的分数部分就可以使用逆元求解了。

现在问题转变为：如何求$n! \bmod p_i^{k_i}$？

现在我们举一个例子 ~~别问为啥，你行你给我整个公式出来~~ 

当$n=22,p_i=3,k_i=2$时

$n!=1\times2\times3\times4\cdots\times 22$

提出其中$p_i$的倍数，

不难想到 ~~小学数学~~ 共有$\lfloor \frac{n}{p_i}\rfloor$个是$p_i$的倍数。

得到

$n!=3^7\times(1\times 2\times 3\times\cdots \times 7) \cdots\times 22$

发现对于被提出$p_i$的倍数的那一部分提完之后就可以作为一个单独的阶乘，我们可以递归求取。不难发现，这个阶乘也是$\lfloor \frac{n}{p_i}\rfloor!$。

我们再次观察剩下的那一堆与$p_i$互质的乘积。

$n!=3^7\times 7!\times 1\times 2\times 4\times 5\times 7\times8\cdots$

我们发现 ~~实际上是先人的智慧~~ :


$1\times 2\times 4\times 5\times 7\times8\equiv 10\times 11\times 13\times 14\times 16\times17$

于是得到剩下的数字是有循环节的。以$1- p_i^{k_i}$为一节。我们先暴力扫出来一节，然后可以直接使用快速幂。剩下的余节暴力扫。

现在，我们称将$n!$分解之后：

前面$p_i^{\lfloor \frac{n}{p_i}\rfloor}$为$part_1$，

中间$\lfloor \frac{n}{p_i}\rfloor!$为$part_2$

最后循环节和余节的部分为$part_3$

我们先前已经去掉了所有$p_i$的倍数，所以$part_1$直接扔。

于是答案就是$part_2\times part_3$。

我们将其写成递归函数的形式


对于求取$n! \bmod p_i^{k_i}$

$
f_{p_i^{k_i}}(x)=
\begin{cases}
x=0 \ \ \ 0\\
x>0 \ \ \ f_{p_i^{k_i}}(\lfloor\frac{n}{p_i}\rfloor)\times(\prod_{i\nmid p_i^{k_i}}^{i\leq p_i^{k_i}})^{\lfloor\frac{n}{p_i^{k_i}}\rfloor}\times\prod_{i\nmid p_i^{k_i}}^{i\leq n \bmod p_i^{k_i}}
\end{cases}
$

现在我们算出了$f_{p_i^{k_i}}(x)$，由于我们已经去除过$p_i$因子，所以最终模数与$p_i^{k_i}$互质，扩欧求逆元即可。

### 于是我们算出了每一个$a_i$！
#### 现在我们就直接上$CRT$或者$exCRT$求解就行

请配合代码食用！！！！

```cpp
#include <bits/stdc++.h//#include <bits/stdc++.h>
#include <iostream>
#include <cstdio>
#include <math.h>
#include <algorithm>
#include <istream>
#include <string>
#include <queue>
#include <deque>
#include <stack>
#include <set>
#include <string.h>
#include <map>
#include <unordered_map>
#define int long long
#define inlind inline void
#define inlinl inline bool
#define inlint inline int
#define inlinc inline char
#define inlins inline string
#define mem0(a) memset(a,0,sizeof(a))
#define memf(a) memset(a,0x3f,sizeof(a))
#define mem_f(a) memset(a,0x80,sizeof(a))
#define mem_1(a) memset(a,-1,sizeof(a))
#define pb(a,b) a.push_back(b)
#define mp(a,b) make_pair(a,b)
#define p1(x) x.first
#define p2(x) x.second
using namespace std;
int n,m,p;
bool pr[1000300];
int prs[100030];
int total;
struct node {int p,pk,m;}a[100030];
int M[100030];
int sum;
inlind opr(){//质数筛预处理，一会要分解质因数
    int n=1e6;
    memset(pr,1,sizeof(pr));
    pr[1]=0;
    for(int i=2;i<=n;i++){
        if(pr[i]){prs[++total]=i;}
               for(int j=1;j<=total;j++){
                   if(i*prs[j]>n) break;
                   pr[i*prs[j]]=0;
                   if(i% prs[j]==0){
                       break;
                   }

           }
       }
}
inlint qp(int a,int x,int p){//快速幂
    int ans=1,res=a;
    while(x){
        if(x&1){
            ans*=res;
            ans%=p;
        }
        res*=res;
        x>>=1;
        res%=p;
    }
    return ans;
}
inlind exgcd(int a,int b,int &x,int &y){//扩展欧几里得
    if (!b) {
        x=1;
        y=0;
        return ;

    }
    exgcd(b,a%b,x,y);
    int t=x;
    x=y;
    y=t-a/b*y;
}
inlint inv(int a,int p){//求逆元
    int x,y;
    exgcd(a,p,x,y);
    return (x%p+p)%p;
}
inlint fr(int n,int p,int pk){//计算阶乘取模
    if(n==0) return 1;
    int ans=1;
    for(int i=1;i<pk;i++)if(i%p)ans*=i,ans%=pk;//暴力计算循环节
    ans=qp(ans,n/pk,pk);
    for(int i=1;i<=n%pk;i++)if(i%p)ans*=i,ans%=pk;//暴力计算余节
    return ans*fr(n/p,p,pk)%pk;//递归求解小阶乘
}
inlint calc(int n,int m,int p,int pk){//计算ai
    if(n<m) return 0;
    int cnt=0;
    int op=p;
    for(int i=1;op<=n;i++)cnt+=n/op,op*=p;//提取pi因子
    op=p;
    for(int i=1;op<=m;i++)cnt-=m/op,op*=p;
    op=p;
    for(int i=1;op<=n-m;i++)cnt-=(n-m)/op,op*=p;
    return qp(p,cnt,pk) *  fr(n,p,pk)  % pk * inv( fr(m,p,pk) ,pk) % pk * inv( fr(n-m,p,pk) ,pk) % pk;//乘上阶乘取模和逆元以及因子个数即为答案

}

signed main(){
    opr();

    cin>>n>>m>>p;
    int w=p;
    for(int i=1;i<=total&&prs[i]<=p;i++){
        int cntt=1;
        while(p%prs[i]==0){
            p/=prs[i];
            cntt*=prs[i];
        }
        if(cntt!=1)
            a[++sum]={prs[i],cntt,calc(n,m,prs[i],cntt)};
    }

    //CRT

    for(int i=1;i<=sum;i++)M[i]=w/a[i].pk;
    int ans=0;
    for(int i=1;i<=sum;i++){
        ans+=inv(M[i],a[i].pk)*M[i]%w*a[i].m%w;
        ans%=w;
    }
    //输出答案，结束
    cout<<ans%w;;
    return 0;//代码再见
}

```

