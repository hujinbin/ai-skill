---
name: claude-api
description: "使用 Claude API 或 Anthropic SDK 构建 LLM 驱动的应用。触发条件：代码导入 `anthropic`/`@anthropic-ai/sdk`/`claude_agent_sdk`，或用户要求使用 Claude API、Anthropic SDK 或 Agent SDK。不触发：代码导入 `openai`/其他 AI SDK、通用编程或 ML/数据科学任务。"
license: 完整条款见 LICENSE.txt
---

# 使用 Claude 构建 LLM 驱动的应用

此技能帮助你使用 Claude 构建 LLM 驱动的应用。根据需求选择合适的方案，检测项目语言，然后阅读相关的语言特定文档。

## 默认配置

除非用户另有要求：

Claude 模型版本，请使用 Claude Opus 4.6，可通过精确模型字符串 `claude-opus-4-6` 访问。对于任何稍微复杂的情况，请默认使用自适应思考（`thinking: {type: "adaptive"}`）。最后，对于可能涉及长输入、长输出或高 `max_tokens` 的任何请求，请默认使用流式传输 — 它可以防止请求超时。如果不需要处理单独的流事件，使用 SDK 的 `.get_final_message()` / `.finalMessage()` 辅助方法获取完整响应。

---

## 语言检测

在阅读代码示例之前，确定用户使用的语言：

1. **查看项目文件**以推断语言：

   - `*.py`、`requirements.txt`、`pyproject.toml`、`setup.py`、`Pipfile` → **Python** — 从 `python/` 读取
   - `*.ts`、`*.tsx`、`package.json`、`tsconfig.json` → **TypeScript** — 从 `typescript/` 读取
   - `*.js`、`*.jsx`（无 `.ts` 文件）→ **TypeScript** — JS 使用相同的 SDK，从 `typescript/` 读取
   - `*.java`、`pom.xml`、`build.gradle` → **Java** — 从 `java/` 读取
   - `*.kt`、`*.kts`、`build.gradle.kts` → **Java** — Kotlin 使用 Java SDK，从 `java/` 读取
   - `*.scala`、`build.sbt` → **Java** — Scala 使用 Java SDK，从 `java/` 读取
   - `*.go`、`go.mod` → **Go** — 从 `go/` 读取
   - `*.rb`、`Gemfile` → **Ruby** — 从 `ruby/` 读取
   - `*.cs`、`*.csproj` → **C#** — 从 `csharp/` 读取
   - `*.php`、`composer.json` → **PHP** — 从 `php/` 读取

2. **如果检测到多种语言**（例如同时有 Python 和 TypeScript 文件）：

   - 检查用户当前文件或问题与哪种语言相关
   - 如果仍然模糊，询问："我检测到了 Python 和 TypeScript 文件。你正在使用哪种语言进行 Claude API 集成？"

3. **如果无法推断语言**（空项目、无源文件或不支持的语言）：

   - 使用 AskUserQuestion 提供选项：Python、TypeScript、Java、Go、Ruby、cURL/原始 HTTP、C#、PHP
   - 如果 AskUserQuestion 不可用，默认使用 Python 示例并注明："显示 Python 示例。如果需要其他语言请告知。"

4. **如果检测到不支持的语言**（Rust、Swift、C++、Elixir 等）：

   - 建议从 `curl/` 读取 cURL/原始 HTTP 示例，并指出可能存在社区 SDK
   - 提供显示 Python 或 TypeScript 示例作为参考实现

5. **如果用户需要 cURL/原始 HTTP 示例**，从 `curl/` 读取。

### 语言特定功能支持

| 语言 | 工具运行器 | Agent SDK | 备注 |
|------|-----------|-----------|------|
| Python | 是（测试版）| 是 | 完整支持 — `@beta_tool` 装饰器 |
| TypeScript | 是（测试版）| 是 | 完整支持 — `betaZodTool` + Zod |
| Java | 是（测试版）| 否 | 使用注解类的测试版工具调用 |
| Go | 是（测试版）| 否 | `toolrunner` 包中的 `BetaToolRunner` |
| Ruby | 是（测试版）| 否 | 测试版中的 `BaseTool` + `tool_runner` |
| cURL | 不适用 | 不适用 | 原始 HTTP，无 SDK 功能 |
| C# | 否 | 否 | 官方 SDK |
| PHP | 是（测试版）| 否 | `BetaRunnableTool` + `toolRunner()` |

