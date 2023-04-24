### 0.1.1 抛硬币：伯努利随机变量
最简单的概率模型就是抛硬币，假设掷到正面的概率是 $p$，则掷反面的概率就是 $1-p$，从概率的角度来说，抛硬币就是“伯努利随机变量”，表示为 $Bernoulli(p)$。
假设正面概率为0.7，反面概率为0.3，这样的分配称为“概率质量函数”。
在很多情况下，有些支出与随机的不同收入相关。例如每次抛出正面给5元，反面赔2元，那平均支出 $C$就是 $E[C] = 0.7 \times 5 + 0.3 \times (-2) = ¥ 2.9$
对这个结果的正确解释是，如果抛硬币 $N$次，其中 $N$是一个非常大的数字，那将赚取 $2.9N$元。
> 在随机事件的大量重复出现中，往往呈现几乎必然的规律，这个规律就是大数定律。
> 通俗地说，这个定理就是，在试验不变的条件下，重复试验多次，随机事件的频率近似于它的概率。偶然中包含着某种必然。


以下模拟之：
```python
# https://github.com/sijichun/MathStatsCode/blob/master/notebook_python/LLN_CLT.ipynb
import numpy as np
from numpy import random as nprd

True_P=0.5

def sampling(N):
    ## 产生Bernouli样本
    x=nprd.rand(N)<True_P
    return x

M=10000 #模拟次数
xbar=np.zeros(M)
N=np.array([i+1 for i in range(M)])
x=sampling(M)
for i in range(M):
    if i==0:
        xbar[i]=x[i]
    else:
        xbar[i]=(x[i]+xbar[i-1]*i)/(i+1)

## 导入matplotlib
import matplotlib.pyplot as plt 
## 使图形直接插入到jupyter中
%matplotlib inline
# 设定图像大小
plt.rcParams['figure.figsize'] = (10.0, 8.0)

plt.plot(N,xbar,label=r'$\bar{x}$',color='pink') ## xbar
xtrue=np.ones(M)*True_P
plt.plot(N,xtrue,label=r'$0.5$',color='black') ## true xbar
plt.xlabel('N')
plt.ylabel(r'$\bar{x}$')
plt.legend(loc='upper right', frameon=True)
plt.show() ## 画图

```
<p align="center">
<img src="https://github.com/IvanaXu/DecisionScience/releases/download/base/0.1.1.0-000.png" height=400>
</p>

简单来说，**大数定律讲的是，样本容量极大时，样本的均值必然趋近于总体的期望。**

### 0.1.2 掷飞镖：均匀随机变量
伯努利随机变量是离散型随机变量的最简单类型。与此相反的随机变量被称为“连续型”随机变量，可在数值范围内取任意值。
最简单的连续型随机变量是均匀随机变量，有时称为 $Uniform(a,b)$,  $Uniform(a,b)$始终在数字 $a$和 $b$之间，也同样可能为取值范围内的任意位置。
对于离散随机变量，概率质量函数为每个可能的结果分配一个有限的概率。对于连续随机变量，其输出具体值的概率几乎为0，但其输出在特定区间的概率则大得多。

### 0.1.3 均匀分布和伪随机数
$Uniform(0,1)$分布是最基本的概率分布。它是最简单的一个，但也是在数学理论和计算实践中构建更复杂概率分布的基础。例如：

- 如果要模拟 $Bernoulli(p)$随机变量 $B$，可以通过模拟 $Uniform(0,1)$分布中的随机值 $u$来实现。如果 $u < p$，则设置 $B$=正面，否则设置 $B$=反面。
- 如果要模拟加权掷骰子，将[0.0, 1.0]取值范围分成6个区域，其中第 $i$个区域的大小与骰子投出第 $i$面的概率相同。然后再次从 $Uniform(0,1)$分布中绘制 $u$值。掷出的骰子即会落入 $u$的 [0.0, 1.0]区间。
- 如果要模拟指数随机变量，从 $Uniform(0,1)$中绘制 $u$，然后取 $\log(u)$的倒数。

但是，技术上讲，用计算机程序模拟随机数是不可行的。它们是确定性的机器，只能遵循预定的规则——没有用于翻转硬币的子程序。一般会用“伪随机数”即某种固定生成算法来实现，比如早期的线性同余法。
当然伪随机数也有优点，可以在一开始就手动设置好部分参数，也被称为“种子/seed”。这样做可以使程序变得更加完全确定，并且可以在下次运行中精确地重现相同的结果。

