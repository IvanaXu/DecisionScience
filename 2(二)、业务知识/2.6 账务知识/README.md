
- [2.6.1 账单日修改](#261-账单日修改)
- [2.6.2 分期产品设置](#262-分期产品设置)

### 2.6.1 账单日修改
虽然账单日修改并不多见，且大多银行也会限制客户年修改次数，但这对很多数据机制均有影响。
业务逻辑上，

- 账单日修改

新的账单日何时生效，取决与旧账单日、新账单日、最后还款日、申请更改日期之间的关系。

- 新账单日生效必须满足：

（1）一个自然月最多进行一次账单日结账处理，特殊情况下一个自然月内可以没有账单日结账处理；
（2）必须遵循账单日－最后还款日－账单日－最后还款日的周期规律；

- 账单日修改-修改生效日期

（1）修改日期在最近一期帐单日（旧账单日）的到期还款日之前
新账单日的生效日期一定要等到原账单日的最后还款日当天批处理完成后，账户的账单日才会变更为新帐单日
（2）修改日期在最近一期账单日（旧账单日）的到期还款日之后
修改当天立即修改成新的账单日，生效日期为最近一次的新帐单日

- Q-A示例
> Q：假设还款宽限期为20天，账户账单日为20号，12月5日申请修改账单日为11号，新账单日何时生效？12月份账单日为几号？
A：12月10号最后还款日时生效，12月的账单日为11号

<p align="center">
<img src="https://github.com/IvanaXu/DecisionScience/releases/download/base/2.6.1.0-000.png" width=1200>
</p>

> Q：假设还款宽限期20天，旧账单日为20号，12月15日申请更改账单日，如果新账单日分别为12号和25号，下一个账单日是哪一天？
A：立即生效，1月12号(12月无账单)，12月25日

<p align="center">
<img src="https://github.com/IvanaXu/DecisionScience/releases/download/base/2.6.1.0-001.png" width=1200>
</p>

### 2.6.2 分期产品设置
在风险定价项目中，出于分期产品费率打折需要，我们从银联发卡核心系统账务逻辑上对分期产品设置，以及后续业务系统对接，进行了如下探索：

- 针对不同分数区间的客户，进行标签管理

<p align="center">
<img src="https://github.com/IvanaXu/DecisionScience/releases/download/base/2.6.2.0-000.png" width=1200>
</p>

账户标签的定义逻辑由外部评分模型等系统提供，以账户号为索引，记录在发卡系统。账户标签可以通过画面CUNEG/4118交易/批量接口COMMONEW对账户标签进行查询与维护。标签字段（长度500位），供银行存放（外部评分模型系统提供的）对客户的分类标识。

在进行账户标签内容操作前，需要通过CODES画面进行对应标签内容的设定。代码类型对于账户级标签固定为ACTAG。代码值需按照0001，0002依次按顺序填写数字型代码值，程序中会进行代码值是否符合该规范的校验。长度值的最大值为20。

	后续再出现标签方面的需求时可以先在CODES表中进行标签内容的设定，然后就可以使用接口进行对应标签内容的配置。

<p align="center">
<img src="https://github.com/IvanaXu/DecisionScience/releases/download/base/2.6.2.0-001.png" width=1200>
</p>

- 分期付款手续费费率

分期付款手续费费率可以采用账户分期费率代码，通过账户绑定不同的分期费率代码来实现分期费率的灵活设置。在基础费率上可以打折或上浮，也可以区分不同的分期产品和渠道打折或者上浮。

（1）MPTCD分期费率代码维护
<p align="center">
<img src="https://github.com/IvanaXu/DecisionScience/releases/download/base/2.6.2.0-002.png" width=1200>
</p>

（2）MPPRD分期费率代码与分期产品关系维护
<p align="center">
<img src="https://github.com/IvanaXu/DecisionScience/releases/download/base/2.6.2.0-003.png" width=1200>
</p>

（3）MPSRC分期费率代码与申请渠道的关系维护
<p align="center">
<img src="https://github.com/IvanaXu/DecisionScience/releases/download/base/2.6.2.0-004.png" width=1200>
</p>

（4）ACMT2分期费率绑定关联账户
<p align="center">
<img src="https://github.com/IvanaXu/DecisionScience/releases/download/base/2.6.2.0-005.png" width=1200>
</p>

根据账户号查询卡号，在ACMT2画面，通过分期费率代码绑定关联账户。分期付款手续费费率的逻辑是PRPRD基础费率与三个画面的费率关系做加乘处理，当PRMSR画面同时设置费率折扣系数时，仍然按照PRPRD基础费率与三个画面的费率关系做加乘处理。

渠道端调用3137交易可以查询分期商品基础费率，调用3014交易查询账户分期费率代码，渠道端加载MPTCD\MPPRD\MPSRC三张表参数。这样既可以给持卡人展示基础费率，也可以展示个性化费率。

