---
title: 08 常见问题
---

## xcrun: error: invalid developer path

此错误仅发生在 macOS 上。修复方法很简单，只需在终端运行以下命令：`xcode-select --install`。参见 #64 作为示例。

## Error: spansSync git ENOENT/ 无法运行 Git 命令

当插件找不到 Git 可执行文件时会出现此错误。插件会从 PATH 环境变量中查找 Git。请前往 [[Installation]] 检查你的平台是否已正确安装。
如果你确认一切设置正确但仍然报错，请尝试以下方法：

如果你知道 Git 的安装路径，可以在设置中“自定义 Git 可执行文件路径”进行设置。如果你不知道 Git 安装在哪里，可以在终端运行以下命令查找：

### Windows

在终端运行 `where git`。它应该会返回 Git 可执行文件的路径。如果失败，说明 Git 没有正确安装。

### Linux/MacOS

在终端运行 `which git`。它应该会返回 Git 可执行文件的路径。如果失败，说明 Git 没有正确安装。

## 无限拉取/推送且无报错

这通常是由于认证问题导致的。请前往 [[Authentication]]。

## /home/\<user>/.ssh/config 权限或所有者错误

在终端运行 `chmod 600 ~/.ssh/config`。

## `.gitignore` 文件未生效

插件使用的是本地 Git 安装。如果 `.gitignore` 文件书写正确且 Git 正常使用，一切都应该正常。

需要注意的是，一旦文件已被提交（或已暂存），修改 `.gitignore` 并不会生效。你需要手动将文件从仓库中删除才能正确忽略该文件：
1. 在终端运行 `git rm --cached <file>`。该文件会保留在你的文件系统中，只是在仓库中被删除。
2. 在 `git status` 中应显示该文件已被删除
3. 提交此次删除操作
4. 之后对该文件的更改就会被正确忽略

## 无法运行 gpg

```
Error: error: cannot run gpg: No such file or directory
error: gpg failed to sign the data
fatal: failed to write commit object
```

请参见 [[Integration with other tools#GPG Signing]] 了解解决方法。

## 此仓库已配置 Git LFS，但未在路径中找到 'git-lfs'

请参见 [[Integration with other tools#Git Large File Storage]] 了解解决方法。
