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
│   ├── finishing-a-development-branch/  # 开发分支完成流程
│   ├── verification-before-completion/  # 完成前验证
│   ├── coding-standards/      # 通用编码标准
│   ├── api-design/            # REST API 设计模式
│   ├── backend-patterns/      # 后端架构模式
│   ├── frontend-patterns/     # 前端开发模式（React/Next.js）
│   ├── springboot-patterns/   # Spring Boot 架构模式
│   ├── improve-codebase-architecture/  # 架构改善
│   ├── ui-ux-pro-max/         # UI/UX 设计智能
│   ├── ui-styling/            # shadcn/ui + Tailwind 样式
│   ├── frontend-design/       # 前端设计实现
│   ├── design/                # 综合设计（Logo、CIP、幻灯片）
│   ├── design-system/         # 设计系统 Token 架构
│   ├── defuddle/              # 网页内容提取
│   ├── json-canvas/           # Obsidian Canvas 文件
│   ├── obsidian-markdown/     # Obsidian Markdown 语法
│   ├── obsidian-bases/        # Obsidian Bases 数据库视图
│   ├── obsidian-cli/          # Obsidian CLI 交互
│   ├── zoom-out/              # 全局视角
│   ├── caveman/               # 原始人压缩模式
│   └── README.md              # 技能使用说明
│
├── claude-md/                 # CLAUDE.md 模板集（项目级 AI 行为规则）
│   ├── vue-frontend.md        # Vue 3 前端项目模板
│   ├── h5-mobile.md           # H5 移动端项目模板
│   └── README.md
│
├── claude-agents/             # ~/.claude/agents 代理角色（专项任务执行者）
│   ├── product-manager.md    # 产品经理（Opus，需求挖掘与举一反三）
│   ├── planner.md             # 规划师（Opus，复杂功能拆解）
│   ├── architect.md           # 架构师（Opus，系统设计）
│   ├── code-reviewer.md      # 通用代码审查（Sonnet）
│   ├── java-reviewer.md      # Java/Spring Boot 审查（Sonnet）
│   ├── typescript-reviewer.md # TypeScript/JS 审查（Sonnet）
│   ├── build-error-resolver.md # 构建错误修复（Sonnet）
│   └── README.md
│
├── claude-rules/              # ~/.claude/rules 规则集（全局 AI 行为约束）
│   ├── common/                # 通用规则（所有语言适用）
│   │   ├── agents.md          # Agent 编排与并行执行
│   │   ├── code-review.md     # 代码审查标准与流程
│   │   ├── coding-style.md    # 编码风格（不可变性、文件组织）
│   │   ├── development-workflow.md  # 开发工作流（研究→计划→TDD→审查）
│   │   ├── git-workflow.md    # Git 提交与 PR 规范
│   │   ├── hooks.md           # Hooks 系统配置
│   │   ├── patterns.md        # 通用设计模式
│   │   ├── performance.md     # 性能优化与模型选择策略
│   │   ├── security.md        # 安全检查清单
│   │   └── testing.md         # 测试要求（80%+ 覆盖率）
│   ├── java/                  # Java 特定规则（条件加载 *.java）
│   │   ├── coding-style.md    # Java 编码风格与现代特性
│   │   ├── hooks.md           # Java 构建 Hooks
│   │   ├── patterns.md        # Java 设计模式（Repository、Builder 等）
│   │   ├── security.md        # Java 安全（SQL 注入、输入验证）
│   │   └── testing.md         # Java 测试（JUnit 5、Testcontainers）
│   ├── typescript/            # TypeScript/JS 特定规则（条件加载 *.ts/*.tsx）
│   │   ├── coding-style.md    # TS 类型系统与编码风格
│   │   ├── hooks.md           # TS Hooks（Prettier、tsc）
│   │   ├── patterns.md        # TS 模式（API Response、Custom Hooks）
│   │   ├── security.md        # TS 安全
│   │   └── testing.md         # TS 测试（Playwright E2E）
│   └── README.md
│
├── templates/                 # 可复制到任意项目的模板
│   ├── CONTEXT.md.template    # 共享语言模板
│   ├── ADR-FORMAT.md          # 架构决策记录格式
│   └── README.md
│
└── README.md                  # 本文件
```

## 五层体系

| 层 | 目录 | 作用 | 作用域 |
|---|------|------|--------|
| **Rules** | `claude-rules/` | 告诉 AI **全局行为约束和编码标准** | 用户级，所有项目生效 |
| **Agents** | `claude-agents/` | 告诉 AI **谁来做专项任务** | 用户级，按需调用 |
| **Skills** | `skills/` | 告诉 AI **怎么工作** | 跨项目通用 |
| **CLAUDE.md** | `claude-md/` | 告诉 AI **在这类项目里该遵守什么** | 按项目类型复用 |
| **Templates** | `templates/` | 告诉 AI **这个项目的业务语言和历史决策** | 每个项目独有 |

## 快速开始

### 1. 部署全局规则

```bash
# 将规则复制到 Claude Code 的全局规则目录
cp -r claude-rules/* ~/.claude/rules/
```

### 2. 部署 Agent 角色

```bash
# 将代理角色复制到 Claude Code 的 agents 目录
cp claude-agents/*.md ~/.claude/agents/
```

### 3. 复制技能到你的项目

将 `skills/` 目录下需要的技能复制到目标项目的 `.agent/skills/` 目录。

### 4. 配置 CLAUDE.md

```bash
# 选择匹配你项目类型的模板
cp claude-md/vue-frontend.md /path/to/your-project/CLAUDE.md
# 然后编辑其中的项目名称等占位符
```

### 5. 建立共享语言（可选但强烈推荐）

```bash
cp templates/CONTEXT.md.template /path/to/your-project/CONTEXT.md
# 随着项目推进，逐步填充领域术语
```

### 6. 记录架构决策（按需）

```bash
mkdir -p /path/to/your-project/docs/adr
# 参照 templates/ADR-FORMAT.md 格式创建决策记录
```
