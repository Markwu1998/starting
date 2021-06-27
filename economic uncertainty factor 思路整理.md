#Economic uncertainty

<!--整理逻辑思路 需要那些数据，有哪些难点-->

## 1.概述

> 本文实际上是探讨economic uncertainty这个因子在资产定价中的作用（==同时考虑α和β==），该因子作为（consumption and investment)的代理变量，反应的是宏观经济稳定性，代表的是uncertainty(==注意和risk的区别，Ellsberg ’s (1961)==)。economic uncertainty的具体指标由Jurado, Ludvigson, and Ng(2015)JLN构建的economic uncertainty index(defined as the conditional volatility of the unforecastable component of a large number of economic indicators)来量化，_数据来源于 Sydney Ludvigson’s website，NYU的宏观经济学教授_，

## 2.理论基础

### 2.1 ICAPM 模型

> 该部分考虑的是β

当economic uncertainty 增加时，investor考虑到未来收益的不确定性增加，会减少当期消费以对冲可能存在的经济下行风险，其中一种对冲方式是投资收益与economic uncertainty正相关的资产（即当经济不确定性增强时，由该不确定因子带来的风险收益增加，引起期望收益上升；当经济不确定性减弱时，由该不确定因子带来的风险收益降低，引起期望收益下降），投资者在预期到未来economic uncertainty 上升时，会购入该类资产。

###2.2 ICAPM模型的补充

> 该部分考虑的是α

Ellsberg ’s (1961)的实证研究表明投资者偏好于在确定的情况（知道收益的分布情况也认定为确定）进行决策，因此当投资者面对不确定的市场收益分布时，他们会要求一个更高的premium（α）才会与愿意持有市场组合（需要市场用额外的α来弥补该不确定性风险）。由此，当经济不确定性上升时，β较高的资产由不确定因子（economic uncertainty)带来的风险收益已经较高，投资者愿意持有，但β较低的资产由不确定因子带来的风险收益较低，只有当资产提供较高的premium（α）进行额外的补偿时，投资者才愿意持有。

### 2.3 进一步的潜在解释

> 该部分结合β和α综合考虑

假设市场上投资者对economic uncertainty的偏好和期待是充分分散且economic uncertainty足够高时，那么对economic uncertainty 厌恶程度较高的投资者不会进入市场，市场上基本上都是积极性的投资者，由此对他们而言，较高的economic uncertainty β对他们而言风险补偿已经足够，不需要再用额外的α来弥补收益。

## 3.数据来源与处理

### 3.1 UNC index数据

该指数反映的是宏观经济的不确定性，数据来源于 Sydney Ludvigsion's website（NYU的宏观经济学教授）.

![image-20210611094714295](C:\Users\Markwu\AppData\Roaming\Typora\typora-user-images\image-20210611094714295.png)

可以发现，在经济危机期间，该指数处于较高值，即宏观经济不确定性较高时会有较高的UNC index（初步印象中与VIX指数表现类似）。且领先1个月、3个月、12个月的UNC index相关性很高，有很好的预测效果。

### 3.2 个股的横截面特征

####3.2.1 UNC的因子载荷

用向前rolling 60 monthly的回归方法计算$UNC_{t}$的因子载荷，公式如下：
$$
(1)R_{i,t} = \alpha_{i,t} + \beta^{UNC}_{i,t}\cdot UNC_{t} + \beta^{MKT}_{i,t}\cdot MKT_{t} + \beta^{SMB}_{i,t}\cdot SMB_{t} + \beta^{HML}_{i,t}\cdot HML_{t} + \beta^{UMD}_{i,t}\cdot UMD_{t} + \beta^{LIQ}_{i,t}\cdot LIQ_{t} + \beta^{R_{I/A}}_{i,t}\cdot R_{I/A,t} + \beta^{R_{ROE}}_{i,t}\cdot R_{ROE,t} + \epsilon_{i,t}
$$
这里其他因子的构建参考Fama——French的做法，通过构建多空双方的因子组合，用组合的收益对$R_{i,t}$进行回归，算出因子载荷，即各项$\beta$。==注意：该公式仅用于提取stock对$UNC$的因子载荷==

