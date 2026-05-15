# Claude Rules

这是 `~/.claude/rules` 目录中的规则文件集合，用于指导 Claude Code 在不同场景下的行为。

## 目录结构

```
claude-rules/
├── common/          # 通用规则（适用于所有语言）
│   ├── agents.md
│   ├── code-review.md
│   ├── coding-style.md
│   ├── development-workflow.md
│   ├── git-workflow.md
│   ├── hooks.md
│   ├── patterns.md
│   ├── performance.md
│   ├── security.md
│   └── testing.md
├── java/            # Java 特定规则
│   ├── coding-style.md
│   ├── hooks.md
│   ├── patterns.md
│   ├── security.md
│   └── testing.md
└── typescript/      # TypeScript/JavaScript 特定规则
    ├── coding-style.md
    ├── hooks.md
    ├── patterns.md
    ├── security.md
    └── testing.md
```

## 使用方式

将这些文件放入 `~/.claude/rules/` 目录中即可生效。

- `common/` 中的规则始终加载
- `java/` 和 `typescript/` 中的规则通过 `paths` 前置元数据按文件类型条件加载

## 规则层级

语言特定规则通过 `> This file extends [common/xxx.md]` 标注继承关系，在通用规则基础上补充语言特定的最佳实践。