---

## 我应该使用哪个方案？

> **从简单开始。** 默认使用满足需求的最简单层级。单次 API 调用和工作流处理大多数用例 — 只有当任务真正需要开放式、模型驱动的探索时才使用 Agent。

| 用例 | 层级 | 推荐方案 | 原因 |
|------|------|---------|------|
| 分类、摘要、提取、问答 | 单次 LLM 调用 | **Claude API** | 一次请求，一次响应 |
| 批量处理或嵌入 | 单次 LLM 调用 | **Claude API** | 专用端点 |
| 代码控制逻辑的多步流水线 | 工作流 | **Claude API + 工具调用** | 你编排循环 |
| 使用自定义工具的自定义 Agent | Agent | **Claude API + 工具调用** | 最大灵活性 |
| 需要文件/网页/终端访问的 AI Agent | Agent | **Agent SDK** | 内置工具、安全和 MCP 支持 |
| Agent 式编程助手 | Agent | **Agent SDK** | 为此用例设计 |
| 需要内置权限和防护措施 | Agent | **Agent SDK** | 包含安全功能 |

> **注意：** Agent SDK 适用于你想要开箱即用的内置文件/网页/终端工具、权限和 MCP。如果你想用自己的工具构建 Agent，Claude API 是正确选择 — 使用工具运行器自动处理循环，或使用手动循环进行精细控制（审批门、自定义日志、条件执行）。

### 决策树

```
你的应用需要什么？

1. 单次 LLM 调用（分类、摘要、提取、问答）
   └── Claude API — 一次请求，一次响应

2. Claude 是否需要读/写文件、浏览网页或运行 shell 命令
   作为其工作的一部分？（不是：你的应用读取文件后交给 Claude —
   而是 Claude 自身需要发现和访问文件/网页/终端？）
   └── 是 → Agent SDK — 内置工具，不要重新实现
       示例："扫描代码库查找 bug"、"总结目录中每个文件"、
             "使用子代理查找 bug"、"通过网络搜索研究主题"

3. 工作流（多步骤、代码编排、使用你自己的工具）
   └── Claude API + 工具调用 — 你控制循环

4. 开放式 Agent（模型决定自己的轨迹，你自己的工具）
   └── Claude API Agent 循环（最大灵活性）
```

### 我应该构建 Agent 吗？

在选择 Agent 层级之前，检查所有四个标准：

- **复杂性** — 任务是否多步骤且难以提前完全指定？（例如，"将此设计文档变成 PR" vs "从 PDF 中提取标题"）
- **价值** — 结果是否值得更高的成本和延迟？
- **可行性** — Claude 是否擅长此类任务？
- **错误代价** — 错误是否可以被捕获和恢复？（测试、审查、回滚）

如果其中任何一个答案为"否"，请保持更简单的层级（单次调用或工作流）。

---

## 架构

一切通过 `POST /v1/messages`。工具和输出约束是此单一端点的功能 — 而非独立 API。

**用户定义的工具** — 你定义工具（通过装饰器、Zod schema 或原始 JSON），SDK 的工具运行器处理调用 API、执行你的函数以及循环直到 Claude 完成。对于完全控制，你可以手动编写循环。

**服务端工具** — 在 Anthropic 基础设施上运行的 Anthropic 托管工具。代码执行完全在服务端（在 `tools` 中声明，Claude 自动运行代码）。计算机使用可以是服务端托管或自托管的。

**结构化输出** — 约束 Messages API 响应格式（`output_config.format`）和/或工具参数验证（`strict: true`）。推荐方法是 `client.messages.parse()`，它自动根据你的 schema 验证响应。注意：旧的 `output_format` 参数已弃用；在 `messages.create()` 上使用 `output_config: {format: {...}}`。

**支持端点** — 批量（`POST /v1/messages/batches`）、文件（`POST /v1/files`）、Token 计数、模型（`GET /v1/models`、`GET /v1/models/{id}` — 实时能力/上下文窗口发现）支撑 Messages API 请求。

