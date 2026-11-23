---
title: "第 12 课 - 使用 GitHub Pages 部署网站"
description: "使用 GitHub Copilot app 配置 Pages 工作流、审查拉取请求，并验证 Space Quiz 是否已上线。"
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

使用 GitHub Pages 部署 Space Quiz，为它创建一个公开访问地址。Copilot 可以准备工作流并引导部署，但仍需审查变更并确认网站已上线。

本课将：

- 请 Copilot 配置 GitHub Pages 部署。
- 在合并前审查工作流和拉取请求。
- 跟进部署并验证已发布的网站。

## 场景

Space Quiz 已经存储在 GitHub 存储库中，但其他人还无法轻松体验它。使用 GitHub Pages 发布这个单文件网站，无需添加服务器或运行时依赖即可通过公开 URL 访问。

## 请 Copilot 准备部署

> [!WARNING]
> 创建拉取请求可能会提交并推送代码。发送此提示前，请检查批准设置和目标存储库。如果选择了**全部批准**，不要期待系统再次单独请求确认。

在 `space-quiz` 的 Copilot app 会话中发送以下提示：

```plaintext
Prepare a GitHub Pages deployment for this repository. Create a GitHub Actions workflow that builds the website if needed and deploys it to GitHub Pages after changes reach the default branch. Add a README describing the app and how to run it locally if one does not already exist. Create a pull request with these changes and report its link and any required Pages settings or permissions. Do not merge, enable auto-merge, change Pages settings, or deploy. Stop and wait for my review.
```

## 审查工作流和拉取请求

1. 确认仅在存储库尚无 README 时才添加 README。
2. 审查拉取请求中的变更，包括工作流的构建和部署步骤、触发条件及权限。
3. 确认工作流从默认分支部署，而不是从尚未合并的拉取请求部署。
4. 要求修正任何问题并审查更新后的差异。授权合并前，等待必需的拉取请求检查全部通过。

## 授权合并和部署

> [!WARNING]
> 以下提示授权智能体合并已审查的拉取请求、更改存储库的 Pages 设置并发布网站。只有在完成审查并确认存储库和拉取请求无误后才发送。批准设置可能允许这些操作不经再次确认就执行。

将 `<PR_NUMBER>` 替换为已审查的拉取请求编号，然后单独发送以下提示：

```plaintext
I have reviewed pull request <PR_NUMBER> in this repository and authorize its merge and deployment. Enable GitHub Pages using GitHub Actions, merge that pull request only after required checks pass, and monitor the deployment until it succeeds. Report the final URL of the live website. If you lack permission to enable Pages or merge, tell me exactly what I need to approve or do; do not report success until the site is live.
```

![三阶段示意图：已合并的 space-quiz 拉取请求；包含 Build website、Upload artifact 和 Deploy to Pages 步骤的成功 GitHub Actions Deploy website 运行；以及位于 yourname.github.io/space-quiz/ 的在线 Space Quiz。](../../../_images/first-steps-app-pages-deploy.svg)

## 验证部署

1. 跟进 GitHub Actions 部署运行，直到成功。
2. 打开报告中的 URL，确认 Space Quiz 已加载。

如果智能体无法启用 Pages 或合并拉取请求，请按它提供的具体说明操作。只有部署成功且网站可访问后，才能将任务视为完成。

## 总结和后续步骤

Space Quiz 现已作为在线网站发布，并且已亲自验证部署，而不是仅依赖智能体的报告。继续学习[第 13 课：审查与后续步骤][next-lesson]。

[next-lesson]: ../13-review/
