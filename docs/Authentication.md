---
aliases:
  - "04 Authentication"
---
# macOS

## HTTPS

运行以下命令，将凭据存储在 macOS 钥匙串中。

```bash
git config --global credential.helper osxkeychain
```

设置好 helper 后，需要在终端执行一次认证操作（如 clone/pull/push）。之后，你就可以在 Obsidian 中无障碍地进行 clone/pull/push 操作了。

## SSH

你仍然需要正确设置 SSH，比如将 SSH 密钥添加到 `ssh-agent`。GitHub 提供了详细的文档，介绍如何[生成新的 SSH 密钥](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent?platform=mac#generating-a-new-ssh-key)，以及如何[将 SSH 密钥添加到 ssh-agent](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent?platform=mac#adding-your-ssh-key-to-the-ssh-agent)。

# Windows

## HTTPS

确保你使用的是 Git 2.29 或更高版本，并且使用 Git Credential Manager 作为凭据助手。
你可以在终端（最好在你的 vault/仓库目录下）执行以下命令进行验证，输出应为 `manager`。

```bash
git config credential.helper
```

如果输出不是 `manager`，请运行 `git config set credential.helper manager`。
之后执行任意认证命令（如 push/pull/clone），会弹出窗口让你登录。

另外，你也可以将该设置留空，每次通过 Obsidian 弹出的窗口手动输入用户名和密码。所有可用的凭据助手可在[这里](https://git-scm.com/doc/credential-helpers)查看。

## SSH

你仍然需要正确设置 SSH，比如将 SSH 密钥添加到 `ssh-agent`。GitHub 提供了详细的文档，介绍如何[生成新的 SSH 密钥](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent?platform=windows#generating-a-new-ssh-key)，以及如何[将 SSH 密钥添加到 ssh-agent](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent?platform=windows#adding-your-ssh-key-to-the-ssh-agent)。

# Linux

## HTTPS

### 存储

要安全地永久存储用户名和密码，避免每次都输入，可以使用 Git 的[凭据助手](https://git-scm.com/book/en/v2/Git-Tools-Credential-Storage)。`libsecret` 会将密码安全地存储起来。在 GNOME 下由 [GNOME Keyring](https://wiki.gnome.org/Projects/GnomeKeyring/) 支持，在 KDE 下由 [KDE Wallet](https://wiki.archlinux.org/title/KDE_Wallet) 支持。
要将 `libsecret` 设置为凭据助手，请在 vault/仓库目录下的终端执行以下命令。你也可以加上 `--global` 参数，将该设置应用于所有仓库。

```bash
git config set credential.helper libsecret
```

设置好 helper 后，需要在终端执行一次认证操作（如 clone/pull/push）。之后，你就可以在 Obsidian 中无障碍地进行 clone/pull/push 操作了。

如果出现 `git: 'credential-libsecret' is not a git command` 的提示，说明系统未安装 libsecret。你需要自行安装。
以下是 Ubuntu 的安装示例：

```bash
sudo apt install libsecret-1-0 libsecret-1-dev make gcc

sudo make --directory=/usr/share/doc/git/contrib/credential/libsecret

# 注意：这会更改你的全局配置，如果不想全局生效，可以去掉 `--global`，在现有仓库中执行。
git config --global credential.helper \
   /usr/share/doc/git/contrib/credential/libsecret/git-credential-libsecret

```

### SSH_PASS 工具

当 Git 没有连接到终端，无法输入用户名/密码时，会依赖 `GIT_ASKPASS`/`SSH_ASKPASS` 环境变量，提供界面让用户输入这些信息。

#### 原生 SSH_ASKPASS

如果你不想永久存储密码，可以安装 `ksshaskpass`（KDE 系统默认已安装），并将其设置为 SSH_ASKPASS 工具。

要在 Obsidian 中使用 `ksshaskpass` 作为 `SSH_ASKPASS` 工具，请在插件设置的“高级”部分的“附加环境变量”中添加如下内容：

```
SSH_ASKPASS=ksshaskpass
```

之后，执行需要认证的 Git 操作时，会弹出窗口让你输入用户名/密码。

#### Obsidian 集成的 SSH_PASS

如果没有设置其他程序，插件会自动为 `SSH_ASKPASS` 环境变量提供集成脚本，在 Git 需要用户名或密码时，会在 Obsidian 中弹出窗口输入。

## SSH

有了上述任一 [[#SSH_PASS 工具]]，你就可以使用带密码的 ssh。你仍然需要正确设置 SSH，比如将 SSH 密钥添加到 `ssh-agent`。GitHub 提供了详细的文档，介绍如何[生成新的 SSH 密钥](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent?platform=linux#generating-a-new-ssh-key)，以及如何[将 SSH 密钥添加到 ssh-agent](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent?platform=linuxu#adding-your-ssh-key-to-the-ssh-agent)。