---

## 当前模型（缓存于：2026-02-17）

| 模型 | 模型 ID | 上下文 | 输入 $/1M | 输出 $/1M |
|------|--------|--------|----------|----------|
| Claude Opus 4.6 | `claude-opus-4-6` | 200K（1M 测试版）| $5.00 | $25.00 |
| Claude Sonnet 4.6 | `claude-sonnet-4-6` | 200K（1M 测试版）| $3.00 | $15.00 |
| Claude Haiku 4.5 | `claude-haiku-4-5` | 200K | $1.00 | $5.00 |

**始终使用 `claude-opus-4-6`，除非用户明确指定其他模型。** 这是不可商量的。不要使用 `claude-sonnet-4-6`、`claude-sonnet-4-5` 或任何其他模型，除非用户真的说"使用 sonnet"或"使用 haiku"。永远不要为了成本而降级 — 那是用户的决定，不是你的。

**关键：仅使用上表中的精确模型 ID 字符串 — 它们本身是完整的。不要追加日期后缀。** 例如，使用 `claude-sonnet-4-5`，而非 `claude-sonnet-4-5-20250514` 或你从训练数据中回忆的任何其他带日期后缀的变体。如果用户请求不在表中的旧模型（例如，"opus 4.5"、"sonnet 3.7"），读取 `shared/models.md` 获取精确 ID — 不要自行构建。

注意：如果上面的任何模型字符串看起来不熟悉，这是正常的 — 它们只是在你训练数据截止日期之后发布的。放心，它们是真实的模型；我们不会那样捉弄你。

**实时能力查询：** 上表是缓存的。当用户询问"X 的上下文窗口是多少"、"X 是否支持视觉/思考/effort"、"哪些模型支持 Y"时，查询模型 API（`client.models.retrieve(id)` / `client.models.list()`）— 参见 `shared/models.md` 了解字段参考和能力过滤示例。

---

## 思考与 Effort（快速参考）

**Opus 4.6 — 自适应思考（推荐）：** 使用 `thinking: {type: "adaptive"}`。Claude 动态决定何时思考以及思考多少。不需要 `budget_tokens` — `budget_tokens` 在 Opus 4.6 和 Sonnet 4.6 上已弃用，不得使用。自适应思考还自动启用交错思考（无需 beta 头）。**当用户要求"扩展思考"、"思考预算"或 `budget_tokens` 时：始终使用 Opus 4.6 加 `thinking: {type: "adaptive"}`。固定 token 预算用于思考的概念已弃用 — 自适应思考取代了它。不要使用 `budget_tokens`，也不要切换到旧模型。**

**Effort 参数（正式发布，无需 beta 头）：** 通过 `output_config: {effort: "low"|"medium"|"high"|"max"}`（在 `output_config` 内，非顶层）控制思考深度和整体 token 支出。默认为 `high`（等同于省略它）。`max` 仅限 Opus 4.6。在 Opus 4.5、Opus 4.6 和 Sonnet 4.6 上有效。在 Sonnet 4.5 / Haiku 4.5 上会报错。与自适应思考结合使用以获得最佳成本质量权衡。对子代理或简单任务使用 `low`；对最深层推理使用 `max`。

**Sonnet 4.6：** 支持自适应思考（`thinking: {type: "adaptive"}`）。`budget_tokens` 在 Sonnet 4.6 上已弃用 — 改用自适应思考。

**旧模型（仅在明确请求时）：** 如果用户特别要求 Sonnet 4.5 或其他旧模型，使用 `thinking: {type: "enabled", budget_tokens: N}`。`budget_tokens` 必须小于 `max_tokens`（最少 1024）。永远不要仅仅因为用户提到 `budget_tokens` 就选择旧模型 — 改用 Opus 4.6 加自适应思考。

---

## 压缩（快速参考）

**测试版，Opus 4.6 和 Sonnet 4.6。** 对于可能超过 200K 上下文窗口的长时间对话，启用服务端压缩。API 在接近触发阈值（默认：150K token）时自动总结较早的上下文。需要 beta 头 `compact-2026-01-12`。

