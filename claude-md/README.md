# CLAUDE.md 模板集

按项目类型预制的 CLAUDE.md 模板。选一个匹配你项目的，复制过去再定制。

## 可用模板

| 模板 | 适用于 | 核心规则 |
|------|--------|----------|
| `vue-frontend.md` | Vue 3 + Vite 前端项目 | Composition API、Pinia、Ant Design Vue 规范 |
| `h5-mobile.md` | H5 移动端应用（WebView） | 移动优先、vw/vh 单位、原生桥接处理 |

## 使用方法

1. 选一个最接近你项目类型的模板
2. 复制到项目根目录并重命名为 `CLAUDE.md`
3. 修改里面的项目名称等占位符
4. AI 代理在每次会话开始时会自动读取这个文件

```bash
cp claude-md/vue-frontend.md /path/to/your-project/CLAUDE.md
```

## CLAUDE.md 和 CONTEXT.md 的区别

| CLAUDE.md | CONTEXT.md |
|-----------|------------|
| **技术规则**：编码风格、命名规范、框架约定 | **领域语言**：业务术语、概念关系 |
| 告诉 AI **必须/禁止**做什么 | 告诉 AI **这个词在项目里是什么意思** |
| 项目建立时写好，很少变 | 随着项目推进不断丰富 |
| 同类项目可复用 | 每个项目独有 |