变量解释：$R_{i,t}$为stock(i)在t时刻的收益率，$UNC_{t}$为t时刻的economic uncertainty index值，$MKT_{t}，SMB_{t}，HML_{t}，UMD_{t}，LIQ_{t}，R_{I/A}，R_{ROE}$是t时刻市场因子、市值因子、账面市值比因子、动量因子、流动性因子、投资因子和盈利因子的组合收益，具体构建方法见相应的参考文献。

#### 3.2.2 13个横截面特征（控制变量）

本文探讨的横截面特征（控制变量）共有13个

即$\beta^{MKT}, \beta^{VXO}, SIZE, BM, MOM, REV, ILLIQ, COSKEW, IVOL, DISP, I/A, ROE, MAX$



需要的数据：

市场特征：

- $\beta^{MKT}$：stock和大盘指数的月度收益率（月度数据）
- $\beta^{VXO}$：大盘指数期权隐含波动率的日变化值（==与VIX概念相似==）（日度数据）

个股特征：

- SIZE：每股股价和在外流通股数（不确定月度or年度数据？）
- BM：需要股票的账面价值、递延所得税、investment tax credit、优先股账面价值以及总市值（年度数据）
- MOM & REV ： 股价历史数据（月度数据）
- COSKEW： 股价历史数据，大盘指数，RF（月度数据）
- ILLIQ：每日股价收益的绝对值，每日成交量（日度数据）
- DISP：年度的EPS预测值以及在外流通股数的预测值（不确定月度or年度数据？）
- IVOL：每日stock的收益率，fama-french 三因子组合的日收益率（日度数据）
- MAX：日度股价数据（日度数据）
- I/A：账面资产变化值（年度数据）
- ROE：季度经营性利润（在扣除非经常性项目前的数值）（季度数据）

行业分类

_==其中BM，DISP，ROE数据不知道能不能拿到，对于拿不到的控制变量，可以尝试用其他类似的特征进行替代，或者干脆不考虑该控制变量的作用==_

## 4.模型设定与分析

### 4.1 单变量组合层面分析

> 该部分仅用$UNC_{t-1}$(上个月的economic uncertainty index)作为解释变量。

$(2)R_{i,t} = \alpha_{i,t} + \beta^{UNC}_{i,t}\cdot UNC_{t-1} + \epsilon_{i,t}$

基于rolling 60个月的历史数据估计出上一个窗口期（60个月，以上个月作为最后一个月）每只stock的$\beta^{UNC}_{i,t}$（每次回归时，β下面的t仅代表窗口期的截止时间点），作为本月该stock对$UNC_{t}$的风险敞口_（**这里假设了上月敞口能代表本月敞口，下一个部分会证明敞口的persistence）**_，随后在横截面上根据窗口期计算出的所有stock风险敞口$\beta^{UNC}_{i,t}$将所有stock从高到低分为10组,构建decile portfolios，计算各组的平均敞口、收益以及超额收益。

计算组合$\beta^{UNC}$是采用等权加权（包括组内截面维度和时间维度）

计算组合收益（RET-RF）以及四种方法下的超额收益（$\alpha^1_5,\alpha^2_5,\alpha_4,\alpha_7$）分别采用等权加权和按市值加权的方式进行计算。

组合超额收益的计算方法为:

$(3)R_{i} = \alpha_{i} + \beta^{MKT}_{i,t}\cdot MKT_{t} + \beta^{SMB}_{i}\cdot SMB_{t} + \beta^{HML}_{i}\cdot HML_{t} + \beta^{UMD}_{i}\cdot UMD_{t} + \beta^{LIQ}_{i}\cdot LIQ_{t} + \epsilon_{i}$

