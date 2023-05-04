
- [5.1.1 模型简介](#511-模型简介)
- [5.1.2 开发流程](#512-开发流程)
  - [5.1.2.1 基本流程](#5121-基本流程)
  - [5.1.2.2 开发目标](#5122-开发目标)
  - [5.1.2.3 参数定义](#5123-参数定义)
  - [5.1.2.4 数据处理](#5124-数据处理)
  - [5.1.2.5 特征探索](#5125-特征探索)
  - [5.1.2.6 模型拟合](#5126-模型拟合)
  - [5.1.2.7 评分校准](#5127-评分校准)
  - [5.1.2.8 模型评估](#5128-模型评估)
- [5.1.3 评分卡模型](#513-评分卡模型)
- [5.1.4 模型应用](#514-模型应用)

快速介绍完整模型开发流程，作为入门。
### 5.1.1 模型简介

<p align="center">
<img src="https://github.com/IvanaXu/DecisionScience/releases/download/base/5.1.1.0-000.png" width=800>
</p>

<p align="center">
<img src="https://github.com/IvanaXu/DecisionScience/releases/download/base/5.1.1.0-001.png" width=800>
</p>

- 什么是模型？
> 对研究对象以统计的方法来量化的一种手段
> 
> 将以下来源的信息转换为统计模型：
> - 研究对象的人口统计信息；
> - 帐户的行为；
> - 外部数据如征信局数据等；
> 
……
>  
> 将模型结果视为目标研究的衡量标准
> 目的是用过去的行为来预测将来的表现
> 传统的建模方法主要有：信用评分卡、决策树分群


<p align="center">
<img src="https://github.com/IvanaXu/DecisionScience/releases/download/base/5.1.1.0-002.png" width=800>
</p>

<p align="center">
<img src="https://github.com/IvanaXu/DecisionScience/releases/download/base/5.1.1.0-003.png" width=800>
</p>

<p align="center">
<img src="https://github.com/IvanaXu/DecisionScience/releases/download/base/5.1.1.0-004.png" width=800>
</p>

- 模型优缺点
> 模型优点：
> - 数据驱动，决策结果客观
> - 准确/可靠
> - 稳健
> - 高效
> 
 
> 模型缺点：
> - 非万能的- 围绕模型实施策略
> - 模型是针对特定主题的，为特定的业务目标和策略服务
> - 不能排除所有的“坏”帐户
> - 需要资源进行开发和维护
> - 随时间的推移精确度变差

<p align="center">
<img src="https://github.com/IvanaXu/DecisionScience/releases/download/base/5.1.1.0-005.png" width=800>
</p>

<p align="center">
<img src="https://github.com/IvanaXu/DecisionScience/releases/download/base/5.1.1.0-006.png" width=800>
</p>

<p align="center">
<img src="https://github.com/IvanaXu/DecisionScience/releases/download/base/5.1.1.0-007.png" width=800>
</p>

### 5.1.2 开发流程
#### 5.1.2.1 基本流程

<p align="center">
<img src="https://github.com/IvanaXu/DecisionScience/releases/download/base/5.1.2.1-000.png" width=800>
</p>

<p align="center">
<img src="https://github.com/IvanaXu/DecisionScience/releases/download/base/5.1.2.1-001.png" width=800>
</p>

#### 5.1.2.2 开发目标
如，上一版模型整体效率下降，统计模型效率的指标KS下降明显。
上一版模型稳定性下降，PSI已上升至0.16。
因此，需要重新开发申请评分。

#### 5.1.2.3 参数定义
目标变量以及项目参数定义。

- 好坏定义

<p align="center">
<img src="https://github.com/IvanaXu/DecisionScience/releases/download/base/5.1.2.3-000.png" width=800>
</p>

- 转期比分析

<p align="center">
<img src="https://github.com/IvanaXu/DecisionScience/releases/download/base/5.1.2.3-001.png" width=800>
</p>

- 坏账表现分析

<p align="center">
<img src="https://github.com/IvanaXu/DecisionScience/releases/download/base/5.1.2.3-002.png" width=800>
</p>

<p align="center">
<img src="https://github.com/IvanaXu/DecisionScience/releases/download/base/5.1.2.3-003.png" width=800>
</p>

- 时间窗口

<p align="center">
<img src="https://github.com/IvanaXu/DecisionScience/releases/download/base/5.1.2.3-004.png" width=800>
</p>

#### 5.1.2.4 数据处理
数据收集和数据预处理。

<p align="center">
<img src="https://github.com/IvanaXu/DecisionScience/releases/download/base/5.1.2.3-005.png" width=800>
</p>

#### 5.1.2.5 特征探索
候选特征变量探索分析。

<p align="center">
<img src="https://github.com/IvanaXu/DecisionScience/releases/download/base/5.1.2.3-006.png" width=800>
</p>

#### 5.1.2.6 模型拟合
评分模型变量筛选以及模型拟合。

Fine Variables - (270>47) - Coarse Variables - (47>8) - Model Variables

- 对数据进行抽样，一般分为训练样本和测试样本；
- 抽样后对数据进行细分和粗分，将样本进行WOE转换并用逻辑回归训练；

#### 5.1.2.7 评分校准

- 校准公式

$Score = factor \times \ln(Odds) + offset$

- 校准参数
> Base Point = 660
Odds = 15
Point Double Odds = 15


#### 5.1.2.8 模型评估
评分模型效果评估（验证）

- KS

<p align="center">
<img src="https://github.com/IvanaXu/DecisionScience/releases/download/base/5.1.2.8-000.png" width=800>
</p>

- Score Distribution & Bad Rate

<p align="center">
<img src="https://github.com/IvanaXu/DecisionScience/releases/download/base/5.1.2.8-001.png" width=800>
</p>

- Divergence

<p align="center">
<img src="https://github.com/IvanaXu/DecisionScience/releases/download/base/5.1.2.8-002.png" width=800>
</p>

- PSI 趋势

<p align="center">
<img src="https://github.com/IvanaXu/DecisionScience/releases/download/base/5.1.2.8-003.png" width=800>
</p>

### 5.1.3 评分卡模型
> 《信用评分模型技术与应用》
> 《信用风险评分卡研究-基于SAS的开发与实施》


### 5.1.4 模型应用

<p align="center">
 <img src="https://github.com/IvanaXu/DecisionScience/releases/download/base/5.1.4.0-000.png" width=800>
 <img src="https://github.com/IvanaXu/DecisionScience/releases/download/base/5.1.4.0-001.png" width=800>
 <img src="https://github.com/IvanaXu/DecisionScience/releases/download/base/5.1.4.0-002.png" width=800>
</p>
