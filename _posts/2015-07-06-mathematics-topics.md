---
layout: post
title: '数学专题'
date: 2015-07-06 07:09
comments: true
categories: 
---
### 想想明年是杜教出题，就果断开了数学的坑
<br>
<del>简直是坑坑坑 <\del><br>
<!--more--><br>
---<br>

<br><br>

# FFT

<br><br>
是一种很有意思的东西<br>
<br><br>
我们定义两个向量的卷积<br>
<br><br>
<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=default"></script><br>
$$ F(n)=\sum_{i} g(i) \times f(n-i) $$<br>
很明显直接暴力计算两个向量的卷积是$$O(n^2)$$的<br><br>
然而我们可以使用一种特殊的手段使得这个过程变为$$O(nlogn)$$<br>
<br><br>
首先，我们知道，我们表示两个多项式，可以使用系数表示法，如：<br>
$$f(x)=a_{n-1}x^{n-1}+a_{n-2}x^{n-2}+ \cdots + a_0$$<br>
我们也可以使用点值表示法，即求出这个多项式上任意$$n$$个点的值，这两种方法是相互等价的<br>
<br><br>
使用点值表示法好处都有啥？我们发现，当计算两多项式的卷积时，直接把点值乘起来就好啦，这样就能做到$$O(n)$$级别，那么我们只需要在$$O(nlogn)$$时间内把系数表示法转换成点值表示法即可。<br>
<br><br>
如果窝们直接计算的话，这同样是$$O(n^2)$$的，这一点也不兹磁。但是窝们可以选择一种具有特殊性质的根，使得复杂度降至$$O(nlogn)$$<br>
<br><br>
窝们定义$$w_n$$是复平面上从$$x$$轴方向旋转$$2 \pi /n$$得到的一个复数，容易知道这就是$$1$$的$$n$$次根，而且其他的$$n-1$$个根都是它旋转$$2 \pi /n$$得到的。<br>
<br><br>
那么我们想要求$$w_n^1 , w_n ^ 2,w_n ^3 \cdots w_n ^ n $$在$$F(x)=a_{n-1}x^{n-1}+a_{n-2}x^{n-2}+ \cdots + a_0$$的取值时，窝们令<br>
$$g_0(x)=a_{n-2}x^{n/2-1}+ \cdots + a_4x+a_2$$<br>
$$g_1(x)=a_{n-1}x^{n/2-1}+ \cdots + a_3x+a_0$$<br>
那么$$F(x)=g_0(x^2)+x g_1(x^2)$$<br>
发现窝们只要知道$$g_0$$的$$w_{n/2-1}^1,w_{n/2-1}^2 \cdots$$和$$g_1$$的$$w_{n/2-1}^1,w_{n/2-1}^2 \cdots$$就能在$$O(n)$$时间内知道$$F(x)$$的$$w_{n}^1,w_{n}^2 \cdots$$ ( 因为$$w_n^i=w_n^{i+n}$$<br>
分治一下就好啦~<br>
<br><br>
那么窝们应该如何把点值表示法变成系数表示法呢？<br>
容易得到<br>
$$a_j=\frac{1}{n} \sum_{k=0}^{n-1}y_k w_n^{-kj}$$<br>
咦..这看起来很熟悉的样子..<br>
我们知道在系数向点值转化的时候$$y_j=\sum_{k=0}^{n-1}a_k w_n^{kj}$$<br>
那我们岂不是把$$w_n$$取一下共轭复数，再与上面的FFT过程运行一样的操作不就好了？<br>
<br><br>
上面说了那么多...可是似乎代码还是不会写啊...<br>
<br><br>
<br><br>
<br><br>
<br><br>
## 背板大法好

<br><br>

---<br>

<br><br>

# 最小割,费用流与线性规划

<br><br>
咦最小割与数学有什么关系啊.....?<br>
似乎并没有什么关系....<br>
但是线性规划算数学对吧对吧对吧............<br>
<br><br>
最小割能解决一些取值为0/1的值的线性规划。<br>
即，若$$x_i$$取0，则这个点归于左边，若$$x_i$$取1，则这个点归于右边，形如$$max(0,x_i-x_j)$$这样的式子，就从i到j连一条边。如果在取$$x_j$$为0(1)之前必先取$$x_i$$为1(0),那么i向j连一条inf边即可<br>
<br><br>
费用流能利用流量守恒解决一些变量系数为1的线性规划。<br>
把每一个方程当场一个节点。$$+x_i$$表示有$$x_i$$的流量流入，$$-x_i$$表示有$$x_i$$的流量流出，常数项表示从S流来或流向T的流量。<br>
<br><br>
<br><br>

---<br>

<br><br>

# 单纯形



<br><br>
是一种通用的用来求解线性规划的办法。<br>
通常用于求解<br>
$$Maxsize  \sum c_ix_i$$<br>
$$b_k=\sum a_{ik}x_i$$<br>
具体是如何运行的呢？<br>
假设$$x_i$$都为0是满足条件的。<br>
每次从目标函数中选择一个$$c_i$$最大的(如果$$c_i$$皆$$< 0$$就完成<br>
然后从下面的几个约束中选择一个最紧的换元把元换上去，这样一直换一直换。<br>
可是如果初始$$x_i$$皆为0不满足条件应该怎么搞呢？<br>
<br><br>
窝们构造一个辅助线性规划<br>
$$Maxsize -x_0$$<br>
满足约束<br>
$$b_k=\sum a_{ik}x_i +x_0$$<br>
然后只要对这个不断地换啊换啊换啊换换出一个可行解然后把$$x_0$$去掉就是一组可行解.<br>
<br><br>
线性规划有一个对偶形式<br>
它原来是求<br>
$$Maxsize \sum c_ix_i$$<br>
$$\sum a_{ki}x_{i} \leq b_i$$<br>
它的对偶形式是<br>
$$Minsize \sum c_ix_i$$<br>
$$\sum a_{ki}x_{i} \geq b_i$$<br>
怎么做呢？把矩阵转置一下，即$$x_{ij}=x_{ji}$$然后用相同的程序跑一份即可<br>
<br><br>
至于为什么...不会证<br>
<br><br>

---<br>

<br><br>

# 莫比乌斯反演

<br><br>
首先我们知道一个叫狄尼克雷卷积的东西..<br>
即两个数论的狄尼克雷卷积可以表示为<br>
$$F_n=\sum_{d|n} f_dg_{\frac{n}{d}}$$<br>
狄尼克雷卷积的单位元显然是<br>
$$e(1)=1  \\  others=0$$<br>
正常向的卷积的单位元就是$$u(x)=1$$<br>
然后窝们定义<br>
<br><br>
![810a19d8bc3eb135782be1b6a61ea8d3fc1f44ca.jpg](http://user-image.logdown.io/user/12229/blog/11527/post/283639/lIz9wo2yQ0ygL0gp47VV_810a19d8bc3eb135782be1b6a61ea8d3fc1f44ca.jpg)<br>
<br><br>
然后发现在狄尼克雷卷积的意义下$$\mu * u=e$$<br>
所以我们有莫比乌斯反演<br>
$$f(n)=\sum_{d|n} g(d)$$<br>
$$g(n)=\sum_{d|n} \mu(d)f(\frac{n}{d})$$<br>
证明:<br>
1.<br>
    $$f=g*u$$<br>
    $$g=f*\mu$$<br>
    用上面的定义一下就能出来<br>
2.<br>
	 $$\sum_{d|n} \mu(d) f(\frac{n}{d})=\sum_{d|n} \mu(d) \sum_{l|\frac{n}{d}}f(l)=\sum_{l|n} f(l)\sum_{d|\frac{n}{l}} \mu(d)$$<br>
   然后只有$$n=1$$时$$\sum_{d|n} \mu(d)=1$$，所以上式$$=g(n)$$<br>
   <br><br>
   <br>
---<br>

<br><br>

# 高斯消元和矩阵乘法

<br><br>
都是很暴力的东西，没什么好说的<br>
<br><br>

---<br>

<br><br>

# Lucas定理

<br><br>
其实窝很喜欢这个证明<br>
<br><br>
Lucas定理的主要内容是<br>
$$a=a_np^n+a_{n-1}p^{n-1}+\cdots+a_0$$<br>
$$b=a_np^n+a_{n-1}p^{n-1}+\cdots+a_0$$<br>
其中<br>
$$a_i,b_i < p,p为质数$$<br>
那么<br>
$$C_{a}^{b} \equiv C_{a_0}^{b_0}C_{a_1}^{b_1} \cdots C_{a_n}^{b_n}(mod ~~ p)$$<br>
怎么证明呢？<br>
首先易得<br>
$$(1+x)^p=1+C_p^1 p+C_p^2 p^2 + \cdots + x^p$$<br>
而$$C_p^j=\frac{p}{j}C_{p-1}^{j-1}$$<br>
注意到$$C_p^j$$是整数，所以$$\frac{p}{j}C_{p-1}^{j-1}$$也是整数，且分数下面没有关于$$p$$的因数，所以$$C_p^j \equiv 0 (mod ~~ p)$$，即$$(1+x)^p \equiv 1+x^p (mod ~~ p)$$<br>
那么<br>
$$(1+x)^a=(1+x)^{a_0}+((1+x)^p)^{a_1} \cdots ((1+x)^{p^k})^{a_k} \equiv (1+x)^{a_0}(1+x^p)^{a_1} \cdots (1+x^{p^k})^{a_k}(mod ~~ p) $$<br>
我们对比两边$$x^b$$项的系数，发现如果把右边$$x^b$$系数看成是一个$$p$$进制数，显然只能在$$a_k$$的时候对应地取$$b_k$$(因为$$p$$进制数的性质可知)，然后结合二项式定理可得<br>
$$C_{a}^{b} \equiv C_{a_0}^{b_0}C_{a_1}^{b_1} \cdots C_{a_n}^{b_n}(mod ~~ p)$$<br>
<br><br>

--------<br>

<br><br>

# 扩展gcd与裴蜀定理

裴蜀定理主要是:<br>
$$ax+by=d$$存在整数解的充分必要条件是$$d|gcd(a,b)$$<br>
证明利用辗转相处进行简单的数学归纳即可。<br>
扩展gcd是裴蜀定理的进一步扩展，它可以求出$$ax+by=d$$的整数解是什么。<br>
同样利用简单的数学归纳就可以手推出来。<br>
<br><br>

---<br>

<br><br>

# 费马小定理与欧拉定理

费马小定理:$$(a,p)=1,p为质数，a^{p-1} \equiv 1 ~~ (mod ~~ p)$$<br>
欧拉定理:$$(a,p)=1,a^{\phi(p)} \equiv 1 ~~ (mod ~~ p)$$<br>
证明(bu)显然。<br>
其实只要考虑一下它的缩系是能证的...<br>
<br><br>

---<br>

<br><br>

# 拉格朗日定理与威尔逊定理

拉格郎日定理：$$p为素数,f(x)为整系数多项式，f(x)=a_nx^n+a_{n-1}x^{n-1}+ \cdots +a_0，p \nmid a_n$$<br>
那么$$f(x) \equiv 0 ~~ (mod ~~ p)$$ 最多只有$$n$$个解。<br>
数学归纳法显然....<br>
<br><br>
威尔逊定理:$$p为素数，(p-1)! \equiv -1 ~~ (mod ~~ p)$$<br>
证明考虑使用拉格朗日定理<br>
构造多项式 $$ f(x)=(x-1)(x-2)(x-3) \cdots (x-(p-1))-(x^{p-1}-1) $$<br>
显然这是一个$$p-2$$次多项式<br>
然而窝们发现，这在模$$p$$意义下...有$$p-1$$个解。<br>
这与拉格朗日定理冲突啦...<br>
所以说明这里的每一项都被$$p$$整除<br>
所以就$$p \mid (p-1)!+1$$<br>
故得证<br>
<br><br>

---<br>

<br><br>

# 中国剩余定理<br>
主要是用于求解线性同余方程组用的..<br>
对于形如以下的线性同余方程组<br>
$$ x \equiv c_i ~~ (mod ~~ m_i) \\ \cdots $$<br>
当$$m_i$$两两互质时，可以利用CRT来合并它们<br>
记$$M=\prod_{i} m_i ~~~ t_i为\frac{M}{m_i}对m_i的逆元$$，<br>
那么答案可以写成$$T=\sum_{i} (\frac{M}{m_i}t_ic_i) $$<br>

唔...如果$$m_i$$之间不互质怎么办呢？<br>
考虑利用扩展gcd与增量法<br>
最开始一个方程，然后每次加入一个方程<br>
$$x \equiv a (mod ~~ b) \\ x \equiv c (mod ~~ d)$$<br>
记$$x=bt+a$$<br>
即$$bt+a \equiv c (mod ~~ d)$$<br>
那么可以通过exgcd来求出$$t \equiv t_0 (mod ~~ \frac{d}{(b,d)})$$<br>
这样就可以通过$$x=bt+a 求出 x \equiv e (mod ~~ \frac{d}{(b,d)})$$<br>
再结合第一式CRT(这时候肯定是互质的<br>
虽然常数比较大，但它一定会在常数炸了之前先炸long long<br>

<br><br>

---<br>

<br><br>
# 阶与原根<br>

对于一个$$a,且(a,m)=1,我们定义a对m的阶是min(r)使得a^r \equiv 1 (mod ~~ m),记为\delta_m(a)$$<br>
那么定义$$m$$的原根为最小的$$ a使得\delta_m(a)=\phi(a)$$<br>
注意$$\delta_m(x)$$是积性函数<br>
素数一定存在原根<br>
不会证<br>
<br><br>

---<br>
<br><br>

#Miller Rabin && pollard rho<br>
是两种奇怪的概率性算法。<br>
Miller Rabin是利用小费马<br>
$$a^p-1 \equiv 1 (mod ~~ p)$$这个性质来判断p是不是质数。为了让正确性进一步提高，它还加入了一个特殊的判断，即如果<br>
$$a^2 \equiv 1(mod ~~ p)$$，那么$$a \equiv 1 \ or \ p-1 (mod ~~ p)$$<br>
<br><br>
对于pollard rho呢....它更是开了脑洞的暴力....<br>
窝们随机$$x_1,x_2$$,算$$gcd(x_1-x_2,n)$$,如果$$gcd(x_1-x_2,n)!=n \ or \ 1$$，那么这个结果就是n的因数之一。<br>
<br><br>

---<br>

<br><br>
# 组合数取模<br>
<br><br>
感谢Stillwell的课件...........<br>
<br><br>
## subtask 1<br>
$$m \leq n \leq 1000，p \leq 10^9$$<br>
利用$$C_n^m=C_{n-1}^{m-1}+C_{n-1}^{m}$$暴力计算即可<br>
复杂度$$O(nm)-O(1)$$<br>
<br><br>
## subtask 2<br>
$$m \leq n \leq 10^5,p \leq 10^9 ~~ p$$为质数。<br>
先介绍一种利用线性筛$$O(n)$$求$$1~n$$的乘法逆元的方法<br>
是窝自己脑补出来的<br>
设$$n=ab ~~ ae \equiv 1 (mod ~~ p) ~~~~ bu \equiv 1 (mod ~~ p)$$<br>
那么$$1 \equiv ae \times bu=n \times (eu) ~~ (mod~~p)$$<br>
那么$$n$$的逆元就是$$eu$$，线性筛一下即可。<br>
那么怎么求$$n!$$的乘法逆元呢？<br>
窝们设$$(n-1)!$$的乘法逆元是$$e$$，$$n$$的乘法逆元是$$u$$。<br>
$$1 \equiv (n-1)!e \times nu=n! \times (eu) ~~ (mod~~p)$$<br>
即$$n!$$的逆元就是$$eu$$，暴力递推一下。<br>
回到窝们原来的问题上<br>
$$C_n^m=\frac{n!}{m! \times (n-m)!}$$<br>
预处理出来上面那些东西就好啦<br>
复杂度$$O(n)-O(1)$$<br>
<br><br>
## subtask 3<br>
$$m \leq n \leq 10^{18}，p \leq 10^5$$<br>
Lucas定理之后就变成了Subtask 2<br>
复杂度$$O(p)-O(log_p^n)$$<br>
<br><br>
## subtask 4<br>
$$m \leq n \leq 10^5,p \leq 10^5 $$<br>
首先，我们利用线性筛，筛出$$1-n$$中所有质数，这是$$O(\frac{n}{ln \ n})$$级别的<br>
然后对于每一个质数$$p$$，它在$$n!$$中出现的次数为$$\sum  \left \lfloor \frac{n}{p^i} \right \rfloor$$<br>
把上面的暴力计算就好了，复杂度约为$$O(\frac{n}{ln \ n} \times log_{p}^{n} \approx n)$$<br>
<br><br>
## subtask 5<br>
$$m \leq n \leq 10^{18},p^c \leq 10^5 ~~  p$$为质数<br>
因为涉及除法和方便计算逆元，我们把质因数带$$p$$的不带的分开计算<br>
容易知道，除去质因数带$$p$$的外，$$\prod_{i=1}^{p^c} \equiv \prod_{i=p^c+1}^{2p^c} ~~ (mod ~~ p^c)$$，所以对于这一部分的只要先预处理出$$1 - p^c$$对$$p^c$$去模之后快速幂即可。<br>
对于质因数带$$p$$的，可以发现如果提出$$p$$和之前上部分是相似的，因此分治即可。<br>
时间复杂度$$O(p^c)$$<br>
<br><br>
## subtask 6<br>
会啦会啦<br>
主要是要快速求$$n!$$<br>
详见后面的FFT部分<br>
 <br><br>
 <br>
 ---<br>
 <br>
 <br><br>
# 容斥原理、抽屉原理与二项式反演<br>
前两项显然<br>
二项式反演: 若$$f(n)=\sum_{k=0}^{n} C_n^k g(k)$$<br>
那么$$g(n)=\sum_{k=0}^{n} (-1)^{n-k}C_n^kf(k)$$<br>
证明:<br>
$$~~~\sum_{k=0}^{n} (-1)^{n-k}C_n^kf(k)$$<br>
    $$=\sum_{k=0}^{n} (-1)^{n-k}C_n^k(\sum_{i=0}^{k}C_k^i g(i))$$<br>
    $$=\sum_{k=0}^{n} \sum_{i=0}^{k} (-1)^{n-k} C_n^k C_k^i g(i)$$<br>
    $$=\sum_{i=0}^{n} (\sum_{k=i}^{n}(-1)^{n-k}C_n^kC_k^i)g(i)$$<br>
发现:<br>
$$C_n^k C_k^i=\frac{n!}{k!(n-k)!} \frac{k!}{i!(k-i)!}=\frac{n!}{i!{(n-i)!}}\frac{(n-i)!}{(k-i)!(n-k)!}=C_n^iC_{n-i}^{k-i}$$<br>
$$~~~\sum_{k=i}^{n} (-1)^{n-k}C_n^k C_k^i $$<br>
$$=\sum_{k=i}^{n} (-1)^{n-k}C_n^iC_{n-i}^{k-i} $$<br>
$$=C_n^i \sum_{k=i}^{n}(-1)^{n-k}C_{n-i}^{k-i} $$<br>
$$=C_n^i \sum_{k=0}^{n-i}(-1)^{n-i-k}C_{n-i}^k $$<br>
$$=C_n^i (1-1)^{n-i}$$<br>
于是，只有当$$i=n$$时，$$\sum_{k=i}^{n}(-1)^{n-k}C_n^kC_k^i=1$$。<br>
故$$g(n)=\sum_{k=0}^{n} (-1)^{n-k}C_n^kf(k)$$<br>
证毕<br>
<br><br>

---<br>

<br><br>
# 奇怪的计数数列


## Stirling 数<br>
分为第一类Stirling数与第二类Stirling数<br>
第一类Stirling数是有正负的，其绝对值是$$n$$个不同元素分成$$k$$组后，每组内再按分顺序的方案数.<br>
容易得到$$S(n,k)=S(n-1,k-1)+（n-1)S(n-1,k)$$<br>

第二组Stirling数是$$n$$个不同元素划分成$$k$$组的方案数.<br>
容易得到$$S(n,k)=S(n-1,k-1)+kS(n-1,k)$$<br>

## Bell 数<br>
$$B_n:$$把n个不同数划分成任意个数个集合的方案书.<br>
$$B_{n+1}=\sum_{i=0}^{n} C_{n}^{i}B_i$$<br>
$$B_n=\sum_{i=1}^{n} S(n,i)$$<br>
$$p ~~ is ~~ prime.B_{p^m+n} \equiv mB_n+B_{n+1} ~ (mod ~~ p)$$,以$$\frac{p^p-1}{p-1}$$为周期<br>

## Catalan 数<br>
有n对括号的括号序列个数<br>
$$C_n=(_{n}^{2n})-(_{n-1}^{2n})$$<br>
$$C_n=\frac{4n-2}{n+1}C_{n-1}$$<br>
$$C_n=\sum_{k=0}^{n-1}C_kC_{n-k-1}$$<br>

## Narayana 数<br>
有n对括号序列，并且k对相邻的序列个数<br>
$$N(n,k)=\frac{1}{n}(_{k}^{n})(_{k-2}^{n})$$<br>
$$\sum_{k=1}^{n}N(n,k)=C_n$$<br>

## Motzkin 数<br>
在圆上等距的n个点连线的方案数，使得这些线不相交<br>
$$M_n=\sum_{k=0}^{\left \lfloor {\frac{n}{2}} \right \rfloor}(_{2k}^{n})C_k$$<br>
$$M_{n+1}=M_n+\sum_{k=0}^{n-1}M_kM_{n-k-1}$$<br>
$$M_{n+1}=\frac{(2n+3)M_n+3nM_{n-1}}{n+3}$$<br>

---<br>

<br><br>
# NTT<br>
感觉也是蛮显然的..只是把单位根换成了在模意义下的单位根而已..<br>

---<br>

<br><br>
# 分治+FFT<br>
可以求解形如$$f_n=\sum_{i=1}^{n-1}f_i*a_{n-i}$$形式的东西..<br>
考虑cdq分治..<br>
假设要求出$$(l,r)$$区间的$$f$$<br>
可以先求出$$(l,mid)$$的$$f$$.<br>
考虑左边对右边的贡献，容易算出是$$f(l,mid)*a(1,mid-l+1)$$<br>
$$T(n)=O(nlogn)+T(n/2)$$<br>
时间复杂度$$O(nlog^2n)$$<br>

---<br>

<br><br>
# 多项式求逆、除法、开方。<br>


## 1.多项式求逆:<br>
即求$$B(x)$$使得$$A(x)B(x) \equiv 1 ~~ (mod ~~ x^n)$$<br>
考虑倍增<br>
假设我们已经算出了$$B_0(x)$$使得$$A(x)B_0(x) \equiv 1 ~~ (mod ~~ x^n)$$,我们接下来要求$$B(x)$$使得$$A(x)B(x) \equiv 1  ~~ (mod ~~ x^{2n})$$<br>
$$A(x)B(x) \equiv 1(mod ~~ x^{2n}) ~~~ -> A(x)B(x) \equiv 1 ~~ (mod ~~ x^n)$$<br>
$$A(x)[B(x)-B_0(x)] \equiv 0  ~~ (mod~~x^n)$$<br>
$$B(x)-B_0(x) \equiv 0  ~~ (mod ~~ x^n)$$<br>
$$(B(x)-B_0(x))^2 \equiv 0  ~~ (mod ~~ x^{2n})$$<br>
$$B_(x)^2-2B(x)B_0(x)+B_0(x)^2 \equiv 0 ~~ (mod ~~ x^{2n})$$<br>
$$B_(x)-2B_0(x)+A(x)B_0(x)^2 \equiv 0 ~~ (mod ~~ x^{2n})$$<br>
$$B_(x) \equiv B_0(x)(2-A(x)B_0(x)) ~~ (mod ~~ x^{2n})$$<br>
注意上面的2不是常数2,而是常数列2，所以每项都要减去。<br>
$$T(n)=T(n/2)+O(nlogn)$$<br>
时间复杂度还是$$O(nlogn)$$<br>
是FFT常数的$$5-10$$倍的样子.........想想FFT的常数还要乘上个$$5$$倍真是令人感动。<br>

## 2.多项式除法<br>
给出$$A(x),B(x)$$<br>
求$$D(x),R(x)使得A(x)=B(x)D(x)+R(x)$$<br>
并且$$deg~D=deg~A-deb~B $$<br>
$$deg~R ~ \leq deg ~ B$$<br>
窝们发现，如果没有$$R(x)$$的话，这是很好做的，只要搞出逆元就可以啦。<br>
这启发我们可以通过在模意义下来搞一搞，把$$R(x)$$去掉，又发现$$R(x)$$的次数是最小的，那么可以把它倒过来模一模就可以啦。<br>
$$设deg~A=n,deg~B=m$$<br>
$$A(\frac{1}{x})=B(\frac{1}{x})D(\frac{1}{x})+R(\frac{1}{x})$$<br>
$$x^nA(\frac{1}{x})=x^mB(\frac{1}{x})x^{n-m}D(\frac{1}{x})+x^nR(\frac{1}{x})$$<br>

若$$f(x)=\sum_{i=0}^{n} a_ix^i,那么记f_{rev}(x)=\sum_{i=0}^{n}a_{n-i}x^i$$<br>
易得$$x^{deg~f}f(\frac{1}{x})=f_{rev}(x)$$<br>


那么继续可得<br>
$$A_{rev}(x)=B_{rev}(x)D_{rev}(x)+x^{n-m+1}R_{rev}(x)$$<br>
$$A_{rev}(x) \equiv B_{rev}(x)D_{rev}(x) ~~ (mod ~~ x^{n-m+1})$$<br>
$$D_{rev}(x) \equiv A_{rev}(x)[B_{rev}(x)]^{-1} ~~ (mod ~~ x^{n-m+1})$$<br>
而$$deg ~ B=n-m< n-m+1$$<br>
故可以直接计算。<br>
然后代回去算一算就能把$$R(x)$$搞出来<br>
不过常数比逆元还要大$$1,2$$个FFT的常数的样子...真是令人感动..(感觉和Top tree有的一拼...？<br>

## 多项式开方<br>
 仍然考虑倍增..<br>
 如果$$A_n(x)^2 \equiv A(x) ~~ (mod ~~ x^n)$$<br>
 $$(A_n(x)^2-A(x))^2 \equiv  0 ~~ (mod ~~ x^{2n})$$<br>
 $$(A_n(x)^2+A(x))^2 \equiv 4A_n(x)^2A(x) ~~ (mod ~~ x^{2n})$$<br>
 $$[(A_n(x)^2+A(x)) \times (2A_n(x))^{-1}]^2 \equiv A(x) ~~ (mod ~~ x^{2n})$$<br>
 每次倍增的时候顺便维护一下$$A_n(x)^{-1}$$就好啦<br>
 感觉常数能是多项式逆元的一倍...？<br>

---<br>

<br><br>

# 生成函数

也叫母函数..<br>
因为之前MO已经学过了母函数所以觉得非常显然= =<br>
一般生成函数主要用于取东西时系数为1的的时候。<br>
指数生成函数主要用于取东西的时候还要乘上一个组合数的时候。<br>
配合上面的多项式各种算法使用效果更佳QAQ..<br>

---<br>
<br><br>
# BSGS && exBSGS<br>
主要用于求解$$A^x \equiv B (mod ~~ p)$$这样的问题。<br>
## BSGS<br>
$$p$$为质数<br>
记$$block=\left \lfloor \sqrt{n} \right \rfloor+1$$<br>
那么可以先暴力算出$$A^{i \times block},1 \leq i \leq block$$，存在hash里<br>
然后暴力枚举$$A^i,1 \leq i \leq block$$，每次查表即可。<br>
<br><br>
## exBSGS<br>
$$p$$不为质数。<br>
首先知道<br>
$$ad \equiv bc (mod ~~ cd) ~~ -> ~~ a \equiv b (mod ~~ c)$$<br>
证明显然...<br>
那么<br>
$$while (gcd(A,P)==1) \\ ~~~~t=gcd(A,P) \\ ~~~~if ~~ (B~~mod~~t  \not=0)~~return~~{ -1} \\ ~~~~count~~\times=~~t\\~~~~A/=t,B/=t,P/=t \\ \\ BSGS(A,B,P)$$<br>
考虑为什么这段伪代码是正确的呢..？<br>
首先，窝们要尽量让$$(A,P)=1$$,这样就能直接套BSGS<br>
那么窝们不断消就可以啦<br>
为什么<br>
$$if ~~ (B~~mod~~t  \not=0)~~return~~{ -1} $$<br>
是正确的呢..？<br>
泥萌可以想想....如果左边$$~~mod ~~ c=0,~~p~~ mod~~c=0$$,但是右边那个$$~~mod~~ c \not=0$$，这当然不对啦<br>
注意到还有小部分没有算过，即被消掉的那部分...那部分显然暴力一下就好啦>_<<br>

---<br>
<br><br>
# 多项式求值，插值<br>
<br><br>
## 牛顿插值和拉格朗日插值<br>
<br><br>
先讲牛顿插值。<br>
考虑待定系数。<br>
$$f(x)=a_0+a_1*(x-x_0)+\cdots+a_{n+1}*(x-x_0) \cdots(x-x_{n}) \\ f(x_0)=y_0 ~~ -> a_0=y_0 \\ f(x_1)=y_1 ~~ ->a_0+(x_1-x_0)*a_1=y_1 ~~ -> a_1=\frac{y_1-y_0}{x_1-x_0} \\ \cdots \\  \\$$<br>
那么窝们定义差商<br>
$$f[x_0,x_1]=\frac{f[x_1]-f[x_0]}{x_1-x_0}$$<br>
$$f[x_0,\cdots,x_n]=\frac{f[x_1,\cdots,x_n]-f[x_0,\cdots,x_{n-1}]}{x_n-x_0}$$<br>
易得$$f[x_0,\cdots,x_n]=\sum_{k=0}^{n}\frac{y_k}{(x_k-x_0)\cdots(x_k-x_{k-1})(x_k-x_{k+1})\cdots(x_k-x_n)}$$<br>
那么<br>
$$f(x)=\sum_{i=0}^{n} f[x_0,\cdots,x_i](x-x_0)\cdots(x-x_{i-1})$$<br>
牛顿插值只要先$$O(n^2)$$预处理出差商表就能$$O(n)$$回答区间$$L-R$$的$$f(x)$$或者再添加一个点辣>_<<br>
<br><br>

拉格朗日插值就简单得多啦..<br>
$$p_j(x)=\frac{(x-x_1)\cdots(x-x_{j-1})(x-x_{j+1})\cdots(x-x_n)}{(x_j-x_1)\cdots(x_j-x_{j-1})(x_j-x_{j+1})\cdots(x_j-x_n)} \\   \\f(x)=\sum_{i=1}^{n}y_ip_i(x)$$<br>

<br><br>
## FFT多点求值、快速插值<br>
<br><br>
### FFT多点求值<br>
考虑分治，将询问分组<br>
$$X^{[0]}=(x_1,\cdots,x_{ \lfloor \frac{n}{2}  \rfloor }) \\ X^{[1]}=(x_{ \lfloor \frac{n}{2} \rfloor +1},\cdots,x_n)$$<br>
并记<br>
$$P^{[0]}=(x-x_1)\cdots (x-x_{ \lfloor \frac{n}{2}  \rfloor }) \\P^{[1]}=(x-x_{ \lfloor \frac{n}{2}  \rfloor })\cdots (x-x_n) \\  $$<br>

<br><br>

$$  \\F(x)=D(x)P^{[0]}(x)+A^{[0]}(x) $$<br>
对于$$x \in X^{[0]},F(x)=A(x)$$<br>
递归下去即可<br>
$$T(n)=2T(n/2)+O(nlogn) \\ $$ 复杂度$$ O(nlog^2n)$$<br>
<br><br>

### FFT快速(?)插值<br>
$$X^{[0]}=((x_i,y_i),1 \leq i \leq  \lfloor \frac{n}{2}  \rfloor)\\X^{[1]}=((x_i,y_i), \lfloor \frac{n}{2}  \rfloor+1 \leq i \leq n)$$<br>
假设窝萌已经求出了$$X^{[0]}$$的插值多项式$$A^{[0]}$$，那么假设<br>
$$P(x)=\prod_{i=1}^{\lfloor \frac{n}{2}  \rfloor}(x-x_i)\\F(x)=A^{[1]}(x)P(x)+A^{[0]}(x)$$<br>
容易知道，对于$$x \in X^{[0]},F(x)$$都是成立哒，接下来窝们只要让$$x \in X^{[1]}$$成立就可以啦。<br>
$$y=A^{[1]}(x)P(x)+A^{[0]}(x)\\A^{[1]}(x)=\frac{y-A^{[0]}(x)}{P(x)}$$<br>
用快速多点求值之后递归下去插值就可以啦<br>
$$T(n)=2T(n/2)+O(nlog^2n)=O(nlog^3n)$$<br>
<del>窝赌五毛跑得没牛顿插值快！<del><br>

## 杜教插值（雾<br>
杜教的黑科技<br>
已知$$F(0),F(1),\cdots,F(k)$$<br>
能在$$O(k)$$时间求出$$F(n)$$<br>
[见这里](http://blog.miskcoo.com/2015/08/special-polynomial-linear-interpolation)<br>
唔..其实就是利用组合数是$$m$$次多项式，然后二项式反演一下。<br>
这里之所以把第二个式子反演出来的带回第一个能出答案就是因为第一个式子和第二个式子在$$x>k$$时是不一样的。我们是利用第二个式子和已知的$$P(0)\cdots P(k)$$反演来确定了$$p_i$$后再带入第一个式子求解。<br>

-----<br>
<br><br>
<br><br>
bzoj 2179:FFT裸题<br>
bzoj 2194:将a数组翻转之后求卷积，之后把ans数组翻转就是答案<br>
bzoj 3527:推一推就会发现是卷积形式...<br>
bzoj 2127:S表示种在A,T表示种在B.<br>
$$~~~~~~~~~~~~~~~$$s->A:cost[A文]+c[文][A][B]/2,s->B:cost[B文]+c[文][A][B]/2;<br>
$$~~~~~~~~~~~~~~~$$A->t:cost[A理]+c[理][A][B]/2,B->t:costB[理]+c[理][A][B]/2;<br>
$$~~~~~~~~~~~~~~~$$A<–>B:c[文][A][B]/2+c[理][A][B]/2<br>
$$~~~~~~~~~~~~~~~$$这样会出现两种割，分别对应两种相同，一种选文一种选理。<br>
bzoj 3438:首先建立源点汇点S、T表示种在a，b两片土里，流量为收益。<br>
$$~~~~~~~~~~~~~~~$$然后每次读到一个集合的时候，新建两个虚拟节点si、ti，表示都种在a，b里的收益<br>
$$~~~~~~~~~~~~~~~$$S向si连边，ti向T连边，流量为收益，然后si、ti向集合内所有点连边,流量无穷大。<br>
$$~~~~~~~~~~~~~~~$$之后跑一边最大流，ans = 总收益 - 最小割<br>
bzoj 4177:上面两题的2 in 1..<br>
bzoj 1061:建立方程组之后以每个方程组为节点跑最小费用流<br>
bzoj 3676:回文树板子题..<br>
bzoj 3112:建立方程之后跑单纯形.<br>
bzoj 1013:建立关于球心的方程，高斯消元就好啦<br>
bzoj 1876:python大法吼！<br>
bzoj 2818:$$\sum_{p}\sum_{i=1}^{n}\sum_{j=1}^{m}[gcd(i,j)==p]$$<br>
$$~~~~~~~~~~~~~~~=\sum_p\sum_{i=1}^{\lfloor\frac{n}{p}\rfloor}\sum_{j=1}^{\lfloor\frac{n}{p}\rfloor}[gcd(i,j)==1]$$<br>
$$~~~~~~~~~~~~~~~=\sum_p\sum_{i=1}^{\lfloor\frac{n}{p}\rfloor}\sum_{j=1}^{\lfloor\frac{n}{p}\rfloor}\sum_{d|gcd(i,j)}\mu(d)$$<br>
$$~~~~~~~~~~~~~~~=\sum_p\sum_{d=1}^{\lfloor\frac{n}{p}\rfloor}\mu(d)\sum_{i=1}^{\lfloor\frac{n}{pd}\rfloor}\sum_{j=1}^{\lfloor\frac{n}{pd}\rfloor}1$$<br>
$$~~~~~~~~~~~~~~~=\sum_p\sum_{d=1}^{\lfloor\frac{n}{p}\rfloor}\mu(d){\lfloor\frac{n}{pd}\rfloor}{\lfloor\frac{n}{pd}\rfloor}$$<br>
$$~~~~~~~~~~~~~~~=\sum_{k=1}^{n}\sum_{p|k}\mu(\frac{k}{p})\lfloor\frac{n}{k}\rfloor\lfloor\frac{n}{k}\rfloor$$<br>
$$~~~~~~~~~~~~~~~=\sum_{k=1}^{n}F(k)\lfloor\frac{n}{k}\rfloor\lfloor\frac{n}{k}\rfloor$$<br>
$$~~~~~~~~~~~~~~~$$预处理出$$F(k)$$函数的前缀和后下底函数分块..<br>
bzoj 2820:同上..<br>
bzoj 2190:$$\sum_{i=1}^{n}\sum_{j=1}^{m}[gcd(i,j)==1]$$<br>
$$~~~~~~~~~~~~~~~=\sum_{i=1}^{n}\sum_{j=1}^{n}\sum_{d|gcd(i,j)}\mu(d)$$<br>
$$~~~~~~~~~~~~~~~=\sum_{d=1}^{n}\mu(d)\lfloor\frac{n}{d}\rfloor\lfloor\frac{n}{d}\rfloor$$<br>
$$~~~~~~~~~~~~~~~$$预处理出$$\mu(k)$$函数的前缀和后下底函数分块..<br>
bzoj 1101:$$\sum_{i=1}^{n}\sum_{j=1}^{n}[gcd(i,j)==d]$$<br>
$$~~~~~~~~~~~~~~~=\sum_{i=1}^{\lfloor\frac{n}{d}\rfloor}\sum_{j=1}^{\lfloor\frac{n}{d}\rfloor}[gcd(i,j)==1]$$<br>
$$~~~~~~~~~~~~~~~=\sum_{i=1}^{\lfloor\frac{n}{d}\rfloor}\sum_{j=1}^{\lfloor\frac{n}{d}\rfloor}\sum_{k|gcd(i,j)}\mu(k)$$<br>
$$~~~~~~~~~~~~~~~=\sum_{k=1}^{\lfloor\frac{n}{d}\rfloor}\mu(k)\lfloor\frac{n}{kd}\rfloor\lfloor\frac{n}{kd}\rfloor$$<br>
$$~~~~~~~~~~~~~~~$$再次是喜闻乐见的下底函数分块....<br>

bzoj 2301:同上...<br>

bzoj 1005:球$$\sum_{i=1}^{n}\sum_{j=1}^{n}gcd(i,j)$$<br>

$$~~~~~~~~~~~~~~~$$令$$f(i)$$表示$$gcd(x,y)==i$$的个数。。暴力容斥就没啦...<br>

bzoj 2705:同上...<br>

bzoj 1441:由裴蜀定理易知答案为所有的gcd..<br>

bzoj 2257:首先，根据裴蜀定理，选择几个数字，可以得到的答案为他们的最大公约数<br>

$$~~~~~~~~~~~~~~~$$于是我们可以求出每个数的所有因数，然后排序，找最大的且个数大等于k的因数<br>

bzoj 2299:注意到题目中相当于有4种操作<br>

$$~~~~~~~~~~~~~~~x或y +-2a$$<br>

$$~~~~~~~~~~~~~~~x或y +-2b$$<br>

$$~~~~~~~~~~~~~~~x+a，y+b$$<br>

$$~~~~~~~~~~~~~~~x+b，y+a$$<br>

$$~~~~~~~~~~~~~~~$$而3，4操作最多用1次，枚举3，4使用或不使用然后裴蜀定理判定即可<br>
bzoj 3667:Miller-Rabin && pollard-rho 板子题.<br>
bzoj 1923:高斯消元解xor方程组..<br>
bzoj 3270:两个男孩在位置(x,y)的期望f(x,y)，列方程后高斯消元<br>
bzoj 1385:$$a_1$$一定会在分子，$$a_2$$一定会在分母。而其他的$$a_3~a_n$$，都有可能通过添括号变为分子,所以只要判断$$\frac{a1*a3*a4~an}{a2}$$是否为整数即可.<br>
SPOJ INS14F:$$n \leq 2k-1$$，显然怎么拿都行，答案为$$C_n^k\times k!$$<br>
$$~~~~~~~~~~~~~~~~~~~~$$否则强制一个相等即可，答案为$$C_{n-1}^{k-1}\times k!$$<br>
SPOJ INS14H:问题等价于求有多少回到原点的长度为T的路径。<br>
$$~~~~~~~~~~~~~~~~~~~~$$把每一维分开考虑，设f[i][j]为用了i个维度，长度为2j的回到原点路径。<br>
$$~~~~~~~~~~~~~~~~~~~~$$f[i][j]对f [i + 1][j + k]的贡献为$$C_{2(j+k)}^{2k}C_{2k}^{k}f[i][j]$$<br>
SPOJ SELTEAM:$$\sum_{i=1}^{k}C_n^i\sum_{j=0}^{k}jC_i^j$$<br>
$$~~~~~~~~~~~~~~~~~~~~~~=\sum_{i=1}^{k}C_n^ii2^{i-1}$$<br>
$$~~~~~~~~~~~~~~~~~~~~~~$$注意模数..所以只要暴力算$$i \leq 23$$即可..<br>
SPOJ IE1:容斥。总方案数等于全部的减去不合法的。怎么构造不合法的呢？就是强制它先拿爆后再和正常情况一样处理<br>
ACdreamOJ 1185:记$$A_i=\sum_{j=1}^{i}a_j,B_i=\sum_{j=1}^{i}b_j$$<br>
$$~~~~~~~~~~~~~~~~~~~~~~~~~~~$$窝萌要求的就是$$(A_{ra} − A_{la−1}) \times (B_{rb} − B_{lb−1})$$被$$C$$整除的方案数。<br>
$$~~~~~~~~~~~~~~~~~~~~~~~~~~~$$设$$f(D)$$为$$gcd(A_{ra} − A_{la−1}, C) = D$$的方案数。<br>
$$~~~~~~~~~~~~~~~~~~~~~~~~~~~$$设$$F(D)$$为$$A_{ra} − A_{la−1}$$被$$D$$整除的方案数。<br>
$$~~~~~~~~~~~~~~~~~~~~~~~~~~~$$设$$G(D)$$为$$B_{rb} −B_{lb−1}$$被$$D$$整除的方案数。<br>
$$~~~~~~~~~~~~~~~~~~~~~~~~~~~$$那么显然我们要求的就是$$\sum_{d|C}f(d)G(\frac{C}{d})$$<br>
$$~~~~~~~~~~~~~~~~~~~~~~~~~~~G,F$$数组显然可以在模意义下算出来，用mobius反演算一算$$f(i)$$就可以啦。<br>
SPOJ SEQN:设H(n)为长度为n的排列的错排方案数,那么我们要求的就是$$C_n^kH(n − k)$$<br>
$$~~~~~~~~~~~~~~~~~~$$考虑计算错排方案数，有$$n! = \sum_{k=0}^{n}︁C_n^kH(k)$$<br>
$$~~~~~~~~~~~~~~~~~~$$由二项式反演得$$H(n) = n!\sum_{k=0}^{n}\frac{(−1)^k}{k!}$$<br>
$$~~~~~~~~~~~~~~~~~~$$m不大..暴力即可..<br>
SPOJ KPGRAPHS:考虑求连通图个数。<br>
$$~~~~~~~~~~~~~~~~~~~~~~~~~~$$连通图个数=$$2^{\frac{n(n−1)}{2}}$ `−不连通图个数。<br>
$$~~~~~~~~~~~~~~~~~~~~~~~~~~$$枚举1号点所在连通块大小m。用$$C_{n−1}^{m−1}$$取出这个连通块，这个连通块以外的所有边都乱选，即$$2^{\frac{(n-m)(n-m−1)}{2}}$$<br>
$$~~~~~~~~~~~~~~~~~~~~~~~~~~$$考虑求欧拉图个数。<br>
$$~~~~~~~~~~~~~~~~~~~~~~~~~~$$欧拉图个数=$$2^{\frac{(n-1)(n−2)}{2}}$$ −不连通欧拉图图个数。<br>
$$~~~~~~~~~~~~~~~~~~~~~~~~~~$$枚举1号点所在连通块大小m。用$$C_{n−1}^{m−1}$$取出这个连通块，这个连通块以外的所有边都乱选，即$$2^{\frac{(n-m-1)(n-m−2)}{2}}$$<br>
$$~~~~~~~~~~~~~~~~~~~~~~~~~~$$考虑求二分图个数。<br>
$$~~~~~~~~~~~~~~~~~~~~~~~~~~$$如果直接确定两个子集内的点的话，不连通的二分图会造成重复计算。<br>
$$~~~~~~~~~~~~~~~~~~~~~~~~~~$$ 为了解决这个问题，先求出连通二分图的数量，再用组合数背包起来。<br>
$$~~~~~~~~~~~~~~~~~~~~~~~~~~$$ 枚举两个子集的大小，用组合数把这两个点集分开，中间的边任意连，这样就胡乱算出了一个解。<br>
$$~~~~~~~~~~~~~~~~~~~~~~~~~~$$强制1号点所在的点集为A集合，剩下的为B集合。<br>
$$~~~~~~~~~~~~~~~~~~~~~~~~~~$$枚举1号点所在连通块的大小，剩下的部分胡乱连，由于没有了1号点确定AB集，所以答案要*2，具体的处理方法和前两次一样。<br>
$$~~~~~~~~~~~~~~~~~~~~~~~~~~$$这样就求出了连通二分图的个数，进而可以求出二分图<br>
的个数。<br>
bzoj 3309:$$\sum_{i=1}^{n}\sum_{j=1}^{n}f(gcd(i,j))$$<br>
$$~~~~~~~~~~~~~~=\sum_{d=1}^{n}f(d)\sum_{i=1}^{\lfloor\frac{n}{d}\rfloor}\sum_{j=1}^{\lfloor\frac{n}{d}\rfloor}[gcd(i,j)==1]$$<br>
$$~~~~~~~~~~~~~~=\sum_{d=1}^{n}f(d)\sum_{i=1}^{\lfloor\frac{n}{d}\rfloor}\sum_{j=1}^{\lfloor\frac{n}{d}\rfloor}\sum_{k|gcd(i,j)}\mu(k)$$<br>
$$~~~~~~~~~~~~~~=\sum_{d=1}^{n}f(d)\sum_{k=1}^{\lfloor\frac{n}{d}\rfloor}\mu(k){\lfloor\frac{n}{dk}\rfloor}{\lfloor\frac{n}{dk}\rfloor}$$<br>
$$~~~~~~~~~~~~~~=\sum_{d=1}^{n}\sum_{k=1}^{\lfloor\frac{n}{d}\rfloor}f(d)\mu(k){\lfloor\frac{n}{dk}\rfloor}{\lfloor\frac{n}{dk}\rfloor}$$<br>
$$~~~~~~~~~~~~~~=\sum_{tot=1}^{n}\sum_{d|tot}^{}f(d)\mu(\frac{tot}{d})\lfloor\frac{n}{tot}\rfloor\frac{n}{tot}\rfloor$$<br>
$$~~~~~~~~~~~~~~记g(n)=\sum_{d|n}f(d)\mu(\frac{n}{d})$$<br>
$$~~~~~~~~~~~~~~$$预处理出g数组之后就是喜闻乐见的下底函数分块..<br>
bzoj 2142:分别求出每个$$p^c$$之后CRT合并<br>
bzoj 3456:写出递推式之后算一算逆元就可以啦<br>
bzoj 3771:生成函数搞一搞...<br>
bzoj 2226:$$\sum_{i=1}^{n}lcm(i,n)$$<br>
$$~~~~~~~~~~~~~~~=\sum_{i=1}^{n}\frac{in}{gcd(i,n)}$$<br>
$$~~~~~~~~~~~~~~~又gcd(n-a,n)=gcd(a,n)$$<br>
$$~~~~~~~~~~~~~~~=\frac{n}{2}\sum_{d|n}d\varphi(d)$$<br>
$$~~~~~~~~~~~~~~~记g(n)=\sum_{d|n}d\varphi(d)$$..积性函数..<br>
bzoj 3239:BSGS裸题..<br>
hdoj 4944:$$\sum_{i=1}^{n}\sum_{j=1}^{n}\sum_{d|(i,j)}\frac{ij}{gcd(\frac{i}{d},\frac{j}{d})}$$<br>
$$~~~~~~~~~~~~~~~=\sum_{i=1}^{n}\sum_{j=1}^{n}\sum_{d|gcd(i,j)}\frac{ijd}{gcd(i,j)}$$<br>
$$~~~~~~~~~~~~~~~=\sum_{i=1}^{n}\sum_{j=1}^{n}\sum_{d|gcd(i,j)}lcm(i,j)d$$<br>
$$~~~~~~~~~~~~~~~=\sum_{d=1}^{n}\sum_{i=1}^{\lfloor\frac{n}{d}\rfloor}\sum_{j=1}^{\lfloor\frac{n}{d}\rfloor}lcm(id,jd)d$$<br>
$$~~~~~~~~~~~~~~~=\sum_{d=1}^{n}d^2\sum_{i=1}^{\lfloor\frac{n}{d}\rfloor}\sum_{j=1}^{\lfloor\frac{n}{d}\rfloor}lcm(i,j)$$<br>
$$~~~~~~~~~~~~~~~记f(n)=\sum_{i=1}^{n}\sum_{j=1}^{n}lcm(i,j)$$<br>
$$~~~~~~~~~~~~~~~原式=\sum_{d=1}^{n}d^2f(\lfloor \frac{n}{d} \rfloor)$$<br>
$$~~~~~~~~~~~~~~~$$直接下底函数分块有点虚..来让我们预处理一下答案..<br>
$$~~~~~~~~~~~~~~~$$考虑每个$$d$$对答案的贡献..按照$$\lfloor\frac{n}{d}\rfloor$$的变化分块...<br>
bzoj 3560:考虑分离质因子进行计算，求出每种质因子对应的$$\varphi$$值即可。<br>
$$~~~~~~~~~~~~~~~$$先不考虑$$\varphi$中$\frac{p-1}{p}$$那一项，把所有可能的组合情况算出来<br>
$$~~~~~~~~~~~~~~~sum=\prod_{i=1}^{n}\sum_{j=0}^{q_i}p_i^j$$<br>
$$~~~~~~~~~~~~~~~$$对答案的贡献为$$\frac{p-1}{p}(sum-1)+1$$<br>
bzoj 3512:设$$S(n,m)=\sum_{i=1}^m\varphi(ni)$$<br>
$$~~~~~~~~~~~~~~~若|\mu(n)|\not=1,找到最大的k，使得k|n且|\mu(k)|=1,易得S(n,m)=\frac{n}{k}S(k,m)$$<br>
$$~~~~~~~~~~~~~~~记gcd(n,k)=m \\ ~~~~~~~~~~~~~~~\varphi(nk)=\varphi(k)\varphi(\frac{n}{m})m=\varphi(k)\sum_{d|m}\varphi(\frac{n}{d}) \\ ~~~~~~~~~~~~~~~ S(n,m)=\sum_{i=0}^{m}\varphi(ni)=\sum_{i=0}^{m}\varphi(i)\sum_{d|gcd(i,n)}\varphi(\frac{n}{d})=\sum_{d|n}\varphi(\frac{n}{d})\sum_{k=1}^{\lfloor \frac{m}{d} \rfloor}\varphi(nk)=\sum_{d|n}\varphi(\frac{n}{d})S(n,\lfloor \frac{m}{d} \rfloor)$$<br>
$$~~~~~~~~~~~~~~~我们接下来只要计算S(1,m)就可以啦$$<br>
$$~~~~~~~~~~~~~~~S(1,m)=\sum_{i=1}^{m}\varphi(i) \\ ~~~~~~~~~~~~~~~=\sum_{i=1}^{m}\lfloor\frac{m}{i}\rfloor\varphi(i)-\sum_{i=1}^{m}(\lfloor\frac{m}{i}\rfloor-1)\varphi(i)\\ ~~~~~~~~~~~~~~~ =\sum_{i=1}^{m}\sum_{d|i}\mu(d)-\sum_{i=2}^{m}\sum_{j=1}^{\lfloor\frac{m}{i}\rfloor}\varphi(i) \\ ~~~~~~~~~~~~~~~=\frac{(m+1)m}{2}-\sum_{i=2}^{m}S(1,\lfloor\frac{m}{i}\rfloor) \\ ~~~~~~~~~~~~~~~~记搜就好啦..当一个函数与1函数的Dirichlet卷积有规律可寻时可以考虑使用该算法$$
