# 模板

可复制到任意项目的模板文件。

## 使用方法

把你需要的模板复制到目标项目的对应位置，然后填入实际内容。

| 模板 | 复制到 | 作用 |
|------|--------|------|
| `AGENTS.md.template` | 项目根目录，重命名为 `AGENTS.md` | 跨工具通用的 AI 入口（Codex/Cursor/Cline/Aider/Copilot 等 20+ 工具收敛的事实标准），统一指向 `CONTEXT.md` 和 `docs/adr/` |
| `CONTEXT.md.template` | 项目根目录，重命名为 `CONTEXT.md` | 建立你和 AI 之间的共享领域语言 |
| `ADR-FORMAT.md` | 作为参考，在项目的 `docs/adr/` 下写具体决策 | 记录重要的架构决策，防止 AI 反复提议已否决的方案 |

## 快速开始

```bash
# 1. 项目根目录放 AGENTS.md（所有 AI 工具的统一入口）
cp templates/AGENTS.md.template /path/to/your-project/AGENTS.md

# 2. 项目根目录放 CONTEXT.md（领域语言）
cp templates/CONTEXT.md.template /path/to/your-project/CONTEXT.md

# 3. 需要记录架构决策时
mkdir -p /path/to/your-project/docs/adr
# 参照 ADR-FORMAT.md 格式写 docs/adr/0001-你的决策.md
```

## 三者的层次关系

```
AGENTS.md           → AI 进项目读的第一份文件（跨工具通用的入口）
  ├─ 指向 CONTEXT.md  → 领域词汇表
  └─ 指向 docs/adr/   → 不可重提的决策档案
```
