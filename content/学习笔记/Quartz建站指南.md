---
title: Quartz 建站指南
description: 这个知识库网站本身是怎么搭起来的：Quartz 的目录结构、写作方式、常用语法。
tags:
  - 学习笔记
  - Quartz
  - 数字花园
---

# Quartz 建站指南

这是这个网站自己的"出生证明" 🌱。记录一下 [[GitHub-Pages部署|GitHub Pages 部署]] 之外，日常使用 Quartz 写笔记需要知道的事。

## 目录结构

```text
blog/
├── content/            ← ✍️ 你的所有笔记都放这里（唯一需要关心的目录）
│   ├── index.md        首页
│   ├── 阅读笔记/       文件夹会自动生成分类页
│   └── 学习笔记/
├── quartz.config.yaml  站点配置（站名、语言、主题、插件）
├── quartz/             Quartz 引擎源码（不用动）
└── .github/workflows/  自动部署脚本（不用动）
```

## 日常写作

1. 在 `content/` 下新建一个 `.md` 文件（中文名没问题）
2. 顶部写 frontmatter（都是可选的，但建议写 title 和 tags）：

```yaml
---
title: 笔记标题
description: 一句话描述（会显示在预览卡片里）
tags:
  - 学习笔记
---
```

3. 正文用 Markdown 写，push 到 GitHub 后 1-2 分钟自动上线

## 常用语法

### 双链（最重要的功能）

```markdown
这篇笔记受到了 [[卡片笔记写作法]] 的启发
也可以用别名：[[卡片笔记写作法|卡片盒方法]]
```

- 被链接的页面右侧会出现**反向链接**（谁引用了我）
- 全站的关系**图谱**也由双链生成

### 标注块（Callout）

```markdown
> [!tip] 这是提示
> [!note] 这是笔记
> [!warning] 这是警告
> [!abstract] 摘要框
```

### 其他

- 数学公式（LaTeX）：`$E = mc^2$` 和 `$$\int_0^1 x dx$$`
- 代码高亮、表格、任务列表 `- [x]`、脚注 `[^1]` 都支持
- 图片放到 `content/` 下任意位置，用 `![[图片名.png]]` 引用（Obsidian 语法）

## 本地预览（可选）

平时不需要预览，push 上去直接看线上效果就行。想本地看的话（需要 Node.js ≥ 22）：

```bash
npm install
npx quartz plugin install --from-config
npx quartz build --serve   # 打开 http://localhost:8080
```

## 相关笔记

- [[GitHub-Pages部署|GitHub Pages 部署]]：网站的发布流程
- [[卡片笔记写作法]]：这个网站的组织方式为什么是"网"而不是"文件夹"
