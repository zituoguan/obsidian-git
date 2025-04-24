---
title: 06 行作者
---

# 快速用户指南

本指南简要展示了所有功能。此功能基于 [git-blame](https://git-scm.com/docs/git-blame)。

ℹ️ 行作者视图仅在实时预览和源码模式下有效——在阅读模式下无效。

ℹ️ 目前仅支持桌面版 Obsidian。

ℹ️ 最新发布的 Obsidian v1.0 已完全支持，但本文档中的图片和 GIF 尚未更新。

## 激活

![](assets/line-author-activate.png)

也可以通过命令面板激活：`Git: Toggle line author information`。

## 默认行作者信息

![](assets/line-author-default.png)

显示作者的缩写以及创作日期（`YYYY-MM-DD` 格式）。

`*` 表示作者和提交者（或其时间戳）不同——例如由于 rebase。

## 提交哈希和全名

![](assets/line-author-commit-hash-full-name.png)

通过配置实现

![](assets/line-author-commit-hash-full-name-config.png)

## 自然语言日期

![](assets/line-author-natural-language-dates.png)

## 自定义日期格式

![](assets/line-author-custom-dates.png)

通过配置实现

![](assets/line-author-custom-dates-config.png)

## 提交时间显示为本地/作者/UTC 时区

**UTC+0000/Z**

最简单的方式是以 `UTC+00:00/Z` 时区显示时间。
这与您的本地时区和作者时区无关。
以 `Z` 后缀显示，避免与本地时间混淆。

![](assets/line-author-tz-utc0000.png)

显示在边栏的时间对所有用户都相同。

**我的本地（默认）**

默认情况下，时间以您的本地时区显示——即“提交时我墙上的时钟显示的时间”。这取决于您的本地时区。例如，以下是 `UTC+01:00` 时区用户的视图。

![](assets/line-author-tz-viewer-plus0100.png)

注意，显示的时间比上面的 `UTC+0000` 快 1 小时。

**作者本地**

也可以选择以作者的时区显示，并带有明确的 `UTC` 偏移量——即“提交时作者墙上的时钟时间及其 UTC 偏移”。

这与您的本地时区无关，对所有用户显示相同的时间。

![](assets/line-author-tz-author-local.png)

**配置**

![](assets/line-author-tz-config.png)

## 基于年龄的边栏颜色

边栏颜色基于提交的年龄自动调整深浅，并适配深色/浅色模式。

![](assets/line-author-dark-light.gif)

红色代表较新，蓝色代表较旧。所有达到或超过最大着色年龄（可配置，默认 `1 年`）的提交都显示最深的蓝色。

颜色可配置，默认配色兼顾可访问性。

![](assets/line-author-color-config.png)

## 根据主题调整文本颜色 CSS

默认情况下，边栏文本颜色使用 `var(--text-muted)`，即主题定义的颜色。您也可以将其更改为其他 CSS 颜色或变量。

![](assets/line-author-text-color.png)

示例：
| `var(--text-muted)` | `var(--text-normal)` |
|----------------------------------------------|-----------------------------------------------|
| ![](assets/line-author-text-color-muted.png) | ![](assets/line-author-text-color-normal.png) |

## 复制提交哈希

![](assets/line-author-copy-commit-hash.png)

## 快速配置边栏

![](assets/line-author-quick-configure-gutter.gif)

## 新增/未提交的行和文件显示 `+++`

![](assets/line-author-untracked.png)

## 跨同一提交/所有提交跟踪剪切复制粘贴的行

默认情况下，每行显示其最后一次更改的提交。
这意味着剪切复制粘贴的行会显示新的提交，即使该行最初并非在该提交中编写。

![](assets/line-author-follow-no-follow.png)

但如果设置为“所有提交”，则如下所示：

![](assets/line-author-follow-all-commits.png)

配置：

![](assets/line-author-follow-config.png)

## 软性且不打扰的异步视图更新

由于计算行作者信息需要时间（需调用 `git blame` 命令），结果会延迟显示。为减少干扰并提升用户体验，视图会以柔和且不打扰的方式更新。

打开文件时，会先显示占位符：

![](assets/line-author-soft-unintrusive-ux.gif)

编辑时，也会显示占位符，直到保存文件并计算出行作者信息。

![](assets/line-author-soft-unintrusive-ux-editing.gif)

## 多行块支持

支持将多行渲染为组合块的 markdown。在此情况下，边栏显示所有行中最新的提交信息。

![](assets/line-author-multi-line-newest.gif)

## 忽略空白和换行

可在设置中启用。

| **原始**                                         | **保留空白的更改**                   | **忽略空白的更改**                   |
| ------------------------------------------------ | ------------------------------------- | ------------------------------------- |
| ![](assets/line-author-ignore-whitespace-before.png) | ![](assets/line-author-ignore-whitespace-preserved.png) | ![](assets/line-author-ignore-whitespace-ignored.png) |

注意，忽略空白后，缩进行不会被标记为更改，因为只添加了额外的空白。

## 子模块支持

子模块中的行作者信息也完全支持。

