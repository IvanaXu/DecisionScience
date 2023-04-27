<p align="center">
<img src="https://github.com/IvanaXu/DecisionScience/releases/download/base/3.4.0.0-000.png" width=1600>
</p>

基于上述数据现状，初步规划数据工程：

### 3.4.1 数据集市
<p align="center">
<img src="https://github.com/IvanaXu/DecisionScience/releases/download/base/3.4.1.0-000.png" width=1600>
</p>

<p align="center">
<img src="https://github.com/IvanaXu/DecisionScience/releases/download/base/3.4.1.0-001.png" width=1600>
</p>

### 3.4.2 数据治理

<p align="center">
<img src="https://github.com/IvanaXu/DecisionScience/releases/download/base/3.4.2.0-000.png" width=1600>
</p>

<p align="center">
<img src="https://github.com/IvanaXu/DecisionScience/releases/download/base/3.4.2.0-001.png" width=1600>
</p>

### 3.4.3 数据安全
#### 3.4.3.1 安全要求

<p align="center">
<img src="https://github.com/IvanaXu/DecisionScience/releases/download/base/3.4.3.1-000.png" width=1600>
</p>

<p align="center">
<img src="https://github.com/IvanaXu/DecisionScience/releases/download/base/3.4.3.1-001.png" width=1600>
</p>

#### 3.4.3.2 导数流程
基于上述数据安全要求，一般情况下，数据环境与外部环境隔绝。但业务开展过程，始终不能避免数据交互，即数据/文件的导入导出。为保障数据安全，设计并实施更为安全可靠的导数流程，是整个决策科学系统设计的重要一环。常见模式如下：

-  数据安全级别设计

如：敏感数据分为三个级别：涉及客户卡号、证件号、手机号明细的为一级数据，需审批至总经理；涉及客户的地址、账户号、风险相关等涉及商业机密或经营指标的信息为二级数据，需审批至分管领导；涉及员工信息以及客户其他信息数据为三级数据，需审批至申请人部门负责人。 

（1）依托人工
配备专门的机器，为数据环境下唯一开通对外接口的机器，并由专门的人员（即数据安全专员）来检查并记录该数据/文件的安全性，并持续留痕，以备翻查。该流程系统配置便捷、实施简易，但需强依赖数据安全专员，并占用一定工作量。

（2）依托人工+系统
人工流程存在一定缺点，如人工操作需检查、手动备份、填写记录较为繁琐，久而久之，易流于形式。我们可以在此基础上加一个包含用户权限+文件上传下载的简单系统（Python Web可快速实现）。这样就可以省去备份、记录等步骤，系统自动化后，使数据安全专员只需负责检查。
这样，申请人如有数据或文件导出，需上传至该系统，输入自己的用户名，选择文件并点击上传。之后会产生唯一识别的随机码，导出人需记下该随机码，同时申请人需要填写《数据审批申请单》，并需写明：批次号、随机码、文件名称、数据用途、以及涉及的数据量级、敏感信息等内容。经审批后，数据安全专员通过该随机码下载，再检查导出给申请人。

（3）依托系统
我们可以设想，如果上述系统功能设计越完善，如审批功能等，相关操作可以更简易 。在项目过程中，我们接触到`QINGCLOUD-ANYBOX`，

> ANYBOX是一个面向企业文档内容协作平台，围绕企业文件应用场景提供海量文件管理、同步/备份、文件共享与协作等文档协作服务，为企业内部成员提供便捷的文件共享协作方式，并打通企业文件流通与协作通道，在文件数据能确保安全控制的同时数据在流转之间实现共享协作，促进文件价值的提升。

<p align="center">
<img src="https://github.com/IvanaXu/DecisionScience/releases/download/base/3.4.3.2-000.png" width=400>
</p>

这也算是个解决方案，只是略大材小用，故探索后实际运用。现仍主要采用模式1、2。

