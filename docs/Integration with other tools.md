---
title: 09 与其他工具集成
---

大多数与其他可安装工具集成的问题，通常是因为它们的安装路径没有添加到 `PATH` 环境变量中。`PATH` 环境变量包含了用于查找可执行程序的目录。你可能在终端中运行这些工具没有问题，因为你在 `.bashrc`、`.zshrc` 等文件中编辑了 `PATH`，但这些文件只对你的 shell 有效，对 Obsidian 这样的桌面应用程序无效。因此，一些安装目录没有包含在 `PATH` 中，插件无法找到它们。

# Git Large File Storage
Git Large File Storage 已被支持，但插件可能需要一些配置才能找到 `git-lfs` 可执行文件。

## MacOS

1. 请确保使用 `brew install git-lfs` 安装 [git-lfs](https://git-lfs.com/)。
	- 这会将 `git-lfs` 安装到 `/opt/homebrew/bin/`，而该路径在使用 Obsidian 时很可能不在你的 `PATH` 环境变量中。
2. 为了让 `/opt/homebrew/bin/` 在 Obsidian 中可用，请在“高级”设置下的“附加 PATH 环境变量路径”中添加 `/opt/homebrew/bin/`。
3. 重启 Obsidian。

## Linux
1. 请确保已安装 [git-lfs](https://git-lfs.com/)。
	- `git-lfs` 的安装位置因包管理器和发行版而异。通常无需手动添加到 `PATH`，但如果插件找不到 `git-lfs`，请按照以下步骤操作。
2. 在终端运行 `which git-lfs` 获取安装路径。输出应类似于 `<some-path>/git-lfs`。
2. 将上一步输出中的 `<some-path>` 部分添加到“高级”设置下的“附加 PATH 环境变量路径”中。
3. 重启 Obsidian。

## Windows
对于插件来说无需做任何更改，因为 git-lfs 会随 Git for Windows 一起安装，只要 Git 可用，git-lfs 也应该可用。

# GPG 签名

GitHub 提供了很好的 [GPG 文档](https://docs.github.com/en/authentication/managing-commit-signature-verification/generating-a-new-gpg-key)，同样适用于 Obsidian。
你可能会遇到如下问题：
```
Error: error: cannot run gpg: No such file or directory
error: gpg failed to sign the data
fatal: failed to write commit object
```

这意味着你的 `PATH` 中没有 `gpg` 可执行文件，你可能只为 shell 正确配置了它。但由于 Obsidian 的启动方式不同，这些 PATH 修改不会影响 Obsidian。要获取 `gpg` 的可执行文件路径，在 Linux 和 Mac-OS 上运行 `which gpg`，在 Windows 上运行 `where gpg`。常见路径可能为 `/usr/local/bin/gpg`。

- 你可以将该路径添加到插件设置中的“附加 PATH 环境变量”，仅为插件提供 gpg 可执行文件。
- 或者通过 `git config --global gpg.program <你的输出路径>` 在 Git 配置中全局设置 gpg 可执行文件，适用于所有 git 仓库。

如果你遇到任何问题，或文档需要改进，请创建 issue 反馈。