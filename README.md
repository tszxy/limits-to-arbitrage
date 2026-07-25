# limits-to-arbitrage

套利限制（Limits to Arbitrage）理论知识库 Skill，基于两篇经典论文的完整精读笔记：

- **Shleifer & Vishny (1997)**, *The Limits of Arbitrage*, Journal of Finance —— agency model / Performance-Based Arbitrage (PBA)：套利者按过去业绩获得资金，误定价最严重时反而遭赎回离场。
- **Kozhan & Tham**, *How Riskless is "Riskless" Arbitrage?* —— execution risk：crowding effect 下竞争套利者彼此施加 illiquidity cost 与 inventory holding cost 负外部性，"无风险"套利也是受限的。

包含全部公式（LaTeX，标注论文编号与页码）、4+4 个 Propositions、数值实例与实证结果。

## 目录结构

```
limits-to-arbitrage/
├── SKILL.md                                        ← 技能入口（触发描述 + 机制总览 + 导航）
└── references/
    ├── shleifer-vishny-limits-of-arbitrage.md      ← eq.(1)–(8)、4 Propositions、7 个实例、速查表
    └── kozhan-tham-execution-risk.md               ← eq.(1)–(12)、4 Propositions、蒙特卡洛回测、9 张实证表
```

## 安装

**Kimi / Kimi Code CLI**：将本文件夹复制到 `~/.config/agents/skills/` 或 `~/.kimi/skills/`

**Claude Code**：复制到 `~/.claude/skills/`（或项目内 `.claude/skills/`）

**其他模型**：`SKILL.md` 与 `references/*.md` 均为自包含 Markdown，可直接放入 system prompt / context / RAG 语料使用。

## 使用

安装后无需手动调用——讨论套利限制、execution risk、PBA、拥挤交易、三角套利等话题时自动触发；也可显式点名"用 limits-to-arbitrage 框架分析……"。
