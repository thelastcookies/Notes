# 约定式提交（Conventional Commits）

![conventional-commits__banner.png](images/conventional-commits__banner.png)

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

## 语义化版本规则

约定式提交常与版本号的递增密不可分。因此本章对版本号的规则进行说明。

版本号通常采用语义化版本规则（[Semantic Versioning](https://semver.org/lang/zh-CN/)，即 `SemVer`）来进行管理和约定。

其主要包括三个部分：主版本号、次版本号和修订版本号。

语义化版本基本规则的格式为 `MAJOR.MINOR.PATCH`，例如 `1.2.3`。

先行版本号及版本编译信息可以加到 `MAJOR.MINOR.PATCH` 的后面作为延伸，例如 `1.0.0-alpha`、`1.0.0-0.3.7`、`1.0.0-x.7.z.92`。

### 主版本号（Major Version）递增场景

- 当做了不兼容的功能新增时；
- 当做了不兼容的功能修改时；
- 当做了不兼容的 API 修改时；
- 当做了不兼容的 Bug 修复时；

### 次版本号（Minor Version）递增场景

- 当做了向下兼容的功能性新增时；
- 当做了向下兼容的功能性修改时；
- 当 API 的功能被标记为弃用时；
- 当做了向下兼容的 Bug 修复时；

### 修订版本号（Patch Version）递增场景

- 当做了向下兼容的问题修正*时；
- 当补丁版本没有功能修改，只有问题修正时；

> Bug 修复与问题修正的额外说明
>
> Bug 修复通常指的是对软件中的缺陷或错误进行修复，这些缺陷可能导致软件无法按照预期的方式运行或产生意外的行为，可能涉及到对程序逻辑、算法、数据处理等方面的修改。
>
> 主要目标是恢复软件的正常运行状态，确保软件能够按照设计要求进行操作。
>
> 问题修正更为广泛，不仅包括了 Bug，还可能包括对性能问题、安全漏洞、用户体验问题等的修正。
>
> 主要目标是改善软件的质量和稳定性，提高用户的使用体验和满意度。
>
> 在语义化版本规则中，Bug 修复更侧重于解决软件功能性方面的问题，恢复软件的正常运行状态，而问题修正则是更侧重于是改善软件的质量和稳定性，提高用户的使用体验和满意度。

### 其他

标记版本号的软件发行后，禁止改变该版软件的内容。任何修改都必须以新版本发行。

主版本号为零（0.y.z）的软件处于开发初始阶段，一切都可能随时被改变。这样的公共 API 不应该被视为稳定版。

1.0.0 的版本号用于界定公共 API 的形成。这一版本之后所有的版本号更新都基于公共 API 及其修改内容。

先行版本号被标注在修订版之后，先加上一个连接号再加上一连串以句点分隔的标识符来修饰。先行版的优先级低于相关联的标准版本。

在使用语义化版本规则管理软件项目时，开发者需要遵循以上约定，根据代码库的变更情况适时地更新版本号，并将版本号信息清晰地反映在项目的发布说明中，以便用户了解到每个版本的变更内容和影响。

详细参考：[语义化版本控制规范（SemVer）](https://semver.org/lang/zh-CN/)

## 格式与用法

```shell
<type>[scope][!]: <description>

[body]

[footer(s)]
```

### `type` 选项说明

每个提交都必须使用类型字段前缀，它由一个名词构成，诸如 `feat` 或 `fix`，其后接可选的范围字段，可选的 `!`，以及必要的冒号（英文半角）和空格。

当一个提交为应用或类库实现了新功能时，必须使用 `feat` 类型。

当一个提交为应用修复了 Bug 时，必须使用 `fix` 类型。

具体参数和描述见下表：

| 参数       | 说明                       | 描述                                    |
|----------|--------------------------|:--------------------------------------|
| feat     | Features                 | 用于提交一个新增功能（这和语义化版本中的 MINOR 相对应）       |
| fix      | Bug Fixes                | 用于 Bug 修复的提交 （这和语义化版本中的 PATCH 相对应）    |
| build    | Builds                   | 用于修改项目构建系统，例如修改依赖库、外部接口或者升级 Node 版本等  |
| chore    | Chores                   | 用于对非业务性代码进行修改，例如修改构建流程或者工具配置等         |
| ci       | Continuous Integrations  | 用于修改持续集成流程，例如修改 Travis、Jenkins 等工作流配置 |
| docs     | Documentation            | 用于修改文档，例如修改 README 文件、API 文档等         |
| style    | Styles                   | 用于修改代码的样式，例如调整缩进、空格、空行等               |
| refactor | Code Refactoring         | 用于重构代码，例如修改代码结构、变量名、函数名等但不修改功能逻辑      |
| perf     | Performance Improvements | 用于优化性能，例如提升代码的性能、减少内存占用等              |
| test     | Tests                    | 用于修改测试用例，例如添加、删除、修改代码的测试用例等           |
| revert*  | Reverting                | 用于恢复代码时                               |

#### feat

用于表示对代码库添加新功能或功能扩展的变更，可能会引入新的功能或改变现有功能的行为。

适用场景：新增业务功能、新增 API、添加新的函数、模块、类、接口、类型声明等、引入新的模块或组件等场景。

提交信息一般格式为：`feat: 添加用户注册功能`、`feat(api): 添加新的 API`等。

#### fix

用于表示对代码库中 Bug 的修复或错误的变更。

适用场景：修复代码中的逻辑错误、修正异常行为、解决系统崩溃或异常退出等场景。

提交信息一般格式为：`fix: 修复登录页面无法跳转的问题`、`fix(api): 修复用户注册接口返回错误状态码`等。

#### chore

用于描述与构建过程、辅助工具、库配置等相关的变更，通常不会影响最终用户的功能或行为。例如：

- 增加、删除或更新项目中的开发依赖，例如添加新的开发工具、测试框架、lint 工具等。
- 更新构建工具或构建过程中的配置文件，如更新 Vite 配置、Webpack 配置、Babel 配置等。

总之，`chore` 更侧重于项目**开发过程中**的辅助任务和环境调整。

#### build

用于描述与构建过程相关的变更，包括但不限于构建工具的更新、构建过程的优化、编译器或打包工具的配置变更等。例如：

- 修改构建过程中使用的插件、加载器、环境变量等配置，更新构建配置文件或构建脚本，例如 Vite 配置、Webpack 配置、Babel 配置等。
- 对构建过程进行了优化，例如减小打包体积、提升构建速度、改进代码拆分等。
- 更新了构建脚本，例如 npm scripts、Makefile、shell 脚本等。

总之，`build` 更侧重于项目**构建过程中**的变更和优化。

#### ci

用于描述持续集成（Continuous Integration，CI）环境的改进和调整，以确保项目的持续集成流程的稳定性和效率，包括但不限于 CI
流程的更新、CI 配置的修改、CI 脚本的更新等。例如：

- CI/CD 工具集成：集成了新的 CI/CD 工具或服务。例如，新增了一个用于代码静态分析的工具、代码质量检查工具、自动化部署工具等。
- CI/CD 流程更新：更新了 CI/CD 流程中的步骤、触发条件、环境变量等配置，例如 GitHub Actions、GitLab CI、Travis CI 等。
- 自动化测试更新：更新了自动化测试脚本或测试用例。例如包括新增、修改或删除测试用例，以及更新测试覆盖率、测试报告等。

总之，`ci` 则关注于**持续集成环境**的改进和调整。

#### style

用于指示样式及代码风格的变更。例如：

- CSS及样式：页面样式、外观或布局的更改，添加、删除或更新页面中的图标、背景、颜色等视觉元素，修改前端框架组件的外观样式，调整文本字体、大小、颜色、对齐方式等。
- 代码格式化：对代码进行格式化，例如调整缩进、添加空行、统一命名等操作。
- 代码注释更新：更新代码注释，例如添加、修改或删除注释。
- 命名规范：调整代码中的变量名、函数名、类名等命名规范。
- 代码风格指南遵循：调整代码以符合项目或团队的代码风格指南。例如代码缩进风格、代码排版规范、代码注释规范等。
- 移除无用代码：移除无用的代码、注释或空行。

总的来说，style 参数用于指示系统样式、代码风格的变更。

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

## 示例

### 基础的提交说明

```shell
feat: add email notifications on new direct messages
```

### 包含范围的提交说明

```shell
feat(lang): add polish language
```

### 包含了描述并且脚注中有破坏性变更的提交说明

```shell
feat: allow provided config object to extend other configs
BREAKING CHANGE: `extends` key in config file is now used for extending other config files
```

### 包含了 ! 字符以提醒注意破坏性变更的提交说明

```shell
feat!: send an email to the customer when a product is shipped
```

### 包含了范围和破坏性变更 ! 的提交说明

```shell
feat(api)!: send an email to the customer when a product is shipped
```

### 包含了 ! 和 BREAKING CHANGE 脚注的提交说明

```shell
chore!: drop support for Node 6
BREAKING CHANGE: use JavaScript features not available in Node 6.
```

### 包含发布版本脚注的提交说明

```shell
feat: add theme with light and dark

Release-As: 0.2.0
```

### 包含多行正文和多行脚注的提交说明

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