如：
```python
>>> import random
>>> random.random()
0.7006269308810754
>>> random.random()
0.4896124288257575
>>> random.seed(10086)
>>> random.random()
0.043562757723543566
>>> random.random()
0.7994528936212764
>>> random.seed(10086)
>>> random.random()
0.043562757723543566
```
同一种子10086时，输出随机数相同。

### 0.1.4 非离散型、非连续型随机变量
从数学角度而言，随机变量既不离散也不连续。例如以种植树木的高度为例，在给定的时间点中，其中不发芽的一部分高度为0，这是在该高度下的有限概率质量。而那些发芽树木的高度则为在一定范围内的任意值。

```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt

z = np.zeros(1000)
x = np.random.exponential(size=1000)
D = pd.Series(data)
X = pd.Series(x)

(D>0).value_counts().rename({True: ">0", False: "<0"}).plot(kind="pie")
X.hist(bins=100)

```
<p align="center">
<img src="https://github.com/IvanaXu/DecisionScience/releases/download/base/0.1.4.0-000.png" height=300>
</p>
<p align="center">
<img src="https://github.com/IvanaXu/DecisionScience/releases/download/base/0.1.4.0-001.png" height=300>
</p>

### 0.1.5 期望和标准偏差
通常使用大写字母$X$表示随机变量，小写字母 $x$表示变量的特定值。
如果写作 $E[X]$是指随机变量 $X$的平均值。这里的 $E$是指期望值，是均值的另一种形式。

- 不连续随机变量的预期值定义为：

$E[X]=\sum^{}_{i}ip_i$

- 连续随机变量的预期值定义为：

$E[X] = \int xf_x(x) dx$
通常由 $\mu_x$表示随机变量 $X$的期望值，根据期望值定义某个事物的关键例子是方差和标准偏差。
$X$的方差定义为：
$var[X] = E[|X-\sigma x|^2]$
标准差是方差的二次方根：
$\sigma_x = \sqrt{var[X]}$
标准差可以用来粗略衡量X与 $\mu_x$的距离。

### 0.1.6 独立概率、边际概率和条件概率
通常需要同时考虑两个随机变量 $X$和 $Y$。如果已知一个变量，能以此了解另一个变量吗？
具体来说，假设变量是离散随机变量，令 $p_{xy}$表示 $X=x$和 $Y=y$的概率。
$X$的**边际**概率为
$Pr[X=x]=p_x=\sum_yP_{xy}$
如果重视一个随机变量而忽略另一个，那么就是概率分布。
另一方面，由已知的 $X$值推断 $Y$的情况，那么就可以在给定的 $X=x$时，得到每个 $y$的条件概率：
$Pr[Y=y|X=x] = P_{y|X=x}=\frac{p_{xy}}{\sum_gp_{xy}}$
条件概率在贝叶斯统计中起着重要的作用。
给定 $X=x$，通常想知道 $Y$的期望值，表达如下：
$E[Y|X=x]$
这些关于 $Y$的统计值与 $X$的取值条件相关， $X$和 $Y$的相关性定义为：
$Corr[X,Y] = \frac{E[(X-\mu_x)(Y-\mu_y)]}{\sigma_x\sigma_y}$
这是随机变量之间的线性关系的度量，其形式为 $Y=mX+b$。
如果已知随机变量 $X$和 $Y$中的任一个，并不能获取另一个随机变量的相关信息，则 $X$和 $Y$是独立随机变量。在数学上意味着：
$p_{xy} = p_xp_y$
> 值得注意的是，独立性是一个非常强大的标准，这比仅仅说相关性为0要强得多。


### 0.1.7 重尾分布
关于概率分布最重要的理解之一就是“重尾”。直观地说，这是指最大值出现的概率。身高就是非重尾的一个很好的例子，因为没有人身高能超过10米。然而净资产是重尾分布，因为偶尔会出现比尔·盖茨。
了解重尾分布很重要，这是因为当事情为重尾分布时，通常用概率分布做的事情都不起作用。
举例，重尾分布的平均值很难估计，如果房间里有100个人，那么这些人的平均净资产可能会有很大的差异，像一个千万富翁就可能极大提高平均净资产。
以下通过帕累托分布中抽取重尾序列，模拟$N$次，可见平均值趋涨：

```python
import numpy as np
import matplotlib.pyplot as plt
np.random.seed(10)
N = 1000
sums, means = 0, []
for i in range(1, N):
    sums += np.random.pareto(1)
    means.append(sums/i)

plt.plot(means)

```

<p align="center">
<img src="https://github.com/IvanaXu/DecisionScience/releases/download/base/0.1.7.0-000.png" height=400>
</p>

