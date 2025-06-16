+++
title = "修复GitHub Pages Zola部署问题：从Jekyll冲突到成功部署"
date = 2025-06-16
description = "详细记录如何解决GitHub Pages上Zola静态网站的部署问题，包括Jekyll冲突、路径冲突和权限配置等常见问题的解决方案"
slug = "github-pages-zola-deployment-fix"
[taxonomies]
tags = ["zola", "github-pages", "deployment", "troubleshooting", "静态网站"]
categories = ["technical"]
+++

在将Zola静态网站部署到GitHub Pages的过程中，我遇到了一系列部署问题。本文详细记录了问题诊断和解决的完整过程，希望能帮助遇到类似问题的开发者。

<!-- more -->

## 背景

我正在使用Zola静态网站生成器部署一个技术博客到GitHub Pages。初始设置看似简单，但在实际部署过程中遇到了多个问题，从路径冲突到Jekyll构建器冲突，每个问题都需要不同的解决方案。

## 问题1：文章路径冲突

### 错误信息
```
Error: Found path collisions:
- `/posts/001/` from files ["/github/workspace/content/posts/2024-12-29-001.md", "/github/workspace/content/posts/2025-06-16-001.md"]
```

### 问题分析
两个文件都使用了相同的文件名后缀 `001.md`，导致Zola生成相同的URL路径 `/posts/001/`，造成路径冲突。

### 解决方案
为每个文章添加唯一的 `slug` 字段：

```markdown
+++
title = "How to Publish New Posts to Your Zola Site"
date = 2025-06-16
slug = "how-to-publish-new-posts"
[taxonomies]
tags = ["tutorial", "blogging"]
categories = ["technical"]
+++
```

```markdown
+++
title = "量子力学中的微分方程"
date = 2024-12-29
slug = "quantum-mechanics-differential-equations"
[taxonomies]
tags = ["physics"]
categories = ["DFT"]
+++
```

## 问题2：GitHub Actions权限问题

### 错误信息
```
remote: Permission to weizhengqing/weizhengqing.github.io.git denied to github-actions[bot].
fatal: unable to access 'https://github.com/weizhengqing/weizhengqing.github.io.git/': The requested URL returned error: 403
```

### 问题分析
使用第三方action `shalzz/zola-deploy-action` 时遇到权限问题，无法推送到gh-pages分支。

### 解决方案
改用官方GitHub Actions实现更可靠的部署流程：

```yaml
name: Deploy Zola site to Pages

on:
  push:
    branches: ["main"]
  workflow_dispatch:

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: "pages"
  cancel-in-progress: false

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4
      
      - name: Setup Zola
        uses: taiki-e/install-action@v2
        with:
          tool: zola@0.20.0
      
      - name: Build with Zola
        run: zola build
      
      - name: Setup Pages
        uses: actions/configure-pages@v5
      
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: './public'

  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    needs: build
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

## 问题3：Jekyll构建器冲突

### 错误信息
```
Liquid Exception: Liquid syntax error (line 33): Variable '{{ note( body=" Some really insightful note here. $$ \sum\_{i=1}' was not properly terminated with regexp: /\}\}/ in content/posts/shortcodes-demo/index.md
```

### 问题分析
GitHub Pages默认使用Jekyll构建器处理文件，但Jekyll无法解析Zola特有的shortcode语法（如 `{{ note(...) }}`），导致构建失败。

### 解决方案

1. **添加 `.nojekyll` 文件**
   在仓库根目录创建空的 `.nojekyll` 文件，禁用Jekyll处理：
   ```bash
   touch .nojekyll
   ```

2. **配置GitHub Pages设置**
   - 进入GitHub仓库的 Settings → Pages
   - 将 "Source" 从 "Deploy from a branch" 改为 **"GitHub Actions"**

3. **删除冲突的工作流**
   移除可能存在的Jekyll相关工作流文件。

## 网站结构重构

在解决部署问题的同时，我还重构了网站的文章组织结构：

### 文章整理
- 将 `content/` 目录下的所有文章移动到 `posts/` 子目录
- 统一文章组织结构，便于管理

### 首页优化
- 首页显示最新10篇文章的预览
- 添加"查看所有文章"链接

### 分页功能
- 在posts页面实现分页浏览
- 每页显示10篇文章
- 添加中文分页导航："上一页"、"下一页"

### 模板更新
更新了 `templates/index.html` 和 `templates/section.html` 以支持新的文章组织结构和分页功能。

## 最终的工作流程

修复后的完整部署流程：

1. **代码推送到main分支**
2. **GitHub Actions自动触发**：
   - 安装Zola 0.20.0
   - 构建网站到 `./public` 目录
   - 上传构建产物作为Pages artifact
3. **自动部署到GitHub Pages**：
   - 直接从artifact部署静态文件
   - 不使用Jekyll处理
   - 支持所有Zola功能

## 经验总结

### 关键要点
1. **正确配置GitHub Pages源**：必须设置为"GitHub Actions"而不是分支部署
2. **使用官方Actions**：比第三方actions更稳定可靠
3. **处理路径冲突**：为文章添加唯一的slug避免URL冲突
4. **禁用Jekyll**：使用 `.nojekyll` 文件和正确的Pages配置

### 最佳实践
1. **权限配置**：使用最小必要权限原则
2. **构建分离**：将构建和部署分为独立的job
3. **错误处理**：详细的日志输出便于问题诊断
4. **版本固定**：指定具体的工具版本确保一致性

## 结果验证

部署成功后，网站具备以下功能：
- ✅ 正确显示所有文章（按时间排序）
- ✅ 首页显示最新10篇文章
- ✅ Posts页面支持分页
- ✅ 所有Zola shortcodes正常工作
- ✅ 搜索功能正常
- ✅ 主题和样式正确加载

## 相关链接

- [Zola官方文档](https://www.getzola.org/)
- [GitHub Pages官方文档](https://docs.github.com/en/pages)
- [GitHub Actions for Pages](https://github.com/actions/configure-pages)

通过这次问题解决过程，我深入了解了静态网站部署的各个环节，也积累了宝贵的troubleshooting经验。希望这篇文章能帮助其他开发者避免类似的坑！
