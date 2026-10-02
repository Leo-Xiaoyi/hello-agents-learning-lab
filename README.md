# Hello Agents Learning Lab

我学习 [Datawhale Hello-Agents](https://github.com/datawhalechina/hello-agents) 时整理的笔记，覆盖第 1–10 章。第 4–10 章记得更细，主要是 Agent 的架构、信息流和设计取舍。联系紧密的章节合在同一篇里。

笔记以我的理解和思路整理为主。代码只记到模块职责和伪代码层面。

## 笔记目录

| 章节 | 笔记 | 记下的问题 |
| --- | --- | --- |
| 第 1–3 章 | [智能体怎样运行](notes/01-03-agent-basics.md) | Agent 的基本循环、发展脉络、模型的能力与限制 |
| 第 4 章 | [ReAct、Plan-and-Solve 与 Reflection](notes/04-agent-patterns.md) | 什么时候边做边想、先规划或事后复查 |
| 第 5–6 章 | [低代码平台与 Agent 框架](notes/05-06-platforms-and-frameworks.md) | 怎样组织流程、状态与多 Agent 协作 |
| 第 7 章 | [Agent 框架怎样分层](notes/07-agent-framework.md) | 模型、消息、Agent、配置和工具各负责什么 |
| 第 8 章 | [记忆与 RAG](notes/08-memory-and-rag.md) | 历史信息和外部文档怎样存、怎样找 |
| 第 9 章 | [上下文工程](notes/09-context-engineering.md) | 每轮从各来源选什么信息给模型 |
| 第 10 章 | [MCP、A2A 与 ANP](notes/10-agent-protocols.md) | 工具、其他 Agent 和开放网络怎样连接 |

[学习记录](learning-log.md)里留着我整理各章时想弄明白的问题。

## Python 练习

| 目录 | 内容 |
| --- | --- |
| [`Try_code/C1T1_WeatherTravelAssistant`](Try_code/C1T1_WeatherTravelAssistant) | 天气与旅行助手练习：工具调用、OpenAI 兼容的模型接口。 |
| [`Try_code/C4T1_AgentParadigms`](Try_code/C4T1_AgentParadigms) | 第 4 章 Agent 范式练习共用的模型接口。 |

天气与旅行助手练习的本地运行命令（API 配置放在 `.env`）：

```sh
cd Try_code/C1T1_WeatherTravelAssistant
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
cp .env.example .env
python main.py
```

`.env` 和本地虚拟环境不纳入 Git。
