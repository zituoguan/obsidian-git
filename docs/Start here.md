---
aliases:
  - "01 Start here"
---

# Git 插件文档

## 主题
- [[Installation|安装]]
- [[Getting Started|快速开始]]
- [[Authentication|身份验证]]
- [[Integration with other tools|与其他工具集成]]
- [[Features|功能]]
- [[Tips-and-Tricks|技巧与提示]]
- [[Common issues|常见问题]]
- [[Line Authoring|行作者]]

> [!warning] 在 Linux 上安装 Obsidian
> 请不要使用 Flatpak 或 Snap 在 Linux 上安装 Obsidian。了解更多信息请点击 [[Installation#Linux|这里]]

![[Getting Started#Performance on mobile]]

## 什么是 Git？

Git 是一个版本控制系统。它可以帮助你跟踪笔记的更改，并回退到之前的版本。它还允许你与其他人协作编辑同一文件。你可以在[这里](https://git-scm.com/book/en/v2/Getting-Started-About-Version-Control)阅读更多关于 Git 的信息。

> [!info] Git/GitHub 不是同步服务！
> Git 并不是用来实时将你的更改同步到云端或其他人的工具。这意味着它不适合多人同时实时编辑同一笔记。但它非常适合异步协作。

你可以通过将多次更改批量提交为一次提交（commit）来构建历史记录。这些提交可以被还原或检出。你可以通过 [Version History Diff](obsidian://show-plugin?id=obsidian-version-history-diff) 插件查看笔记不同版本之间的差异。
Git 本身只管理本地仓库。结合在线远程仓库使用时会非常方便。你可以将提交推送到远程仓库或从远程仓库拉取提交，以便共享或备份你的库。最流行的远程仓库服务商是 [GitHub](https://github.com)。

Git 主要被开发者使用，因此有时需要使用命令行。Obsidian-Git 是 Obsidian 的一个插件，可以让你在 Obsidian 内直接使用 Git，无需频繁切换到命令行或离开 Obsidian。

## 术语与概念

### 备份（已不再使用）
为简化说明，"备份" 指的是暂存所有更改 -> 提交 -> 拉取 -> 推送。

### 同步

同步是指将更改从远程仓库拉取到本地，或将本地更改推送到远程仓库的过程。这样可以确保你的本地仓库与如 GitHub 等远程仓库保持一致。

### 提交并同步

提交并同步是指暂存所有更改 -> 提交 -> 拉取 -> 推送。理想情况下，这应该是你定期执行的单一操作，以保持本地和远程仓库同步。建议你在插件设置中配置自动定时执行。你也可以在插件设置的“提交并同步”部分禁用拉取或推送操作，这样“提交并同步”就可以只执行“提交并拉取”、“提交并推送”或仅提交操作。
