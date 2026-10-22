# V3.0: 模块化治理与资产解耦

## 🎯 阶段目标
将 V2 中高度耦合在 Dify Workflow 画布中的节点，重构为**平台无关的独立 AI Skill（技能）**，并预留 MCP (Model Context Protocol) 接口，为未来接入更广泛的 Agent 生态做准备。

## 🏗️ 架构设计：从 Pipeline 到 Skill Registry

在 V2 阶段，业务逻辑被写死在画布的节点中（Vendor Lock-in）。在 V3 阶段，我们进行了“微服务化”改造：
1. **逻辑抽离**：将节点内的系统提示词（System Prompt）抽离为独立的 `.md` 文件。
2. **契约前置**：将节点的输出约束抽离为标准的 `.json` Schema 文件。
3. **协议封装**：编写 `mcp_manifest.json`，向外部调度系统声明技能的能力、入参和出参。

### 目录结构
- `mcp_manifest.json`：标准化技能清单。
- `skills/jd_parser/`：JD 解析技能（包含 Prompt 与 Schema）。
- `skills/comprehensive_evaluator/`：综合评估技能（包含 Prompt 与 Schema）。

## 💡 为什么这么做？(Design Decisions)
1. **跨平台复用**：这些 `.md` 和 `.json` 资产不仅可以在 Dify 中使用，还可以无缝接入 LangChain、AutoGen 或企业自研的 Agent 框架。
2. **拥抱 MCP 协议**：通过 `mcp_manifest.json`，系统具备了被类似 Windows Copilot 这种超级智能体直接调用的潜力。
3. **独立测试与迭代**：每一个 Skill 都可以脱离庞大的 Workflow，单独进行测试与回归评估。

## 🚀 如何使用
任何支持读取 JSON Schema 的大模型框架，均可直接读取 `skills/` 目录下的资产。通过传入对应的输入参数，即可获得高度结构化的 AI 返回结果。