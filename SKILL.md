---
name: limits-to-arbitrage
description: 套利限制（Limits to Arbitrage）理论与模型知识库，基于两篇经典论文的完整精读笔记：Shleifer & Vishny (1997, Journal of Finance) "The Limits of Arbitrage"（代理模型/PBA、noise trader risk、业绩赎回导致套利在极端情形失效）与 Kozhan & Tham "How Riskless is 'Riskless' Arbitrage?"（execution risk、crowding effect、illiquidity/inventory 负外部性、外汇三角套利实证）。包含全部公式（LaTeX）、Propositions、数值实例与实证结果。当用户讨论套利限制、有限套利、套利为什么不消除误定价、代理问题与套利、execution risk、拥挤交易、三角套利、对冲基金赎回、noise trader、市场效率异象，或需要引用这两篇论文的公式/例子/结论时使用。Use when the user asks about limits to arbitrage, execution risk in arbitrage, performance-based arbitrage (PBA), crowded trades, or the Shleifer-Vishny / Kozhan-Tham models and formulas.
---

# Limits to Arbitrage（套利限制）理论技能

基于两篇论文的完整精读笔记，回答"为什么套利不能消除误定价"的两大互补机制。

## 两篇论文与核心机制

1. **Shleifer & Vishny (1997), "The Limits of Arbitrage", Journal of Finance**
   - 机制：**agency model of arbitrage + Performance-Based Arbitrage (PBA)**。套利者用外部投资者的钱套利；投资者按过去业绩增资/赎回 → 噪声冲击加深、套利者亏损时遭赎回 → 恰在误定价最严重、期望回报最高时被迫清算离场（bail out when opportunities are best）→ 价格对冲击超比例反应（dp₂/dS < −1），市场在极端情形丧失韧性（resiliency）。
   - 解释：套利资源为何集中于债券/外汇等低 idiosyncratic 风险市场；value/glamour 等异象为何长期存在。

2. **Kozhan & Tham, "How Riskless is 'Riskless' Arbitrage?"**
   - 机制：**execution risk in arbitrage exploitation**。即使存在完美替代品与可转换性、无需 convergence trading，由于资产不可分割（最小交易单位）+ 多个套利者竞争最优价位上的稀缺供给（crowding effect），失败者承担 **illiquidity cost**（滑点）或 **inventory holding cost**（未建完组合的非意愿库存）→ "无风险"套利也是受限的。
   - 实证：Reuters D3000 外汇三角套利（EUR/USD、GBP/USD、EUR/GBP），40,166 个套利簇，四个假说全部验证；竞争者 ≥4 人时回测亏损。

两篇论文互补：前者讲"资金端"约束（资本与赎回），后者讲"执行端"约束（竞争与流动性），共同构成 limits to arbitrage 的完整图景。

## 参考文档导航（按需读取，勿一次全部加载）

- **[references/shleifer-vishny-limits-of-arbitrage.md](references/shleifer-vishny-limits-of-arbitrage.md)**（约 490 行）
  完整还原模型全部公式 eq. (1)–(8)、4 个 Propositions（含中文陈述+直觉）、LIFFE/DTB Bund 期货等 7 个具体实例（含全部数字）、公式速查表、命题速查表、公式还原的数值验证记录。
  需要：PBA 模型公式、均衡价格 p₂、极端情形分析、经验含义、论文具体数字时读取。

- **[references/kozhan-tham-execution-risk.md](references/kozhan-tham-execution-risk.md)**（约 580 行）
  完整模型设定（限价订单簿、套利偏差）、公式 eq. (1)–(12)、4 个 Assumptions、4 个 Propositions（含证明思路）、数值实例与蒙特卡洛回测、全部 9 张实证表格（Tables 1–9）结果。
  需要：execution risk 模型、混合策略 Nash 均衡、crowding 外部性、外汇三角套利实证细节时读取。

## 使用指引

- **回答概念性问题**（什么是 PBA / execution risk / noise trader risk / crowding effect）：直接用上述核心机制回答，必要时读对应参考文档的相关章节核实细节。
- **需要公式或命题**：参考文档中所有公式均为 LaTeX，标注论文编号与页码，可直接引用；引用时注明论文出处。
- **需要具体例子/数字**：两篇参考文档均含实例汇总（Shleifer-Vishny 见"第 8 节 实例与数字汇总"；Kozhan-Tham 见"第 3 节 数值实例"与"第 4 节 实证部分"），优先使用其中的数字而非凭记忆。
- **做建模、回测或策略分析**：以参考文档中的模型设定为起点（如 Kozhan-Tham 的套利偏差条件 A = ΣΔ·P̄、零利润均衡；Shleifer-Vishny 的资金-业绩敏感性），保持原论文符号以便对照。
- 参考文档中标注 [原公式见论文第N页] 之处表示 PDF 提取不确定，引用前建议核对原 PDF（`/Users/zhang/Desktop/套利/Kozhan_Tham.pdf`、`/Users/zhang/Desktop/套利/Limits to arbitrage.pdf`）。
