# limits-to-arbitrage Skill — 各 Agent 平台接入配置

> 仓库：https://github.com/tszxy/limits-to-arbitrage
> 三个知识文件（raw 地址，可直接抓取）：
> - 入口导航：`https://raw.githubusercontent.com/tszxy/limits-to-arbitrage/main/SKILL.md`
> - Shleifer-Vishny：`https://raw.githubusercontent.com/tszxy/limits-to-arbitrage/main/references/shleifer-vishny-limits-of-arbitrage.md`
> - Kozhan-Tham：`https://raw.githubusercontent.com/tszxy/limits-to-arbitrage/main/references/kozhan-tham-execution-risk.md`

---

## 1. Coze（扣子）

1. 进入 Bot 编排页 → **知识库** → 创建知识库 → 选择「在线数据」→ 依次添加以上 3 个 raw URL（文本类型）
2. 在 Bot 的**人设与回复逻辑**（系统提示）中追加：

```text
你配备了一个套利限制理论知识库（两篇经典论文精读：Shleifer-Vishny 1997 与
Kozhan-Tham）。当用户讨论套利、误定价、execution risk、PBA、noise trader、
拥挤交易、三角套利、对冲基金赎回等话题时，必须先检索知识库，引用其中的公式
（LaTeX，含论文编号与页码）、Propositions 和实例数字回答；知识库没有的内容
明确说明，不得凭记忆编造。
```

## 2. Dify

1. 「知识库」→ 创建 → 「导入已有文本 / 同步自 Web 站点」→ 添加 3 个 raw URL
2. 在应用的**提示词编排**中追加：

```text
你是套利理论专家。回答套利限制、execution risk、Performance-Based Arbitrage、
noise trader risk、三角套利相关问题时，优先使用已关联知识库
"limits-to-arbitrage" 中的内容：公式须保留 LaTeX 与论文编号/页码，实例数字
须与文档一致，文档未覆盖的内容要明说。
```

3. 在「上下文」中关联该知识库，召回模式建议选「多路召回」，Top K ≥ 5

## 3. LangChain / LangGraph

```python
import requests
from langchain_core.documents import Document
from langchain_community.vectorstores import FAISS
from langchain_openai import OpenAIEmbeddings

BASE = "https://raw.githubusercontent.com/tszxy/limits-to-arbitrage/main/"
FILES = [
    "SKILL.md",
    "references/shleifer-vishny-limits-of-arbitrage.md",
    "references/kozhan-tham-execution-risk.md",
]

docs = [Document(page_content=requests.get(BASE + f).text, metadata={"source": f})
        for f in FILES]

# 方案 A：小体量直接全文进 system prompt（总计约 10 万字符，适合长上下文模型）
system_prompt = (
    "你是套利理论专家。以下知识库是两篇套利限制经典论文的精读笔记，"
    "回答相关问题时以此为准，公式与数字必须引用文档内容：\n\n"
    + "\n\n---\n\n".join(d.page_content for d in docs)
)

# 方案 B：RAG（适合短上下文或低成本调用）
vectorstore = FAISS.from_documents(docs, OpenAIEmbeddings())
retriever = vectorstore.as_retriever(search_kwargs={"k": 5})
# 在 chain 中：当问题涉及套利/execution risk/PBA/三角套利时调用 retriever
```

## 4. AutoGen（微软）

```python
SYSTEM_MESSAGE = """你是套利理论专家。你的知识库位于 GitHub：
https://github.com/tszxy/limits-to-arbitrage

回答套利限制、execution risk、PBA、noise trader、三角套利问题前：
1. 先 fetch https://raw.githubusercontent.com/tszxy/limits-to-arbitrage/main/SKILL.md
2. 按其导航决定读取 references/ 下哪份文档（raw URL 同仓库路径）
3. 公式（LaTeX，含论文编号/页码）与实例数字必须引自文档，不得凭记忆编造。
"""

assistant = autogen.AssistantAgent(
    name="arbitrage_expert",
    system_message=SYSTEM_MESSAGE,
    llm_config={"config_list": config_list, "tools": [...]},  # 挂 fetch/requests 工具
)
```

## 5. CrewAI

```python
from crewai import Agent, Task

arbitrage_expert = Agent(
    role="套利限制理论专家",
    goal="基于 Shleifer-Vishny (1997) 与 Kozhan-Tham 论文知识库准确回答套利问题",
    backstory=(
        "你的知识库在 https://github.com/tszxy/limits-to-arbitrage 。"
        "回答前先读取 SKILL.md（raw URL），按其导航读取 references/ 下文档，"
        "所有公式与数字引用文档并注明论文页码。"
    ),
    tools=[scrape_tool],  # 挂 WebsiteSearchTool 或自定义 requests 工具
)
```

## 6. OpenAI GPTs / Assistants API

- **GPTs**：「Configure」→「Knowledge」→ 上传 3 个 .md 文件（从 GitHub 下载后上传）；Instructions 末尾追加场景 1 的触发规则文本
- **Assistants API**：用 `file_search` 工具上传同一批文件；instructions 同样追加触发规则

## 7. 通用触发规则文本（任何平台都可粘贴到系统提示）

```text
【知识库：limits-to-arbitrage】
来源：https://github.com/tszxy/limits-to-arbitrage（三个 Markdown 文件）
触发：用户讨论套利、误定价、market efficiency、execution risk、
Performance-Based Arbitrage (PBA)、noise trader risk、拥挤交易、三角套利、
对冲基金赎回/流动性危机时。
规则：
1. 先读 SKILL.md，按其"参考文档导航"选择读取对应 references 文档；
2. 公式、Propositions、实例数字必须引用文档原文（保留 LaTeX、论文编号、页码）；
3. 文档未覆盖的内容明确告知用户，禁止凭记忆编造论文内容。
```
