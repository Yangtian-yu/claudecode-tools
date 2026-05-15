# Skills（技能指令）

技能是给 AI 代理的结构化工作流指令，定义了在特定场景下应该如何思考和行动。

## 使用方式

将需要的技能目录复制到目标项目的 `.agent/skills/`（或 `~/.claude/skills/`）目录中。

## 技能列表

### 开发工作流

| 技能 | 说明 |
|------|------|
| `writing-plans/` | 计划拆解与实现规划 |
| `brainstorming/` | 设计与头脑风暴，含可视化伴侣 |
| `test-driven-development/` | TDD 红绿重构工作流 |
| `systematic-debugging/` | 结构化调试方法论 |
| `requesting-code-review/` | 代码审查请求流程 |
| `finishing-a-development-branch/` | 开发分支完成与合并流程 |
| `verification-before-completion/` | 完成前验证（证据先于断言） |

### 编码规范与模式

| 技能 | 说明 |
|------|------|
| `coding-standards/` | 通用编码标准（TS/JS/React/Node.js） |
| `api-design/` | REST API 设计模式（命名、状态码、分页、限流） |
| `backend-patterns/` | 后端架构模式（Repository、缓存、队列） |
| `frontend-patterns/` | 前端开发模式（React 组件、Hooks、状态管理） |
| `springboot-patterns/` | Spring Boot 架构模式（Java 后端） |
| `improve-codebase-architecture/` | 代码库架构改善 |

### UI/UX 与设计

| 技能 | 说明 |
|------|------|
| `ui-ux-pro-max/` | UI/UX 设计智能（50+ 风格、161 色板、57 字体配对） |
| `ui-styling/` | shadcn/ui + Tailwind CSS 样式系统 |
| `frontend-design/` | 前端设计实现 |
| `design/` | 综合设计（品牌、Logo、CIP、幻灯片、Banner、图标） |
| `design-system/` | 设计系统 Token 架构与组件规范 |

### 工具集成

| 技能 | 说明 |
|------|------|
| `find-skills/` | 发现和安装开源 Agent 技能（skills.sh 生态） |
| `defuddle/` | 网页内容提取（清洁 Markdown，节省 Token） |
| `json-canvas/` | Obsidian Canvas 文件创建与编辑 |
| `obsidian-markdown/` | Obsidian 风格 Markdown（wikilinks、callouts） |
| `obsidian-bases/` | Obsidian Bases 数据库视图 |
| `obsidian-cli/` | Obsidian CLI 交互与插件开发 |

### 思维模式

| 技能 | 说明 |
|------|------|
| `zoom-out/` | 全局视角审视 |
| `caveman/` | 原始人压缩模式（极简表达） |
