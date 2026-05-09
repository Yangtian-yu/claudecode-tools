# claudecode-tools

我的 AI 工程工具箱 —— 集中管理 AI 编程代理的技能、规则和模板。

类比程序员的 dotfiles 仓库，但不是配置 vim/zsh，而是配置 AI 代理。

## 目录结构

```
claudecode-tools/
├── skills/                    # 技能指令（给 AI 的结构化工作流）
│   ├── brainstorming/         # 设计与头脑风暴
│   ├── writing-plans/         # 计划拆解
│   ├── test-driven-development/  # TDD 红绿重构
│   ├── systematic-debugging/  # 结构化调试
│   ├── requesting-code-review/  # 代码审查
│   ├── zoom-out/              # 全局视角
│   ├── improve-codebase-architecture/  # 架构改善
│   ├── caveman/               # 原始人压缩模式
│   ├── frontend-design/       # 前端设计
│   └── README.md              # 技能使用说明
│
├── claude-md/                 # CLAUDE.md 模板集（项目级 AI 行为规则）
│   ├── vue-frontend.md        # Vue 3 前端项目模板
│   ├── h5-mobile.md           # H5 移动端项目模板
│   └── README.md
│
├── templates/                 # 可复制到任意项目的模板
│   ├── CONTEXT.md.template    # 共享语言模板
│   ├── ADR-FORMAT.md          # 架构决策记录格式
│   └── README.md
│
└── README.md                  # 本文件
```

## 三层体系

| 层 | 目录 | 作用 | 作用域 |
|---|------|------|--------|
| **Skills** | `skills/` | 告诉 AI **怎么工作** | 跨项目通用 |
| **CLAUDE.md** | `claude-md/` | 告诉 AI **在这类项目里该遵守什么** | 按项目类型复用 |
| **Templates** | `templates/` | 告诉 AI **这个项目的业务语言和历史决策** | 每个项目独有 |

## 快速开始

### 1. 复制技能到你的项目

将 `skills/` 目录下需要的技能复制到目标项目的 `.agent/skills/` 目录。

### 2. 配置 CLAUDE.md

```bash
# 选择匹配你项目类型的模板
cp claude-md/vue-frontend.md /path/to/your-project/CLAUDE.md
# 然后编辑其中的项目名称等占位符
```

### 3. 建立共享语言（可选但强烈推荐）

```bash
cp templates/CONTEXT.md.template /path/to/your-project/CONTEXT.md
# 随着项目推进，逐步填充领域术语
```

### 4. 记录架构决策（按需）

```bash
mkdir -p /path/to/your-project/docs/adr
# 参照 templates/ADR-FORMAT.md 格式创建决策记录
```
