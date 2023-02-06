---
title: CF1607D Blue-Red Permutation
date: 2021-11-10 13:19:08
tags:
  - 贪心
categories: 题解
author: Jocker_CW
mathjax: true
---

## [CF1607D ](https://www.luogu.com.cn/problem/CF1607D)

题意简述：给定$n$个数字，每个数字有一个颜色，若为红色则该数字可以进行$+1$的操作，若为蓝色可以进行$-1$的操作。问是否存在一种操作的序列，使得最后的$n$个数互不相同且均小于等于$n$

我们先转换一下题意：红色的数字表示这一位可以取到所有大于等于他的数字，蓝色的则可以取到所有大于等于它的数字。

首先，我们对他排个序，然后贪心的去取。

贪心策略：将颜色作为第一关键字排序，该数数值作为第二关键字排序。

取的时候：对于一个数字，他若是蓝色的，则找到最小的一个可被取到且未被其他数字取到的数值。不存在就不取。红色的话就找最小的大于等于它的未被取的数值。不存在就不取。

正确性的话，前面好多位大佬说的还是挺清楚的，只不过他们是枚举$1-n$然后去找，其实和我这个是差不多的。（~~主要还是因为本人语文太差，讲不清楚。~~）

然后来看一下怎么实现。

我们首先将$1-n$扔到一个集合$S$中，蓝色就取$s.begin()$~~毕竟直接排序不用白不用~~，要是$s.begin()$的值大于就不取。红色就$lower\_ bound$二分查找。要是找不到就不取。取走的直接$erase$掉。

最后判一下$S$是否为空即可，为空就意味着都被取走了。反之则没有。

#### 但是！！！
这么干我T飞了啊。。。。。。。。。。。。。理论上来说增删改查$set$不是$O(log_2 n)$嘛。。。。。这样算下来应该是$O(n\ast log_2 n)$的

#### ~~也可能是我人菜自带大常数吧~~

于是我选择了另一种方法，理论上来说复杂度是没有很优的，整体多套了一个$log_2 n$上去。。。

 不过他过了(doge

#### 他就是二分前缀和加树状数组优化！！！

数组上一开始给这$n$个数每个数赋为$1$，然后搞成树状数组。

查找在未取集合之中大于等于$x$的最小值的时候。

我们就二分前缀和，边界设为$l=x,r=n+1$，每次调取$mid$到$l$的区间和，若是非零则代表这其中有数字还没有被取走，$r=mid$。反之没有，$l=mid+1$。

然后要是想要取最小的未被取走的值，相当于查找最小的大于等于$1$的数字，直接套就行了。

详情请食用代码注释！

```cpp
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
using namespace std;
#define int long long
int t;
int n;

int tr[200030];//树状数组
inline int lb(int x){
    return x&-x;
}
inline void up(int x,int k){//树状数组更新
    while(x<=n){
        tr[x]+=k;
        x+=lb(x);
    }
}
inline int g(int x){//树状数组求前缀和
    int res=0;
    while(x>0){
        res+=tr[x];
        x-=lb(x);
    }
    return res;
}
struct node {int val,col;}a[200300];
inline bool cmp(node x,node y){
    return x.col!=y.col?x.col>y.col:x.val<y.val;//蓝色在前，然后数值排序
}
inline int trbin_b(int x){
    int l=x,r=n+1;
    while(l<r){
        int mid=(l+r)>>1;
        if((g(mid)-g(l-1))>0)r=mid;//二分前缀和
        else l=mid+1;
    }
    return r;
}

int cnt;
signed main(){
    cin>>t;
    while(t--){
        cin>>n;
        for(int i=1;i<=n;i++)scanf("%lld",&a[i].val);
        for(int i=1;i<=n;i++){
            char p;
            cin>>p;
            if(p=='B')
                a[i].col=1;
            else a[i].col=0;
            up(i,1);//先给每个值的当成没有被取走
        }
        cnt=0;//记录取走了几个，每次记得更新
        sort(a+1,a+n+1,cmp);//排序

        for(int i=1;i<=n;i++){
            if(a[i].col==1){
                int it=trbin_b(1);
                if(a[i].val>=it&&it<n+1){//判断是否越界，要是n-1的话说明不存在
                    cnt++;//记录个数
                    up(it,-1);//把它取走
                }
            }

            else{
                int it=trbin_b(a[i].val);
                if(it<n+1){//判断是否越界
                    cnt++;
                    up(it,-1);
                }
            }
        }
        for(int i=1;i<=n;i++){
            tr[i]=0;//盘回来
        }
        if(cnt!=n)printf("NO\n");//cnt判断，等于n就是取完了，反之则失败
        else printf("YES\n");


    }

return 0;}
```

