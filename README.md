# p9-dev-doc-summarizer

一个用于“开发文档归纳”的通用技能模板：
- 以资深工程师视角总结背景、改动逻辑、代码影响、风险与验证计划
- 支持按需生成流程图（`mermaid` / `draw`）
- 支持输出到本地或知识库（如语雀）
- 明确只读归纳：不修改被归纳的源文件

## Features

- Clarification-first：先确认归纳文件、输出目标、图模式、受众语言
- Read-only summarization：只读取源文件，不改动源文件
- Structured output：统一输出结构，适合评审/沉淀
- Diagram templates：内置 Mermaid 和 Draw 模版库
- Minimal-diff diagram strategy：稳定节点 ID，减少后续改图成本

## Quick Start

1. 准备需要归纳的文档或代码文件路径
2. 选择输出参数：
   - 输出位置：`local` / `yuque` / `other`
   - 图模式：`mermaid` / `draw`
   - 输出语言与受众：如 `zh-CN + engineering`
3. 按 [SKILL.md](./SKILL.md) 工作流执行

## Output Contract

建议输出结构：
1. Executive Summary
2. Background
3. Change Logic
4. Code Impact
5. Risks and Mitigations
6. Validation Plan
7. Diagram (optional)
8. Destination Metadata

## Templates

- Mermaid quick templates: [references/diagram-templates.md](./references/diagram-templates.md)
- Mermaid library: [references/mermaid-template-library.md](./references/mermaid-template-library.md)
- Draw.io library: [references/drawio-template-library.md](./references/drawio-template-library.md)
- Publish payload templates: [references/publish-templates.md](./references/publish-templates.md)
- CC integration template: [references/cc-integration-template.md](./references/cc-integration-template.md)

## Repo Structure

```text
p9-dev-doc-summarizer/
├── SKILL.md
├── README.md
└── references/
    ├── cc-integration-template.md
    ├── diagram-templates.md
    ├── drawio-template-library.md
    ├── mermaid-template-library.md
    └── publish-templates.md
```

## Example Use Cases

- 新增 Controller + 数据表 CRUD 的需求归纳
- 定时任务批量刷新状态的改动归纳
- 代码评审前的改动影响梳理与流程图输出

## Guardrails

- 不修改被归纳的源文件
- 事实与推断分离（推断需标注）
- 证据不足时，明确指出缺失信息与下一步检查项

## License

按你的仓库策略补充（MIT/Apache-2.0/Proprietary）。
