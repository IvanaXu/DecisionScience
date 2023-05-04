
- [0.3.1 关键书籍](#031-关键书籍)
- [0.3.2 学习示例](#032-学习示例)
  - [0.3.2.1 牛顿法原理](#0321-牛顿法原理)
  - [0.3.2.2 计算过程](#0322-计算过程)

### 0.3.1 关键书籍
- 《2012.李航.统计学习方法》
> - 第1章 统计学习方法概论
> - 第2章 感知机
> - 第3章 k近邻法
> - 第4章 朴素贝叶斯法
> - 第5章 决策树
> - 第6章 逻辑斯谛回归与最大熵模型
> - 第7章 支持向量机
> - 第8章 提升法
> - 第9章 EM算法及其推广
> - 第10章 隐马尔可夫模型
> - 第11章 条件随机场
> - 第12章 统计学习方法总结
> - 附录A 梯度下降法
> - 附录B 牛顿法与拟牛顿法
> - 附录C 拉格朗日对偶性
> 
...


### 0.3.2 学习示例
以下风险定价中求解无约束最优化问题时，为加快收敛速度所用牛顿法为例：
#### 0.3.2.1 牛顿法原理

<p align="center">
<img src="https://github.com/IvanaXu/DecisionScience/releases/download/base/0.3.2.0-000.png" height=400>
</p>

#### 0.3.2.2 计算过程
构建示例：
$f(x) = x^2-2, f(x)=0 ?$

据上述牛顿法，下一个 $x$取值 $x_1$为：

$$
x_1 = x_0 - \frac{f(x_0)}{f^`(x_0)} = x_0 - \frac{x_0^2-2}{2x_0} = (x_0 + 2/x_0)/2
$$

- 代码明细（SAS）

```python
%macro fcal(x=,t=,);
    data &t.;
    &t. = (&x.) ** 2 - 2;
    run;
%mend;

%macro fcal_x(b=,fl=0.000001);
    %let bf = 1;

    %do i = 0 %to 100;
        %fcal(x=&b., t=f);
        %fcal(x=&b.-&fl., t=f_0);
        %fcal(x=&b.+&fl., t=f_1);

        data f_all;
        merge f f_0 f_1;
        x0 = &b.;
        f_ = (f_1 - f_0)/(&fl.*2); # 导函数模拟
/*         f_ = 2 * x0; */
        x1 = x0 - f/f_;
        run;

        data _null_;
        set f_all;
        call symput("bb", x1);
        run;
        
        %fcal(x=&bb., t=f1);

        data _null_;
        set f1;
        f1_ = f1 < 0.01;
/*         f1_ = abs(&bb. - &b.) < 0.01; */
        call symput("brf", f1);
        call symput("bf", f1_);
        run;
        %put --------------------------- &bb. &brf. &bf.;
        
        %if &bf. = 1
            %then %let i = 100;
        %else %let b = &bb.;
    %end;
%mend;

%fcal_x(b=4);
/*
  --------------------------- 2.2500000001 3.0625000005            0
  --------------------------- 1.5694444446 0.4631558647            0
  --------------------------- 1.4218903638 0.0217722067            0
  --------------------------- 1.4142342859 0.0000586154            1
*/

```

- 代码明细（Python）

```python
def sqrt_by_bisection(n):
    low, last = 0, 0
    up = n
    mid = (low + up)/2
    while abs(mid - last) > eps:
        if mid * mid > n:
            up = mid
        else:
            low = mid
        last = mid
        mid = (up + low)/2
    return mid
print(sqrt_by_bisection(2))

%timeit sqrt_by_bisection(2)
# 1.4142135605216026
# 6.26 µs ± 6.98 ns per loop (mean ± std. dev. of 7 runs, 100000 loops each)

```

如上，快速计算 $\sqrt{2} \approx 1.414$.

