# 🌱 我的知识库

基于 [Quartz v5](https://quartz.jzhao.xyz) 搭建的个人知识库网站：阅读笔记、学习笔记，以及一切值得连接起来的想法。托管在 **GitHub Pages** 上，免费、无服务器、24 小时可访问。

```text
写 Markdown 笔记 ──► git push ──► GitHub Actions 云端构建 ──► 网站自动更新
   （本地零服务）                （GitHub 的机器执行）
```

## 一次性上线（5 分钟）

1. **改配置**：打开 `quartz.config.yaml`，把 `baseUrl` 改成你自己的地址，格式 `用户名.github.io/仓库名`（例如 `zhangsan.github.io/notes`）；顺手把 `pageTitle` 和页脚的 `GitHub 仓库` 链接改成自己的
2. **建仓库**：在 GitHub 上新建一个**公开**仓库（名字随意，比如 `notes`；不要初始化 README）
3. **设置部署方式**：仓库页面 → `Settings` → `Pages` → `Build and deployment` → `Source` 选 **GitHub Actions**
4. **推送代码**：在本目录执行（把 URL 换成你的仓库地址）：

   ```bash
   git init -b main        # 如果还没初始化
   git add .
   git commit -m "init: 我的知识库"
   git remote add origin https://github.com/你的用户名/你的仓库名.git
   git push -u origin main
   ```

5. **等 1~2 分钟**：仓库的 `Actions` 标签页里可以看到构建进度，绿色 ✓ 后访问 `https://你的用户名.github.io/仓库名/`

## 日常写作

- 笔记全部放在 `content/` 目录下，新建 `.md` 文件即可，中文名没问题
- 文件夹会自动生成分类页（如 `阅读笔记/`）
- 用 `[[笔记名]]` 建立双链，右侧自动出现反向链接和关系图谱
- 用 frontmatter 写 `title` / `description` / `tags`
- 写完 `git add . && git commit -m "笔记标题" && git push`，1~2 分钟后上线
- 也可以直接在 GitHub 网页上新建/编辑 Markdown 文件，同样会触发自动部署——**完全不用本机环境**

> [!WARNING]
> - **公开性**：GitHub Pages 免费托管要求仓库公开，所有提交的内容全世界可见。不想公开的笔记放进 `content/private/`（已被配置忽略：既不构建也不会被 git 提交）
> - **图片**：放 `content/` 下任意位置，用 `![[图片.png]]` 引用；注意仓库单文件限 100MB，图片建议压缩
> - **本地预览是可选的**：需要 Node.js ≥ 22（`npm install` → `npx quartz plugin install --from-config` → `npx quartz build --serve`）。日常不预览、push 后直接看线上效果完全没问题

## 目录结构

```text
├── content/              # ✍️ 你的笔记（唯一需要日常关心的目录）
│   ├── index.md          # 首页
│   ├── 阅读笔记/
│   ├── 学习笔记/
│   └── private/          # 不公开的笔记（被忽略，不会发布）
├── quartz.config.yaml    # 站点配置：站名、语言、主题、插件、baseUrl
├── .github/workflows/    # 自动部署脚本（不用动）
└── quartz/               # Quartz 引擎源码（不用动，升级时整体替换）
```

## 常见问题

- **构建失败**：看仓库 `Actions` 页的红色 ✗ → 点进去看日志；最常见原因是 `quartz.config.yaml` 改错（YAML 缩进）
- **改了配置没生效**：确认 push 的是 `main` 分支（工作流只监听 `main`）
- **升级 Quartz**：参考官方 [upgrading 文档](https://quartz.jzhao.xyz/upgrading)；`content/` 和 `quartz.config.yaml` 是你的东西，其余整体更新

## 致谢

本站基于 [jackyzha0/quartz](https://github.com/jackyzha0/quartz)（MIT License）搭建，感谢这个出色的开源项目。
