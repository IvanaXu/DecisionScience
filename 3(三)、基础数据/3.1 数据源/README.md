
- [3.1.1 业务系统](#311-业务系统)
- [3.1.2 发卡核心数据](#312-发卡核心数据)
  - [3.1.2.1 业务流程](#3121-业务流程)
  - [3.1.2.2 数据框架](#3122-数据框架)
- [3.1.3 人行征信数据](#313-人行征信数据)
  - [3.1.3.1 数据概况](#3131-数据概况)
  - [3.1.3.2 人行示例](#3132-人行示例)
  - [3.1.3.3 人行评分](#3133-人行评分)
  - [3.1.3.4 数据应用](#3134-数据应用)
- [3.1.4 外部数据](#314-外部数据)
- [3.1.5 总行数据](#315-总行数据)

### 3.1.1 业务系统
如下示例（其中英文简称不绝对规范，仅作排序参考）：

> APP，手机APP系统
>
> APS，贷前审批系统
>
> BOBCMS，卡务管理系统
>
> CCS，银联发卡核心系统
>
> COR，总行核心
>
> CSR，客服系统
>
> CUP，中国银联
>
> CUPDSIGN，银数统一签约管理平台
>
> DH，贷后管理系统
>
> DHRH，贷后人行
>
> DX，电销系统
>
> ETC，总行ETC
>
> ICIP，催收系统
>
> INS，反欺诈系统
>
> PAD，PAD移动营销
>
> PRSM，个人CRM
>
> RTDM，决策引擎管理系统
>
> WSPT，网申平台
>
> YL，银联数据
>
> YL_DXPT，银联短信平台
>
> YL_MGM，银联MGM推荐获客系统
>
> YL_POSP，银联POSP
>
> YL_UPT，银联银行卡联网
>
> ZJYW，中间业务平台
>
> ZXPT，第三方征信平台
>


### 3.1.2 发卡核心数据
#### 3.1.2.1 业务流程

<p align="center">
<img src="https://github.com/IvanaXu/DecisionScience/releases/download/base/3.1.2.1-000.png" width=1000>
</p>

上述整个信用卡业务流程在银联发卡核心系统上面流转。

PS：当然这里的发卡核心系统，还可以是通联、江融信、行内自建等等，**本文仅指向银联**。

- CUSTR
> 我们的客户从哪来？

分支行、直营、电营&亲访、网络和微信申请；
> 客户表都记录了什么？

以身份证(CUSTR_NBR)为主键，客户的个人信息，如手机、地址、单位等客户物理标签；

- ACHG

所有客户表(CUSTR)变更过的信息都会记录进来。
例如，手机号的变化、地址的变化、单位的变化。

- APMA/APS

从各个渠道挖掘出来的客户填写申请表；
APMA则记录了申请表中的信息，主键：APP_SEQ、APP_JDAY组成申请书编号；
APS则记录了申请审批整个过程的信息，主键：申请书条形码；

- ACCT/CARD

针对审批通过的人设立了账户，设置账户的同时发信用卡。
账户是卡的集合，主键是账户号(XACCOUNT)；
卡作为消费的介质，共享账户的额度和账单，主键是卡号(CARD_NBR)；
我们公司的卡产品：普卡、金卡、白金卡、公务卡、VISA白金卡和移动联名卡。

- CHGS

所有账户表(ACCT)或者卡表(CARD)变更过的信息都会记录进来。
例如，卡片标志的转换、账户状态的变更、额度的变化等。

- EVENT

有了卡这一种介质，客户就可以去消费。
交易表记录了我们有史以来的交易流水（包括刷卡、还款、利费等）。
因此我们会对交易表按年或者半年切割。

- MPUR

为什么会有分期业务？
我们的分期产品：信用卡分期：消费分期、账单分期、邮购分期，大小额分期（消费信贷分期）。

- AUTH

交易（除了还款）和分期都需要授权。
与交易表类似，授权也是一张流水表，记录了所有授权相关的信息，授权的过程有成功，也有失败。

- STMT

定期的对账单，有客户的交易和分期等行为产生。
> 账单、最低还款额、逾期如何计算？


#### 3.1.2.2 数据框架

<p align="center">
<img src="https://github.com/IvanaXu/DecisionScience/releases/download/base/3.1.2.2-000.png" width=1000>
</p>

### 3.1.3 人行征信数据
人行信用报告是人行征信服务中心全面记录个人信用活动，反映个人信用状况的文件，是个人信用信息基础数据库的基础产品。包含个人基本信息、信贷交易信息明细等。
那么一般我们所指的人行征信数据，即根据人行信用报告等查询报文结果，经数据解析、数据处理、数据入仓后的一系列以人行报告编号为主键的数据表。

#### 3.1.3.1 数据概况

1、个人基本信息

（1）身份信息

（2）配偶信息

（3）居住信息

（4）职业信息

2、信息概要

（1）信用提示 信用提示

（2）信用提示 中征信分数

（3）逾期及违约信息概要 呆账、资产处置、保证人代偿信息概要

（4）逾期及违约信息概要 逾期（透支）信息汇总

（5）授信及负债信息概要 未结清贷款信息汇总

（6）授信及负债信息概要 未销户贷记卡信息汇总

（7）授信及负债信息概要 未销户准贷记卡信息汇总

（8）授信及负债信息概要 对外担保信息汇总

3、信贷交易信息明细

（1）资产处置信息

（2）保证人代偿信息

（3）贷款明细信息

（4）贷记卡明细信息

（5）准贷记卡明细信息

（6）担保信息 对外贷款担保信息

（7）担保信息 对外信用卡担保信息

（8）5年逾期信息

4、公共信息明细

（1）欠税记录

（2）法院民事判决记录

（3）法院强制执行记录

（4）行政处罚记录

（5）住房公积金参缴记录

（6）养老保险金缴存记录

（7）养老保险金发放记录

（8）低保救助记录

（9）执业资格记录

（10）行政奖励记录

（11）车辆交易和抵押记录

（12）电信缴费记录
 
5、声明信息

6、异议标注

7、查询记录

（1）查询记录汇总

（2）信贷审批查询记录明细


#### 3.1.3.2 人行示例
人行信用报告样例，这里版本为二代，标注部分为与一代差异，主要为新增和调整，整体数据结构一致。

<p align="center">
  <img src="https://github.com/IvanaXu/DecisionScience/releases/download/base/3.1.3.2-001.png" width=1000>
  <img src="https://github.com/IvanaXu/DecisionScience/releases/download/base/3.1.3.2-002.png" width=1000>
  <img src="https://github.com/IvanaXu/DecisionScience/releases/download/base/3.1.3.2-003.png" width=1000>
  <img src="https://github.com/IvanaXu/DecisionScience/releases/download/base/3.1.3.2-004.png" width=1000>
  <img src="https://github.com/IvanaXu/DecisionScience/releases/download/base/3.1.3.2-005.png" width=1000>
  
  <img src="https://github.com/IvanaXu/DecisionScience/releases/download/base/3.1.3.2-006.png" width=1000>
  <img src="https://github.com/IvanaXu/DecisionScience/releases/download/base/3.1.3.2-007.png" width=1000>
  <img src="https://github.com/IvanaXu/DecisionScience/releases/download/base/3.1.3.2-008.png" width=1000>
  <img src="https://github.com/IvanaXu/DecisionScience/releases/download/base/3.1.3.2-009.png" width=1000>
  <img src="https://github.com/IvanaXu/DecisionScience/releases/download/base/3.1.3.2-010.png" width=1000>
  
  <img src="https://github.com/IvanaXu/DecisionScience/releases/download/base/3.1.3.2-011.png" width=1000>
  <img src="https://github.com/IvanaXu/DecisionScience/releases/download/base/3.1.3.2-012.png" width=1000>
  <img src="https://github.com/IvanaXu/DecisionScience/releases/download/base/3.1.3.2-013.png" width=1000>
  <img src="https://github.com/IvanaXu/DecisionScience/releases/download/base/3.1.3.2-014.png" width=1000>
  <img src="https://github.com/IvanaXu/DecisionScience/releases/download/base/3.1.3.2-015.png" width=1000>
</p>


#### 3.1.3.3 人行评分
人行评分只是一个通俗名称，人行报告上称其为“数字解读”，如下：

<p align="center">
<img src="https://github.com/IvanaXu/DecisionScience/releases/download/base/3.1.3.3-000.png" width=1000>
</p>

即该客户因存在逾期还款记录影响，当前人行评分为468，人群相对位置10%-20%，较低。
如《征信系统数据查询接口规格说明(信用报告数字解读)》所示：

> 为满足查询机构在贷后风险管理工作中对征信系统的需求，进一步提升征信系统服务质量，查询机构可通过接口方式向征信系统发送信用报告数字解读查询请求，征信系统向查询机构反馈信用报告数字解读查询结果，实现查询机构系统与征信系统的无缝对接。
> 查询机构通过接口方式进行个人信用报告数字解读查询的查询方式为批量查询。遵循《人民银行征信系统数据查询接口规范 通用要求》（Q/PBCCRC 3.1-2017）中规定，查询文件处理状态接口接入采用 WebService 或 MQ 方式；批量查询接口接入采用基于征信系统文件传输平台方式。


一般我们可以通过每月查询更新客户人行评分用于加强客户风险管理，属于重要数据源。
根据相关经验可知：

- 查询准备：行内可接征信中心，并已备**查询机构代码、查询用户代码、查询用户密码**；
- 可通过接口查询或系统查询，一般不超过1000000条；
- 原则上查询客户应为查询时有效户，筛选条件为该客户名下有未销卡片（需谨慎！避免过度查询导致封号）；
- 人行评分每月度更新，月初（一般5-8号）更新**上月评分**，建议查询时间为10号；
- 一般来说，人行评分范围为0-985分，但0一般由于对**非40返回码**的数据处理引起；

> 返回码，内容
> 
> **40，查询成功**
> 
> 10，查无此人
> 
> 21，无信贷记录而不可评分
> 
> 22，信用历史长度小于 3 个月而不可评分
> 
> 23，缺少近两年的信贷信息而不可评分
> 
> 30，评分失败
> 
> 24，由于未计算完成而未查到评分结果
> 
> 26，因异常原因而未查到评分结果
> 
> 25，超出历史 24 个月而未查到评分结果


由于人行评分形成了统一行业标准，详见最新版本《个人信用报告数字解读-验证几率表》，如2013.12-2015.12日期示例：

<p align="center">
<img src="https://github.com/IvanaXu/DecisionScience/releases/download/base/3.1.3.3-001.png" width=1000>
</p>

那么，其评分占比分布也是观测/对比客群结构最直观的方法，如下：

<p align="center">
<img src="https://github.com/IvanaXu/DecisionScience/releases/download/base/3.1.3.3-002.png" width=1000>
</p>

#### 3.1.3.4 数据应用
人行信用报告样例（已数据加密）：

（1）贷款

<p align="center">
<img src="https://github.com/IvanaXu/DecisionScience/releases/download/base/3.1.3.4-000.png" width=600>
</p>

（2）贷记卡

<p align="center">
<img src="https://github.com/IvanaXu/DecisionScience/releases/download/base/3.1.3.4-001.png" width=600>
</p>

如例，该客户有房有车，相关贷款、贷记卡历史未有逾期；现持32.2W的车贷月应还13981，126W的房贷月应还8445，**有一定的资质**；有ZF银行10W授信贷记卡，且历史最大额度使用率为30.3%，**但近6个月无使用**，另有SE银行8.5W授信贷记卡，近6个月平均使用24445，当前额度使用率100%，但本月实还、应还款均为12292，**推测近6个月消费自ZF银行转移SE银行，且在SE银行新办有灵账分期业务**。
_注：ZF、SE为人行报告商业银行代号._

> 通俗来说，至少有三份人行报告：贷前，根据客户资质、收入负债、综合信用等予以审批通过并差异化授信，根据他行信用卡使用情况预测客户激活率等；贷中，根据客户消费能力、钱包位置予以运营管理，贷中阶段他行风险综合表现也是重要参考等；贷后，客户风险预警或实际逾期时，根据贷后人行报告细分风险等级，进行贷后风险管理。


### 3.1.4 外部数据
以下示例之：

```
> 身份核验
（1）公安联网核查；
（2）手机号三要素核验；
> 黑名单查询
略；
> 学历查询）
（1）学信网；
> 信用类
（1）同盾风控
（2）鹏元征信、鹏元社保
（3）百融评分
（4）惠民征信
如，车辆处罚、车辆信息、法院失信、个人信息、公积金信息、婚姻信息、企业信息、社保信息、水务缴费；
（5）芝麻信用
> 欺诈类
（1）外部欺诈分
如，同盾申请反欺诈评分；
（2）关联网络
自建；
（3）设备指纹
如，同盾设备指纹；
> 法院信息
（1）高院失信被执行人
> 其它
（1）公积金查询
（2）市民卡数据
```

### 3.1.5 总行数据
一般银行总行各核心业务，主要包括：客户信息、交易行为、渠道数据、理财业务等。
如下图示例，其数据盘点：

<p align="center">
<img src="https://github.com/IvanaXu/DecisionScience/releases/download/base/3.1.5.0-000.png" width=1000>
</p>

以**交易行为**为例，一般涵盖存取现金、支付转账、内部转账、网上支付、日常消费等，以每户每日逐条记录的交易流水存在，具体到时间和金额。
若进一步细化，交易行为可分为：

<p align="center">
<img src="https://github.com/IvanaXu/DecisionScience/releases/download/base/3.1.5.0-001.png" width=640>
</p>