$(4) R_{i} = \alpha_{i} + \beta^{MKT}_{i}\cdot MKT_{t} + \beta^{SMB}_{i}\cdot SMB_{t} + \beta^{HML}_{i}\cdot HML_{t} + \beta^{R_{I/A}}_{i}\cdot R_{I/A,t} + \beta^{R_{ROE}}_{i}\cdot R_{ROE,t} + \epsilon_{i}$

$(5) R_{i} = \alpha_{i} + \beta^{MKT}_{i}\cdot MKT_{t} + \beta^{SMB}_{i}\cdot SMB_{t} + \beta^{R_{I/A}}_{i}\cdot R_{I/A,t} + \beta^{R_{ROE}}_{i}\cdot R_{ROE,t} + \epsilon_{i}$

$(6) R_{i} = \alpha_{i} + \beta^{MKT}_{i}\cdot MKT_{t} + \beta^{SMB}_{i}\cdot SMB_{t} + \beta^{HML}_{i}\cdot HML_{t} + \beta^{UMD}_{i}\cdot UMD_{t} + \beta^{LIQ}_{i}\cdot LIQ_{t} + \beta^{R_{I/A}}_{i}\cdot R_{I/A,t} + \beta^{R_{ROE}}_{i}\cdot R_{ROE,t} + \epsilon_{i}$

其他因子（$MKT_{t}$等)来源于3.2.1。

得到Table1的结果。

![image-20210613092401821](C:\Users\Markwu\AppData\Roaming\Typora\typora-user-images\image-20210613092401821.png)



Table 1中分组时所用$\beta^{UNC}_{i,t}$的是滞后一期的历史值，此处我们假定了$\beta^{UNC}_{i,t}$的延续性（persistence），即上一期有较高的$\beta^{UNC}_{i,t-1}$stock在本期也会有相应较高的$\beta^{UNC}_{i,t}$。为了验证这个assumption，我们进行下面的分析：

$(7) \beta^{UNC}_{i,t} = a_{i} + \lambda_{i}\cdot \beta^{UNC}_{i,t-12n} + \epsilon_{i,t}$

$(8)\beta^{UNC}_{i,t} = a_{i} +\lambda_{i}\cdot \beta^{UNC}_{i,t-12n} + \sum\mu_{i}\cdot X^{UNC}_{i,t-12n}+ \epsilon_{i,t}$

其中$X^{UNC}_{i,t-12n}$横截面控制变量，共13个；n为滞后年份，分别计算了n=1,2,3,4,5的回归结果，==这部分回归的时候是否需要加入截距项？==

得到Table2的回归结果

![image-20210611091556055](C:\Users\Markwu\AppData\Roaming\Typora\typora-user-images\image-20210611091556055.png)

### 4.2平均股票特征层面分析

> 该部分研究$\beta^{UNC}_{i}$与stock(i)某些横截面特征之间的联系

回归方程如下：

$(9)\beta^{UNC}_{i,t} = \lambda_{0,t} + \lambda_{1,t}X_{i,t} + \epsilon_{i,t}$

其中$\beta^{UNC}_{i,t}$为3.2.1中回归得到的结果，$X_{i,t}$为3.2.2中同期stock(i) 的横截面特征，共13个。

得到Table3的回归结果，共做了13个单变量回归和一个全变量回归。

![image-20210611093936555](C:\Users\Markwu\AppData\Roaming\Typora\typora-user-images\image-20210611093936555.png)

横截面的维度上做回归，时间维度上做平均。每个t下跑一次回归，得到$\lambda_{0,t}和\lambda_{1,t}$值，不同t之间用算数平均得到$\lambda_{0}和\lambda_{1}$。

###4.3双变量组合层面分析

