---
title: 蒟蒻的基础数论
date: 2021-07-22 13:48:46
tags:
  - 数学
categories: 学习笔记
author: Jocker_CW
mathjax: true
---

# ~~令人裂开的~~$\color{#F2A55E}{\text{基础数论}}$

### Zmzl老师曾曰：“不要讲矩阵了，那个你们用不到，再讲就太多了，这只是基础数论”
### Yywd学长曾曰：“基础数论，你还差一个组合数取模没讲，讲一下卢卡斯定理就行，代码就三行。还有，既然你已经讲了欧拉定理，那么你得讲一讲乘法逆元。如果你要讲乘法逆元，还得知道怎么求，那你还得讲一下扩展欧几里得。都讲到这了，那你再讲个同余方程不过分吧”
### Zmzl老师曾又曰：“听学长的话”
### Zyc学姐曾曰：“其实你没必要讲这么多”
### Zmzl老师曾又又曰：“学姐的话和学长的话你选一个听就行”
##这就是为啥我的目录从五章咔一下变成八章的原因
------------
# $\color{#67AEC9}{\text {目录}}$
#### - 1. 基础介绍——各种 ~~阴间~~ 符号  和  一些 ~~基本~~ 概念
#### - 2. 欧几里得算法
#### - 3. 欧拉欧拉欧拉欧拉函数和欧拉欧拉欧拉欧拉定理
#### - 4. 扩展欧几里得
#### - 5. 乘法逆元
#### - 6.组合数取模——卢卡斯定理与乘法逆元
#### - 7.同余方程——中国剩余定理与一般线性同余方程
#### - 8.质数筛法——埃/欧
#### - 9.一些题目
------------

# - 1. 基础介绍
## - 1.1 符号篇
##### ~~阴间~~ 符号！

### 求和： $\sum_{i=1}^{n}$  $a_i$
### 求积 $\prod_{i=1}^{n}$  $a_i$
### 向下取整 $\lfloor{\dfrac a b}\rfloor$
### 取模 $a$ $\equiv$ $b$ $(\bmod \ p)$

------------

## -1.2概念篇
##### ~~基本~~ 概念！
### -1.2.1 素数与合数！
 素数，又称质数，正整数$a$被称为素数当且仅当它仅拥有$a$和$1$两个正因子

 除了$1$以外所有的非素数都是合数


------------

### -1.2.2 整除与余数！
 整除：设有两整数$a,b$,有且仅有一整数$k$使得$b=ak$,则称$a$整除$b$，表示为 $a \mid b$;表示$a$是$b$的因子，同时也表示$b$是$a$的倍数。

带~~鱼~~余除法：设有两整数$a,b$,有且仅有一对整数$k，r$使得$b=ak+r \ (r<=0)$,其中$r$是$b$除以$a$的余数,记作$b \equiv r \ (mod \ a)$或$ r=b \ mod \ a$
#### p.s：整除和带余除法的性质(性质来源于我蒜哥)
##### 整除：
1.$a \ | \ b \ \ \& \ \ b \ | \ c \ \ \Rightarrow \ \ a \ | \ c$

2.$a \ | \ b \ \& \ \ a \ | \ c \ \ \ \  \forall \ x,y  \ \ \Rightarrow\ \ a|xb\ +\ yc$


3.$m \ \ne \ 0 \ \ \& \ \ ma \ | \ mb \ \ \Rightarrow \ \ a \ | \ b$

4.$b \ \ne \ 0 \ \ \& \ \ a \ | \ b \ \ \Rightarrow \ \ |a| \ \le \ |b|$

5.$a \ | \ b \ \ \& \ \ b \ | \ a \ \ \Rightarrow \ \ a \ = \ \pm b$

##### 带余除法：

1.$a+b \ \ \equiv \ \ ((a\ \bmod \ p)+(b \ \bmod \ p))$

2.$a \times b \ \ \equiv \ \ ((a\ \bmod \ p) \times(b \ \bmod \ p)) $


3.$ am \equiv bm (\bmod \ p) \Rightarrow \  \ a \equiv b \pmod {\frac{p}{gcd(p,m)}}$

4.$ a\equiv b, x \equiv y \Rightarrow ax \equiv by$

------------

### -1.2.3 最大公因数和最小公倍数
 **最大公因数**：

对于两个整数$a,b$,若有一整数$k$，$ k \ | \ a $且$k \ | \ b$,则称$k$是$a$和$b$的公因数。


$a,b$的诸多公因数中最大的一个，被称为$a,b$的最大公因数，记作$gcd(a,b)$。


当$gcd(a,b)=1$时，我们称$a,b$互质

 **最小公倍数**：

对于两个整数$a,b$，若存在一$k$，$a \ | \ k$且$b \ | \ k$,则称$k$是$a,b$的公倍数

$a,b$的诸多公倍数中最小的一个，被我们称为最小公倍数，计作$lcm(a,b)$。

当$a,b$互质的时候，$lcm(a,b)=a*b$;

**~~阴间~~性质**:

#### 1.最常用的一条：$gcd(a,b)*lcm=a*b$

2.$p \ | \ a \ \& \ p \ | \ b \ \ \ \ \Rightarrow \ \ p \ | \ lcm(a,b)$

3.$a \ | \ m \ \& \ b \ | \ m \ \ \ \ \Rightarrow \ \ \gcd(a,b) \ | \ m$

