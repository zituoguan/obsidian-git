# Obsidian Git 插件

一个强大的 [Obsidian.md](Obsidian.md) 社区插件，将 Git 集成带入你的笔记库。自动提交、拉取、推送，并在 Obsidian 内查看你的更改。

## 📚 文档

所有安装说明（包括移动端）、常见问题、技巧和高级配置都在 📖 [完整文档](https://publish.obsidian.md/git-doc) 中。

> 👉 移动端用户：请查看下方专门的 [移动端](#mobile) 部分。

## ✨ 主要功能

- 🔁 **自动提交与同步**（定时提交、拉取和推送）
- 📥 **Obsidian 启动时自动拉取**
- 📂 **子模块支持**，可管理多个仓库（仅限桌面端，需手动开启）
- 🔧 **源代码控制视图**，可暂存/取消暂存、提交和对比文件——通过 `打开源代码控制视图` 命令打开
- 📜 **历史视图**，浏览提交日志和变更文件——通过 `打开历史视图` 命令打开
- 🔍 **差异视图**，查看文件更改——通过 `打开差异视图` 命令打开
- 🔗 GitHub 集成，可在浏览器中打开文件和历史记录

> 🧩 如需详细的文件历史，建议搭配 [Version History Diff](obsidian://show-plugin?id=obsidian-version-history-diff) 插件使用。

## 🖥️ 界面预览

### 🔧 源代码控制视图

在 Obsidian 内直接管理文件更改，如暂存/取消暂存单个文件并提交。

![源代码控制视图](https://raw.githubusercontent.com/Vinzent03/obsidian-git/master/images/source-view.png)

### 📜 历史视图

显示仓库的提交历史。可显示提交信息、作者、日期和变更文件。作者和日期默认关闭，可在设置中开启。

![历史视图](https://raw.githubusercontent.com/Vinzent03/obsidian-git/master/images/history-view.png)

### 🔍 差异视图

通过清晰简洁的差异查看器对比版本。
可从源代码控制视图或通过 `打开差异视图` 命令打开。

![差异视图](https://raw.githubusercontent.com/Vinzent03/obsidian-git/master/images/diff-view.png)

## ⚙️ 可用命令
> 非全部，仅列出常用命令。完整列表请见 Obsidian 的命令面板。

- 🔄 变更
  - `列出已更改文件`：在弹窗中列出所有更改
  - `打开差异视图`：为当前文件打开差异视图
  - `暂存当前文件`
  - `取消暂存当前文件`
- ✅ 提交
  - `提交所有更改`：仅提交所有更改，不推送
  - `使用指定信息提交所有更改`：同上，但可自定义提交信息
  - `提交已暂存文件`：仅提交已暂存的文件
  - `使用指定信息提交已暂存文件`：同上，但可自定义提交信息
- 🔀 提交并同步
  - `提交并同步`：默认设置下，提交所有更改、拉取并推送
  - `使用指定信息提交并同步`：同上，但可自定义提交信息
  - `提交并同步并关闭`：同 `提交并同步`，但在桌面端会关闭 Obsidian 窗口。移动端不会退出 Obsidian 应用。
- 🌐 远程
  - `推送`、`拉取`
  - `编辑远程仓库`
  - `移除远程仓库`
  - `克隆已有远程仓库`：弹窗提示输入 URL 和认证信息以克隆远程仓库
  - `在 GitHub 上打开文件`：在浏览器中打开当前文件的 GitHub 页面，仅限桌面端
  - `在 GitHub 上打开文件历史`：在浏览器中打开当前文件的 GitHub 历史，仅限桌面端
- 🏠 本地仓库管理
  - `初始化新仓库`
  - `创建新分支`
  - `删除分支`
  - `注意：删除仓库`
- 🧪 其他
  - `打开源代码控制视图`：打开侧边栏显示 [源代码控制视图](#sidebar-view)
  - `编辑 .gitignore`
  - `将文件添加到 .gitignore`：将当前文件添加到 `.gitignore`

## 💻 桌面端说明

### 🔐 认证

部分 Git 服务可能需要额外设置 HTTPS/SSH 认证。请参考 [认证指南](https://publish.obsidian.md/git-doc/Authentication)

### Obsidian 在 Linux 上

- ⚠️ 不支持 Snap。
- ⚠️ 不推荐 Flatpak，因为无法访问所有系统文件。
- ✅ 请使用 AppImage 或系统包管理器的完整访问安装方式（[Linux 安装指南](https://publish.obsidian.md/git-doc/Installation#Linux)）

## 📱 移动端支持（⚠️ 实验性）

移动端的 Git 实现**非常不稳定**！不建议在移动端使用此插件，建议尝试其他同步服务。
> 🧪 移动端 Git 插件基于 [isomorphic-git](https://isomorphic-git.org/)，这是一个 JavaScript 实现的 Git，但有严重限制和问题。Obsidian 插件无法在 Android 或 iOS 上调用原生 Git。

### ❌ 移动端限制

- 不支持 **SSH 认证**（[isomorphic-git 问题](https://github.com/isomorphic-git/isomorphic-git/issues/231)）
- 仓库大小有限制，因内存受限
- 不支持 rebase 合并策略
- 不支持子模块

### ⚠️ 性能警告

> [!caution]
> 根据你的设备和可用内存，Obsidian 可能会
>
> - 在克隆/拉取时崩溃
> - 出现缓冲区溢出错误
> - 无限运行
>
> 这是因为移动端底层 git 实现效率低下，暂时无法修复。如果遇到这些问题，说明插件不适合你的设备，反馈或提 issue 也无济于事，敬请谅解。

**测试设备：** iPad Pro M1，使用 [repo](https://github.com/Vinzent03/obsidian-git-stress-test)（3000 个文件，源自 [10000 markdown 文件](https://github.com/Zettelkasten-Method/10000-markdown-files)）

初次克隆耗时 25 秒。之后，最耗时的是检查整个工作区的文件变更。在该设备上，检查所有文件并暂存耗时 3 分 40 秒。其他命令如拉取、推送和提交都很快（1-5 秒）。

### 移动端使用建议：

如果你的仓库/笔记库很大，移动端最快的方式是只暂存单个文件并仅提交已暂存文件。

## 🙋 联系与致谢

- 行级编辑功能由 [GollyTicker](https://github.com/GollyTicker) 开发，相关问题可向他咨询。
- 本插件最初由 [denolehov](https://github.com/denolehov) 开发。自 2021 年 3 月起，由我 [Vinzent03](https://github.com/Vinzent03) 继续开发，因此 2024 年 7 月仓库迁移到了我的账号下。
- 如有任何反馈或问题，欢迎通过 GitHub issue 或在 [Obsidian Discord 服务器](https://discord.com/invite/veuWUTm) 联系 `vinzent3`。

## ☕ 支持

如果你觉得本插件有用，欢迎通过 Ko-fi 支持我。

[![ko-fi](https://ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/F1F195IQ5)
