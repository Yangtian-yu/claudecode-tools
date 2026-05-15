# Claude Agents（AI 代理角色）

Agents 是预定义的 AI 代理角色，每个 Agent 有特定的职责、工具权限和推荐模型。它们可以被主会话调用来执行专项任务。

## 使用方式

将这些文件放入 `~/.claude/agents/` 目录中即可生效。在 Claude Code 中通过 `/agent <name>` 或在规则中引用 agent 名称来调用。

## Agent 列表

| Agent | 模型 | 职责 | 触发场景 |
|-------|------|------|----------|
| `product-manager` | Opus | 产品经理（需求挖掘、举一反三） | 需求讨论、功能规划、用户故事 |
| `planner` | Opus | 实现规划专家 | 复杂功能开发、架构变更、大型重构 |
| `architect` | Opus | 系统架构师 | 新功能设计、技术决策、可扩展性评估 |
| `code-reviewer` | Sonnet | 通用代码审查 | 任何代码修改后立即使用 |
| `java-reviewer` | Sonnet | Java/Spring Boot 审查 | Java 代码变更 |
| `typescript-reviewer` | Sonnet | TypeScript/JS 审查 | TS/JS 代码变更 |
| `build-error-resolver` | Sonnet | 构建错误修复 | 编译失败、类型错误 |

## Agent 详情

### product-manager（产品经理）

**用途**：需求挖掘、功能定义、用户故事编写、优先级决策。

**工具权限**：Read, Grep, Glob（只读）

**核心特点**：
- 通过对话理解模糊想法，不急于给方案
- 举一反三：从一个点子推导完整功能矩阵和边界场景
- 主动补充你没想到的场景和异常情况
- 敢于挑战不合理需求，给出理由
- 输出 PRD、用户故事、优先级矩阵

**思维模型**：JTBD、RICE 评分、Kano 模型、用户故事地图、影响地图

---

### planner（规划师）

**用途**：将复杂需求拆解为可执行的实现计划。

**工具权限**：Read, Grep, Glob（只读，不修改代码）

**工作流程**：
1. 需求分析 → 理解功能、明确成功标准
2. 架构审查 → 分析现有代码结构、识别影响范围
3. 任务分解 → 按依赖关系排序、估算复杂度
4. 风险识别 → 列出潜在问题和缓解策略
5. 输出计划文档

---

### architect（架构师）

**用途**：系统设计、技术选型、架构决策。

**工具权限**：Read, Grep, Glob（只读）

**工作流程**：
1. 现状分析 → 审查现有架构、识别技术债务
2. 需求收集 → 功能性 + 非功能性需求
3. 方案设计 → 多方案对比、权衡取舍
4. 输出 ADR（架构决策记录）

---

### code-reviewer（代码审查员）

**用途**：通用代码质量、安全性、可维护性审查。

**工具权限**：Read, Grep, Glob, Bash

**审查优先级**：
- CRITICAL：安全漏洞、数据丢失风险
- HIGH：Bug、显著质量问题
- MEDIUM：可维护性问题
- LOW：风格建议

**置信度过滤**：只报告 >80% 确信的真实问题，不制造噪音。

---

### java-reviewer（Java 审查员）

**用途**：Java 和 Spring Boot 代码专项审查。

**工具权限**：Read, Grep, Glob, Bash

**重点关注**：
- SQL 注入、命令注入、路径遍历
- 硬编码密钥、PII 日志泄露
- 缺少 `@Valid` 验证
- 事务管理、并发安全
- JPA N+1 查询、连接池配置

---

### typescript-reviewer（TypeScript 审查员）

**用途**：TypeScript/JavaScript 代码专项审查。

**工具权限**：Read, Grep, Glob, Bash

**重点关注**：
- `eval` / `new Function` 注入
- 类型安全（避免 `any`）
- 异步错误处理
- XSS 防护
- 内存泄漏（未清理的订阅/定时器）

---

### build-error-resolver（构建错误修复）

**用途**：快速修复编译错误，让构建恢复绿色。

**工具权限**：Read, Write, Edit, Bash, Grep, Glob（可修改代码）

**原则**：
- 最小改动修复错误
- 不做架构变更
- 不做重构
- 只关注让构建通过

## 与 Rules 的协作

这些 Agent 在 `claude-rules/common/agents.md` 中被引用，定义了何时自动调用：

- 复杂功能请求 → 自动使用 `planner`
- 代码修改后 → 自动使用 `code-reviewer`
- Bug 修复 → 自动使用对应语言的 reviewer
- 构建失败 → 自动使用 `build-error-resolver`

## 模型选择策略

| 模型 | 适用场景 | Agent |
|------|----------|-------|
| **Opus** | 深度推理、复杂设计 | planner, architect |
| **Sonnet** | 代码分析、快速审查 | code-reviewer, java-reviewer, typescript-reviewer, build-error-resolver |
