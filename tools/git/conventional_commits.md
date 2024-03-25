# 约定式提交（Conventional Commits）

![conventional-commits__banner.png](../../static/images/tools/git/conventional-commits__banner.png)

## 什么是约定式提交？

约定式提交（Conventional Commits）是一种软件开发中使用的标准化的提交消息格式。

它的主要目的是通过统一的提交消息格式，帮助开发者更容易地理解项目的变更历史，以及自动化生成 changelog。

Conventional Commits 的格式通常包括了三个部分：类型、作用范围和描述。

## Why Conventional Commit？

- 清晰的提交历史：Conventional Commit 提供了一种标准化的提交消息格式，使得提交历史更加清晰易读。每个提交都有明确的类型、作用范围和描述，使得其他开发人员可以快速了解每次变更的目的和内容。
- 自动化生成 changelog：借助 Conventional Commit，可以轻松地自动生成 changelog。通过解析提交消息，可以自动提取出每个版本中的变更内容，得到清晰的更新日志。
- 更好的团队协作：使用统一的提交消息格式可以帮助团队成员更好地协作。每个人都能够快速理解其他人的提交，从而更好地协调工作、避免重复劳动，提高团队的生产效率。
- 更好的版本控制管理：Conventional Commit 使得版本控制管理更加方便。通过明确的提交消息，可以更轻松地跟踪每个版本的变更，快速定位和修复问题，确保代码质量。
- 更容易的集成和工具支持：许多版本控制工具、CI/CD 工具和代码托管平台已经支持 Conventional
  Commit。因此，采用这种标准化的提交消息格式，可以更容易地集成到现有的工作流程中，享受更多的工具支持和便利。

## SemVer 规范

本文中约定式提交规范遵循[语义化版本控制规范（SemVer）](https://semver.org/lang/zh-CN/)

## 格式与用法

```shell
<type>[scope][!]: <description>

[body]

[footer(s)]
```

### `type` 选项说明

每个提交都必须使用类型字段前缀，它由一个名词构成，诸如 `feat` 或 `fix`，其后接可选的范围字段，可选的 `!`，以及必要的冒号（英文半角）和空格。

当一个提交为应用或类库实现了新功能时，必须使用 `feat` 类型。

当一个提交为应用修复了 bug 时，必须使用 `fix` 类型。

具体参数和描述见下表：

| 参数       | 说明                       | 描述                                    |
|----------|--------------------------|:--------------------------------------|
| feat     | Features                 | 用于提交一个新增功能（这和语义化版本中的 MINOR 相对应）       |
| fix      | Bug Fixes                | 用于 bug 修复的提交 （这和语义化版本中的 PATCH 相对应）    |
| build    | Builds                   | 用于修改项目构建系统，例如修改依赖库、外部接口或者升级 Node 版本等  |
| chore    | Chores                   | 用于对非业务性代码进行修改，例如修改构建流程或者工具配置等         |
| ci       | Continuous Integrations  | 用于修改持续集成流程，例如修改 Travis、Jenkins 等工作流配置 |
| docs     | Documentation            | 用于修改文档，例如修改 README 文件、API 文档等         |
| style    | Styles                   | 用于修改代码的样式，例如调整缩进、空格、空行等               |
| refactor | Code Refactoring         | 用于重构代码，例如修改代码结构、变量名、函数名等但不修改功能逻辑      |
| perf     | Performance Improvements | 用于优化性能，例如提升代码的性能、减少内存占用等              |
| test     | Tests                    | 用于修改测试用例，例如添加、删除、修改代码的测试用例等           |
| revert*  | Reverting                | 用于恢复代码时                               |

> *revert
>
> 约定式提交不能明确的定义还原行为。所以需要开发者，基于类型和脚注的灵活性来开发他们自己的还原处理逻辑。
>
> 一种建议是使用 revert 类型，和一个指向被还原提交摘要的脚注：
>
> revert: let us never again speak of the noodle incident
>
> Refs: 676104e, a215868

### `scope` 选项说明

范围（scope）选项提供了额外的上下文信息，可以跟随在类型字段后面，为可选字段。

范围必须是一个描述某部分代码、功能、模块的名词并用圆括号包围，例如：`fix(parser):`。

### `!` 选项说明

`!` 字符选项用来提醒注意破坏性变更的提交说明，为可选字段。包含在 `<type>(scope)` 前缀后 `:` 前。

如果使用了 `!`，那么脚注中可以不写 `BREAKING CHANGE:`， 同时提交信息的描述中应该用来描述破坏性变更。

### `description` 选项说明

描述（description）选项包含了对提交内容的简明描述，内容简短精炼，为必需字段。

描述字段必须直接跟在 `<type>(scope)` 前缀的冒号和空格之后，使用现在时祈使句，“此提交会...”或“此提交应该...”。
例如：要使用“更新(update)，修改（fix），新增（add）”，而不是“更新了(updated)，修改了（fixed），新增了（added）”。

例如：`fix: array parsing issue when multiple spaces were contained in string`

描述字段不应以 `.` 结尾。

### `body` 选项说明

在简短描述（description）字段之后，可以编写较长的提交正文（body），为代码变更提供额外的上下文信息。

正文必须起始于描述字段结束的一个空行后，使用现在时祈使句，提交的正文内容自由编写，并可以使用空行分隔不同段落。

### `footer(s)` 选项说明

在正文结束的一个空行之后，可以编写一行或多行脚注（footer）。

每行脚注都必须包含一个令牌（token），令牌必须使用 `-` 作为连字符，比如 `Acked-by` (这样有助于区分脚注和多行正文)。

有一种例外情况就是 `BREAKING CHANGE`，它可以被认为是一个令牌。令牌后面紧跟 `:`
作为分隔符，后面再紧跟令牌的值。例如：`BREAKING CHANGE: environment variables now take precedence over config files`。

脚注的值可以包含空格和换行，值的解析过程必须直到下一个脚注的令牌/分隔符出现为止。

脚注常包含有关重大更改 `BREAKING CHANGE` 的信息，或是此提交所涉及问题的引用 `Refs: #123`。

### 示例

#### 基础的提交说明

```shell
feat: add email notifications on new direct messages
```

#### 包含范围的提交说明

```shell
feat(lang): add polish language
```

#### 包含了描述并且脚注中有破坏性变更的提交说明

```shell
feat: allow provided config object to extend other configs
BREAKING CHANGE: `extends` key in config file is now used for extending other config files
```

#### 包含了 ! 字符以提醒注意破坏性变更的提交说明

```shell
feat!: send an email to the customer when a product is shipped
```

#### 包含了范围和破坏性变更 ! 的提交说明

```shell
feat(api)!: send an email to the customer when a product is shipped
```

#### 包含了 ! 和 BREAKING CHANGE 脚注的提交说明

```shell
chore!: drop support for Node 6
BREAKING CHANGE: use JavaScript features not available in Node 6.
```

#### 包含多行正文和多行脚注的提交说明

```shell
fix: prevent racing of requests

Introduce a request id and a reference to latest request. Dismiss
incoming responses other than from latest request.

Remove timeouts which were used to mitigate the racing issue but are
obsolete now.

Reviewed-by: Z
Signed-off-by: Alice <alice@example.com>
Refs: #123
```


## 参考资料

https://www.conventionalcommits.org/en/v1.0.0/
https://gist.github.com/qoomon/5dfcdf8eec66a051ecd85625518cfd13
