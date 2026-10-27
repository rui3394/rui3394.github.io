---
title: GitHub Pages 部署
description: 知识库如何做到"只写 Markdown、推送到 GitHub 就自动发布"——GitHub Actions 自动构建全流程。
tags:
  - 学习笔记
  - GitHub
  - CI/CD
---

# GitHub Pages 部署

这个网站**没有服务器**——它是一堆静态 HTML 文件，由 GitHub 免费托管在全世界各地的 CDN 上，24 小时可访问。本文记录它的工作原理。

## 发布流程

```text
我写笔记            推送              GitHub Actions         GitHub Pages
content/*.md  ──►  git push  ──►  云端自动构建出 HTML  ──►  全球 CDN 分发
（本地零服务）      （唯一命令）     （GitHub 的机器跑）      （24h 在线）
```

整个过程里我的电脑只负责两件事：**写文件、推代码**。构建发生在 GitHub 的服务器上（`.github/workflows/deploy.yml` 定义了步骤），所以本机不需要安装任何运行环境。

## 关键配置

### 1. 部署工作流 `.github/workflows/deploy.yml`

监听 `main` 分支的 push，触发后：

1. 检出代码（拉全量历史，用于显示笔记的创建/修改日期）
2. 安装 Node.js 24 + 项目依赖
3. 安装 Quartz 插件（`npx quartz plugin install --from-config`）
4. 执行 `npx quartz build`，把 `content/` 里的 Markdown 编译成 `public/` 下的静态网站
5. 把 `public/` 发布到 GitHub Pages

### 2. 仓库设置

GitHub 仓库 → **Settings → Pages → Build and deployment → Source** 必须选择 **GitHub Actions**（而不是默认的 "Deploy from a branch"）。

### 3. 站点地址

地址格式为 `https://<用户名>.github.io/<仓库名>/`，对应 `quartz.config.yaml` 里的 `baseUrl` 字段——所以仓库建好后要把 baseUrl 改成自己的地址，RSS 和 sitemap 才能正常工作。

## 为什么选静态站 + GitHub Pages

| 对比项 | 静态站（本站） | 传统动态网站 |
|--------|--------------|-------------|
| 服务器 | 不需要 | 要买/租，要运维 |
| 费用 | **免费**（公开仓库） | 每月几十元起 |
| 安全 | 没有后端可攻击 | 面攻击 |
| 速度 | 纯静态 + CDN，极快 | 取决于服务器 |

> [!warning] 注意公开性
> GitHub Pages 免费托管要求仓库是**公开**的——推上来的所有笔记全世界可见。不适合公开的内容放进 `content/private/` 文件夹（已被配置忽略，不会构建也不会被 git 提交）。

## 踩坑记录

- ~~本地 Node 18 跑不了构建~~：Quartz v5 要求 Node ≥ 22，但反正构建在云端做，本地版本无所谓
- 代理环境下 git 推送不稳定：TLS 握手失败时换节点或直连

## 相关笔记

- [[Quartz建站指南|Quartz 建站指南]]：日常怎么写笔记