> 该部分在4.1的基础上加入控制变量进行组别细分保证组内某个特征保持一致，即将$\beta^{UNC}$分别和13个横截面特征结合进行分组，再进行4.1一样的回归分析。（==该部分的控制变量应该和4.1中的$\beta^{UNC}$一样都是上一期的数据吧？==）

 以$\beta^{UNC}$和$\beta^{MKT}$结合为例，先根据上一期的$\beta^{MKT}$将stock分为10组，再按照上一期的$\beta^{UNC}$再分10组，得到100组，再每组进行4.1的过程以四种α。

每个横截面特征都组合过后发现结果依旧显著，说明这个4.1中的四个α都与这13个因子无关，很可能是由补偿economic uncertainty带来的。

### 4.4个股层面的横截面回归

> 前面部分是从因子收益出发来求因子载荷和超额收益（α），假定了因子收益之间的线性模型。该部分从因子载荷出发，探索$\beta^{UNC}$与未来收益之间的关系，探求是哪个因子的风险暴露需要额外用premium来补偿。
>
> ==这部分Fama-MacBeth回归的逻辑不清楚，参考Comparing Cross-Section and Time-Series Factor Models
> Eugene F. Fama and Kenneth R. French看一下==

$(10) R_{i,t+1} = \lambda_{0.t} + \lambda_{1,t}\cdot \beta^{UNC}_{i,t} + \lambda_{2,t}\cdot \beta^{MKT}_{i,t} + \lambda_{3,t}\cdot \beta^{VXO}_{i,t} + \lambda_{4,t}\cdot X_{i,t} + \epsilon_{i,t+1}$

$X_{i,t}$是其他stock层面的控制变量（共11个）

其中因子载荷以及相应控制变量具体值的计算参考3.2部分

![image-20210611115211353](C:\Users\Markwu\AppData\Roaming\Typora\typora-user-images\image-20210611115211353.png)

![image-20210611115226115](C:\Users\Markwu\AppData\Roaming\Typora\typora-user-images\image-20210611115226115.png)

panelA部分：在每个横截面（时间点）上将不同stocks放一起跑一次回归，估计出$\lambda_{0,t}$，再对不同时间下的各个λ进行算数平均，得到panelA左边部分。右边部分是在回归前先按行业分组（==不确定是各组内进行回归还是添加虚拟变量进行这总体回归==），再进行回归。

panelB部分：将所有控制变量都考虑进来（==不确定这里的控制变量是否包括了行业效应==），并让时间滞后项从一个月前到11个月前，发现效果一直显著，但时间一旦大于1年，效果便不再显著。

### 4.5不确定性溢价的非线性时变特征

> 该部分探究uncertainty premium （Table 4 中的$\lambda_{0,t}$）是否具有时变性，即在经济衰退以及经济不确定性较大时期uncertainty premium是否较高。

我们采用Chicago FED National Activity Index (CFNAI)指标来衡量经济状态，将CFNAI指数的三个月平均值和Table 4 中第一列估计出的$\beta^{UNC}_{i,t}$前的系数==$\lambda_{1,t}$（为什么是这个系数？这个系数的经济含义是什么？）==的三个月移动平均值进行比较，并用$\lambda_{1,t}$对CFANI指数月度值进行回归，这两步都反映出两者有较高的相关性。

随后，考查不同经济时期下（高不确定性与低不额定性，根据JLN指数均值进行区分）高$\beta^{UNC}$和低$\beta^{UNC}$组合之间return和α的差异变化。发现在较高的不确定经济环境下，高$\beta^{UNC}$和低$\beta^{UNC}$组合之间return和α的差异更大。

由此说明了在经济不确定性较高的环境下，uncertain premium会更高。随着经济不确定性增加，$\beta^{UNC}$为负的stock收益下降，因此，风险厌恶的投资者会要求以uncertainty premium的形式要求收益补偿。

![image-20210612200842411](C:\Users\Markwu\AppData\Roaming\Typora\typora-user-images\image-20210612200842411.png)

==好像还有CFNAI是 $\lambda_{1,t}$的先行指标？==这一性质

### 4.6用权益组合代替stock进行检验

