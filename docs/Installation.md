---
aliases:
  - 02 安装
---

> [!important]
> 虽然该插件本身与桌面平台无关，但 Obsidian 或 Git 安装不正确可能会导致插件无法正常工作。

## 插件安装

### 在 Obsidian 内安装
进入“设置” -> “社区插件” -> “浏览”，搜索“Git”，安装并启用它。

### 手动安装
1. 从[最新发布页面](https://github.com/denolehov/obsidian-git/releases/latest)下载 `obsidian-git-<latest-version>.zip`
2. 将压缩包解压到 `<vault>/.obsidian/plugins/obsidian-git`
3. 重新加载 Obsidian（CTRL + R）
4. 进入设置并关闭受限模式
5. 启用 `Git`

# Windows

仅安装 [GitHub Desktop](https://github.com/apps/desktop) **并不**足够！你还需要安装常规的 Git。
## Git 安装

> [!info]
> 请确保你使用的是 Git 2.29 或更高版本。

从官方[网站](https://git-scm.com/download/win)使用所有默认设置安装 Git。
确保已启用 `3rd-party software` 访问权限。

![[third-party-windows-git.png]]

启用 Git Credential Manager。你可以通过执行以下命令验证现有安装。输出应为 `manager`。

```bash
git config credential.helper
```

![[credential-manager-windows-git.png]]


# Linux

## Obsidian 安装

已知**支持**的 Obsidian 安装方式：
- AppImage

已知**不完全支持**的软件包管理器：
- Snap（Snap 会将 Obsidian 放入沙箱，导致 Obsidian 无法访问 Git）
- [Flatpak](https://flathub.org/apps/details/md.obsidian.Obsidian) 可以访问 Git，但无法访问所有系统文件，因此不推荐。

如果你之前通过 **Flatpak** 安装了 Obsidian 并且无法正常工作，请运行以下命令：

```
$ flatpak update md.obsidian.Obsidian
$ flatpak override --reset md.obsidian.Obsidian
$ flatpak run md.obsidian.Obsidian
```
[命令来源](https://github.com/flathub/md.obsidian.Obsidian/issues/5#issuecomment-736974662)

## MacOS

无特殊要求。