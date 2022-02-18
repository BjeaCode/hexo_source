---
title: 蒟蒻的插头DP基础入门初步
date: 2022-01-03 12:02:16
tags:
  - DP
categories: 学习笔记
author: Mr_Stranger_CW
mathjax: true
---

### 一个基于状压$and$哈希的动态规划算法

~~本蒟蒻学了半天菜菜把模板题整明白~~

其实分类讨论整熟练之后插头$DP$还是好理解的

**~~只要您不像我这蒟蒻一样建完哈希表不插入东西~~**

## **进入正文：**

插头$DP$，其核心在于插头       ~~废话~~。

插头大概长成这个样子：

![](https://cdn.luogu.com.cn/upload/image_hosting/qizdclhm.png)

（有点丑）

就是在网格图上，每一个格子都有四个插头上下左右。

现在我们先讨论模板题，在下面这道模板题中我们只需要用到下插头和右插头。

[P5056 【模板】插头DP](https://www.luogu.com.cn/problem/P5056)

题意简述：给定一张$n\times m$的网格图，保证$n,m\le12$，其上有若干障碍物，问把所有空地铺上线且使得这些线称为闭合回路的方案数。

稍微思考一下，你会发现一个神奇的事情，对于一个方格$a_{i,j}$，他上面铺的线的形态应该是由$a_{i-1,j}$和$a_{i,j-1}$决定的。这个时候我们来分类讨论一下。（插头的事先别急后面会讲）

我们先引入轮廓线这个概念，轮廓线指的是已决策和未决策状态的分界线。

如果说$a_{i-1,j}$和$a_{i,j-1}$长成这个样子：

![](https://cdn.luogu.com.cn/upload/image_hosting/3pw14me4.png)

那因为我们要保证闭合回路，所以我们只能这么做：

![](https://cdn.luogu.com.cn/upload/image_hosting/0rb0w2a8.png)

第二种和解决办法：

![](https://cdn.luogu.com.cn/upload/image_hosting/9s4mqqqx.png)

第三种和两种解决办法：

![](https://cdn.luogu.com.cn/upload/image_hosting/g949yo73.png)

![](https://cdn.luogu.com.cn/upload/image_hosting/xv15wy7s.png)

![](https://cdn.luogu.com.cn/upload/image_hosting/miou5l99.png)

多画几组你会发现，$a_{i-1,j}$和$a_{i,j-1}$本身长成啥样不是特别重要，我们需要知道的只是下图中$P1,P2$两个位置存不存在线头。

![](https://cdn.luogu.com.cn/upload/image_hosting/pf7d30uy.png)

这里我们就可以引入一个概念了，如果说一个位置存在一条线必须按照$vec$这个方向从此延伸，则称这个位置存在一个$vec$方向的插头。

问题接踵而至，知道了他有啥用，闭合回路怎么处理？

别急，我们观察一下，因为保证闭合回路，我们一定会保证在轮廓线上方不存在断点，所有的断点只可能在线上产生，也就是插头。

所以我们会发现，断点一定成对出现。每一对可以描述一条线轮廓线上方的线，这一大堆线覆盖了轮廓线上方的所有空地。

而每一个断点都有两种类型：靠左或者靠右~~废话~~

我们以此也把插头分为两类：一类左断点插头，我们称之为$1$类，也叫起点插头；一类右端点插头， 我们称之为$2$类，也叫终点插头。

所以对于每一个轮廓线上点，有三种情况：没有插头，插头$1$，插头$2$。

这个味道。。。是状压。

故因为插头$DP$基于状压，所以一般来说数据不大。

可以三进制压，但是蒟蒻比较懒，一般直接四进制压了，使用位运算还会快一点。

对于一个$n\times m$的网格图，我们每一次压进去$m+1$个插头，$a_{i,j}$决策后的状态表示的是第$a_{i,1-j}$的下插头$+a_{i,j}$的右插头$+ a_{i-1,j+1-m}$的下插头。

我们记$nzt$为当前的状态，两个二进制位表示一个插头，$d_i=1<<(2*(i-1))$。

我们的状态是一行一行来的，所以我们在计算$a_{i,j}$这个方格时可以用$(nzt>>(2*(j-1))) \% 4 $提取$a_{i,j-1}$的右插头，用$(nzt>>(2*j))\%4$提取$a_{i,j-1}$的左插头，即$P1,P2$。

所以对于格子$a_{i,j}$，我们可以

分类讨论开始了：

特殊情况先考虑一下：这个格子有障碍物，那么显然他的上方和左方不应该存在插头，此时原本状态的第$j,j+1$两个插头均为零，转移之后这格没有线，所以第$j+1,j+2$（下标此时变化了一些）两个插头也变成了零，略加思考，原封转移即可。否则无法转移，直接略过，

以下情况均是该格子不存在障碍物。

情况$A:P1=P2=0$，这个时候因为不存在$a_{i,j}$左和上的两个插头，所以我们只能将这个格子的右方和下放连接。这个时候的转移因为我们加入了两个插头，一个在$j$位置的$1$插头，一个在$j+1$位置的$2$插头，这个时候把原状态改一下，这个新的状态应为$nzt+d_j+2*d_{j+1}$。（状态咋改的这里一定要理解，因为下边就全是“大体同上，不会看代码了”，因为确实改个状态真的不难$OuO$）

情况$B:P1=0,P2\not=0$，这种情况我们有两种办法，第一种是从上到下，在位置$j$建立一个插头$P= P2$，第二种是从上到左，在$j+1$位置建立一个插头$P=P2$。状态更改实现大体同上自行思考。

情况$C:P1\not =0,P2=0$同上，只不过是方向不一样而已。

情况$D:P1=P2=1$，这种情况就是两个起点碰到一起了，这样的话我们就需要将两条线中终点靠左的那条线的终点变为起点，因为我们知道，对于轮廓线上四个位置$ABCD$，不可能有$AC$是一条线的同时$BD$是一条线，这样会矛盾，所以这个靠左的终点一定是$P2$所对应的终点，直接从$j$开始向右扫就行。找到之后直接更改。方法和括号匹配一样，为啥的话。。。。都看到这了读者应该是知道的吧。。。。。两两配对不可交叉有左有右。。。。

情况$E:P1=P2=2$，同上，只不过是要找起点，从$j-1$开始向左扫即可。

情况$F:P1=2,P2=1$，一个终点碰到了一个起点，直接连接变成一个，原来两个插头全部变成$0$就可以。

情况$G:P1=1,P2=2$，相当于是要闭合回路了，这个时候能够闭合的要求就是只剩这俩插头了且被闭合的位置是终点，这就不用转移了，直接塞进答案里即可。记$edx,edy$表示从上到下从左到右遍历过程中遇到的最后一个没有障碍点的位置，情况$G$能够计算进答案当且仅当$i=edx\land j=edy$。

$P.S:$注意新建插头的时候判断一下涉及到的方格有没有障碍物。

分类讨论搞完了，来说实现，记$dp[i][j][zt]$表示在格子$a_{i,j}$决策之后轮廓线上插头状态为$zt$的方案数。

这样的话空间就是$O(nm3^{m+1})$，$LG$上模板题的范围是$n,m \leq 12$的，空间只有$125MB$，我们要是这么干的话空间大概是$1000MB$不到，所以 **~~可以通过本题~~** 必须要优化

我们发现很多状态不可能合法，而且直接压缩不能完美利用空间，我们可以使用哈希表来记录状态。

然后我们发现求我们在计算一个格子的时候只使用了上一个被计算的格子的状态，所以可以滚动数组优化一下。

哈希的话，我们发现他的总状态是$3^{13}$次方左右，在要求括号匹配，暴搜一下，大概总方案数不超过$3\times 10^5$，所以大胆的哈希吧，下面代码用的是链表法哈希表。

现在的空间只有$6\times 10^5$了，显然是够的，时间复杂度的话就是大概就是在空间的基础上乘上个$144$左右。

##  可以通过本题。



$AC$代码：

```c++
//#include <bits/stdc++.h>
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
#define lc(x) sgt[x].ls
#define rc(x) sgt[x].rs
#define int long long
using namespace std;
inlind re(int &x) {
    x=0;
    bool f=0;
    int w=getchar();
    while(w<'0'||w>'9'){if(w=='-')f = 1;w=getchar();}
    while(w<='9'&&w>='0')x=(x<<3)+(x<<1)+w-'0',w=getchar();
    if(f)x=-x;
}
inlind re(int &x1,int &x2) {
    re(x1);
    re(x2);
}
inlind wr(int x) {
    if(x<0)putchar('-'),x=-x;
    if(x>9)wr(x/10);
    putchar(x%10+'0');
}
int tot[2];
int ha[2][300000],flag,edx,edy,dp[2][300000];
int sjz;
int jz4[15];
struct has{
    int hd[300000],nt[300000];
    const int M=299993;
    inlind ins(int zt,int s){
        int ztm=zt%M+1;
        for(int i=hd[ztm];i;i=nt[i])
            if(ha[flag][i]==zt){
                dp[flag][i]+=s;
                return ;
            }
        nt[++tot[flag]]=hd[ztm];
        hd[ztm]=tot[flag];
        ha[flag][tot[flag]]=zt;
        dp[flag][tot[flag]]=s;
    }
    inlind cr(){
        mem0(hd);
        tot[flag]=0;
    }
}H;
int n,m;
bool op[15][15];
signed main(){
    re(n,m);
    for(int i=1;i<=n;i++)
        for(char j=1,x;j<=m;j++){
            cin>>x;
            if(x=='.')op[i][(int)(j)]=1,edx=i, edy=j;
        }
    for(int i=1;i<=max(n,m)+1;i++)jz4[i]=1<<(2*(i-1));
    flag=0;
    ha[flag][1]=(tot[0]=dp[0][1]=1)-1;
    int ans=0;
    for(int i=1;i<=n;i++){
        for(int j=1;j<=tot[flag];j++)ha[flag][j]<<=2;
        for(int j=1;j<=m;j++){
            flag^=1;
            H.cr();
         for(int k=1;k<=tot[flag^1];k++){
                int nzt=ha[flag^1][k],p1=(nzt>>(2*j-2))%4,p2=(nzt>>(2*j))%4;
                int diputs03_AKIOI=dp[flag^1][k];
                if(!op[i][j]){if(!p1&&!p2)H.ins(nzt,diputs03_AKIOI);}
                else if(!p1&&p2){if(op[i][j+1])H.ins(nzt,diputs03_AKIOI);
                    if(op[i+1][j])H.ins(nzt+jz4[j]*p2-jz4[j+1]*p2,diputs03_AKIOI); }
                else if(p1&&!p2){if(op[i+1][j])H.ins(nzt,diputs03_AKIOI);
                    if(op[i][j+1])H.ins(nzt+jz4[j+1]*p1-jz4[j]*p1,diputs03_AKIOI);}
                else if(!p1&&!p2){if(op[i+1][j]&&op[i][j+1])
                        H.ins(nzt+jz4[j]+2*jz4[j+1],diputs03_AKIOI);}
                else if(p1==1&&p2==2){if(i==edx&&j==edy&&!nzt-(p1<<(2*j-2))-(p2<<(2*j))){
                        ans+=diputs03_AKIOI;}}
                else if(p1==2&&p2==1){
                    H.ins(nzt-2*jz4[j]-jz4[j+1],diputs03_AKIOI);

                }
                else if(p1==1&&p2==1){
                    int ct=0;
                    for(int p=j;p<=m;p++){
                        if((nzt>>(2*p))%4==1)ct++;
                        else if((nzt>>(2*p))%4==2)ct--;
                        if(!ct){ H.ins(nzt-jz4[j]-jz4[j+1]-jz4[p+1],diputs03_AKIOI);break;}
                    }}
                else if(p1==2&&p2==2){
                    int ct=0;
                    for(int p=j-1;p>=1;p--){
                        if((nzt>>(2*p))%4==1)ct++;
                        else if((nzt>>(2*p))%4==2)ct--;
                        if(!ct){H.ins(nzt-2*jz4[j]-2*jz4[j+1]+jz4[p+1],diputs03_AKIOI);
                                break;}}}
            }
        }
    }
    cout<<ans;
        return 0;
}

```