### 0.1.8 二项分布
二项式($n$，$p$) 的分布是抛硬币$n$次得到正面的次数，其中每次抛掷正面的独立概率为$p$。
即$n$次抛掷硬币得到$k$次正面的特定序列概率为$p^k(1-p)^n-k$。
如何在$n$次投掷硬币中抛出$k$次正面是一个组合问题，确切公式表示为$\lgroup^n_k\rgroup$，即从$n$个元素中选择$k$个元素
$\lgroup^n_k\rgroup = \frac{n!}{k!(n-k)!}$
以下模拟之：

```python
import numpy as np
import pandas as pd

sample = np.random.binomial(200, 0.3)
print(sample)

N = 100
sample = []
for _ in range(N):
    sample.append(np.random.binomial(1, 0.3))
pd.value_counts(sample)
"""
55
0    73
1    27
dtype: int64
"""

```

### 0.1.9 泊松分布
泊松分布用于模拟可能发生许多事件的系统，并且所有事件都相互独立，但平均而言，只有少数时间会发生。一个很好的例子就是会有多少人在某一天访问一个网站，世界上有数十亿人可以访问这个网址，但平均而言，也许只有几百人会访问这个网站。
日常生活中，大量事件是有固定频率的，如：
> - 某医院平均每小时出生3个婴儿
> - 某公司平均每10分钟接到1个电话
> - 某超市平均每天销售4包xx牌奶粉
> - 某网站平均每分钟有2次访问

它们的特点就是，我们可以预估这些事件的总数，但是没法知道具体的发生时间。已知平均每小时出生3个婴儿，请问下一个小时，会出生几个？有可能一下子出生6个，也有可能一个都不出生。这是我们没法知道的。
泊松分布就是描述某段时间内，事件具体的发生概率。

假设采用二项式($n$，$p$)分布。将$n$设置得非常大，将$p$设置得足够小，则
$np=\lambda$
式中，$\lambda$是固定常数。
在使得$n$大而$p$小同时$\lambda$不变的约束下，二项分布将收敛于泊松分布。概率质量函数由下式给出：
$p_k = e^{-\lambda}\frac{\lambda^k}{k!}$
加入时间维度$t$，
$P(N(t)=n)=e^{-\lambda t}\frac{(\lambda t)^k}{k!}$
已知1小时内出生3个婴儿的概率，就表示为$P(N(1)=3)$，那么接下来两个小时，一个婴儿都不出生的概率是0.25%，基本不可能发生。因为：
$P(N(2)=0)=\frac{(3\times2)^0e^{-3\times2}}{0!} \approx 0.0025$
接下来一个小时，至少出生两个婴儿的概率是80%：
$\begin{align}
P(N(1)\ge2) &= 1-P(N(1)=1)-P(N(1)=0) \\
&= 1 - \frac{(3\times1)^1e^{-3\times1}}{1!} - \frac{(3\times1)^0e^{-3\times1}}{0!}\\
&= 1 - 3e^{-3} - e^{-3} \\
&= 1 - 4e^{-3} \\
&\approx 0.8009
\end{align}$
以下模拟之：

```python
import numpy as np
import pandas as pd

sample = np.random.poisson(lam=5, size=5)
print(sample)

N = 100
sample = []
for _ in range(N):
    sample.append(np.random.poisson(5))
pd.value_counts(sample)
"""
[9 4 5 5 4]
4     22
3     15
6     14
5     14
7      9
2      9
10     6
8      6
9      2
1      2
0      1
dtype: int64
"""

```
### 0.1.A 正态分布
正态分布是非常重要的一种概率分布，也称为高斯分布。它是典型的钟形曲线，其概率密度函数：
$f(x) = \frac{1}{\sqrt{2\pi\sigma^2}}e^{-(x-\mu)/2\sigma^2}$
式中，$\mu$是其平均值，$\sigma$是标准偏差。
这种正态分布通常称为$N(\mu,\sigma^2)$。
正态分布最重要的性质是其概率密度紧密聚集在均值附近，尾巴较小，并且不大可能会出现大量异常值。出于这个原因，简单的用正态分布来拟合数据可能会产生严重问题。通常在进行曲线拟合之前，识别并移除主要异常值是常用方法。

<p align="center">
<img src="https://github.com/IvanaXu/DecisionScience/releases/download/base/0.1.A.0-000.jpeg" height=300>
</p>