4.$a,n,m \in Z^ \ast \ \& \ \Rightarrow \ \ lcm(ma,mb) = m \times lcm(a,b) $

5.$m \in Z \ \& \  m \ne 0\ $且$m$是$a_1,a_2,a_3 \  \cdots \ a_n$的公倍数，则$lcm(a_1,a_2,a_3 \ \cdots \ a_n) \ | \ m$


------------
# -2. 欧几里得算法！！！
### 又称辗转相除法
###### ~~（小声bb：这名字跟上面比好土啊）~~
-----------
## 进入欧几里得算法的正文：

欧几里得算法是一种快速求取公因数的好 ~~玩意儿~~ 算法

其时间复杂度 ~~也许~~ ~~大概~~ ~~没准~~ 约为$ O(log_2 n)$，其中$n= max(a,b)$

欧几里得算法的实现原理是 $\gcd(a,b) = \gcd (b, a \bmod b )$
## 那么这个实现原理是如何被证明的嘞？
###### ~~别问我我也不知道~~
### 然而我最后还是找到了
###### ~~（其实是百度查的）~~
###### ~~小声bb:其实知不知道无所谓，反正考场上需要的是你知道这个定理咋用而不是证明)~~
_________
## 不说废话，证明开始

首先我们要求的是$gcd(a,b)$

设$a>b$

$\exists \ q,r \ \ \ r=a \ \bmod  b \ \   \ a=qb+r $

$\exists  \ p, p \ | \ a \ \& \ p \ | \ b$

$a=qb+r \Rightarrow r=a-qb$

$ p \ | \ b  \ \Rightarrow p \ | \ qb$

$ p \ | \ a \ \& \ p \ | \ qb \ \& \ r=bq+a \ \Rightarrow p \ | \ r$

$\therefore a,b$ 的公因数和 $b,r$的公因数相同

