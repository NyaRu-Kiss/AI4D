# 工具对照

检索日期：2026-10-30。每一行都能指回当时打开的仓库说明、文档或论文摘要。没有打开的产品不写优缺点。许可证只写页面上能看到的名称。星标和价格会变，表内不比较星标。

接入原则对所有层相同：业务代码依赖自己的接口，供应商 SDK 放在适配器里。

## 1. 需求与规格

| 工具 | 角色 | 接入 | 优点 | 局限 |
| --- | --- | --- | --- | --- |
| [GitHub Spec Kit](https://github.com/github/spec-kit) | 给 Coding Agent 的规格过程：原则、规格、计划、任务、实现、收敛；可加缺陷修复和想法评估 | Python 3.11+、`uv`、`specify init --integration`（文档示例是 GitHub Copilot）。命令在 Agent 对话里逐步跑，人看过再继续 | MIT。规格是 Markdown，可换 Agent，可加预设、钩子和私有目录 | 小改动也多一轮产物。社区目录页面写明不是审计。它不核对业务事实 |

Issue 系统仍负责优先级和负责人。Spec Kit 的已接受规格再链到评测例子。

## 2. 文档解析

解析器输出统一中间文档后再切分和嵌入。

| 工具 | 角色 | 接入 | 优点 | 局限 |
| --- | --- | --- | --- | --- |
| [Docling](https://github.com/docling-project/docling) | 解析 PDF、Office、HTML、图片等，导出 Markdown、HTML、JSON | Python 库；文档写明可接 LangChain、LlamaIndex 等，也可本地跑 | MIT。版面、阅读顺序、表格是单独能力。适合敏感数据留在本地 | 质量仍取决于版面。扫描件和复杂表格要用自己的黄金集验收 |
| [Marker](https://github.com/datalab-to/marker) | PDF 转 Markdown 和 JSON | 仓库提供转换脚本和可选服务 | Apache-2.0。面向快速、较高准确度的 PDF 转换 | 模型权重可能另有 MODEL_LICENSE，使用前要看仓库里的模型许可证，不假设与代码许可证相同 |
| [Apache Tika](https://github.com/apache/tika) | 识别并抽取上千种文件的元数据和文本 | Java 库或服务，输出文本和元数据 | Apache-2.0。格式覆盖广 | 抽取的是文本和元数据，不负责版面级 RAG 切片 |
| [Unstructured](https://github.com/Unstructured-IO/unstructured) | 把复杂文档转成适合模型的结构化块；另有商业平台做生产流水线 | 开源库做分区；平台做分块和嵌入 | Apache-2.0 的开源库。ETL 边界清楚 | 开源库和商业平台不是同一个支持范围，选型时要分开评 |
| [LlamaParse / Parse](https://developers.llamaindex.ai/python/cloud/llamaparse/) | 面向 LLM 的托管解析，输出 Markdown、文本或 JSON | LlamaIndex 云 API | 文档写明面向表格、图表和扫描件 | 文件要上传到该服务。不出域的场景不能用，除非合同允许 |

## 3. 知识库应用

这些产品把解析、索引、工作流和问答包在一起。它们适合先跑通，不适合当成不可替换的业务核心。

| 工具 | 角色 | 接入 | 优点 | 局限 |
| --- | --- | --- | --- | --- |
| [Dify](https://docs.dify.ai/) | 开源平台：Agent、工作流、聊天和自有数据 | 云或 Docker 自托管；应用通过 API 调用 | 文档区分云和社区版。有插件和工作流 | 平台对象容易变成业务模型。复杂权限和评测仍要接到外部门禁 |
| [RAGFlow](https://github.com/infiniflow/ragflow) | 开源 RAG 引擎，并带 Agent | 自托管；仓库含 SDK | Apache-2.0。检索和 Agent 在同一项目里 | 升级要跟着整个引擎，而不是只换重排器 |
| [FastGPT](https://doc.fastgpt.io/) | 知识库问答、可视化工作流、Agent 和工具 | 国际站或中国站；也有商业版和社区版 | 文档把数据集、工作流和发布分开说明。适合业务人员搭流程 | 文档同时存在云、商业和社区三种发行，功能不要互相套用 |
| [MaxKB](https://github.com/1Panel-dev/MaxKB) | 开源企业智能体平台，主题含知识库、MCP、RAG | 按仓库安装 | 仓库写明 GPL-3.0。和多种模型、pgvector 的主题相关 | GPL-3.0 对分发衍生作品有传染性义务，嵌入专有产品前要做法务确认 |
| [AnythingLLM](https://github.com/Mintplex-Labs/anything-llm) | 本地优先的 Agent 和 RAG 桌面/服务 | 自托管 | MIT。定位是数据自己掌握 | 它是完整个体验，不是一个可以单换的向量库适配器 |

腾讯云 ADP 知识库和知识引擎原子能力的分工见第 4.3 章，依据是知识库笔记引用的公开文档，不在本表重复成另一套优缺点。

## 4. RAG 编排与图检索

| 工具 | 角色 | 接入 | 优点 | 局限 |
| --- | --- | --- | --- | --- |
| [LangGraph](https://docs.langchain.com/oss/python/langgraph/overview) | 底层编排：同一张图里混合确定步骤和模型步骤 | `pip install langgraph`。持久化、流式、人工插入由运行时提供。可以不使用 LangChain 的其它组件 | 文档强调控制力、耐久执行和人工介入。观测可接到 LangSmith | 图和检查点是额外系统。简单的固定 RAG 不必用它 |
| [Microsoft GraphRAG](https://github.com/microsoft/graphrag) | 从图索引回答面向整库的问题 | Python 项目，先建图再查询 | MIT。论文说明它针对“整个文集的主题是什么”这类全局问题，而不是单点检索 | 建图要调用模型，成本在索引阶段。实体关系不稳定时图会过期 |
| [LightRAG](https://github.com/HKUDS/LightRAG) | 把图放进索引和检索，做双层检索 | Python 包 | MIT。论文摘要写的是同时覆盖细粒度和高层知识 | 图的质量依赖抽取模型。它不替代权限过滤和引用核对 |

论文摘要来源：GraphRAG [2404.16130](https://arxiv.org/abs/2404.16130)，LightRAG [2410.05779](https://arxiv.org/abs/2410.05779)。

固定管道仍然是默认。上面的图检索在多跳或全局问题的评测优于扁平切片时再加。

## 5. Embedding

公共榜单不直接决定生产模型。MTEB 是跨语言、跨任务的嵌入评测框架，仓库许可证为 Apache-2.0：<https://github.com/embeddings-benchmark/mteb>。下面模型卡上的名次只引用卡片原文，不延伸成“当前总榜第一”。

| 模型或服务 | 文档里写明的特点 | 接入注意 | 局限 |
| --- | --- | --- | --- |
| [BAAI/bge-m3](https://huggingface.co/BAAI/bge-m3) | 密集、多向量、稀疏三种检索；100 多种语言；最长约 8192 token。MIT。卡片建议混合检索再加重排 | 稀疏权重可当作词法信号，和 BM25 一类方法同类评估 | 多功能不等于三种输出都要进生产。每种索引分别评测 |
| [nomic-embed-text-v1.5](https://huggingface.co/nomic-ai/nomic-embed-text-v1.5) | Apache-2.0。Matryoshka，可调整维度。文档要求文档用 `search_document:`、查询用 `search_query:` 前缀 | 前缀是模型的一部分。漏前缀等于换了一种输入 | 卡片写它已与视觉模型对齐；文本索引和图像索引仍要分别建 |
| [Qwen3-Embedding-8B](https://huggingface.co/Qwen/Qwen3-Embedding-8B) | Apache-2.0。系列含 0.6B、4B、8B 和对应重排模型。卡片写 8B 在 2025-06-05 的 MTEB 多语言榜为当时第一（70.58） | 同系列可以按成本换尺寸，但换尺寸要重建索引 | “当时第一”不是 2026-10-30 的榜单结论 |
| [Voyage 4 系列](https://docs.voyageai.com/docs/embeddings) | 托管 API。`voyage-4-large`、`voyage-4`、`voyage-4-lite` 默认 1024 维，可选 256、512、2048，上下文 32000。同系列向量互相兼容。另有代码、金融、法律模型 | 通过 API 嵌入。4 系列内部可换尺寸时仍要按文档的兼容说明 | 数据出域。旧模型文档建议迁到新系列 |
| [Cohere Embed](https://docs.cohere.com/docs/embeddings) | 向量表示文本或图像；文档要求设置 `input_type`；支持多语言、Matryoshka 和压缩 | 查询和文档的 `input_type` 不要混用 | 托管 API。压缩格式要和索引距离一起评测 |
| Jina Embedding API | 托管嵌入，文档含归一化、float/binary 输出和多语言示例 | `POST https://api.jina.ai/v1/embeddings` | 页面同时存在多代模型。生产请求要写死模型名。Apache-2.0 的说明出现在其旧一代开源模型的段落，不要套到全部 API 模型 |

晚交互是另一类索引，不是把单向量模型换个参数：

| 项目 | 文档里的做法 | 优点 | 局限 |
| --- | --- | --- | --- |
| [ColBERT](https://github.com/stanford-futuredata/ColBERT) | 查询和文档保留多向量，查询时再交互。MIT | 比单向量保留更细的匹配 | 索引更大，查询更重 |
| [ColPali](https://github.com/illuin-tech/colpali) | 用视觉语言模型直接检索文档页面。MIT。论文 [2407.01449](https://arxiv.org/abs/2407.01449) 说明它少做一次脆弱的版面转文本 | 表格和版面线索还在 | 页级检索，引用粒度是页，不是条款。存储和算力更高 |

[Late chunking](https://arxiv.org/abs/2409.04701) 先用长上下文模型编码全文，再在池化前切块，减轻块与块之间丢上下文。它要求嵌入模型的上下文装得下全文，否则没有收益。

[Lost in the Middle](https://arxiv.org/abs/2307.03172) 说明长上下文里相关信息放在中间时，模型更不容易用到。所以拼装阶段要控制切片数量，而不是只换更长的窗口。

## 6. 向量库

| 产品 | 形态 | 文档中能确认的能力 | 优点 | 局限 | 许可证 |
| --- | --- | --- | --- | --- | --- |
| [pgvector](https://github.com/pgvector/pgvector) | Postgres 扩展 | 精确和近似搜索；半精度、二进制、稀疏；L2、内积、余弦、L1、汉明、杰卡德；SQL `WHERE` 与向量排序可以写在一起；ACID、时间点恢复、JOIN | 向量和业务行在同一套事务和备份里 | 超大规模召回和独立向量引擎的运维模型不同。过滤是否走索引要看执行计划 | 本次 README 摘录未单独引用许可证名，以仓库 LICENSE 为准 |
| [Qdrant](https://qdrant.tech/documentation/) | 独立服务、云、以及文档中的嵌入式 Qdrant Edge | 文档列了过滤、混合查询、量化和多租户 | 过滤和多租户写进产品文档，适合作为知识索引 | 又一套集群的备份和升级。Edge 和服务器不是同一种部署 | 仓库徽章为 Apache 2.0 |
| [Milvus](https://github.com/milvus-io/milvus) | 独立或集群；文档描述存储和计算分离 | 面向向量相似搜索的开源数据库 | LF AI & Data 项目，集群路径清楚 | 组件多，小数据量的运维比单库扩展重 | Apache 2.0 |
| [Weaviate](https://docs.weaviate.io/) | 开源服务或云 | 文档提供数据库、部署和云 | 社区版可自托管 | README 写明社区版是 BSD 3-Clause，企业功能要许可证密钥。镜像里两者叠在一起 | BSD 3-Clause；企业功能另计 |
| [Chroma](https://docs.trychroma.com/) | 库、自托管或 Chroma Cloud | 文档写明元数据过滤、密集、稀疏和混合、全文 | 上手路径短 | 云和自托管的运维责任不同，不要用演示配置当生产 | 仓库徽章为 Apache 2.0 |
| [LanceDB](https://github.com/lancedb/lancedb) | 嵌入式或服务；基于 Lance 列式格式 | README 写明可存向量、元数据和多模态，并能过滤 | Apache-2.0。和 Arrow、DuckDB 等表工具靠近，适合随数据文件走 | 嵌入式和分布式服务的恢复方式不同，要按选定形态写运行手册 | Apache-2.0 |
| [OpenSearch](https://github.com/opensearch-project/OpenSearch) | 搜索集群，含向量 | 文档有向量检索 | 关键词和向量可以在同一搜索栈 | 集群运维重。只为向量引入整套搜索引擎通常不划算 | 仓库徽章为 Apache 2.0 |
| [Elasticsearch](https://github.com/elastic/elasticsearch) | 搜索与分析引擎，README 写明含向量检索 | 托管云或自行安装 | 全文、日志和向量能共用查询栈 | Elastic 许可 FAQ 写明自 7.11 起源码从 Apache 2.0 改为 SSPL 与 Elastic License 双许可 | SSPL 与 Elastic License。和 OpenSearch 不要当成同一个许可证 |
| [Vespa](https://docs.vespa.ai/) | 搜索和排序引擎 | 文档含最近邻、文本匹配、过滤和排序表达式 | 混合排序可以写进同一个应用包 | 应用包、schema 和集群模型学习成本高 | Apache 2.0 |
| [Pinecone](https://docs.pinecone.io/) | 托管向量库 | 文档是 Database、索引和限制的托管服务 | 不自己管进程 | 数据出域；容量和限流以当时的限制页为准。它不是可嵌入的库 | 托管服务，不是上面那种开源许可证 |

重建索引是换模型和换维度的正路。备份策略按“能否从原文重建”设计，不把向量库当成唯一副本。

## 7. 评测

| 工具 | 角色 | 接入 | 优点 | 局限 |
| --- | --- | --- | --- | --- |
| [promptfoo](https://www.promptfoo.dev/docs/intro/) | 评测和红队的 CLI/库 | 声明式用例，可进 CI。支持多家模型 API | 面向测试驱动，而不是笔记本里的一次性比较 | 自动分数仍然要人抽查。红队报告不是上线许可 |
| [Ragas](https://github.com/vibrantlabsai/ragas) | LLM 应用评测库 | Python，文档在 docs.ragas.io | 徽章为 Apache-2.0。面向检索和生成的评测 | 指标依赖评判模型时，要记录评判模型版本 |
| [DeepEval](https://deepeval.com/docs/metrics-faithfulness) | 评测指标库 | 文档中的 RAG 指标包括 Answer Relevancy、Faithfulness、Contextual Recall、Contextual Precision、Contextual Relevancy | 指标拆开，能对应本白皮书“分开看召回和答案”的要求 | 指标多，不代表都要进门禁。门禁只留和错误成本对应的几项 |

DeepEval 文档还列出 MCP 任务完成度等 Agent 指标。那是另一组测试，不和 RAG 指标混成一个分数。

## 8. Agent 协议和编排

| 项目 | 角色 | 接入 | 优点 | 局限 |
| --- | --- | --- | --- | --- |
| LangGraph | 见第 4 节 | 图运行时 | 确定步骤和模型步骤可共存 | 简单问答不必引入 |
| [A2A](https://github.com/a2aproject/A2A) | Agent 之间的开放协议，不暴露对方内部工具 | JSON-RPC 2.0 over HTTP(S)；Agent Card 发现；Python、Go、JS、Java、.NET、Rust SDK | Apache-2.0，Linux Foundation。文档写明与 MCP 互补 | 只在跨团队、跨进程协作时使用。进程内调用不需要 |

本体工具的角色在第 4.5 章。已经打开过 LICENSE 或 README 的项目见第 17 节。

## 9. Coding Agent

这些工具改的是仓库，不是生产系统。合并责任在人。凭证留在执行环境，不进提示词。

| 工具 | 角色 | 接入 | 优点 | 局限 |
| --- | --- | --- | --- | --- |
| [GitHub Copilot](https://docs.github.com/en/copilot/get-started/about-github-copilot) | 写在 GitHub 工作流里的助手：补全、解释、审查，以及按任务改仓库并开拉取请求 | 文档写它用仓库、Issue、拉取请求和仓库里的自动化。也可在 IDE 里用。自定义指令、技能、MCP 和第三方 Coding Agent 由同一套审查接住 | 文档把能力分成协助、代理、定制、外部 Agent。代理模式仍要求人审查和批准 | 产品页没有开源许可证。离开 GitHub 时要用文档里的 IDE 或 CLI。价格不在本表 |
| [Claude Code](https://github.com/anthropics/claude-code) | 终端里的代理式编码工具，README 也写了 IDE，以及在 GitHub 上 @claude | README 推荐安装脚本、Homebrew 或 WinGet。`npm install -g` 已标明弃用。官方文档指向 code.claude.com | README 写它能执行例行任务、解释代码和处理 git | 本次未打开 LICENSE。README 写明会收集使用数据、关联对话和 `/bug` 反馈，并指向商业条款与隐私政策。同一段写有不把反馈用于模型训练的政策 |
| [Continue](https://github.com/continuedev/continue) | 开源 Coding Agent：CLI、VS Code、JetBrains | 按仓库文档配置。README 建议用 CLI，而不是 JetBrains 插件 | Apache-2.0。规格和配置可以留在自己的环境 | README 写明该仓库已不再维护，对所有用户只读，最后版本是 2.0.0 |
| [Aider](https://github.com/Aider-AI/aider) | 终端结对编程 | 连接云端或本地模型。README 写它会做代码库地图，并自动用 git 提交 | Apache-2.0。改动留在 git 历史里，便于 diff 和撤销 | README 点名的“最好用模型”会过时。自动提交和“由人提交”的团队规则冲突时要关掉 |
| [OpenHands](https://github.com/OpenHands/OpenHands) | README 现称 Agent Canvas：自托管的 Coding Agent 控制面，可跑 OpenHands 代理，也可接 Claude Code、Codex、Gemini CLI 等 | 默认本机。也可接 Docker、虚拟机、公司内后端，或 OpenHands Cloud / Enterprise | MIT。一个控制面上挂多种代理 | 选了云后端，代码和对话就出域。控制面不代替工具网关 |
| [Cline](https://github.com/cline/cline) | IDE、终端、桌面和 SDK 形态的编码代理 | npm 安装 CLI；也有 VS Code、JetBrains、桌面应用。VS Code 说明写了人工批准后再执行 | Apache-2.0。SDK 与编辑器共用引擎 | 形态多，评审规则要按 CLI、桌面、编辑器分别写，不能假设每一处都有同样的批准步骤 |
| [Codex CLI](https://github.com/openai/codex) | OpenAI 的本地编码代理。README 同时指向 IDE 插件和云端 Codex | 安装脚本默认从 `releases.openai.com` 拉包，失败再回退 GitHub Releases。也可用 npm 或 Homebrew | Apache-2.0。本地 CLI 和云端代理是两种数据边界 | 云端入口在 chatgpt.com/codex。用云端时代码会离开本机 |
| [Gemini CLI](https://github.com/google-gemini/gemini-cli) | 终端编码代理 | 仓库仍可安装。README 的状态说明把现用范围收窄 | Apache-2.0 | README 写明：面向 Gemini Code Assist 标准版/企业版和 GitHub 上的 Code Assist；免费与 Google AI Pro/Ultra 已于 2026-06-18 迁到 Antigravity CLI；本 CLI 未针对 Gemini 4 及以后调优，也不应用于模型跑分 |

Antigravity CLI 只出现在 Gemini CLI 的 README 里，本次没有单独打开它的产品页，所以不单列优缺点。

## 10. 模型网关与本地推理

网关统一的是调用格式、密钥和限额。本地推理解决的是权重不出域。二者不要合成一个“已自托管”的结论。

| 项目 | 角色 | 接入 | 优点 | 局限 |
| --- | --- | --- | --- | --- |
| [LiteLLM](https://github.com/BerriAI/litellm) | AI 网关：用 OpenAI 格式调用多家模型 | Python SDK，或单独部署 Proxy | 仓库 LICENSE 写明 `enterprise/` 以外为 MIT。README 写 100 多家供应商、虚拟密钥和花费跟踪 | `enterprise/` 另有许可证。换供应商前仍要用本白皮书的评测集，格式兼容不等于行为相同 |
| [vLLM](https://github.com/vllm-project/vllm) | LLM 推理服务 | 按 vllm.ai 文档部署。README 点名 PagedAttention、连续批处理、分块预填充和前缀缓存 | Apache-2.0 | 服务要自己管 GPU、模型和配额。它不提供业务权限 |
| [SGLang](https://github.com/sgl-project/sglang) | 语言、视觉语言和扩散模型的推理框架。README 写它面向代理负载、大规模服务和 RL rollout | Docker 镜像 `lmsysorg/sglang`，或 `uv pip install sglang` | Apache-2.0。扩散引擎在同一仓库 | 运行时和 vLLM 二选一作为服务入口，避免两套调度器同时对同一模型负责 |
| [Ollama](https://github.com/ollama/ollama) | 拉取并运行开放模型 | 安装脚本、桌面安装包或 Docker。另有 Python 和 JS 库 | MIT。README 写本地或云端 | README 里的隐私表述随安装方式变化。用云端时按云端条款，不按“本地”理解 |
| [llama.cpp](https://github.com/ggml-org/llama.cpp) | C/C++ 推理，含视觉语言模型 | Docker、发布页二进制，或自行编译。README 写 1.5 到 8 bit 量化，并把 Apple 芯片当作一等目标 | MIT。依赖少，适合单机 | 量化是另一套模型制品，要和全精度分开评测。它不是集群调度器 |

## 11. Agent 框架

框架选择看控制流能不能画出来、工具调用能不能被网关拦住、状态能不能重放。多 Agent 不是默认。

| 项目 | 角色 | 接入 | 优点 | 局限 |
| --- | --- | --- | --- | --- |
| LangGraph | 见第 4 节 | 图运行时 | 确定步骤和模型步骤可共存 | 简单问答不必引入 |
| [OpenAI Agents SDK](https://github.com/openai/openai-agents-python) | 多 Agent 工作流的 Python SDK。README 写它支持 Responses、Chat Completions 和其他模型 | 库。概念包括 Agent、工具、护栏、交接、MCP | MIT | 另有 JS 仓库。交接和多 Agent 会放大权限，仍要走工具网关 |
| [Pydantic AI](https://github.com/pydantic/pydantic-ai) | 有类型的 Python Agent 循环 | 库。README 写模型可以用字符串替换，并带失败回退；同一 Agent 可进 Web、终端、语音、持久队列或 GitHub Actions | MIT | 类型约束的是参数形状。它不证明回答符合业务事实 |
| [Mastra](https://github.com/mastra-ai/mastra) | TypeScript Agent 应用框架 | 可嵌 React、Next.js、Node，或独立服务。README 写模型路由覆盖 40 多家供应商 | LICENSE 写明 `ee/` 以外为 Apache-2.0 | `ee/` 另有许可证。不要把企业目录里的能力写成默认开源功能 |
| [CrewAI](https://github.com/crewAIInc/crewAI) | Python 多 Agent。README 区分 Crews（角色协作）和 Flows（事件驱动） | 库。商业控制面产品名是 AMP Suite | MIT | 高自主协作要有退出条件和评测。AMP 是另一套商业控制面 |
| [AutoGen](https://github.com/microsoft/autogen) | 曾用于多 Agent 实验的框架 | 仓库仍可阅读 | README 写明文档为 CC BY 4.0，代码为 MIT（`LICENSE-CODE`） | README 写明项目已归档，微软不再修安全和功能，并建议不要用于生产 |
| [LlamaIndex](https://github.com/run-llama/llama_index) | 仍提供 RAG 和 Agent 的开源工具包 | Python 库 | MIT | README 写公司重点已转到 LlamaParse、LiteParse 和解析评测。编排能力还在，但是次要维护面 |
| [smolagents](https://github.com/huggingface/smolagents) | 短小的 Agent 库，动作写成代码 | 库。代码执行可放进 README 点名的 Blaxel、E2B、Modal 或 Docker | Apache-2.0 | 代码动作的爆炸半径比 JSON 工具调用大，必须在沙箱里跑 |
| [Haystack](https://github.com/deepset-ai/haystack) | Python 编排：管道、检索、路由、记忆和生成 | `pip` 安装。README 写 RAG、多模态、搜索、问答和 Agent | Apache-2.0 | README 另有企业支持与平台。企业功能不算进默认开源能力 |

## 12. MCP

| 项目 | 角色 | 接入 | 优点 | 局限 |
| --- | --- | --- | --- | --- |
| [MCP 规范](https://github.com/modelcontextprotocol/specification) | 协议、schema 和官方文档站 modelcontextprotocol.io | README 写 schema 以 TypeScript 为源，并提供 JSON Schema。本次打开的路径含 `schema/2026-07-28` | README 写 MIT | 规范版本要钉死。主机和服务器各升各的，不假设永远兼容 |
| [MCP Python SDK](https://github.com/modelcontextprotocol/python-sdk) | 用 Python 写 MCP 服务器和客户端 | 传输：stdio、Streamable HTTP、SSE。文档在 py.sdk.modelcontextprotocol.io | MIT。工具、资源和提示是分开的能力 | SDK 不执行权限决策。允许哪些工具仍由网关和策略决定 |

A2A 与 MCP 的分工见第 8 节。

## 13. 输入输出防护

防护栏是运行时检查，不是评测集的替代。

| 项目 | 角色 | 接入 | 优点 | 局限 |
| --- | --- | --- | --- | --- |
| [NeMo Guardrails](https://github.com/NVIDIA/NeMo-Guardrails) | 可编程的对话防护：话题、对话路径、风格、结构化抽取 | `pip install nemoguardrails`。README 要求 Python 3.10–3.13。文档在 docs.nvidia.com/nemo/guardrails | Apache-2.0。README 指向论文 2310.10501 | 规则要自己写，并放进评测。库不了解订单状态这类业务不变量 |
| [Guardrails](https://github.com/guardrails-ai/guardrails) | 对输入和输出做风险检测与缓解 | Python 库。校验器按 README 改为普通 PyPI 包 | Apache-2.0 | README 在 2026-07-06 写明将停掉托管远程推理，计划截止 2026-08-25。依赖远端推理的校验器要改成本地包 |

## 14. 工作流引擎

固定步骤、人工任务和补偿走引擎。开放式判断才进 Agent 循环。

| 项目 | 角色 | 接入 | 优点 | 局限 |
| --- | --- | --- | --- | --- |
| [Temporal](https://github.com/temporalio/temporal) | 耐久执行。服务器把 Workflow 跑完，并重试失败的操作 | 服务端加语言 SDK。文档在 docs.temporal.io。README 写它源自 Uber Cadence | MIT | 又一套集群。把模型调用放进 Workflow 时，要把模型标识和提示哈希写成步骤输入，否则重放对不上 |
| [Camunda 8](https://github.com/camunda/camunda) | BPMN 流程和 DMN 决策。仓库含 Zeebe、Tasklist（人工任务）、Operate | Docker 或 Kubernetes。README 写可接 Kafka 等消息 | 人工任务和流程历史是产品的一部分 | README 的 License 节写主体是 Camunda License 1.0，列出的部分才是 Apache-2.0。嵌入专有产品前要读那份许可证 |

## 15. 发布与版本

| 项目 | 角色 | 接入 | 优点 | 局限 |
| --- | --- | --- | --- | --- |
| [Argo CD](https://github.com/argoproj/argo-cd) | Kubernetes 上的声明式 GitOps 持续交付 | 集群控制器盯 Git 中的应用定义。文档在 argo-cd.readthedocs.io | Apache-2.0。发布状态以 Git 为准 | 只覆盖 Kubernetes。模型标识和提示哈希要进同一份声明，否则它只同步了业务容器 |
| [Flux](https://github.com/fluxcd/flux2) | 让集群与 Git 或 OCI 中的配置保持一致，并在有新制品时更新 | GitOps Toolkit 的一组 Kubernetes API。README 写多租户、可同步多个 Git 仓库，并与 Prometheus 集成 | Apache-2.0。README 写它是 CNCF 毕业项目 | 同样假设工作负载在 Kubernetes 上 |
| [DVC](https://github.com/iterative/dvc) | 数据、模型和实验的版本。大文件在对象存储，版本信息在 Git | CLI 和 VS Code 扩展。README 写受影响的流水线步骤才重跑，实验可在本地 Git 比较 | Apache-2.0 | 它版本化的是数据科学制品，不代替容器发布。评测集和索引快照可以借用这套指针，但运行手册要另写 |

GitHub Actions、GitLab CI 这一轮没有打开产品页，不写优缺点。发布门禁仍按部署章执行。

## 16. 观测、追踪和实验

离线评测库见第 7 节。这里是把追踪、数据和实验接进工程平台的产品。

| 项目 | 角色 | 接入 | 优点 | 局限 |
| --- | --- | --- | --- | --- |
| [Langfuse](https://github.com/langfuse/langfuse) | LLM 工程平台：开发、监控、评测、调试 | README 写可自托管。追踪文档在 langfuse.com/docs | LICENSE 写明 `ee/` 等目录另有许可证，其余为 MIT Expat。README 写 2026 年 1 月起属于 ClickHouse | 企业目录的功能不要算进默认安装。追踪字段仍要按本白皮书钉模型标识和知识快照 |
| [Arize Phoenix](https://github.com/Arize-ai/phoenix) | README 称其用于实验、评测和排障的 AI 可观测 | 仓库自托管路径。托管生产流程 README 指向另一产品 Arize AX | 实验和追踪可以放在应用旁边 | 打开的 LICENSE 文件以 Elastic License 2.0 开头。使用限制以该文件为准，不要按 Apache/MIT 理解 |
| [Helicone](https://github.com/Helicone/helicone) | README 的定位是 AI 网关加 LLM 可观测：路由、回退、追踪、花费和延迟 | 通过其网关，或在 OpenAI、Anthropic、LangChain、Gemini、Vercel AI SDK 上按文档加记录 | Apache-2.0 | 网关形态默认经他们的入口。密钥和日志的驻留地要在接入时写明 |
| [MLflow](https://github.com/mlflow/mlflow) | README 写的 AI 工程平台：追踪、评测、监控、提示管理，以及模型和数据的访问控制 | 按 mlflow.org 文档部署 | Apache-2.0。传统 ML 实验和 LLM 追踪在同一项目 | 功能面宽。门禁只启用和错误成本对应的评测，不把平台全部指标设成发布条件 |

## 17. 本体工具里已经打开过的许可证和说明

角色的正文在第 4.5 章。下表只补充本次打开的仓库文件。

| 项目 | 本次打开的文件说明 | 局限 |
| --- | --- | --- |
| [Apache Jena](https://github.com/apache/jena) | LICENSE 为 Apache-2.0 | 本次未打开功能 README，不新增能力描述 |
| [RDFLib](https://github.com/RDFLib/rdflib) | BSD-3-Clause。README：纯 Python 的 RDF 库，含多种语法、可替换的 Store、SPARQL 1.1 查询和更新 | Store 插件和远端 SPARQL 的运维不在库里 |
| [pySHACL](https://github.com/RDFLib/pySHACL) | Apache-2.0。README：用 RDFLib 对 RDF 图做 SHACL 校验，并依赖 OWL-RL 做 OWL 2 RL 展开 | 校验的是形状，不产生业务批准 |
| [Oxigraph](https://github.com/oxigraph/oxigraph) | README 写 Apache-2.0 或 MIT 双许可。它是基于 RocksDB 的图数据库和 SPARQL 工具包，含解析、序列化和 SPARQL 查询/更新 | README 写查询执行尚未优化，项目仍在密集开发 |
| [Eclipse RDF4J](https://github.com/eclipse-rdf4j/rdf4j) | README：Java 库，以及 Server 和 Workbench。文档在 rdf4j.org | 本次未打开 LICENSE 文件，不写许可证名 |

Protégé、GraphDB、HermiT、ELK、RDFox、Neo4j 的许可证页这一轮没有打开，仍不写许可证和优缺点。

## 18. 这一轮没有打开产品页的层

不写优缺点：OpenRouter、Portkey、GitHub Actions、GitLab CI、LangSmith 产品页、Braintrust、Weights & Biases、Llama Guard、Google ADK、Claude Agent SDK、Antigravity CLI 自身的文档。架构图工具（例如 Structurizr）也没有打开。设计阶段用第 2.3 节的接入表。
