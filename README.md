# 张磊的技术笔记

基于 [Fuwari](https://github.com/saicaca/fuwari) 和 Astro 的中文技术博客。
站点地址：https://zhanglei1949.github.io/

## 本地运行

使用 Node.js 22 LTS 和 pnpm 9.14.4（版本由 package.json 指定）。

```sh
pnpm install --frozen-lockfile
pnpm dev
```

正式构建和预览（包含真实全文搜索）：

```sh
pnpm check
pnpm build
pnpm preview
```

开发模式的搜索使用主题的演示结果，检查搜索请使用正式构建预览。

## 写文章

```sh
pnpm new-post my-article
```

文章位于 `src/content/posts/`，使用 Markdown。典型文章头：

```yaml
---
title: 我的技术文章
published: 2026-09-30
description: 一句话说明文章内容
category: 工程实践
tags: [数据库, 性能优化]
draft: true
---
```

写好后将 `draft` 改为 `false`。文件名决定文章地址，发布后尽量不要改名；
需要固定地址时可以显式设置 `slug: my-article`。图片可以放在文章同目录，使用相对路径引用。

站点名称、头像、主题色、导航位于 `src/config.ts`；关于页位于
`src/content/spec/about.md`。当前头像为 `public/avatar-warrior.webp`（原图为同名 PNG）：梵高风格的孤独战士主题画作，由内置 imagegen 生成。

## 部署到 GitHub Pages

仓库 Settings → Pages → Build and deployment → Source 选择 GitHub Actions。
推送到现有 `master` 分支后，工作流会检查、构建、生成搜索索引并发布 `dist/`。
PR 只执行检查与构建，不发布。无需提交 `dist/` 或 `node_modules/`。

本地完成迁移并不代表已上线；远端发布需另行执行。

## 迁移记录

- Fuwari 上游基线：`6d39b0dec41282e7852e23e032998a5789abee28`。
- 保留 31 篇实际文章正文，转换日期、分类等元数据；原地址 `/posts/<slug>/` 保持不变。
- 完整对应关系见 `migration-manifest.json`。
- 已删除两篇无实际内容的模板文章：Hello, Jekyll 和 Coding Post。
- 原简历 PDF 和静态资源保持旧路径；关于页已重写为中文，不再展示过时的简历入口。
- RSS 位于 `/rss.xml`，同时保留 `/feed.xml` 兼容入口。
- 未为文章新增开放许可；主题的默认 CC 许可面板已关闭。
- 旧文标题或内容中已有的笔误未在此次主题迁移中擅自改写；外部图片仍依赖原站点。

升级主题时建议比较上游变更，保留本站配置与文章，再运行检查、构建和预览。
Fuwari 代码遵循 MIT 许可，见 LICENSE；旧主题许可保存在 LICENSE-legacy.txt。

## 本次兼容修复

补齐上游缺失的全局样式入口，收窄图片导入范围；修复 Svelte 组件类型声明，
并避免搜索异步结果乱序覆盖当前输入。保留主题原有布局。