从理论上，正态分布被作为最有名的概率分布，是因为如果有足够多的时间采样并对结果进行平均，许多分布将收敛于正态分布。这适用于二项分布、泊松分布以及任何可能遇到的其他分布。从技术上讲，任何一个分布的平均值和标准偏差都是有限的。
这被归结于“中心极限定理”中：
> **中心极限定理**，设$X$是具有有限的均值$\mu$和标准差$\sigma$的随机变量。令$X_1$，$X_2$，...，$X_n$是$X$的独立样本序列。然后当$n$趋向于无穷大时：
> $\sqrt{n}\lgroup\frac{1}{n}\sum X_i - \mu \rgroup\to N(0,\sigma^2)$


它是概率论中最重要的一类定理，有广泛的实际应用背景。在自然界与生产中，一些现象受到许多相互独立的随机因素的影响，如果每个因素所产生的影响都很微小时，总的影响可以看作是服从正态分布的。中心极限定理就是从数学上证明了这一现象。最早的中心极限定理是讨论重点，伯努利试验中，事件A出现的次数渐近于正态分布的问题。
以下模拟之：

```python
# https://github.com/sijichun/MathStatsCode/blob/master/notebook_python/LLN_CLT.ipynb
from numpy import random as nprd

def sampling(N):
    ## 产生一组样本，以0.5的概率为z+3，0.5的概率为z-3，其中z~N(0,1)
    d = nprd.rand(N)<0.5
    z = nprd.randn(N)
    x = np.array([z[i]+3 if d[i] else z[i]-3 for i in range(N)])
    return x

N = [2,3,4,10,100,1000] # sample size
M = 2000
MEANS = []
for n in N:
    mean_x = np.zeros(M)
    for i in range(M):
        x = sampling(n)
        mean_x[i] = np.mean(x)/np.sqrt(10/n) ## 标准化，因为var(x)=10
    MEANS.append(mean_x)

## 导入matplotlib
import matplotlib.pyplot as plt
import matplotlib.mlab as mlab
## 使图形直接插入到jupyter中
%matplotlib inline
# 设定图像大小
plt.rcParams['figure.figsize'] = (10.0, 8.0)

x = sampling(1000)
plt.xlabel('x')
plt.ylabel('Density')
plt.title('Histogram of Mixed Normal')
plt.hist(x, bins=30, normed=1) ## histgram
plt.show() ## 画图

## 均值
ax1 = plt.subplot(2,3,1)
ax2 = plt.subplot(2,3,2)
ax3 = plt.subplot(2,3,3)
ax4 = plt.subplot(2,3,4)
ax5 = plt.subplot(2,3,5)
ax6 = plt.subplot(2,3,6)

## normal density
x = np.linspace(-3,3,100)
d = [1.0/np.sqrt(2*np.pi)*np.exp(-i**2/2) for i in x]

def plot_density(ax,data,N):
    ax.hist(data, bins=30, normed=1) ## histgram
    ax.plot(x, d)
    ax.set_title(r'Histogram of $\bar{x}$:N=%d' % N)

plot_density(ax1,MEANS[0],N[0])
plot_density(ax2,MEANS[1],N[1])
plot_density(ax3,MEANS[2],N[2])
plot_density(ax4,MEANS[3],N[3])
plot_density(ax5,MEANS[4],N[4])
plot_density(ax6,MEANS[5],N[5])

plt.show() ## 画图

```

<p align="center">
<img src="https://github.com/IvanaXu/DecisionScience/releases/download/base/0.1.A.0-001.png" height=400>
</p>

简单来说，**中心极限定理讲的是，样本容量极大时，样本均值的抽样分布趋近于正态分布。这和样本所属的总体的分布的类型无关，样本所属总体的分布可以是正态分布，也可以不是。

**
### 0.1.B 多元正态分布
如果我们以显著的方式推广上述分布变量到更高维度，如正态分布，正态分布可以定义任意维度$d$。密度函数类似于一个山丘，它在分布的平均值处达到峰值，并且总体呈现为椭圆形。

<p align="center">
<img src="https://github.com/IvanaXu/DecisionScience/releases/download/base/0.1.B.0-000.jpeg" height=300>
</p>

应该注意到，椭圆体可以向任意方向伸展，不必沿着某一个轴伸展。

### 0.1.C 指数分布
指数分布在其模拟某些事件发生的时间或事件之间的时间长度时最有用。比方说，对于进入商店的人，每个时刻人们走进商店的概率是一个较小的固定值，并且每个时刻都是相互独立的。在这种情况下，事件之间的时间量将呈指数分布。
指数分布是事件的时间间隔的概率。
> - 婴儿出生的时间间隔
> - 来电的时间间隔
> - 奶粉销售的时间间隔
> - 网站访问的时间间隔