$\therefore gcd(a,b)=gcd(b,a \ \bmod  b)$
###### ~~(终于证完了)~~
------------------
## 算法实现！！
-------
###### ~~递归就行了，我懒得写了~~
### 代码如下
```cpp
    int gcd(int a,int b){
        if(b==0) return a;
        else return gcd(b,a % b);
    }
```
#### $\ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \uparrow$~~没错就这么点~~
------------
# -3.欧拉欧拉欧拉欧拉函数和欧拉欧拉欧拉欧拉定理
### 3-1. 欧拉函数，字符表示为$ \varphi$或者$\phi$ ~~咋读我也不知道，好像是phi~~
#### 3-1.1.欧拉函数的整体介绍和公式证明
 $\phi (n)$表示在小于等$n$的正整数中，与$n$互质的数的个数

 ！栗子！：$\phi (8) = 4 \ \ (1,3,5,7)$
 **特殊地 $\phi (p)=p-1$，其中$p$是质数(显然质数与任何不是他的倍数的数互质）**


------------

#### 重点来了： 欧拉函数是积性函数，这一条在后面欧拉函数的求解和其公式证明中会用到
##### 积性函数满足下列性质：$f(nm)=f(n)  \times f(m)$
 下面来讲一下求取欧拉函数的公式：

~~神经病才用的写法：~~$\varphi(n)=n \times \prod_{i=1}^{i=k}1- \frac{1}{p_i}$

就是这个：$\varphi (n)=n(1-\frac{1}{p_1})(1-\frac{1}{p_2})(1-\frac{1}{p_3}) \cdots(1-\frac{1}{p_k})$


------------

## 问题来了：为啥嘞？

显然，$\varphi(p)=p-1$，而在不比$p^k$大的数中，仅有$p$的倍数与其不互质，而这些数的数量就是$ \frac {p^k}{p}$，即$p^{k-1}$个。

所以$\varphi(p^k)=p^k-p^{k-1}=p*(1-\frac{1}{p})$

那么对于一个非质数$n$，我们可以将其分解质因数，得到$n=p_1^{a_1} \ast p_2^{a_2} \ast p_3^{a_3} \cdots  \ast p_k^{a_k} $

然后因为欧拉函数是积性函数，所以可以将$\varphi(n)$进行分解，得到$\varphi(n)=\varphi(p_1^{a_1})\ast \varphi(p_2^{a_2}) \ast \varphi(p_3^{a_3})\cdots \ast \varphi (p_k^{a_k})$

进而得到$\varphi (n)=p_1^{a_1}(1-\frac{1}{p_1})\ast p_2^{a_2}(1-\frac{1}{p_2})\ast p_3^{a_3}(1-\frac{1}{p_3}) \cdots\ast p_k^{a_k}(1-\frac{1}{p_k})$

因为$n=p_1^{a_1} \ast p_2^{a_2} \ast p_3^{a_3} \cdots  \ast p_k^{a_k} $，所以可以取出所有的$p_i^{a_i}$将其合并为$n$;

最终得到：$\varphi (n)=n(1-\frac{1}{p_1})(1-\frac{1}{p_2})(1-\frac{1}{p_3}) \cdots(1-\frac{1}{p_k})$


-----------
------------
## 显然现在大家都已经清楚求取欧拉函数的 ~~阴间~~ 公式
## 然后
## ~~就可以准备上代码了~~
#### 首先我们要处理的是学习单个数的欧拉函数求取

我们先设$\varphi(n)=n$

单个数的欧拉函数求取可以利用公式 ~~(废话不用公式用什么)~~进行求解，所以我们的第一步就是得出它的质因数都有啥

然后再通过公式中的$(1-\frac{1}{p_i})$与当前的函数值相乘得出下一步的函数值

一直到求取完毕为止 (时间复杂度最差情况下约为$O(\sqrt{n}$)
## 来人！上 ~~才艺~~ 代码
-------
```cpp
int phi(int n){
    int phin=n;
    for(int i=2;i*i<=n;i++){
           if(n% i==0){
                   phin=phin*(i-1)/i;
                  while(n % i==0){
                      n/=i;//去除所有已经被整活过的质因子
                  }
           }
    }
    if(n>1){//因为i上界是sqrt(n),所以最后有可能会剩下一个最大的质因子
              phin=phin*(n-1)/n;
    }
    return phin;
}
```
------

好了我们已经整活完单个欧拉函数的求取了，那么接下来讲一下区间求取欧拉函数的方法

方法以及思路类似质数筛，质数筛一会会讲到 （~~其实在写很前面的时候我就发现自己其实应该先讲筛法，但是我懒得改~~）

同上单个求取欧拉函数，我们先默认每一个$\varphi(n)=n$

然后从$2$到$n$开始循环

每次当我们发现某个数$i$满足$\varphi(i)=i$时，就把其在$n$以内的倍数（包括自己）全部套一遍公式，因为我们每次都会扫发现的数$i$的倍数，所以我们这时发现的$\varphi(i)=i$的$i$一定是一个质数，所以可以直接给所有含有它的数乘上$(1-\frac {1}{i})$

然后没了
###### （~~就是这么简单~~）
## 继续上 ~~才艺~~ 代码
```cpp
int phi[MAXN]
void  LOP(int n){//lots of phi,缩写LOP
    for(int i=1;i<=n;i++)phi[i]=i;
   for(int i=2;i<=n;i++){
    if(phi[i]==i){
        for(int j=i;j<=n;j+=i){
                phi[j]=phi[j]*(i-1)/i;
        }
    }
   }
}
```
---------
## 啊
## 这 ~~恶心人~~ 美妙的欧拉函数终于 ~~**的~~ 讲完了
-----------------
## 3-2. ~~天杀的~~ 欧拉定理的基本介绍和其 ~~又难又恶心的~~证明
###### 欧拉定理：若$a,n$互质，则有$a^{\varphi(n)} \equiv 1 \ (mod\ n)$
 费马小定理：可以视为欧拉定理的特例 $a^{p-1} \equiv 1 (mod \ p)$其实就是因为$\varphi(p)=p-1,$本质上和欧拉定理没啥区别。
###### 那么我们开始证明这个 ~~阴间玩意~~ 著名定理

 首先我们定义集合$S= \{x_1,x_2,x_3,\cdots,x_{\varphi(n)}\}$（对于$\forall \ x_i,gcd(x_i,a)=1$）

然后再定义集合$T=\{ t_1=a \ast x_1 \ \bmod n,t_2=a \ast x_2 \ \bmod n,t_3 =a \ast x_2 \ \bmod n,\cdots , t_{\varphi (n)}=a\ast x_{\varphi(n)} \ \bmod  n\}$

对于$x_i$,我们有$\gcd(x_i,n)=1$，且$\gcd(a,n)=1$,于是我们可得$ \gcd(a \ast x_i,n)=1$

再根据刚才我们对欧几里得算法原理的证明，可得$gcd(a\ast x_i \ \bmod n,n)=1$

又因为$a\ast x_i \ \bmod n<n$,我们可以得出$a\ast x_i \ \bmod n \in S$

进而我们得出$S=T$（之所以没有两个数重合的原因是$a,n$互质，根据上面讲的余数可除性，假设存在$a\ast x_i \ \bmod n =a\ast x_j \ \bmod n$ ，因为$gcd(a,n)=1$，故若命题情况如上，则$x_i =x_j$。所以当$x_i \not= x_j $时，必有$a\ast x_i \ \bmod n \not=a\ast x_j \ \bmod n$）

继而可得$\prod^{\varphi (n)}_{i=1}x_i = \prod^{\varphi (n)}_{i=1} t_i  $

再将$\\bmod  n$以及$a$取出，得到$ \prod^{\varphi (n)}_{i=1}(x_i\ast a)\equiv \prod^{\varphi (n)}_{i=1}x_i ( \bmod \ n) $


再提取出 $a$,得到$a^{\varphi (n)} \ast \prod^{\varphi (n)}_{i=1}x_i \equiv \prod^{\varphi (n)}_{i=1}x_i (\bmod \ n)$

又因为$ \gcd(x_i,n)=1$,可得$\gcd(\prod^{\varphi (n)}_{i=1}x_i,n)=1$

根据上文带余除法的性质第三条，最终得出$a^{\varphi(n)} \equiv 1 \ (\bmod \ n)$
--------
## ~~终于证完了我快吐了~~

# -4.~~过度~~ 扩展欧几里得
#### 欧几里得算法是用于求取最小公倍数的
#### 而扩展欧几里得就厉害了
#### ~~它本质上还是和那个狗屁公因数有关~~

咳咳，其实它是用于求解关于$a,b$的贝祖公式（即裴蜀公式）$ax+by=d$的解的，其中$x,y$有解当且仅当$\gcd(a,b) \ | \ d$，而绝大多数时候我们需要求解的是$ax+by=\gcd(a,b)$这类方程。

特殊的，$ax+by=1$有解是$a,b$互质的充分必要条件

###### 回到正题，既然它叫做扩展欧几里得，那么它和普通的欧几里得算法的关联在哪里？

如果你发现了，那么恭喜你，你是个 ~~大聪明~~

咳咳，没错，因为$\gcd(a,b) = \gcd(b,a \ \bmod  b)$，所以说，我们可以把原不定方程$ax+by=d$转化成一个新方程$b\ast x_0+(a \ \bmod b)\ast y_0=d$，这样我们就可以通过递归迅速的缩小问题规模。
###### 那么问题来了，当我们缩小问题规模之后，很明显原问题被改变了，那么原来的答案怎么通过小规模问题的答案回推回去呢？

 zjy学长曾曰：“先打表小规模答案，然后瞪眼找规律”


###### ~~没错我们直接瞪眼找规律~~
虽然找规律也是一种方法，但是这个玩意是要当课件用的，我要是被问如何证明正确性我会当场暴毙

###### 所以找规律的自己试，这边我们直接讲证明:
首先我们有 $ax+by=b\ast x_0+(a \ \bmod b)\ast y_0=0$

$\because a\ \bmod b=a-\lfloor \frac{a}{b}\rfloor \ast b$

$\therefore b\ast x_0+(a\ \bmod b)\ast y_0$


$= b\ast x_0+(a-\lfloor \frac{a}{b}\rfloor \ast b)\ast y_0$

$=b\ast x_0+a\ast y_0-\lfloor\frac{a}{b}\rfloor \ast y_0\ast b$

$=y_0\ast a+(x_0-\lfloor\frac{a}{b}\rfloor\ast y_0) \ast b$

然后对$a,b$的系数进行比较
得到$x=y_0,y=x_0-\lfloor\frac{a}{b}\rfloor\ast y_0$。

然后递归求解即可

递归结束的终止条件与正常的欧几里得算法相同，当$b=0$时，$a=gcd(a,b),x=0$

然后再按照公式一个个倒回去即可

###### 这么说太干巴，上代码
```cpp
int exgcd(int a,int b,int& x,int& y){
    if(b==0){x=1;y=0;return a;}
    int ans=exgcd(b,a % b,x,y);
    int t=x;
    x=y;
    y=t-(a/b)*y;
    return ans;
}
```
很明显，我们刚才说过了，贝祖公式要么有无数组解，要么无解。所以现在我们求出来的只是无数个解中的一个

那么通解呢？

我们易证：$a\ast \frac{b}{c}- b\ast \frac{a}{c}=0$其中$c | a  \ \& \ c | b$

那么我们在在原方程中加上这么一个东西，不管加多少倍，他对结果有影响么？

没有影响

我们设$k\in N$

则仍有$a \ast(x+k\ast\frac{b}{gcd(a,b)})+b\ast(y-k\ast\frac{a}{gcd(a,b)})=d $

于是我们得到了贝祖公式的通解

$X=x+k\ast\frac{b}{gcd(a,b)}$

$Y=y-k\ast\frac{b}{gcd(a,b)}$

###### 扩展欧几里得算法在后边我们求乘法逆元时会用到。

# -5. 乘法逆元
###### ~~罕见没加啥奇怪的东西在标题上~~
--------
###### 定义，对于整数$a,p$,若存在$b$使得$a\ast x\equiv 1 (\mod \ p )$，则称$x$为$a$在模$p$意义下的逆元，记做$x=inv(a)$或$x=a^{-1}$。当我们需要多次表示不同模意义下的逆元的时候，可以用$x=inv(a)_p$或$x=a^{-1}_p$表示$a$在模$p$意义下的逆元
需要注意的是，$a$在模$p$意义下存在逆元的充分必要条件是$gcd(a,p)=1$

至于逆元出现的意义是啥嘞？它定义了剩余系（可以理解为取模运算）下的的除法。

因为整数的乘法、加法、减法都具有封闭性，但是除法没有啊

同时因为取模运算仅存在于整数之间

这就导致了除法取模不成立

即$(a/b)\ \bmod p\not= \frac {a\ \bmod p}{b\ \bmod p}\ \bmod p$

但是伟大的数学家们是无法忍受这种恶心人的事的

所以他们开发了乘法逆元这个玩意

乘法逆元同时也可视为倒数的延伸

这样我们就可以通过倒数的性质——我乘我的倒数等于$1$来进行分数取模

同时乘法逆元也在求解同余方程组等时有着很大的作用

最后说一点，在模同一个数意义下，逆元是积性函数，即$(a\ast b) \ast (inv(a)\ast inv(b))$
#### 概念讲的差不多了，我们来求一下好了
#### 在这里，我们讲两种求取乘法逆元的方法
#####
##### 第一种：欧拉定理和费马小定理
 看到这个式子：$a\ast x \equiv 1 (\mod \ p)$，而且有$gcd(a,p)=1$

我们会很自然地联想到欧拉定理（$a^{\varphi(n)} \equiv 1 \ (mod\ n)$）以及他的特殊情况（$n$是质数）费马小定理（$a^{p-1} \equiv 1 (mod \ p)$）

回到逆元这一部分，当我们已经知道了$p$的欧拉函数，我们能很快的解出$a$的逆元

$\because a^{\varphi(p)} \equiv 1 \ (\mod p)$

$\therefore a\ast a^{\varphi(p)-1}\equiv 1 \ (\mod p)$

$\therefore a^{\varphi(p)-1}=inv(a)$

###### 很明显，在此处我们使用了幂运算，而正常需要我们进行取模的都是一些极大质数（通常是$1e9+7$）
这代表该数的欧拉函数值也将很大

所以，为了时间不会爆炸，我们便可以将他的幂运算进行优化

即进行快速幂运算

正常的幂运算的时间复杂度是$O(n)$，

当$n$是偶数时，$a^n=a^{\frac {n}{2}} \ast a^{\frac {n}{2}}$

当$n$是奇数时，$a^n=a^{\frac {n-1}{2}} \ast a^{\frac {n-1}{2}}\ast a$

所以，我们可以根据以上性质将原本时间复杂度从$O(n)$优化到$O(\log n)$

这边就只展示一个快速幂的模板，欧拉函数的求法上面讲过了

快速幂模板:
```cpp
int q_po(int a,int n){
    if(n==1) return a;
    int res=q_po(a,n>>1);
    res=res*res;
    if(n&1)res*=a;
    return res;
}
//当然也可以用循环实现
int q_pox(int a,int n){
    int ans=a,res=1;
    while(n){
        if(n&1)
            res*=ans;
        ans*=ans;
        n=n>>1;
    }
    return res;
}
```


##### 第二种：扩展欧几里得算法

既然我们所求的是在模$p$意义下$a$的逆元，那么我们可以将该逆元$x$和$a,p$构建成一个方程

即$a\ast x-p\ast y=1$

我们将$-p$转化为$b$，这样会好想一些

$a\ast x+ b\ast y=1$

又$\because gcd(a,p)=1$

$\therefore gcd(a,-p)=1$

$\therefore gcd(a,b)=1$

所以$a\ast x+b\ast y=1$是一个有解的关于$a,b$的贝祖公式

然后我们就可以直接扩展欧几里得求解了。


##### 处理区间内逆元($O(n)$)（来源于我蒜哥）

最后讲一下这个，因为在后面讲组合数取模的时候会用到

对于不超过$p-1$的$n$,该算法可以线性处理$1$~$n$模p的逆元

对于一个数$i$的逆元$inv(i)$，我们可以用一个式子来表示

### 该式为:$inv(i)=(p-\lfloor\frac{p}{i} \rfloor)\ast inv(p \ \bmod  i)\ \bmod p$
 至于这个式子他为啥是对的嘞？

 求证：

 $i\ast inv(i)\ \bmod  p=1$

 ~~非常的简单~~(证明来源于我蒜哥)：

 $i\ast inv(i)\ \bmod  p$

 $= i\ast (p-\lfloor \frac {p}{i}\rfloor)\ast inv(p\ \bmod i)\ \bmod p$


$= i\ast(p-\frac{p-p\ \bmod i}{i})\ast inv(p\ \bmod i)\ \bmod p$

$=i\ast p-(p-p\ \bmod i)\ast inv(p\ \bmod i)\ \bmod p$

$\because p\ \bmod p=0$

$\therefore i\ast p\ \bmod p=0$

$\therefore$ 原式 $=p\ \bmod i\ast inv(p\ \bmod i)\ \bmod p$

$=1$

还有一种，利用前缀积维护。

###### 实现：
###### 第一种：
```cpp
//n<p,p是质数，不然不能保证所有数字都有逆元
in[1]=1;
    for(int i=2;i<=n;i++){
        in[i]=(p-p/i)*in[p%i]% p;
    }
```


### 没错讲完了
# -6.组合数取模——卢卡斯定理与乘法逆元
在日常 ~~划水~~ 做题的时候，我们经常会遇到对于某个组合数取模的问题

然而当数据规模巨大无比的时候

直接把这个组合数求出来然后再取模

第一会爆$long \ long $

~~然而我们很懒又不想写高精度~~

第二点也是最主要的一点：时间不够

那么我们就可以在运算的时候做一点优化，使得他可做

这边讲两种方法，一个用乘法逆元，一个用卢卡斯定理。（都需要模的是质数， ~~需要用不是质数的自己去看扩展卢卡斯~~ ）
#### 乘法逆元
我们知道组合数$C_n^m=\frac{n!}{m!(n-m)!}$，那么因为这里有用到除法，所以我们很自然的可以想到使用逆元。

同时又因为逆元是积性函数，所以这题的阶乘逆元会很好整活

$\frac{n!}{m!(n-m)!}\ \bmod p=(n!\ \bmod p\ast (inv(m!)
\ \bmod p)*(inv((n-m)!)\ \bmod p))\ \bmod p$

同时有$inv(a!)=(\prod^{a}_{i=1} inv(i))\ \bmod p$

再使用我们的线性预处理逆元的算法

可以迅速求出答案

###### 求完了你没看错
#### 卢卡斯定理
对于一个组合数$C^n_m$

我们要用它去模$p$

设$q_n=\lfloor \frac{n}{p} \rfloor,q_m=\lfloor \frac{m}{p} \rfloor,r=n\ \bmod  p,t=m \ \bmod  p$

那么 $C_m^n \equiv C_{q_m}^{q_n} \ast C_t^r (\bmod \ p)$

##### 证明时刻！（来源于度娘）：
首先我们定义，当$n>m$时，$C_m^n=0$（显然在不可能有任何取法能在$m$里面取出来一个$n$）

再来证一个前置定理，设$x<p$，则有$C_p^x=\frac{p!}{x!\ast(p-x)!}=\frac{p\ast(p-1)!}{x\ast(x-1)!\ast(x-p)!}=\frac{p}{x}\ast C_{p-1}^{x-1}$

又因$p$是质数，所以必定存在在模$p$意义下关于$x$的逆元

所以有原式$\equiv p\ast inv(x)\ast C_{p-1}^{x-1}$

等式右边显然是$p$的倍数，所以对$\forall \ x<p $，都有$p \ |  \ C_p^x$

然后我们再引入一个前置定理，**二项式定理**

二项式定理：$(1+x)^n=\sum_{i=0}^{n}(C_n^i\ast x^i)$


##### 正式开始证明！！！！！

由二项式定理，我们有$(1+x)^p=\sum_{i=0}^{p}(C_p^i\ast x^i)$

因为我们可知对于$\forall \ x<p$，有$p \  |\ C_p^x$

所以仅当$i=0$或者$i=p$时，$(C_p^i\ast x^i)\ \bmod p \not= 0$

所以我们有$(1+x)^p\ \bmod p=1+x^p$

设$q_n=\lfloor \frac{n}{p} \rfloor,q_m=\lfloor \frac{m}{p} \rfloor,r=n\ \bmod  p,t=m \ \bmod  p$

 $\bmod \ p:$

$(1+x)^m=(1+x)^{q_m\ast p+t}$

$= (1+x)^{q_m\ast p}\ast(1+x)^{t}$

$= ((1+x)^p)^{q_m}\ast(1+x)^{t}$

$\equiv (1+x^p)^{q_m}\ast(1+x)^{t}$

$= \sum_{i=0}^{qm}(C_{q_m}^i\ast x^{ip})\ast\sum_{j=0}^{t}(C_{t}^j\ast x^{j})$

$=\sum_{i=0}^{qm}\sum_{j=0}^{t}(C_{q_m}^i\ast C_{t}^j\ast x^{ip+j})$

因为当$j>t$时，$C_{q_m}^i\ast C_{t}^j\ast x^{ip+j}=0$，所以以上求和式遍历了所有可能的非零项

换句话说，他遍历了从$0$到$m$的$x$的所有系数

所以我们可以转而枚举$k=ip+j$

有$(1+x)^m\equiv\sum_{k=0}^{m}(C_{q_m}^{\lfloor\frac{k}{p}\rfloor}\ast C_{t}^{k\ \bmod p}\ast x^k)$

得到$\sum_{k=0}^{m}(C_m^k\ast x^k)\equiv\sum_{k=0}^{m}(C_{q_m}^{\lfloor\frac{k}{p}\rfloor}\ast C_t^{k\ \bmod p}\ast x^k)$

我们对比该式系数，得到$C_m^k\equiv C_{q_m}^{\lfloor\frac{k}{p}\rfloor}\ast C_t^{k\ \bmod p}$

将$n$带入$k$，得到$C_m^n\equiv C_{q_m}^{\lfloor\frac{n}{p}\rfloor}\ast C_t^{n\ \bmod p}$

又因我们已经定义$q_n=\lfloor \frac{n}{p}\rfloor,r=n\ \bmod  p$

所以原式$C_m^n\equiv C_{q_m}^{q_n}\ast C_t^{r}$

###### 证完了

准备上代码

原式中$C_{q_m}^{q_n}$可以进行递归求解，而$C_t^{r}$因为数据规模较小，直接求取即可

```cpp
int c(int m,int n,int p){
    //组合数求取
    return something;
}
int lucas(int m,int n,int p){
    if(n==0) return 1;
    else return lucas(m/p,n/p,p)*c(m%p,n% p,p)% p;
}
```
复杂度：$O(Plog_2 \ n)$
###### ~~写了这么半天证明然后代码就三句。。。。~~

需要使用$long\ long$的自行修改

#### 扩展卢卡斯戳[这里](https://www.luogu.com.cn/blog/Mr-Stranger-CW-AWA/exLucas#)

### 讲完了
# -7.同余方程(组)——中国剩余定理与一般线性同余方程

同余方程：又称模方程，指形如$ax \equiv r(mod \ p)$ ，其中$a,p$是正整数，$r$是小于$p$的非负整数，$a,r,p$皆为已知。

对于一个单个的同余方程而言，我们仅需要使用扩展欧几里得求解即可，就像求逆元一样将其转化成$ax+by=r$的形式，若是$gcd(a,b) \nmid r$，则原方程无解

但是对于绝大多数的题目，这些题目要求我们求取得往往是一个同余方程的最小非负整数解。

那么当我们知道一组特解的时候该如何求出最小的非负整数解呢？

首先我们注意到这里我们要求的最小非负整数解指的只有$x$，所以我们大可不必关心$y$

再拿出我们之前得出的$ax+by=r$的$x$的通解公式$x=x_0+k\ast\frac{b}{\gcd(a,b)}$

这是我们就只需要保证我们的解$x$最小且非负即可

此时我们已经知道了$x$，那么我们来分类讨论一下

$1.x>=0$，此时我们可以更改$k$的值，使得$X$最小且非负。显然$x=x\ \bmod  \frac{b}{gcd(a,b)}$

$2.x<0$，此时我们只需要一直增加$k$的值，就可以让我们的$x$成为一个正整数，便可以化作$1.x>=0$解决

###### 同余方程组：又称模方程组，指多个未知数相同的同余方程组成的方程组

###### 同余方程组如$\begin {cases} x\equiv a_1 \pmod {m_1}\\ x\equiv a_2 \pmod{m_2}\\\cdots\\ x\equiv a_k \pmod{m_k}\ \ \end {cases}$

前面的单个同余方程还是很好求的

但是到了同余方程组这里难度将直线上升

对于同余方程组的解决，这里我们给两种办法

1.中国剩余定理，简称$CRT$，缺点是只能解决$gcd(m_1,m_2,\cdots,m_k)=1$的情况

2.扩展中国剩余定理，简称$exCRT$，缺点是~~除了名字长得像和1没有任何关系而且还非常的晦涩难懂~~，他的优点是可以解决任何情况下的同余方程（无解除外）

#### 1.中国剩余定理
对于一个同余方程组

$$
\begin {cases} x\equiv a_1 \pmod{m_1}\\ x\equiv a_2 \pmod{m_2}\\\cdots\\ x\equiv a_k \pmod{m_k}\\ \end {cases}
$$

我们现在已知$gcd(m_1,m_2,\cdots,m_k)=1$

设$M=\prod_{i=1}^k(m_i),M_i=\frac{M}{m_i},S_i=inv(M_i)_{m_i}$

则有$x=\sum_{i=1}^k (M_i\ast S_i\ast a_i)$是原方程的一个解

~~Yywd学长:先不要问碳基生物是怎么想到这个式子的。~~

关于这个式子的正确性还是很好证明的

对于方程组中的第$i$个方程

因为$\gcd(m_1,m_2,\cdots,m_k)=1$，$M_i=\frac{M}{m_i}$，所以$M_i$可以被除了$m_i$以外的的所有$m_j$整除

所以我们有 $x\equiv M_i\ast S_i\ast a_i (\bmod \ m_i)$

又因$M_i\ast S_i \equiv1(\bmod \ m_i)$

由此得证$x\equiv a_i(\bmod \ m_i)$

最后说一句，通解只要在原$x$上加任意倍的$M$就行

最小非负整数解的话同上
##### 上代码
```cpp
int inv(int u,int p){
    //do something
    return something;
}
int CRT(){
    int x=0;
    int M=1;
    for(int i=1;i<=n;i++)M*=m[i];
    for(int i=1;i<=n;i++)Mx[i]=M/m[i];
    for(int i=1;i<=n;i++)x+=inv(Mx[i],m[i])*Mx[i]*a[i];
        return x% M;
}
```
#### 2.扩展中国剩余定理
还是对于一个同余方程组
$$
\begin {cases} x\equiv a_1 \pmod{m_1}\\ x\equiv a_2 \pmod{m_2}\\\cdots\\ x\equiv a_k \pmod{m_k}\\ \end {cases}
$$

但是此时我们失去了任何限定条件

我们该如何求解

不放先从只有两个方程的方程组开始推导

对于两个方程$\begin {cases} x\equiv a_1 \pmod{m_1}\\ x\equiv a_2 \pmod {m_2}\end {cases}$

我们可以得到$\begin {cases} x+k_1m_1=a_1\\ x+k_2m_2=a_2\\ \end {cases}$

两式相减得到$k_1m_1-k_2m_2=a_1-a_2$

这又是一个贝祖公式

他有解的条件就不多说了

若有解的话，此时我们设$\gcd(m_1,m_2)=d,K=k_1$的某一个可行解

于是我们有$k_1$的通解$k_1=K+v\frac{m_2}{d}$

此时我们再将$k_1$带入方程$x+k_1m_1=a_1$

得到$x+(K+\frac{vm_2}{d})m_1=a_1$

移项得到$x=a_1-(K+\frac{vm_2}{d})m_1$

因式运算得到$x=a_1-Km_1-\frac{vm_1m_2}{d}$

写成同余方程的形式$x\equiv a_1-Km_1 \pmod {\frac{m_1m_2}{d}}$

##### 这样我们就成功地把两个同余方程合并成了一个
那么我们在进行很多次合并之后，便可以把整个方程组合并为一个单个的同余方程

此时直接取模求解即可

##### 上代码（洛谷的题过不了，因为会爆$long \ long $     , ~~使用$int128$即可通过~~ ）

```cpp
long long  exgcd(long long  a,long long  b,long long & x,long long & y){
    if(b==0){x=1;y=0;return a;}
    long long  ans=exgcd(b,a% b,x,y);
    long long  t=x;
    x=y;
    y=t-(a/b)*y;
    return ans;
}
long long  exCRT(){
    for(int i=2;i<=n;i++){
        long long  na=0,nm=0;
        long long  d=exgcd(m[i-1],m[i],x,y);
        if((a[i-1]-a[i])% d!=0) return -1;
        x*=(a[i-1]-a[i])/d;

        nm=m[i]*m[i-1]/d;
        na=a[i-1]-x*m[i-1];
        a[i]=(na% nm+nm)%nm;
        m[i]=nm;
    }
    return a[n]% m[n];
}
```

# 来来来插一道题：数论全家桶！！！！！

给定$n,g$，求$ g^{\sum_{d | n}C_n^d} \ \  mod \ \  999911659$

$\ \ $

$\ \ $

$\ \ $

$\ \ $

$\ \ $

#### 提示：第一步：欧拉定理；
----
$\ \ $

$\ \ $

$\ \ $

$\ \ $

$\ \ $

$\ \ $

$\ \ $

$\ \ $

$\ \ $

$\ \ $

$\ \ $

$\ \ $

$\ \ $

#### 提示：第二步：卢卡斯定理：
----
$\ \ $

$\ \ $

$\ \ $

$\ \ $

$\ \ $

$\ \ $

$\ \ $

$\ \ $

$\ \ $

$\ \ $

$\ \ $


$\ \ $
#### 提示： $99911658 =2 \times 3 \times  4679\times 35617 $；
----
$\ \ $

$\ \ $

$\ \ $

$\ \ $

$\ \ $

$\ \ $

$\ \ $

$\ \ $

$\ \ $

#### 提示：第三步：中国剩余定理；
----
$\ \ $

$\ \ $

$\ \ $

$\ \ $

$\ \ $

$\ \ $

#### 提示：第四步：输出答案；
 $\ \ $

 $\ \ $

 $\ \ $

-----
# -8.质数筛法——埃/欧

筛法，指在短时间内预处理$1$到$n$的数字是否为质数。

我们先来看如何判断单个数字是否为质数

我们只需要枚举他的因数，如果除$1$和其本身以外还有别的因数的话，那么他就是一个合数。

由于因数成对出现，所以我们仅需要枚举所有小于$\sqrt n$的数即可

###### 上代码
```cpp
bool  ip(int n){
    if(n==1) return 0;
    for(int i=2;i*i<=n;i++){
        if(n% i==0)return 0;
    }
    return 1;
}
```
下面开始就是正正经经的筛法了

这边讲两种筛法：埃氏筛和欧拉筛。前者的时间复杂度为$O(n\ log_2 n)$，后者的时间复杂度为$O(n)$。

#### 1.埃氏筛
因为一个数只要有了至少两个不是$1$的质因数（可以一样）就一定不是质数

所以我们可以先假设每个数字都是质数，每当我们找到一个质数的时候，我们可以去枚举他的倍数，进而将这些数筛出去

###### 上代码
```cpp
void epr(int n){
    memset(pr,1,sizeof(pr));
    pr[1]=0;
    for(int i=2;i<=n;i++){
        if(pr[i]){
            for(int j=2*i;j<=n;j+=i){
                pr[j]=0;
            }
        }
    }

}
```
这里我们再讲个优化，在枚举$j$的时候可以发现，我们可以选择将上面代码中的$2\ast i$替换成$i\ast i$因为那些小于$i$的倍数枚举，都一定在$i$之前被遍历过。

#### 2.欧拉筛

欧拉筛更加快速的原因是他保证了每一个被侦测出的合数只被其最小的质因子遍历。

我们使用一个$prs$数组来记录现在已知的素数

布尔数组$pr$记录是否为素数

每当我们遇到一个为$true$的$pr[i]$，我们就把$i$加入$prs$中，因为他是素数数。

每次枚举$i$的同时，我们用它和已知的素数取遍历。

至于如何保证每个数字只被遍历一次呢？

我们只要在发现$prs[j] \ | \ i$时跳出即可

为什么他是正确的？

因为当我们发现了$i=k\ast prs[j]$时，我们在用$i$和$prs[j+1]$遍历区间时会重复。因为$i=k\ast prs[j]$
所以$i\ast prs[j+1]=k\ast prs[j]\ast prs[j+1]$

因为$prs$数组中的素数是单调递增的，所以这样会使得在后面便利的时候$prs[j+1]\ast i$被重复遍历。

###### 上代码

```cpp
void opr(int n){
    memset(pr,1,sizeof(pr));
    int total=0;
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
```

# -9.一些题目

### [ $\color{#F39C11}{\text{P3383    【模板】线性筛素数}}$ ](https://www.luogu.com.cn/problem/P3383)
### [ $\color{#FFC116}{\text{P1865 A\% B Problem}}$ ](https://www.luogu.com.cn/problem/P1865)
### [ $\color{#FFC116}{\text{P3811    【模板】乘法逆元}}$ ](https://www.luogu.com.cn/problem/P3811)
### [ $\color{#FFC116}{\text{P4549    【模板】裴蜀定理}}$ ](https://www.luogu.com.cn/problem/P4549)
### [ $\color{#52C41A}{\text{P1835    素数密度}}$ ](https://www.luogu.com.cn/problem/P1835)
### [ $\color{#52C41A}{\text{P1082    【NOIP2012 提高组】 同余方程}}$ ](https://www.luogu.com.cn/problem/P1082)
### [ $\color{#3498DB}{\text{P1495    【模板】中国剩余定理(CRT)/曹冲养猪}}$ ](https://www.luogu.com.cn/problem/P1495)
### [ $\color{#3498DB}{\text{P3807    【模板】卢卡斯定理/Lucas 定理}}$ ](https://www.luogu.com.cn/problem/P3807)
### [ $\color{#9D3DCF}{\text{P4777    【模板】扩展中国剩余定理（EXCRT）}}$ ](https://www.luogu.com.cn/problem/P4777)
### [ $\color{#9D3DCF}{\text{P2480 【SDOI2010】古代猪文（你也可以叫他数论全家桶）}}$ ](https://www.luogu.com.cn/problem/P2480)
___
___
___
___
___
___
___
___
___
___
___
___
___
___
___
___
___
___
___
___






# 完结撒花!