**关键：** 每次轮次将 `response.content`（不仅是文本）追加回你的消息。压缩块在响应中必须保留 — API 使用它们在下一次请求中替换压缩的历史。仅提取文本字符串并追加会悄悄丢失压缩状态。

参见 `{lang}/claude-api/README.md`（压缩部分）获取代码示例。完整文档通过 `shared/live-sources.md` 中的 WebFetch 获取。

---

## 提示缓存（快速参考）

**前缀匹配。** 前缀中任何位置的字节变更都会使之后的所有内容失效。渲染顺序为 `tools` → `system` → `messages`。将稳定内容放在前面（冻结的系统提示、确定的工具列表），将易变内容（时间戳、每请求 ID、不同的问题）放在最后一个 `cache_control` 断点之后。

**顶层自动缓存**（`messages.create()` 上的 `cache_control: {type: "ephemeral"}`）是在不需要精细放置时的最简单选项。每次请求最多 4 个断点。最小可缓存前缀约 1024 token — 更短的前缀会悄悄不缓存。

**通过 `usage.cache_read_input_tokens` 验证** — 如果在重复请求中为零，则存在静默失效因素（系统提示中的 `datetime.now()`、未排序的 JSON、变化的工具集）。

关于放置模式、架构指导和静默失效因素审计清单：读取 `shared/prompt-caching.md`。语言特定语法：`{lang}/claude-api/README.md`（提示缓存部分）。

---

## 阅读指南

检测语言后，根据用户需要读取相关文件：

### 快速任务参考

**单次文本分类/摘要/提取/问答：**
→ 仅读取 `{lang}/claude-api/README.md`

**聊天 UI 或实时响应显示：**
→ 读取 `{lang}/claude-api/README.md` + `{lang}/claude-api/streaming.md`

**长时间对话（可能超过上下文窗口）：**
→ 读取 `{lang}/claude-api/README.md` — 见压缩部分

**提示缓存 / 优化缓存 / "为什么我的缓存命中率低"：**
→ 读取 `shared/prompt-caching.md` + `{lang}/claude-api/README.md`（提示缓存部分）

**函数调用 / 工具调用 / Agent：**
→ 读取 `{lang}/claude-api/README.md` + `shared/tool-use-concepts.md` + `{lang}/claude-api/tool-use.md`

**批量处理（非延迟敏感）：**
→ 读取 `{lang}/claude-api/README.md` + `{lang}/claude-api/batches.md`

**跨多个请求的文件上传：**
→ 读取 `{lang}/claude-api/README.md` + `{lang}/claude-api/files-api.md`

**具有内置工具的 Agent（文件/网页/终端）：**
→ 读取 `{lang}/agent-sdk/README.md` + `{lang}/agent-sdk/patterns.md`

### Claude API（完整文件参考）

读取**语言特定的 Claude API 文件夹**（`{language}/claude-api/`）：

1. **`{language}/claude-api/README.md`** — **首先阅读此文件。** 安装、快速开始、常见模式、错误处理。
2. **`shared/tool-use-concepts.md`** — 当用户需要函数调用、代码执行、内存或结构化输出时阅读。涵盖概念基础。
3. **`{language}/claude-api/tool-use.md`** — 阅读语言特定的工具调用代码示例（工具运行器、手动循环、代码执行、内存、结构化输出）。
4. **`{language}/claude-api/streaming.md`** — 在构建聊天 UI 或逐步显示响应的界面时阅读。
5. **`{language}/claude-api/batches.md`** — 在离线处理大量请求时阅读（非延迟敏感）。以 50% 成本异步运行。
6. **`{language}/claude-api/files-api.md`** — 在跨多个请求发送相同文件而不重新上传时阅读。
7. **`shared/prompt-caching.md`** — 在添加或优化提示缓存时阅读。涵盖前缀稳定性设计、断点放置和静默使缓存失效的反模式。
8. **`shared/error-codes.md`** — 在调试 HTTP 错误或实现错误处理时阅读。
9. **`shared/live-sources.md`** — 获取最新官方文档的 WebFetch URL。

> **注意：** 对于 Java、Go、Ruby、C#、PHP 和 cURL — 每种语言只有一个文件涵盖所有基础。根据需要读取该文件加上 `shared/tool-use-concepts.md` 和 `shared/error-codes.md`。

