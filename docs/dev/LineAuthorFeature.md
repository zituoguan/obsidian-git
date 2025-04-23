# 行作者功能 - 开发者文档

-   本功能由 [GollyTicker](https://github.com/GollyTicker) 开发。
-   [面向用户的功能文档](https://publish.obsidian.md/git-doc/Line+Authoring)

## 架构

要理解该功能如何集成到 Obsidian 编辑器所用的 [Codemirror 6 编辑器](https://codemirror.net/)，建议阅读 [Codemirror 指南](https://codemirror.net/docs/guide/) 的以下部分：

-   架构概览 > （全部）
-   数据模型
    -   配置
    -   Facets
    -   事务（Transactions）
-   视图 > （简介）
-   扩展 Codemirror
    -   状态字段（State Fields）

此外，还需了解以下概念：

-   [EditorState](https://codemirror.net/docs/ref/#state.EditorState)
-   [State Field](https://codemirror.net/docs/ref/#state.StateField)
-   [Transaction](https://codemirror.net/docs/ref/#state.Transaction)
-   [创建事务](https://codemirror.net/docs/ref/#state.EditorState.update)
-   [事务中的注解](https://codemirror.net/docs/ref/#state.Annotation)
-   [ChangeSet](https://codemirror.net/docs/ref/#state.ChangeSet)（用于未保存更改的 gutter 更新）
-   [示例：文档更改](https://codemirror.net/examples/change/)
-   [示例：配置和扩展](https://codemirror.net/examples/config/)

当 Obsidian 文件或文件视图发生更改/更新时，我们希望通过 [git-blame](https://git-scm.com/docs/git-blame) 重新计算行作者信息，并在编辑器左侧的 gutter 中显示。

实现时需与 Codemirror 的声明式建模集成，并在相关数据变更时自动更新视图。

实现步骤如下：

1. 每个新的编辑器面板会通过文件路径（[LineAuthoringSubcriber](/src/lineAuthor/control.ts)）进行订阅，并在内部发布-订阅模型（[eventsPerFilepath.ts](/src/lineAuthor/eventsPerFilepath.ts)）中监听该路径的更新。
2. 当 Obsidian Vault 中的文件发生更改或新文件被打开时，[lineAuthorProvider](/src/lineAuthor/lineAuthoProvider.ts) 会启动异步计算 [LineAuthoring](/src/lineAuthor/model.ts)，通过 [simpleGit.ts](/src/simpleGit.ts) 解析 `git-blame` 输出。
3. 一旦 `LineAuthoring` 计算完成，发布-订阅模型会通知对应文件路径的新值。
4. 被通知的 `LineAuthoringSubcriber` 会创建一个新事务（通过 [newComputationResultAsTransaction](/src/lineAuthor/model.ts)），包含 `LineAuthoring`。
5. `LineAuthoringSubscriber` [在当前 EditorView 上分发事务](https://codemirror.net/docs/ref/#view.EditorView.dispatch)。
6. 由于事务分发，[StateField 的 update 方法](https://codemirror.net/docs/ref/#state.StateField^define^config.update) 被 Codemirror 调用。[lineAuthorState](/src/lineAuthor/model.ts) 会用事务中提供的最新 `LineAuthoring` 更新自身。
7. [lineAuthorGutter](/src/lineAuthor/view/view.ts) 会因状态字段变更自动重新渲染，渲染时会读取最新的状态字段值，生成新的 DOM。

## 开发

可使用此测试库：https://github.com/GollyTicker/obsidian-git-test-vault-online。

启动 npm watch 模式后，只需在 Obsidian 中打开 `test-vault` 即可测试插件。Git 插件文件为符号链接，指向仓库根目录下自动重新编译的文件。

还可使用
[此分支的 docker-setup 进行可复现的开发环境搭建](https://github.com/GollyTicker/obsidian-git/tree/docker-setup)。

## 边界情况与错误场景

更改此功能后应测试以下情况：

-   在非 git 仓库环境下运行
-   打开未被跟踪的文件
-   打开和关闭 Obsidian 窗口或面板/笔记
-   文件名以 "--" 开头的笔记
-   文件名包含特殊字符
-   文件名为 unicode
-   空文件
-   最后一行有内容的文件
-   多行块有不同提交的情况
-   移动/复制跟踪的示例
-   子模块
-   vault 根目录与仓库根目录不一致
-   git blame 结果出错
-   同时打开多个文件
-   多次打开同一文件并编辑
-   在多个窗口中打开同一文件并编辑
-   打开空的已跟踪文件并编辑，快速更新应有合理响应
-   在大型复杂的真实 vault 中打开文件并反复按回车（Enter）
    -   预期无错误，但在添加未保存更改 gutter 更新功能后，曾出现渲染错误导致视图混乱的 bug。
-   无论是否显示行号，UI 都应正确渲染。
    -   [[见 obsidian 论坛讨论](https://forum.obsidian.md/t/added-editor-gutter-overlaps-and-obscures-editor-content/45217)]
-   启用/禁用“忽略空白”时，缩进变更和最后一行后的变更
-   [未保存更改 gutter 更新场景](#unsaved-changes-gutter-update-scenario)
-   文件提交时区与当前 Obsidian 用户不同
    -   检查时区“本地”格式是否正确
    -   时区“UTC”应始终显示相同结果
-   行作者 id 应正确使用子模块 HEAD 修订而非父项目

    -   旧的父项目标识存在 bug，未能正确处理子模块，导致显示的行作者信息与实际不符。

    1. 记住 vault 中子模块文件的 lineAuthoringId A

        - 它使用的是 git 父项目的 HEAD，而不是文件所在子模块的 HEAD。

    2. 在文件中添加几行。插件会检测到文件内容哈希变化，触发重新计算和渲染。
    3. 在子模块中提交更改，但不在父项目中提交。
    4. 关闭并重新打开该文件。

        - 子模块 HEAD 已变，但父项目未变。
        - 提交后文件路径和内容未变。
        - 当前缓存键未检测到此变化，视图未更新。
        - 完全重启 Obsidian 后缓存被清除，行作者信息才会正确显示。

### 未保存更改 gutter 更新场景

此场景包含两种主要情况：

#### 1. 未被跟踪的文件

1. 打开未被跟踪的文件，应全部显示 +++。
2. 插入、删除、行内更改，始终显示 +++。

#### 2. 已跟踪的文件

1. 打开一个有不同作者日期和颜色的已跟踪文件
2. 插入、删除、行内更改

-   首先应显示 %，直到更改被保存并重新计算行作者信息。
-   % 应保留更改行的颜色，插入/删除应相应地移动后续行的作者信息。

3. 进行多行插入、删除、行内更改（如剪切粘贴文本块）

-   提示：可用 Ctrl+Z。
-   行为应与上述一致。

4. 在未保存和已保存更改的交界处进行更改，结果应与上述一致。

## 潜在的未来改进

-   点击/悬停 gutter 时显示提交信息
-   悬停/点击 gutter 时显示/高亮 diff
-   悬停/右键行作者 gutter 时显示作者/哈希等小型提示窗口
-   显示已删除的行
-   将新行末的换行符解释为非更改，使 gutter 标记更直观
    -   [可添加设置切换兼容模式与舒适模式](https://github.com/denolehov/obsidian-git/pull/288)
-   区分未被跟踪和已更改的行（如用 "~" 和 "+"）
-   在 settings.ts 配置行作者日期格式时使用 addMomentFormat
-   main.ts: refreshUpdatedHead(): 检测 git HEAD 是否被外部（如脚本）更改，并在此时运行回调
-   关闭独立 Obsidian 窗口时避免 "Uncaught illegal access error"
    目前对用户体验无影响……
-   唯一首字母选项：[开发分支](https://github.com/GollyTicker/obsidian-git/tree/line-author-unique-initials)
