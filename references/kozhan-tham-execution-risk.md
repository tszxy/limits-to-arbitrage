# Kozhan & Tham：How Riskless is "Riskless" Arbitrage?（套利中的执行风险）——论文精读参考文档

> 原文：Roman Kozhan (Warwick Business School) & Wing Wah Tham (Erasmus University Rotterdam), *"How riskless is 'riskless' arbitrage?"*（工作论文版本，51 页）。
> 本文档完整提取论文的模型设定、公式（eq. (1)–(12)）、4 个 Assumptions、4 个 Propositions、数值/蒙特卡洛实例与全部实证结果。公式按论文编号排列并标注页码；PDF 文本提取错乱的公式已按金融数学常识还原为 LaTeX。

## 目录

- [1. 研究问题与核心论点](#1-研究问题与核心论点)
  - [1.1 核心论点：execution risk in arbitrage exploitation](#11-核心论点execution-risk-in-arbitrage-exploitation)
  - [1.2 与既有"有限套利"文献的区别](#12-与既有限套利文献的区别)
  - [1.3 四个可检验假说](#13-四个可检验假说)
- [2. 理论模型](#2-理论模型)
  - [2.1 市场与资产（Markets and assets）](#21-市场与资产markets-and-assets)
  - [2.2 交易者（Traders）](#22-交易者traders)
  - [2.3 限价订单簿（Limit order book）](#23-限价订单簿limit-order-book)
  - [2.4 套利偏差（Arbitrage deviation）](#24-套利偏差arbitrage-deviation)
  - [2.5 套利策略与竞争进入（Arbitrage strategies）](#25-套利策略与竞争进入arbitrage-strategies)
  - [2.6 均衡（Equilibrium）与 Propositions 1–4](#26-均衡equilibrium与-propositions-14)
  - [2.7 限价单与库存成本（Limit orders and inventory cost）](#27-限价单与库存成本limit-orders-and-inventory-cost)
  - [2.8 与 Stein (2009) crowding effect 的关系](#28-与-stein-2009-crowding-effect-的关系)
  - [2.9 模型的三大含义](#29-模型的三大含义)
- [3. 数值实例与直观例子](#3-数值实例与直观例子)
  - [3.1 引言中的两套利者竞争例子](#31-引言中的两套利者竞争例子)
  - [3.2 三套利者竞争一个最优价单位的概率例子](#32-三套利者竞争一个最优价单位的概率例子)
  - [3.3 错过一个最优价的利润计算（脚注 16）](#33-错过一个最优价的利润计算脚注-16)
  - [3.4 蒙特卡洛回测实例（Hypotheses 2 & 3）](#34-蒙特卡洛回测实例hypotheses-2--3)
- [4. 实证部分](#4-实证部分)
  - [4.1 外汇市场三角套利背景与无套利条件](#41-外汇市场三角套利背景与无套利条件)
  - [4.2 数据](#42-数据)
  - [4.3 描述性统计（Tables 1–2）](#43-描述性统计tables-12)
  - [4.4 Hypothesis 1：套利机会不被瞬间消除（Tables 3–5）](#44-hypothesis-1套利机会不被瞬间消除tables-35)
  - [4.5 Hypotheses 2 & 3：竞争导致亏损且亏损随人数增加（Tables 6–7）](#45-hypotheses-2--3竞争导致亏损且亏损随人数增加tables-67)
  - [4.6 Hypothesis 4：偏差与 illiquidity、inventory cost 的关系（Tables 8–9）](#46-hypothesis-4偏差与-illiquidityinventory-cost-的关系tables-89)
- [5. 结论与实践启示](#5-结论与实践启示)
- [附录：关键证明思路](#附录关键证明思路)

---

## 1. 研究问题与核心论点

### 1.1 核心论点：execution risk in arbitrage exploitation

论文挑战"无风险套利（riskless arbitrage）是无风险的"这一教科书信念。

- **"无风险"套利**（textbook "riskless" arbitrage）定义为：对**完全相同（identical）**的资产同时买入与卖出，套利者无需动用自有资金，只需同时订立买卖合约，用卖出合约的收入支付买入合约的成本（Mitchell, Pulvino and Stafford 2002；Liu and Timmermann 2009）。它不依赖 convergence trading（即赌两个相似但不完全相同的资产未来价差收敛）。
- **核心发现**：当理性套利者面临**"能否完整建成套利组合"的不确定性**时，即使市场存在完美替代品和完美可转换性（perfect substitutes and convertibility），套利仍然是受限的（arbitrage is limited）。作者称此现象为 **"execution risk in arbitrage exploitation"（套利执行中的执行风险）**。
- **机制**：该风险来自竞争套利者涌入同一交易产生的 **crowding effect（拥挤效应）**，套利者彼此施加 **negative externalities（负外部性）**。两类负外部性：
  1. **Illiquidity cost（非流动性成本）**：最优价位上供给有限，竞争失败的套利者只能以次优价格成交（"walk up the limit order book"），遭受 price slippage；
  2. **Inventory holding cost（库存持有成本）**：套利组合只完成一部分（如买入 A 成功、卖空 B 失败），留下非意愿库存敞口（unwanted inventory），暴露于价格变动风险。
- 该机制不依赖 convergence trading、税收、监管或卖空约束，适用于所有资产市场。
- 注意区分：本文的 execution risk in arbitrage exploitation ≠ 交易中的 execution risk（后者指限价单何时成交的时间不确定性，见 Parlour 1998；Engle and Ferstenberg 2006）。（第 3 页，脚注 5）

**政策与实践背景**（第 3 页）：TABB Group（Tabb, Iati and Sussman 2009）估计高频交易年利润约 210 亿美元；欧洲证券监管委员会（CESR/10-142）和美国 SEC（2010 年 1 月 13 日 Concept Release on Equity Market Structure, Release No. 34-61358; File No. S7-02-10）均关注高频套利策略对市场质量的影响。

### 1.2 与既有"有限套利"文献的区别

- 既有限制（limits to arbitrage）多依赖 convergence trading：noise trader risk（De Long et al. 1990）、fundamental risk、synchronization risk（Abreu and Brunnermeier 2002）、wealth constraints 下的被迫平仓（Shleifer and Vishny 1992）等。
- **Kondor (2009)** 研究竞争对套利的影响；**Oehmke (2009)** 研究流动性摩擦下策略性套利者与价格收敛速度。他们的模型中**效率随套利者数量增加而提高**；本模型中**对稀缺套利资产的过度竞争阻碍误定价被立即消除**——这是关键区别（第 5 页）。
- **Stein (2009)** 的 crowding effect 依赖两个假设：(i) 单个套利者对总套利容量（aggregate arbitrage capacity）信息不完全；(ii) 交易策略没有 fundamental anchor（套利者不知道基本面价值）。Stein 认为若有 fundamental anchor，价格机制会化解拥挤。本文表明：由于金融资产存在**最小交易单位限制（minimum trade size restriction）**而**不可分割（indivisible）**，即使有 fundamental anchor、误定价对所有套利者直接可见，过度拥挤仍然有风险——因为利润无法像无限可分资产那样在所有套利者之间分割（第 5–6、16–17 页）。
- Kleidon (1992)、Kumar and Seppi (1994)、Holden (1995) 指出 1987 年股灾中指数套利（index arbitrage）因 stale prices（滞后价格）而存在 execution risk；其机制源于 1987 年 NYSE 的纸质化环境与软件缺陷，与本文机制不同（第 6 页，脚注 8）。
- 本文也是**首篇研究 inventory cost 对套利者（而非做市商）影响**的文献；并使用完整 LOB 信息构造流动性度量实证研究高频套利（第 6–7 页）。

### 1.3 四个可检验假说

（第 18 页，Section 2.9 末）

1. **Hypothesis 1**："Riskless" arbitrage opportunities are not eliminated instantly in financial markets.（"无风险"套利机会在市场中不会被瞬间消除。）
2. **Hypothesis 2**：The existence of competing arbitrageurs induces potential losses in arbitraging.（竞争套利者的存在使套利产生潜在亏损。）
3. **Hypothesis 3**：These losses increase with the number of competing arbitrageurs.（亏损随竞争套利者人数增加而扩大。）
4. **Hypothesis 4**：The size of arbitrage deviations increases with market illiquidity and cost of inventory.（套利偏差幅度随市场非流动性和库存成本增加而扩大。）

---

## 2. 理论模型

（Section 2，第 7–18 页）

### 2.1 市场与资产（Markets and assets）

（第 7–8 页）

- 有 $I$ 个资产 $i \in \{1, 2, \ldots, I\}$，分别在 $I$ 个分割的市场（segmented markets）中交易。
- 存在一个复制组合（replicating portfolio）$R^P$，由集合 $\{2, \ldots, I\}$ 中的全部资产构成，其**收益结构（payoff structure）与分红流（dividend stream）与资产 1 完全相同**。组合中对每个资产各持一单位多头或空头，用向量 $[w_2, \ldots, w_I]$ 表示：

$$w_i = \begin{cases} 1, & \text{持资产 } i \text{ 多头（long）} \\ -1, & \text{持资产 } i \text{ 空头（short）} \end{cases}$$

- 市场无卖空约束（no short sale constraints）。

> **Assumption 1（第 7 页）**：There is perfect convertibility between asset 1 and portfolio $R^P$.（资产 1 与组合 $R^P$ 之间可完美相互转换——一单位资产 1 可兑换为一单位组合 $R^P$，反之亦然。）

**例**：外汇市场——一种货币可以直接买入（asset 1），也可以通过其他货币间接买入（portfolio）。**反例**：dual-listed companies（DLC，两地上市公司）——如 Royal Dutch NV 的股东不能把股票转换为 Shell Transport and Trading PLC 的股票，因此 DLC 套利只能依靠 convergence trading。

**含义**：在 Assumption 1 下，传统的套利障碍——fundamental risk、noise trader risk、synchronization risk——在模型设定中均不存在（第 8 页）。

### 2.2 交易者（Traders）

（第 8 页）

- **Local traders（本地交易者）**：仿照 Kondor (2009)，有 $I$ 组本地交易者，第 $i$ 组只在自己的市场 $i$ 中交易。其中的 liquidity traders（流动性交易者）出于模型外生原因交易，通过在限价订单簿（LOB）中挂出订单提供流动性。本地交易者的不对称需求与收入冲击会造成各市场间对相同资产的瞬时需要差异——从而相同资产暂时以不同价格交易，直到套利者消除误定价。
- **Arbitrageurs（套利者）**：存在 $k$ 个相互竞争的风险中性（risk-neutral）套利者，可跨市场交易、利用任何误定价。所有套利操作均以**同时买卖相同资产**的方式进行、无需动用自有资金；套利者使用**市价单（market orders）**以保证买卖的同时性（限价单情形在 2.7 节讨论）。
- 记套利者集合 $K = \{1, \ldots, k\}$，套利者 $j$ 的竞争者集合 $K_{-j} = K \setminus \{j\}$。

> **Assumption 2（第 8 页）**：All arbitrageurs can only buy one unit of each asset needed to form the arbitrage portfolio.（每个套利者对构建套利组合所需的每种资产只能买入一单位。）作者说明：允许买更多只会加剧 execution risk。

### 2.3 限价订单簿（Limit order book）

（第 9 页）

- 所有参与者都能看到公开电子屏幕上每个价位的价格与数量；挂出、撤改限价单无成本（成交瞬间除外）；所有人可看到 LOB 全部价格和深度（demand and supply schedules）。
- 交易所存在**最小交易单位限制**，因此资产在实践中被视为**不可分割品（indivisible goods）**；价格放在离散网格上，最小交易单位为 1 单位。
- 离散供求表只有**两层（two layers）**：
  - 第一层：最优 bid 价 $p_i^b$、最优 ask 价 $p_i^a$，对应数量（breadth，宽度）$n_i^b$、$n_i^a$；
  - 第二层：次优 bid 价 $p_i^b - \Delta_i^b$、次优 ask 价 $p_i^a + \Delta_i^a$。$\Delta_i^b$、$\Delta_i^a$ 分别为最优与次优买卖价之差。
  - **简化假设**：第二层价格处供给无限（infinite supply）。加入更多层只会加剧 execution risk（第 9 页，脚注 10）。

### 2.4 套利偏差（Arbitrage deviation）

（第 9–10 页）

组合 $R^P$ 的最优买入价为（未编号公式，第 9 页）：

$$P^a = \sum_{i=2}^{I} w_i\, p_i(w_i), \qquad p_i(w_i) = \begin{cases} p_i^b, & w_i = -1 \\ p_i^a, & w_i = 1 \end{cases}$$

（即做多按 ask 价买入、做空按 bid 价卖出。）记组合 $R^P$ 的最优卖出价为 $P^b$。

由于 $R^P$ 与资产 1 的收益结构、分红流与风险敞口完全相同，两者应有相同价格。计入交易成本后，**误定价（mispricing）**发生当且仅当（未编号不等式，第 10 页）：

$$P^a < p_1^b \quad \text{或} \quad P^b > p_1^a$$

注意在 bid-ask spread 为正（$p_1^a > p_1^b$ 且 $P^a > P^b$）的假设下，两个不等式 $P^b - p_1^a > 0$ 与 $p_1^b - P^a > 0$ 至多一个成立（脚注 11）。

**套利偏差幅度（arbitrage deviation）定义为**（未编号公式，第 10 页）：

$$A = \max\left\{ 0,\; P^b - p_1^a,\; p_1^b - P^a \right\}$$

与 Holden (1995)、Abreu and Brunnermeier (2002) 类似，套利偏差源于流动性提供者的流动性冲击或需求冲击（脚注 12）。

### 2.5 套利策略与竞争进入（Arbitrage strategies）

（第 10–11 页）

> **Assumption 3（第 10 页）**：$\max\{n_i^a, n_i^b\} < k$ 对每个 $i = 1, \ldots, I$ 成立。（对所需资产存在超额需求：最优价位上的最大可用单位数总少于套利者人数 $k$。）作者说明此假设仅为叙述方便；只要至少一种所需资产供给短缺，execution risk 就存在（脚注 13）。

- 算法交易（algorithmic trading）使大量专业套利者几乎同时利用套利机会，故假设观察到误定价的套利者**同时提交市价单**（只要套利者不知道自己市价单的排队位置，同时性假设即有效，脚注 15）。

> **Assumption 4（第 11 页）**：All arbitrageurs have the same probability of executing their market orders at the best available price when they submit market orders simultaneously.（同时提交市价单时，所有套利者以最优价成交的概率相同。）

- **失败的惩罚**：未能在最优价成交的套利者以次优价成交，即买单为 $p_i^a + \Delta_i^a$、卖单为 $p_i^b - \Delta_i^b$。在资产 $i$ 上错过最优买价的惩罚为 $\Delta_i^a$，此时套利者收益为 $A - \Delta_i^a$（推导见脚注 16，收录于第 3.3 节）。
- **最坏情形**：套利者在所有资产上都未拿到最优价，收益为 $A - \sum_{i=1}^{I} \Delta_i(w_i)$，其中 $\Delta_i(w_i) = \Delta_i^b$ 若 $w_i = -1$，$\Delta_i(w_i) = \Delta_i^a$ 若 $w_i = 1$。假设最坏情形收益为负（未编号不等式，第 11 页）：

$$A - \sum_{i=1}^{I} \Delta_i(w_i) < 0$$

- 每个套利者观察到套利机会后有两种策略：**"to trade"（交易）**或**"not to trade"（不交易）**；不交易收益为零。所有信息、策略、偏好与信念为共同知识（common knowledge）。

### 2.6 均衡（Equilibrium）与 Propositions 1–4

（第 12–16 页）

> **Proposition 1（第 12 页）**：若套利者 $j$ 在资产 $i$ 上拿到最优价的概率为 $P_i^j$，则其期望收益为
>
> $$E(U^j) = A - \sum_{i=1}^{I} \Delta_i(w_i)\left(1 - P_i^j\right). \tag{1}$$
>
> （证明见附录，用归纳法，思路见本文档附录。）

**含义**：期望收益 = 观察到的误定价 $A$ − 执行风险导致的期望损失。项 $\Delta_i(w_i)(1 - P_i^j)$ 是资产 $i$ 上的期望损失，由 price slippage（价格滑点）$\Delta_i(w_i)$ 与拿不到最优价的概率 $(1 - P_i^j)$ 构成。损失的严重程度随 $\Delta_i(w_i)$ 增加，而 $\Delta_i(w_i)$ 随市场非流动性增加——**对稀缺资产的竞争与市场非流动性共同加剧 execution risk**。

**全额参与（full participation）情形**：所有套利者同时提交市价单时，套利者 $j$ 在资产 $i$ 最优价成交的概率为（未编号公式，第 12 页）：

$$P_{i \mid n_i(w_i),\, k}^j = \frac{n_i(w_i)}{k}$$

其中 $n_i(w_i)$ 为最优价可用数量（即 breadth，广度/宽度，定义为最优价处的可用数量，脚注 17），$k$ 为竞争套利者人数。随 $k$ 增加该概率趋于 0；$n_i(w_i)$ 越大越易成交。此时套利者 $j$ 的期望损失为：

$$E(L^j) = \sum_{i=1}^{I} \Delta_i(w_i)\left(1 - \frac{n_i(w_i)}{k}\right). \tag{2}$$

（eq. (2)，第 13 页）若 $A \ge E(L^j)$，"trade" 为最优策略且收益为正。随 $k$ 增大，$E(U^j)$ 收敛到 $A - \sum_{i=1}^I \Delta_i(w_i) < 0$——**竞争加剧时套利者预期亏损**。

**混合策略（mixed strategies）**：设套利者 $j$ 的参与概率为 $\pi_j \in [0,1]$，混合策略组合 $\Pi = (\pi_1, \ldots, \pi_k)$，$\Pi_{-j} = (\pi_1, \ldots, \pi_{j-1}, \pi_{j+1}, \ldots, \pi_k)$。记对手策略下 $j$ 在资产 $i$ 最优价成交的概率为 $P_{i \mid n_i, k, \Pi_{-j}}^j$，并记失败概率

$$\bar{P}_{i \mid n_i, k, \Pi_{-j}}^j = 1 - P_{i \mid n_i, k, \Pi_{-j}}^j.$$

套利者 $j$ 采用混合策略时的期望收益为 $\pi_j E(U^j \mid \Pi_{-j})$，其中（由 Proposition 1）：

$$E\left(U^j \mid \Pi_{-j}\right) = A - \sum_{i=1}^{I} \Delta_i(w_i)\, \bar{P}_{i \mid n_i, k, \Pi_{-j}}^j \tag{3}$$

（eq. (3)，第 13 页）为 $j$ 采用纯策略 "trade"、对手采用混合策略 $\Pi_{-j}$ 时的期望收益。

> **Proposition 2（第 13–14 页）**：给定混合策略组合 $\Pi = (\pi_1, \ldots, \pi_k)$：
> (i) 概率 $P_{i \mid n_i, k, \Pi_{-j}}^j$ 随参与套利者人数 $k$ 单调递减；
> (ii) 套利者 $j$ 的期望利润 $\pi_j E(U^j \mid \Pi_{-j})$ 随参与套利者人数 $k$ 单调递减。
> （证明见附录；失败概率由全概率公式给出，见 eq. (12)。）

**直觉**：参与的套利者越多，任何人完成全部必要交易、兑现利润的概率越小，多数人可能面临亏损，且亏损随竞争加剧而扩大。

由 Nash 定理存在混合策略 Nash 均衡。

> **Proposition 3（第 14 页）**：若混合策略组合 $\Pi = (\pi_1, \ldots, \pi_k)$（$\pi_j \in (0,1)$）是该博弈的 Nash 均衡，则对所有 $j, j' \in K$ 有 $\pi_j = \pi_{j'} = \pi$（参与概率相同），且观察到的套利偏差是各市场最优价与次最优价之差的线性函数：
>
> $$A = \sum_{i=1}^{I} \Delta_i(w_i)\, \bar{P}_{i \mid n_i, k, \pi}. \tag{4}$$
>
> （证明见附录：混合策略均衡要求零期望利润 $E(U^j \mid \Pi_{-j}) = 0$；再由任意两套利者的 2×2 子博弈无差异条件推出 $\pi_j = \pi_{j'}$。）

**直觉**：风险中性套利者在混合策略均衡中要求期望收益为零（zero-profit condition），并以相同概率 $\pi$ 参与。**均衡套利偏差 $A$ 正是对 execution risk 的补偿**——它随市场非流动性（$\Delta_i$）与竞争失败概率（$\bar{P}$）上升而上升。此后因策略相同，省略上标 $j$。

### 2.7 限价单与库存成本（Limit orders and inventory cost）

（第 14–15 页）

到目前为止，对失败套利者的惩罚只包含**流动性成本（cost of liquidity）**——这与使用市价单的情形相符。套利者也可使用限价单以避免流动性成本，但此时存在**库存风险（inventory risk）**：例如三角套利中成功成交两个货币对、但第三个货币对失败，套利者留下非意愿库存并暴露于大幅价格变动的风险。拥挤交易通过流动性成本与库存成本彼此施加负外部性。均衡利润的一般形式为：

$$A = \sum_{i=1}^{I} \phi_i(w_i)\, \bar{P}_{i \mid n_i, k, \pi}, \tag{5}$$

（eq. (5)，第 15 页）其中 $\phi_i$ 为加总的流动性与库存成本（aggregate liquidity and inventory costs）。

**含义**：套利偏差 $A$（即 execution risk 的总补偿）等于组合中 $I$ 个资产各自 execution risk 补偿之和；每个分量取决于执行失败成本 $\phi_i(w_i)$ 与最优价市价单失败概率 $\bar{P}_{i \mid n_i, k, \pi}$。最优参与概率 $\pi$ 是套利偏差、资产供给广度（breadth）与套利者人数的函数。

**套利机会不立即消失**：要消除观察到的套利偏差，必须合计执行掉至少一种资产 $\{1, \ldots, I\}$ 的全部可用单位。记最小广度

$$n = \min_{i \in I} \{ n_i(w_i) \}.$$

混合策略均衡下 $\pi < 1$，套利机会立即消失的概率等于 $k$ 个套利者中至少 $n$ 个决定参与竞争的概率（二项分布）：

> **Proposition 4（第 15–16 页）**：在混合策略均衡中，套利机会立即消失的概率为
>
> $$\mathcal{P} = \sum_{s=n}^{k} \binom{k}{s} \pi^s (1 - \pi)^{k-s} < 1. \tag{6}$$

**含义**：在对套利组合所需稀缺资产的竞争下，套利机会可能在市场中存续一段时间；面对 execution risk，存在某个正概率所有套利者都决定不参与。论文未显式建模误定价持续期，但由 eq. (6) 知立即消除的概率严格小于 1。

### 2.8 与 Stein (2009) crowding effect 的关系

（第 16–17 页）

- Stein (2009) 的拥挤交易效应依赖：(i) 套利者实时不确定有多少竞争者用同一模型、持同一仓位（对 aggregate arbitrage capacity 信息不完全）；(ii) 策略无 fundamental anchor。他认为若有 fundamental anchor 且资产无限可分，每个套利者会调整需求以容纳竞争、大家分享利润，价格机制化解拥挤，无过度拥挤之虞。
- 本文指出：实践中金融资产因**最小交易单位限制而不可分割**，没有足够单位让所有套利者都获得正利润，竞争造成对不可分割资产的超额需求，不保证所有套利者都能赚钱。因此即使误定价对所有套利者直接可见（有 fundamental anchor），**竞争也会对（而非改善）市场效率造成摩擦**——这与"竞争改善价格效率"的常识相反。

### 2.9 模型的三大含义

（第 17–18 页）

1. **存在竞争时套利者面临 execution risk**：源于以有利价格建成组合的不确定性。套利者要求对 execution risk 的补偿，只有当观察到的误定价超过 eq. (2) 的期望损失时才以确定性参与（纯策略 "trade"）；否则采用 $\pi > 0$ 的混合策略，误定价可能不被立即消除（Proposition 4）。这与"套利机会因利用它有风险而存续"的有限套利文献一致，但本文风险的驱动因素是**套利竞争**而非 convergence trading。
2. **Execution risk 随竞争套利者人数增加而恶化**：以有利价格完成组合的失败概率随人数上升，亏损扩大。这类似经济学中的稀缺性问题（scarcity）——有限的可利用套利机会面对近乎无限的套利者需求。
3. **均衡中对 execution risk 的要求补偿随市场非流动性与库存成本上升**：为 Roll et al. (2007)、Deville and Riva (2007)、Akram et al. (2008)、Fong et al. (2008)、Marshall et al. (2008) 等"一价定律偏差与市场非流动性正相关"的实证证据提供了理论框架。失败成本与供求表的斜率（slope of the demand and supply schedules）及库存成本相关。

---

## 3. 数值实例与直观例子

### 3.1 引言中的两套利者竞争例子

（第 3–4 页，引言中的直觉例子）

考虑 2 个套利者竞争构建一个 long-short 套利组合：两个完全相同但被误定价的资产 A 和 B，各自在有利价格上**只有 1 个可用单位**。此时利用该套利是有风险的：其中一个套利者必然无法以有利价格买入/卖空 A 和 B，可能承担：

- **liquidity cost**：被迫沿限价订单簿向上成交（walking up the limit order book）；或
- **inventory cost**：成功买入 A 但卖空 B 失败，留下非意愿库存（unwanted inventory）。

### 3.2 三套利者竞争一个最优价单位的概率例子

（第 11 页，Assumption 4 后的说明）

若有 3 个套利者争夺最优价处的 1 个可用单位，则每个套利者成功获得该资产的概率为 $1/3$。未成功者以次优价成交。一般地，$k$ 个套利者竞争 $n_i(w_i)$ 个单位时，成功概率为 $n_i(w_i)/k$（见 2.6 节未编号公式）。

### 3.3 错过一个最优价的利润计算（脚注 16）

（第 11 页，脚注 16——论文中唯一的显式逐步计算）

设误定价为 $A = p_1^b - P^a > 0$（即卖出资产 1、买入复制组合方向），套利者 $j$ 未能在市场 $i$ 拿到最优价。若 $w_i = 1$（需要买入资产 $i$），其利润为：

$$p_1^b - \sum_{\iota=2}^{i-1} w_\iota\, p_\iota(w_\iota) - \left(p_i^a + \Delta_i^a\right) - \sum_{\iota=i+1}^{I} w_\iota\, p_\iota(w_\iota) = p_1^b - P^a - \Delta_i^a = A - \Delta_i^a$$

即：错过资产 $i$ 的最优买入价，使利润恰好减少 price slippage $\Delta_i^a$。

### 3.4 蒙特卡洛回测实例（Hypotheses 2 & 3）

（第 25–29 页，Section 5.2 后半部分——论文的核心"数值实验"）

**设定**：

- $k$ 个套利者竞争三个货币对（$I = 3$：GBP/USD、EUR/USD、EUR/GBP）的有限供给，均能看到整个 LOB（价格与数量的完全信息）。
- 套利机会出现时所有人同时观察并用市价单在三个市场同时下单；个体需求 $d = 1$ 单位（= 100 万基础货币，Reuters D3000 最小交易单位），总需求 $D = d \times k$。起始/基准货币为 GBP。
- 所有人都获利的充要条件：组合中每种货币最优价处的最小可用数量 $\ge k$。若某货币供给不足且参与概率为 1，每个套利者以概率 $P = n_1^a / k$ 拿到最优价。
- 蒙特卡洛步骤：先用 Bernoulli 分布按参与概率 $\pi$ 生成参与人数 $|S| + 1$；再以成功概率 $P = n_i^a / (|S| + 1)$ 决定套利者是否在最优价拿到货币 $i$。
- 未能完成三条腿（three legs）的套利者：以**次优价完成剩余腿**，或**以最优可得价卖出多余库存**，取损失较小者（若次优价仍给出正利润，则收益仍可为正，见脚注 26）。
- 残余头寸（residual positions，因只能以 100 万整数倍交易）按市价平仓，日终换回 GBP。
- 样本 2 年（2003-01-02 至 2004-12-30），重复蒙特卡洛 1000 次，套利者人数从 2 变化到 16；触发阈值为偏离平价 1 个基点（1 pip）；与 Akram et al. (2008) 一致，限价单间隔超过 2 分钟的套利机会不予考虑；存续超过 1 秒的机会只在第一次违反平价条件时利用一次。所有买卖分别按 ask、bid 价成交，利润已扣除 bid-ask spread。作者强调该设定给了套利者**最好的可能情形**。

**结果（Table 6，第 40 页；单位：百万 GBP，括号内为标准差；1000 次模拟）**：

| 套利者数 $k$ | $\pi{=}0.1$ | $\pi{=}0.2$ | $\pi{=}0.3$ | $\pi{=}0.4$ | $\pi{=}0.5$ | $\pi{=}0.6$ | $\pi{=}0.7$ | $\pi{=}0.8$ | $\pi{=}0.9$ | $\pi{=}1.0$ |
|---|---|---|---|---|---|---|---|---|---|---|
| 2 | 0.323 (0.0566) | 0.619 (0.0750) | 0.881 (0.0850) | 1.120 (0.0932) | 1.328 (0.0934) | 1.498 (0.0924) | 1.647 (0.0879) | 1.763 (0.0774) | 1.846 (0.0582) | **1.902** (0.0224) |
| 4 | 0.295 (0.0556) | 0.496 (0.0761) | 0.598 (0.0841) | 0.598 (0.0928) | 0.490 (0.0964) | 0.271 (0.0926) | −0.064 (0.0938) | −0.523 (0.0838) | −1.109 (0.0733) | **−1.828** (0.0561) |
| 6 | 0.263 (0.0546) | 0.364 (0.0747) | 0.284 (0.0875) | 0.015 (0.0951) | −0.448 (0.0983) | −1.128 (0.1007) | −2.023 (0.1003) | −3.140 (0.0984) | −4.495 (0.0960) | **−6.108** (0.0922) |
| 8 | 0.229 (0.0559) | 0.223 (0.0745) | −0.049 (0.0876) | −0.610 (0.0969) | −1.472 (0.1037) | −2.657 (0.1070) | −4.178 (0.1144) | −6.046 (0.1175) | −8.271 (0.1258) | **−10.85** (0.1278) |
| 10 | 0.197 (0.0563) | 0.077 (0.0755) | −0.401 (0.0903) | −1.271 (0.1007) | −2.558 (0.1103) | −4.288 (0.1186) | −6.477 (0.1281) | −9.125 (0.1406) | −12.25 (0.1551) | **−15.86** (0.1706) |
| 12 | 0.164 (0.2530) | −0.102 (0.3378) | −0.865 (0.3856) | −2.168 (0.4135) | −4.055 (0.4248) | −6.549 (0.4215) | −9.675 (0.4020) | −13.44 (0.3677) | −17.89 (0.3127) | **−23.04** (0.2282) |
| 14 | 0.127 (0.0567) | −0.231 (0.0774) | −1.154 (0.0948) | −2.693 (0.1105) | −4.892 (0.1289) | −7.781 (0.1459) | −11.38 (0.1695) | −15.71 (0.1973) | −20.80 (0.2263) | **−26.64** (0.2568) |
| 16 | 0.091 (0.0564) | −0.394 (0.0782) | −1.551 (0.0973) | −3.446 (0.1163) | −6.123 (0.1374) | −9.619 (0.1650) | −13.96 (0.1917) | −19.16 (0.2238) | −25.23 (0.2601) | **−32.14** (0.2978) |

**要点**（第 27–29 页）：

- 仅 2 个参与者时利润为正，且 $\pi = 1$ 时利润最高：两年样本期平均利润约 **200 万 GBP**。
- 4 个参与者且 $\pi = 1$ 时已转为亏损：平均亏损约 **180 万 GBP**（与 2 人时的 +200 万形成鲜明对比）。
- 每个货币对平均 breadth 约 300 万（Table 1），8 个套利者各需求 100 万 ⇒ 明显超额需求。
- $\pi = 1$（纯策略）时亏损随 $k$ 单调上升，$k = 16$ 时亏损高达 **3214 万 GBP**（Figure 4 也展示该单调关系）。
- 但参与概率低于 20% 的混合策略（$\pi < 0.2$）在较多人数下仍保持正利润——**混合策略（$\pi < 1$）在某些情形下优于纯策略（$\pi = 1$）**。

**回归检验（eq. (9)，第 29 页；Table 7，第 41 页）**：

$$PL = x_0 + x_1 \times k \tag{9}$$

其中 $PL$ 为回测中 $k$ 个套利者的平均损益（百万 GBP）。Table 7 给出各参与概率下的 $x_1$ 估计（括号内为 t 统计量，第三行为 $R^2$）：

| $\pi$ | 0.1 | 0.2 | 0.3 | 0.4 | 0.5 | 0.6 | 0.7 | 0.8 | 0.9 | 1.0 |
|---|---|---|---|---|---|---|---|---|---|---|
| $x_1$ | −0.01664 | −0.0731 | −0.1762 | −0.3312 | −0.5416 | −0.8094 | −1.1378 | −1.5267 | −1.9773 | −2.4892 |
| (t) | (−73.63) | (−54.23) | (−42.74) | (−39.03) | (−36.47) | (−35.06) | (−34.37) | (−33.91) | (−33.63) | (−33.42) |
| $R^2$ | 99.89% | 99.80% | 99.67% | 99.61% | 99.55% | 99.51% | 99.49% | 99.48% | 99.47% | 99.47% |

所有参数高度显著：平均损益与竞争套利者人数之间存在**强负相关**，且该关系对所有参与概率成立。$\pi = 1$ 时每增加一个套利者，两年期平均利润减少约 249 万 GBP。

**隐含参与概率与隐含消除概率（Figures 2–3，第 28、35–36 页）**：利用 LOB 流动性变量的时间序列，把观察到的簇内平均偏差 $A$、平均价差 $\Delta_i(w_i)$、平均广度 $n_i(w_i)$ 代入均衡条件 $A = \sum_{i=1}^I \Delta_i(w_i) \bar{P}_{i \mid n_i, k, \pi}$ 解出隐含参与概率 $\pi$（$\bar{P}$ 按 eq. (12) 计算），再代入 eq. (6) 得到隐含套利消除概率。结果：参与概率经常低于 1，且随竞争者增多而下降；消除概率在许多情形下低于 1，这些情形与套利资产的超额需求和非流动性时期相吻合。

---

## 4. 实证部分

### 4.1 外汇市场三角套利背景与无套利条件

（Section 3，第 18–19 页）

三角套利（triangular arbitrage）涉及同一汇率的两个价格：直接价格与经由第三种货币的间接价格（vis-a-vis）。记 $S(A/B)$ 为每单位货币 B 兑换货币 A 的单位数（即"A/B 的汇率"，数值表示 1 单位 B 值多少单位 A）。考虑交易成本后，三角无套利条件为：

$$S\left(A/B\ ask\right) \ge S\left(C/B\ bid\right) \times S\left(A/C\ bid\right), \tag{7}$$

$$S\left(B/A\ ask\right) \ge S\left(C/A\ bid\right) \times S\left(B/C\ bid\right). \tag{8}$$

（eq. (7)–(8)，第 19 页）条件含义：买入的东西不能立即以更高价格卖出（脚注 18）。任何对式 (7) 或 (8) 的偏离都代表教科书式的无风险套利机会。

**选择三角套利的理由**：它是**非 convergence trading** 的套利，且不受税收、监管、卖空约束影响，从而能把 execution risk 与既有套利障碍隔离开来（第 4、18 页）。样本期选 2003–2004 以控制 settlement risk（结算风险）：CLS Bank 于 2002 年投入运营，结算美元、欧元、日元、英镑、瑞郎、加元、澳元七种货币，大幅降低结算风险（第 20 页，脚注 19）。

**相关文献**（第 19 页）：Aiba et al. (2002) 用 1999-01-25 至 1999-03-12 的 yen-dollar、dollar-euro、yen-euro 交易数据发现每天约 90 分钟可利用的套利机会；Marshall et al. (2008) 用 EBS 一年可成交报价数据发现可利用机会，并认为这些是补偿套利者缓解做市商订单不平衡的"桌上之钱"；Lyons and Moore (2009) 假设套利者利用三角偏差时考虑自身交易的价格冲击——本文模型与发现支持该假设。

### 4.2 数据

（Section 4，第 19–21 页）

- **数据源**：Reuters Dealing 3000（D3000）交易系统逐笔（tick-by-tick）数据；三个货币对：EUR/USD、GBP/USD、EUR/GBP。
- **样本期**：2003 年 1 月 2 日至 2004 年 12 月 30 日，剔除周末与假日。
- 据 BIS (2004) 估计，这些货币占 FX 现货交易量的 60%，其中 53% 为交易商间交易（interdealer trades）。
- **数据优势**：含所有限价单的数量，无需任何 ad-hoc 假设即可重建完整 LOB。每笔限价单报告：货币对、唯一订单标识、价格、订单数量、隐藏数量（D3000 功能）、成交数量、订单类型、进入/撤消订单的交易标识、市价单状态、订单进入类型、撤消原因、订单进入与撤消时间。**时间戳精确到千分之一秒**。D3000 最小交易单位为基础货币 100 万单位。
- **套利簇（cluster）定义**：至少一个连续的三角套利偏差；簇的持续时间为偏差出现后汇率回到无套利值所需的时间。样本中共有 **40,166 个套利簇**。
- **往返套利机会（round-trip）识别流程**（第 21 页）：
  1. 记录三个货币对 LOB 的最新最优 bid/ask 价；
  2. 买入货币对 1（如 GBP/USD）并买入货币对 2（如 EUR/GBP）——等价于卖出 USD 换 GBP、再用 GBP 买 EUR，净头寸为 short USD / long EUR；与货币对 3（如 EUR/USD）的汇率比较：若其更低则买入货币对 3 获利；若更高则反向操作（卖出货币对 1、卖出货币对 2、卖出货币对 3）并检验是否有正利润。所有买入按 ask 价、卖出按 bid 价。

### 4.3 描述性统计（Tables 1–2）

**Table 1（第 38 页）：流动性统计**（样本期 2003-01-02 至 2004-12-30）：

| 指标 | EUR/GBP | EUR/USD | GBP/USD |
|---|---|---|---|
| 限价单平均间隔（秒） | 1.31 | 1.05 | 1.71 |
| 平均 bid-ask spread（pips） | 1.03 | 2.13 | 2.07 |
| 需求表平均斜率（bp/十亿） | 31.37 | 85.73 | 68.37 |
| 供给表平均斜率（bp/十亿） | 36.41 | 99.07 | 74.60 |
| 需求表平均深度（百万） | 41.29 | 29.46 | 45.08 |
| 供给表平均深度（百万） | 49.68 | 32.80 | 48.78 |
| 需求表平均广度 breadth（百万） | 3.33 | 2.79 | 2.72 |
| 供给表平均广度 breadth（百万） | 3.25 | 2.88 | 2.78 |
| 最优价−次优价：需求表（pips） | 1.12 | 2.82 | 3.11 |
| 最优价−次优价：供给表（pips） | 1.28 | 3.40 | 2.86 |

要点：限价单到达频率（每 1.05–1.71 秒一单）远快于 Engle and Russell (1998)、Bollerslev and Domowitz (1993) 报告的 15–20 秒报价间隔；spread 很窄（Tham 2009 指出 D3000 是非常紧的市场）；斜率用最优两档 bid/ask 价及其深度构造（脚注 22）。1 pip：EUR/USD、GBP/USD 为 0.0001；GBP/EUR 显示到小数点后 5 位（第 5 位只能是 0 或 5，用于半 pip 显示）（脚注 21）。

**Table 2（第 38 页）：套利偏差统计**：

Panel A：

| 指标 | Mean | Std. Dev | Min | Q1 | Median | Q3 | Max |
|---|---|---|---|---|---|---|---|
| 簇内平均套利偏差（pips） | 1.56 | 1.92 | 1 | 1.15 | 1.35 | 1.65 | 94.2 |
| 套利持续时间（秒） | 0.77 | 1.54 | 0.01 | 0.04 | 0.35 | 1.01 | 96.5 |
| 簇内套利机会数 | 4.36 | 4.96 | 1 | 1 | 3 | 6 | 88 |

Panel B：t-stat（平均偏差 = 0）= 162.84；t-stat（持续时间 = 0）= 100.21；可盈利簇数 = 40,166。

要点：簇内平均套利利润约 1.56 pips（已扣除 bid-ask spread 与 0.2 pip 交易费，脚注 23），统计显著；平均持续 0.77 秒、标准差 1.54 秒，均值与标准差差异大表明持续时间不服从指数分布——存在持续时间系统性偏高的市场状态（如低流动性时期）。Reuters 系统获取成本对银行交易商而言是沉没成本（脚注 23）。

### 4.4 Hypothesis 1：套利机会不被瞬间消除（Tables 3–5）

（第 23–25 页）

- 初步检验：Table 2 Panel B 的 t 统计量拒绝"持续时间为零"的原假设。但"零持续"原假设本身不现实——有效市场中套利机会通常被**下一个**进入的订单消除，而这往往超过零点几秒。
- 因此将套利簇分两组检验：
  - **Textbook arbitrage（教科书套利）**：簇中只有一次可盈利偏差，被下一个进入的订单（市价单、限价单或撤单）消除；
  - **Risky arbitrage（风险套利）**：其余簇——市场参与者需"斟酌"是否参与利用观察到的机会。

**Table 3（第 39 页）：套利簇持续时间**：

| 指标 | Textbook Arbitrage | Risky Arbitrage |
|---|---|---|
| Mean（秒） | 0.12 | 1.15 |
| Standard deviation | 0.36 | 1.82 |
| Median | 0.03 | 0.77 |
| Number of clusters | 15,001 | 25,165 |

t-stat（textbook 持续 = risky 持续）= **86.97**——拒绝"三角套利被瞬间消除"的假设。

**Data latency（数据延迟）检验**（第 24–25 页）：数据延迟定义为市价单进入系统到成交的时间差。

**Table 4（第 39 页）：延迟描述统计（秒）**：

| 汇率 | Mean | Std. Dev | Min | Q1 | Median | Q3 | Max |
|---|---|---|---|---|---|---|---|
| **2003 年** | | | | | | | |
| EUR/USD | 0.037 | 0.031 | 0.020 | 0.030 | 0.030 | 0.040 | 2.790 |
| GBP/USD | 0.034 | 0.028 | 0.020 | 0.030 | 0.030 | 0.040 | 2.520 |
| EUR/GBP | 0.035 | 0.028 | 0.010 | 0.030 | 0.030 | 0.040 | 2.980 |
| **2004 年** | | | | | | | |
| EUR/USD | 0.033 | 0.036 | 0.010 | 0.020 | 0.030 | 0.040 | 2.400 |
| GBP/USD | 0.031 | 0.034 | 0.010 | 0.020 | 0.030 | 0.030 | 2.800 |
| EUR/GBP | 0.032 | 0.036 | 0.010 | 0.020 | 0.030 | 0.030 | 2.310 |

平均延迟 2003 年为 34–37 毫秒、2004 年降为 31–33 毫秒（反映交易平台改进）。**延迟成本度量**：取套利出现到 37 毫秒后（样本中各货币对最大平均延迟，脚注 24）的偏差变化均值，作为套利者因数据延迟的平均损失估计。若套利机会仅由延迟造成，延迟成本应 ≥ 平均偏差，或簇平均持续时间应 < 34 毫秒。

**Table 5（第 40 页）：延迟成本（垄断套利者、无竞争情形）**：

| 指标 | Without Latency | With Latency |
|---|---|---|
| Total Profit（GBP） | 6,265,896.07 | 2,438,758.95 |
| Mean（pips） | 1.56 | 0.63 |
| Standard deviation | 1.92 | 2.07 |
| t-stat（平均利润 = 0） | 162.8 | 60.9 |

簇数 40,166；t-stat（无延迟利润 = 有延迟利润）= 66.0。控制延迟成本后总利润下降，但**剩余利润仍显著为正**——拒绝"三角套利的存在仅由数据延迟造成"的假设。**Hypothesis 1 成立**：即使扣除 bid-ask spread 与延迟成本，套利机会也未被立即消除。

### 4.5 Hypotheses 2 & 3：竞争导致亏损且亏损随人数增加（Tables 6–7）

（第 25–29 页）蒙特卡洛回测设定与结果已完整收录于上文第 3.4 节。结论：

- **Hypothesis 2 成立**：4 个及以上竞争套利者（$\pi = 1$）时套利者平均亏损（如 $k=4$ 时 −1.828 百万 GBP）；
- **Hypothesis 3 成立**：$\pi = 1$ 时亏损随 $k$ 单调增加（$k=16$ 时 −32.14 百万 GBP）；回归 $PL = x_0 + x_1 k$（eq. (9)）中 $x_1$ 对所有 $\pi$ 均显著为负，$R^2$ 超过 99%。
- 作者指出：现实中存在数百个竞争套利者且算法交易日益普及，execution risk 使套利可能成为风险很大的业务。

### 4.6 Hypothesis 4：偏差与 illiquidity、inventory cost 的关系（Tables 8–9）

（第 29–32 页）

**非流动性度量**（两个代理变量）：

1. $\Delta_i$：套利簇内最优价与次优价之差的平均值；
2. $\lambda_i$：LOB 相应一侧（买或卖，取决于套利所需交易方向）供求表斜率的簇内平均，即（最优价 − 次优价）÷ 最优价处数量。

**库存成本度量（inventory cost, $IC_i$）**：假设套利者三条腿中只以最优报价完成两条腿（系统性"错过"某一汇率），在 10、60、120 秒后以最便宜方式清掉非意愿库存；用该损益分布的**标准差**度量库存持有成本（可视为套利结束后 10/60/120 秒的价格风险）。回归中使用按小时计算的该标准差（持有 120 秒情形）。

**Table 8（第 41 页）：持有库存的损益描述统计（基点）**：

| 汇率（被错过的腿） | Mean | Std. Dev | Min | Q1 | Median | Q3 | Max |
|---|---|---|---|---|---|---|---|
| **持有 10 秒** | | | | | | | |
| EUR/USD | 0.7125 | 3.4422 | −53.739 | −0.7981 | 0.4427 | 1.8798 | 145.09 |
| GBP/USD | 0.3775 | 2.1080 | −43.945 | −0.5661 | 0.4226 | 1.2503 | 74.999 |
| EUR/GBP | 0.0851 | 1.9993 | −64.695 | −0.8114 | 0.0870 | 1.0798 | 42.674 |
| **持有 60 秒** | | | | | | | |
| EUR/USD | 0.5574 | **4.3750** | −90.865 | −1.4003 | 0.3693 | 2.1944 | 106.95 |
| GBP/USD | 0.3638 | 3.3881 | −72.001 | −1.2152 | 0.4030 | 1.8631 | 89.732 |
| EUR/GBP | −0.0099 | 2.8333 | −26.951 | −1.3874 | 0.0000 | 1.3418 | 50.164 |
| **持有 120 秒** | | | | | | | |
| EUR/USD | 0.4590 | 5.1570 | −121.18 | −1.9191 | 0.3013 | 2.6516 | 62.597 |
| GBP/USD | 0.3718 | 4.2768 | −45.866 | −1.6734 | 0.3903 | 2.3585 | 104.34 |
| EUR/GBP | −0.0400 | 3.4924 | −33.323 | −1.8139 | 0.0000 | 1.7620 | 43.271 |

要点：均值有正有负（持有库存可能赚也可能赔），但**标准差在经济意义上很大**——如系统性错过 EUR/USD 并 60 秒后清仓的损益标准差为 4.3750 个基点；持有越久标准差越大。execution risk 与库存成本是消除套利机会的关键摩擦。

**回归模型（eq. (10)，第 31 页）**：

$$A = a_0 + a_1 \times illiq_{GBP/USD}(w_{GBP/USD}) + a_2 \times illiq_{EUR/USD}(w_{EUR/USD}) + a_3 \times illiq_{EUR/GBP}(w_{EUR/GBP})$$
$$\quad +\, b_1 \times IC_{GBP/USD}(w_{GBP/USD}) + b_2 \times IC_{EUR/USD}(w_{EUR/USD}) + b_3 \times IC_{EUR/GBP}(w_{EUR/GBP}) \tag{10}$$
$$\quad +\, c_1 \times Tr.Vol + c_2 \times TED$$

其中：

- $A$：簇内平均套利偏差（仅用偏差 > 1 bp 的簇，共 40,166 个簇）；
- $illiq_i$：市场 $i$ 的非流动性度量（$\Delta_i$ 或 $\lambda_i$）；
- $IC_i$：市场 $i$ 的库存成本度量；
- $Tr.Vol$：相应套利簇三个市场的小时合计交易量 ÷ 当日总交易量——作为**竞争套利者人数的代理**（因套利活动与交易量正相关，Cornelli and Li 2002）；
- $TED$：TED spread（伦敦 Eurodollar 存款利率相对美国国债的溢价），控制 counterparty risk（对手方风险）；TED 走阔反映银行间贷款违约风险上升；
- $a_0$：可解释为 Grossman and Stiglitz (1976, 1980) 意义上对监控市场的补偿/对 execution risk 不确定性支付的风险溢价；
- 估计方法：**GMM**（Hansen 1982），Newey-West 修正自相关与异方差。

**Table 9（第 42 页）：回归结果**（括号内为经自相关调整的 t 统计量；五行分别为五个设定）：

设定 1（只用 $\Delta$ 度量 illiquidity）：

| $\Delta_{GBP/USD}$ | $\Delta_{EUR/USD}$ | $\Delta_{EUR/GBP}$ | $Tr.Vol$ | $TED$ | Adj. $R^2$ |
|---|---|---|---|---|---|
| 0.1837 (90.2) | 0.0521 (41.3) | 0.0044 (3.00) | 1.5964 (6.91) | −0.000029 (−2.30) | 24.52 |

设定 2（只用 $\lambda$ 度量 illiquidity）：

| $\lambda_{GBP/USD}$ | $\lambda_{EUR/USD}$ | $\lambda_{EUR/GBP}$ | $Tr.Vol$ | $TED$ | Adj. $R^2$ |
|---|---|---|---|---|---|
| 0.2904 (41.6) | 0.1321 (33.4) | 0.0912 (6.19) | 1.3579 (5.13) | −0.000010 (−0.71) | 8.79 |

设定 3（只用 $IC$）：

| $IC_{GBP/USD}$ | $IC_{EUR/USD}$ | $IC_{EUR/GBP}$ | $Tr.Vol$ | $TED$ | Adj. $R^2$ |
|---|---|---|---|---|---|
| 0.0132 (3.75) | 0.0239 (8.46) | 0.0266 (6.51) | 1.0298 (3.87) | −0.000031 (−2.16) | 1.97 |

设定 4（$\Delta$ + $IC$）：

| $\Delta_{GBP/USD}$ | $\Delta_{EUR/USD}$ | $\Delta_{EUR/GBP}$ | $IC_{GBP/USD}$ | $IC_{EUR/USD}$ | $IC_{EUR/GBP}$ | $Tr.Vol$ | $TED$ | Adj. $R^2$ |
|---|---|---|---|---|---|---|---|---|
| 0.1734 (88.9) | 0.0512 (40.2) | 0.0043 (2.94) | 0.0015 (0.48) | 0.0017 (0.69) | 0.0139 (3.87) | 1.5261 (6.67) | −0.000028 (−2.15) | 24.59 |

设定 5（$\lambda$ + $IC$）：

| $\lambda_{GBP/USD}$ | $\lambda_{EUR/USD}$ | $\lambda_{EUR/GBP}$ | $IC_{GBP/USD}$ | $IC_{EUR/USD}$ | $IC_{EUR/GBP}$ | $Tr.Vol$ | $TED$ | Adj. $R^2$ |
|---|---|---|---|---|---|---|---|---|
| 0.2896 (39.6) | 0.1184 (31.3) | 0.0753 (5.13) | 0.0097 (2.84) | 0.0134 (4.93) | 0.0187 (4.77) | 1.0732 (4.12) | −0.000014 (−1.01) | 9.52 |

**主要发现**（第 31–32 页）：

- 非流动性度量（$\Delta_i$ 或 $\lambda_i$）系数均为正且高度显著（p < 0.0001）：**套利偏差随市场非流动性增加而扩大**——与 Roll et al. (2007)"市场越流动、一价定律偏差越小"一致，但本文指出其经济原因是 execution risk：交易的价格冲击随非流动性上升，拥挤交易导致的执行失败成本也随之上升。
- 库存成本 $IC_i$ 与套利偏差存在**正且统计显著**的关系；加入 $IC$ 与控制变量后 illiquidity 仍显著。
- $Tr.Vol$（套利者竞争代理）系数为正且显著：竞争者越多、潜在亏损越大，均衡中要求的 execution risk 补偿（即偏差 $A$）越高——与 Hypothesis 2 互为稳健性验证。
- TED spread 在所有回归中要么统计不显著、要么经济上可忽略：对手方风险作用很小，可能因 CLS 结算降低了结算失败与对手方风险。
- **Hypothesis 4 成立**。

---

## 5. 结论与实践启示

（Section 6，第 33 页）

- 论文提出新的套利障碍——**execution risk**：与行为交易者存在与否、误定价收敛不确定性均无关，而源于套利者对**不可分割**套利资产的竞争（crowding effect），套利者彼此施加负外部性（illiquidity cost 与 inventory holding cost）。结果是价格可能显著且长期偏离有效水平。
- 实证支持四个假说：扣除交易成本与延迟成本后套利机会仍不被立即消除；竞争套利者使套利亏损且亏损随人数扩大；负外部性具体表现为流动性成本与库存成本。
- **对实践的启示**：
  1. 教科书式"无风险"套利（三角套利、covered interest parity、put-call parity、cross-listed parity 等）的实际利用并非无风险；高频套利策略需把执行失败概率、订单簿深度/广度、库存处置成本纳入预期收益计算；
  2. 套利者人数并非越多越好——超过资产最优价供给后，拥挤反而制造亏损并阻碍误定价消除（与"竞争改善效率"的常识相反）；
  3. 对监管者：高度相关的算法交易（Chaboud et al. 2010）与危机中的拥挤平仓（Khandani and Lo 2007，2007 年 8 月量化基金事件）都涉及类似的 crowding 机制；
  4. 对风险管理者：库存敞口损益的标准差（Table 8 的 $IC$ 度量）可作为"套利失败尾部风险"的实用度量。

---

## 附录：关键证明思路

（论文 Appendix，第 43–46 页）

**Proposition 1 证明（归纳法）**：

- 基例 $I = 2$：两个市场独立。套利者两市场都赢（概率 $P_1^j P_2^j$，赚 $A$）；只输市场 1（概率 $(1-P_1^j)P_2^j$，赚 $A - \Delta_1(w_1)$）；只输市场 2（概率 $P_1^j(1-P_2^j)$，赚 $A - \Delta_2(w_2)$）；都输（概率 $(1-P_1^j)(1-P_2^j)$，赚 $A - \Delta_1(w_1) - \Delta_2(w_2)$）。加总化简即得 $E(U^j) = A - \Delta_1(w_1)(1-P_1^j) - \Delta_2(w_2)(1-P_2^j)$。
- 归纳步：假设对 $I-1$ 个市场成立。对 $I$ 个市场，套利者恰在子集 $J \subseteq \mathcal{I}$ 的市场失败、在 $\mathcal{I} \setminus J$ 成功的事件概率为 $\prod_{i \in J} \bar{P}_i^j \cdot \prod_{\iota \in \mathcal{I} \setminus J} P_\iota^j$，相应收益为 $A - \sum_{i \in J} \Delta_i(w_i)$。期望收益为所有 $J \in 2^{\mathcal{I}}$ 的加权和：

$$E(U^j) = \sum_{J \in 2^{\mathcal{I}}} \left(A - \sum_{i \in J} \Delta_i(w_i)\right) \cdot \prod_{i \in J} \left(1 - P_i^j\right) \cdot \prod_{\iota \in \mathcal{I} \setminus J} P_\iota^j = A - \sum_{J \in 2^{\mathcal{I}}} \left(\sum_{i \in J} \Delta_i(w_i)\right) \cdot \prod_{i \in J} \left(1 - P_i^j\right) \cdot \prod_{\iota \in \mathcal{I} \setminus J} P_\iota^j \tag{11}$$

（eq. (11)，第 44 页）其中用到恒等式 $\sum_{J \in 2^{\mathcal{I}}} \prod_{i \in J}(1-P_i^j) \prod_{\iota \in \mathcal{I}\setminus J} P_\iota^j = \prod_{i=1}^I \left((1-P_i^j) + P_i^j\right) = 1$。再把最后一个和式按是否含 $\Delta_I(w_I)$ 分解，利用归纳假设，即得 $E(U^j) = A - \sum_{i=1}^I \Delta_i(w_i)(1-P_i^j)$。Q.E.D.

**Proposition 2 证明**：设 $2^{K_{-j}}$ 为 $j$ 的对手集合的幂集，事件 $X_S$（$S \in 2^{K_{-j}}$）表示 $S$ 中对手确定参与、其余不参与，其概率为

$$\mathcal{P}(X_S) = \prod_{s \in S} \pi_s \prod_{s \in K_{-j} \setminus S} (1 - \pi_s).$$

给定 $X_S$ 时 $j$ 在市场 $i$ 失败的概率为

$$\bar{P}_{i \mid n_i,\, |X_S|}^j = \begin{cases} 0, & |S| \le n_i(w_i) - 1 \\ 1 - \dfrac{n_i(w_i)}{|S| + 1}, & |S| > n_i(w_i) - 1 \end{cases}$$

由全概率公式：

$$\bar{P}_{i \mid n_i, k, \Pi_{-j}}^j = \sum_{S \in 2^{K_{-j}}} \prod_{s \in S} \pi_s \prod_{s \in K_{-j} \setminus S} (1 - \pi_s)\; \bar{P}_{i \mid n_i,\, |S|}^j \tag{12}$$

（eq. (12)，第 45 页）加入第 $k+1$ 个套利者后，把和式按 $S$ 是否包含她分解，可证 $\bar{P}_{i \mid n_i, k+1, \Pi'_{-j}}^j > \bar{P}_{i \mid n_i, k, \Pi_{-j}}^j$（严格不等号由 Assumption 3 保证），即 (i) 成立；(ii) 由 (i) 与 eq. (3) 直接推出。

**Proposition 3 证明**：混合策略均衡中若 $E(U^j \mid \Pi_{-j}) > 0$，套利者 $j$ 可提高 $\pi_j$ 获利，矛盾；若 $< 0$ 则 $\pi_j = 0$ 更优。故均衡中零利润条件 $E(U^j \mid \Pi_{-j}) = 0$ 成立。再取任意两套利者 $j, j'$ 的 2×2 子博弈：记 $y$ 为一方"trade"另一方"not trade"时 trade 方的收益，$z$ 为双方都 "trade" 时各自收益；由 Proposition 2 有 $z < y$。均衡无差异条件 $\pi_j z + (1-\pi_j) y = 0$ 与 $\pi_{j'} z + (1-\pi_{j'}) y = 0$ 给出 $(\pi_j - \pi_{j'})(z - y) = 0$，因 $z \ne y$ 得 $\pi_j = \pi_{j'}$。结合 Proposition 1 与零利润条件即得 $A = \sum_{i=1}^I \Delta_i(w_i) \bar{P}_{i \mid n_i, k, \pi}$。Q.E.D.

> 注：PDF 提取文本中 Proposition 3 证明末尾写 "$z - y > 0$"，按上下文（$z < y$）应为 $z - y \neq 0$；此处按逻辑还原。