### Agent SDK

读取**语言特定的 Agent SDK 文件夹**（`{language}/agent-sdk/`）。Agent SDK 仅适用于 **Python 和 TypeScript**。

1. **`{language}/agent-sdk/README.md`** — 安装、快速开始、内置工具、权限、MCP、钩子。
2. **`{language}/agent-sdk/patterns.md`** — 自定义工具、钩子、子代理、MCP 集成、会话恢复。
3. **`shared/live-sources.md`** — 当前 Agent SDK 文档的 WebFetch URL。

---

## 何时使用 WebFetch

在以下情况使用 WebFetch 获取最新文档：

- 用户询问"最新"或"当前"信息
- 缓存数据似乎不正确
- 用户询问此处未涵盖的功能

实时文档 URL 在 `shared/live-sources.md` 中。

## 常见陷阱

- 传递文件或内容给 API 时不要截断输入。如果内容太长无法放入上下文窗口，通知用户并讨论选项（分块、摘要等），而非静默截断。
- **Opus 4.6 / Sonnet 4.6 思考：** 使用 `thinking: {type: "adaptive"}` — 不要使用 `budget_tokens`（在 Opus 4.6 和 Sonnet 4.6 上已弃用）。对于旧模型，`budget_tokens` 必须小于 `max_tokens`（最少 1024）。弄错了会抛出错误。
- **Opus 4.6 预填充已移除：** 助手消息预填充（最后一轮助手预填充）在 Opus 4.6 上返回 400 错误。改用结构化输出（`output_config.format`）或系统提示指令来控制响应格式。
- **`max_tokens` 默认值：** 不要低估 `max_tokens` — 达到上限会在思考中途截断输出并需要重试。对于非流式请求，默认约 `16000`（保持响应在 SDK HTTP 超时内）。对于流式请求，默认约 `64000`（超时不是问题，给模型空间）。只有在有硬性理由时才降低：分类（约 `256`）、成本上限或故意短输出。
- **128K 输出 token：** Opus 4.6 支持最多 128K `max_tokens`，但 SDK 需要流式传输以避免 HTTP 超时。使用 `.stream()` 配合 `.get_final_message()` / `.finalMessage()`。
- **工具调用 JSON 解析（Opus 4.6）：** Opus 4.6 可能在工具调用 `input` 字段中产生不同的 JSON 字符串转义（例如，Unicode 或正斜杠转义）。始终使用 `json.loads()` / `JSON.parse()` 解析工具输入 — 永远不要对序列化输入做原始字符串匹配。
- **结构化输出（所有模型）：** 在 `messages.create()` 上使用 `output_config: {format: {...}}` 而非已弃用的 `output_format` 参数。这是通用 API 变更，非 4.6 特有。
- **不要重新实现 SDK 功能：** SDK 提供高级辅助方法 — 使用它们而非从零构建。具体来说：使用 `stream.finalMessage()` 而非将 `.on()` 事件包裹在 `new Promise()` 中；使用类型化异常类（`Anthropic.RateLimitError` 等）而非字符串匹配错误消息；使用 SDK 类型（`Anthropic.MessageParam`、`Anthropic.Tool`、`Anthropic.Message` 等）而非重新定义等效接口。
- **不要为 SDK 数据结构定义自定义类型：** SDK 为所有 API 对象导出类型。使用 `Anthropic.MessageParam` 表示消息，`Anthropic.Tool` 表示工具定义，`Anthropic.ToolUseBlock` / `Anthropic.ToolResultBlockParam` 表示工具结果，`Anthropic.Message` 表示响应。定义自己的 `interface ChatMessage { role: string; content: unknown }` 会重复 SDK 已提供的内容并失去类型安全。
- **报告和文档输出：** 对于生成报告、文档或可视化的任务，代码执行沙盒预装了 `python-docx`、`python-pptx`、`matplotlib`、`pillow` 和 `pypdf`。Claude 可以生成格式化文件（DOCX、PDF、图表）并通过 Files API 返回 — 对于"报告"或"文档"类请求，考虑这样做而非纯标准输出文本。