> 该部分通过构建uncertainty beta factor，构建方式参考 Fana—French的因子组合构建法，该factor在做到市值中性的前提下反应了与uncertainty beta有关的收益。
>
> 目的是检验4.1-4.5的性质在底层标的资产由stock换为portfolio是是否依然成立。

按照市值加权和等权加权的方式构建了$\beta^{UNC}$ factor（本质上是一种投资组合），Panel A 反应了组合的平均月度收益和四种模型下的超额收益（$\alpha$）。

Panel B基于$\beta^{UNC}$构建单变量组合的组合，第一层组合是49个行业组合和3*100个（ size and book-to-market, size and investment, and size and profitability）组合，共计349个（==这部分不是很懂，每只stock应该都进入了四组，为什么要这么把349个组合搞到一起，这样控制变量不是混到一起了？？？==，）；第二层组合是第一层349个组合基于$\beta^{UNC}_{t-n}$，（n=1,3,12）的十分位组合，再进行4.1中的回归分析。

结论表明：uncertainty beta 不仅在个股中被定价了，在组合中也被考虑到了。



![image-20210612202745082](C:\Users\Markwu\AppData\Roaming\Typora\typora-user-images\image-20210612202745082.png)

### 4.7稳健性检验

- 验证了uncertainty premium不是源于小市值、非流动性和低价股票。

- 在标普500指数中，只有低$\beta^{UNC}$的股票能带来超额收益，高$\beta^{UNC}$并不能带来显著的超额收益。

- 用以下两种模型代替（1）来估计$\beta^{UNC}$以检验该两种情况下估计得到的$\beta^{UNC}$是否依然具有良好的预测能力

  $$(11)R_{i,t} = \alpha_{i,t} + \beta^{UNC}_{i,t}\cdot UNC_{t} + \beta^{MKT}_{i,t}\cdot MKT_{t} + \beta^{SMB}_{i,t}\cdot SMB_{t} + \beta^{HML}_{i,t}\cdot HML_{t} + \beta^{UMD}_{i,t}\cdot UMD_{t} + \beta^{PS}_{i,t}\cdot PS_{t} + \epsilon_{i,t}$$

  $$(12)R_{i,t} = \alpha_{i,t} + \beta^{UNC}_{i,t}\cdot UNC_{t} + \beta^{MKT}_{i,t}\cdot MKT_{t} + \beta^{SMB}_{i,t}\cdot SMB_{t} + \beta^{HML}_{i,t}\cdot HML_{t} + \beta^{R_{I/A}}_{i,t}\cdot R_{I/A,t} + \beta^{R_{ROE}}_{i,t}\cdot R_{ROE,t} + \epsilon_{i,t}$$

  发现通过这两种方法得到的$\beta^{UNC}$仍然具有良好的预测效果。

  Table 6 展示了两种方法得到的结果

  ![image-20210612210713115](C:\Users\Markwu\AppData\Roaming\Typora\typora-user-images\image-20210612210713115.png)

- 随后又将4.1自变量中的one-month-ahead uncertainty index替换为three-month-ahead uncertainty index与twelve-month-ahead uncertainty index重复跑了一遍Table1，得到的结果不变，说明我们的模型对uncertainty index的构建并不敏感，具有该因子的普遍适用性。

- 最终检验了细分行业下uncertainty premium的显著性，发现十个细分行业中有八个显著。



##5.问题：

1.中国VIX数据怎么选取？有效记录的时间似乎比较短，在做月频回归时样本时间跨度要适当调整一下。

2.stock横截面特征中计算BM，ROE，DISP指标的底层数据能拿到吗？如果用其他类似的特征进行替换，得检查一下共线性问题。

3.区分一下risk和uncertainty，似乎本文选取的UNC index只反映uncertainty而不是risk，而VIX反应的是uncertainty还是risk?

4.做一下美国VIX和UNC index的相关性 看看能不能替换。







