# 桌面端

你可以选择克隆一个已有的远程仓库（详见[[#已有远程仓库|此处]]），或者在本地初始化一个新仓库并可选地推送到远程仓库（详见[[#创建本地新仓库|此处]]）。

## 创建本地新仓库

1. 按照你的操作系统，完成[[安装]]指引
2. 执行 `初始化新仓库` 命令
3. 创建一些文件并使用 `带指定信息提交全部更改` 命令完成首次提交
4. 如果你希望推送到远程仓库（如 GitHub）：
    1. 配置[[身份验证]]
    2. 确保远程仓库为空，否则请删除该仓库并按照[[#已有远程仓库|下一节]]的方式克隆远程仓库
    3. 执行 `推送` 命令。系统会要求你输入远程仓库名称和 URL。远程名称建议填写 `origin`，URL 可从你的远程 git 服务复制。

## 已有远程仓库

克隆仓库时需要使用远程 URL。可选协议有两种：`https` 或 `ssh`，取决于你的[[身份验证]]方式。
- `https`：`https://github.com/<用户名>/<仓库名>.git`
- `ssh`：`git@github.com:<用户名>/<仓库名>.git`

1. 按照你的操作系统，完成[[安装]]指引
2. 配置[[身份验证]]
3. Git 只能将远程仓库克隆到新文件夹。你有两种选择：
    - 使用“克隆已有远程仓库”命令，将仓库克隆到你的库的子文件夹。此时你有两个选择：
        - 将所有文件（包括 `.git` 目录）移动到库根目录
        - 将新子文件夹作为新库打开，可能需要重新安装插件
    - 在命令行中运行 `git clone <你的远程 URL>`，将库克隆到你希望的位置
4. 阅读如何最佳配置你的 [[Tips-and-Tricks#Gitignore|.gitignore]]

> [!info] iCloud 与 Git
> 如果你用 iCloud 同步库，并在桌面设备上使用 Git，整个 `.git` 目录也会同步到你的移动设备，这可能会导致 Obsidian 启动变慢。
> - 一种解决方案是将 git 仓库放在 Obsidian 库的上层目录，使你的库成为 git 仓库的子目录。
> - 另一种方案是将 `.git` 目录移动到其他位置，并在库中创建一个 `.git` 文件，内容仅为：`gitdir: <你的实际 git 目录路径>`

# 移动端

移动端的 git 实现**非常不稳定**！

## 限制

我使用了 [isomorphic-git](https://isomorphic-git.org/)，它是用 JavaScript 重写的 Git，因为在 Android 或 iOS 上无法使用原生 Git。

- 不支持 SSH 身份验证（[isomorphic-git issue](https://github.com/isomorphic-git/isomorphic-git/issues/231)）
- 仓库大小有限制，受内存影响
- 不支持 rebase 合并策略
- 不支持子模块

## 移动端性能

> [!danger] 警告
> 根据你的设备和可用内存，Obsidian 可能会：
> - 在克隆/拉取时崩溃
> - 出现缓冲区溢出错误
> - 无限运行
>
> 这是因为移动端底层 git 实现效率低下。如果遇到这种情况，说明该插件可能不适合你，提交 issue 或新建 issue 也无法解决。很抱歉。

## 使用已有远程仓库开始

### 通过插件克隆

按照以下步骤，在移动设备上设置已备份到远程 git 仓库的 Obsidian 库。

以 [GitHub](https://github.com) 为例，其他服务商可类比操作。

1. 确保所有设备上的更改都已推送并与远程仓库同步
2. 安装 Android 或 iOS 版 Obsidian
3. 创建新库（或让 Obsidian 指向一个空目录）。如果是 iOS，请勿选择“存储到 iCloud”
4. 如果你的仓库托管在 GitHub，[必须使用个人访问令牌进行身份验证](https://github.blog/2020-12-15-token-authentication-requirements-for-git-operations/)。详细流程见[这里](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token)。
    - 最低权限要求：
        - “读取元数据权限”
        - “读取和写入内容及提交状态权限”
5. 在 Obsidian 设置中启用社区插件，浏览插件并安装 Git
6. 启用 Git 插件
7. 进入 Git 插件选项（主设置页面底部，社区插件部分）
8. 在“身份验证/提交作者”部分填写你的 git 服务器用户名和密码/个人访问令牌
9. 不要修改“高级”下的任何设置
10. 退出插件设置，打开命令面板，选择“Git: 克隆已有远程仓库”
11. 在文本框中填写仓库 URL 并点击下方按钮。仓库 URL 不是浏览器地址，需以 `.git` 结尾，如：`https://github.com/<用户名>/<仓库名>.git`
    - 例如：`https://github.com/denolehov/obsidian-git.git`
12. 按提示选择仓库放置文件夹，并确认是否已有 `.obsidian` 目录
13. 克隆开始后，弹窗会显示进度（如未禁用）。直到弹窗提示“请重启 Obsidian”前请勿退出

### 通过 iOS 上的 Working Copy 克隆

根据仓库大小和设备性能，插件克隆时 Obsidian 可能会崩溃。你也可以用 [Working Copy](https://workingcopy.app/) 完成初次克隆（该应用为付费）。后续提交和同步可用插件完成。以下流程假设你未提交 `.obsidian` 目录。

1. 确保所有设备上的更改都已推送并与远程仓库同步
2. 安装 Android 或 iOS 版 Obsidian
3. 创建新库（或让 Obsidian 指向一个空目录）。如果是 iOS，请勿选择“存储到 iCloud”
4. 如果你的仓库托管在 GitHub，[必须使用个人访问令牌进行身份验证](https://github.blog/2020-12-15-token-authentication-requirements-for-git-operations/)。详细流程见[这里](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token)。
    - 最低权限要求：
        - “读取元数据权限”
        - “读取和写入内容及提交状态权限”
5. 上滑关闭 Obsidian，打开 Working Copy 应用
6. 用 Working Copy 克隆仓库。无需通过 Working Copy 登录 GitHub，直接输入克隆 URL，然后输入用户名，密码处填写个人访问令牌
7. 打开文件应用
8. 从 Working Copy 复制仓库。删除 Obsidian 中的库并粘贴仓库（仓库名与库名一致）
9. 打开 Obsidian
10. 所有克隆的文件应可见
11. 安装并启用 Git 插件
12. 在插件设置的“身份验证/提交作者”部分填写你的姓名/邮箱
13. 使用命令面板执行“拉取”命令

## 新建仓库

步骤与[已有仓库](#existing-repo)类似，只是先用 `初始化新仓库` 命令，然后用 `编辑远程` 添加远程仓库。远程仓库需已存在且为空。也请阅读如何最佳配置你的 [[Tips-and-Tricks#Gitignore|.gitignore]]。