指数分布由其平均值$\theta$（事件之间的平均时间）进行参数化。有时会使用$\lambda=1/\theta$（事件发生的平均速率）对其进行参数化，其概率密度函数：
$f(x)=\left\{
\begin{array}{rcl}
\frac{1}{\theta}e^{-x/\theta} & & {x\ge0}\\
0 & & {other}\\
\end{array} \right.$
指数分布的公式可以从泊松分布推断出来，引用上例，
如果下一个婴儿要间隔时间 t ，就等同于 t 之内没有任何婴儿出生。
$\begin{align}
P(X > t) &= P(N(t)=0) = \frac{(\lambda t)^0e^{-\lambda t}}{0!}\\
&= e^{-\lambda t}
\end{align}$
反过来，事件在时间 t 之内发生的概率，就是1减去上面的值。
$P(X \le t) = 1 - P(X \gt t) = 1 - e^{-\lambda t}$
接下来15分钟，会有婴儿出生的概率是52.76%
$\begin{align}
P(X \le 0.25 ) &= 1 - e^{-3\times0.25}\\
&\approx 0.5276
\end{align}$
接下来的15分钟到30分钟，会有婴儿出生的概率是24.92%
$\begin{align}
P(0.25 \le X \le 0.5) &= P(X \le 0.5) - P(X \le 0.25)\\
&= (1-e^{-3\times0.5})-(1-e^{-3\times0.25}) \\
&= e^{-0.75} - e^{-1.5} \\
&\approx 0.2492
\end{align}$
以下模拟之：

```python
import numpy as np
import pandas as pd

sample = np.random.exponential(10)
print(sample)

N = 10
sample = []
for _ in range(N):
    sample.append(np.random.exponential(1))
pd.value_counts(sample)
"""
34.293011008718764
1.426454    1
0.205591    1
0.119978    1
0.404349    1
0.220207    1
0.463974    1
0.495403    1
0.301081    1
2.966221    1
0.009234    1
dtype: int64
"""

```

在很多应用中，指数分布的关键属性是“无记忆”。无论等待事件发生的时间有多久，剩余的等待时间仍然遵循相同的指数分布。一个事件在下一个时刻是否发生，与之前已发生的其他时间无关。
指数分布的无记忆特性通常被认为是重尾分布与非重尾分布的分界线。如果已经等待了一个事件发生的时间为$x$，那么期望等待的时间比刚开始的时间长还是短呢？指数随机变量不会有任何结果。相比之下，20岁的人可能倾向于再等20多年，但90岁的人可能不会。因此年龄并不是重尾的。街上随机的一个人不太可能是百万富翁，但是如果碰巧挑选到的人都至少有80万元，那么百万富翁的可能性就大很多。因此，净资产是重尾的。

### 0.1.D 对数正态分布
重尾分布是对数正态分布，对其理解和模拟很简单。同时，对数正态分布的平均值和标准差都是有限的。对于所有现实世界的现象都是如此。
该分布中有一个明显的峰值，且峰值大于0，峰值的左侧迅速下降，在$x=0$时变为0.0，在右边逐渐变窄，会使其有规律地出现大的异常值。对数正态分布最好这样考虑，从正态($\mu$，$\sigma^2$)中抽取一个$x$值，则$e^x$是对数正态分布的。
以下模拟之：

```python
import numpy as np
import pandas as pd

sample = np.random.lognormal(1, 2)
print(sample)

N = 10
sample = []
for _ in range(N):
    sample.append(np.random.lognormal(1, 2))
pd.value_counts(sample)
"""
18.761595456554964
0.232312     1
9.743775     1
18.472365    1
21.047856    1
61.887021    1
13.400393    1
37.689687    1
5.035124     1
0.026245     1
0.156932     1
dtype: int64
"""

```

> 这就是说，财富的对数值满足正态分布。如果平均财富是10,000元，那么1000元～10,000元之间的穷人（比平均值低一个数量级，宽度为9000）与10,000元~100,000元之间的富人（比平均值高一个数量级，宽度为90,000）人数一样多。因此，财富曲线左侧的范围比较窄，右侧出现重尾。


### 0.1.E 熵
熵是一种衡量随机变量“随机性”的方法，概念来自信息论领域。直观地说，公平硬币的随机性要高于99%出现正面的硬币。同样，如果一个正态分布的标准差很小，那么它的概率质量将紧紧地围绕它的平均值分布，且它比更大标准差的分布随机性更差。
如果随机变量$X$是离散的，熵即
$H[X] = E[Surpise[X]] = -\sum_xp_x\ln(p_x)$
如果随机变量$X$是连续的，熵即
$H[X] = -\int f(x) \ln(f(x)) dx$
我们很少直接计算熵，但概念无处不在。
