# Claude Code 接入 DeepSeek V4

## 环境准备

### - Node.js 环境
### - Git 环境
### - cc-switch 环境

在 Github [cc-switch](https://github.com/farion1231/cc-switch/blob/main/docs/release-notes/v3.14.1-zh.md) 仓库下载与系统对应的 release 文件并安装。

## CC 命令行安装

```shell
npm install -g @anthropic-ai/claude-code
```

版本检查
```shell
claude --version
```

直接启动 `claude` 会弹出 `Unable to connect` `Failed to connect` 是正常情况，需要按下文修改配置文件。

## CC 配置文件修改

在用户 Home 文件夹下打开 .claude.json（注意是隐藏文件）。

在 json 配置内容末尾加入配置：
```
"hasCompletedOnboarding": true
```

修改配置文件后，打开终端输入 `claude`，如果出现了 `Security guide`，并提示是否信任文件夹。即视为配置成功，否则检查配置文件格式。

## 接入 Deepseek V4

### 登录 [Deepseek API 开放平台](https://platform.deepseek.com/usage)

### 充值，需保持余额不为零

### 进入 API Keys 菜单，申请 API key

> 注意，生成的 API key 要**复制**并**妥善保存**，也注意不要泄露。

## 配置 cc-switch

### 右上角点击 + 号，并选择 DeepSeek 供应商

### 下拉页面，在 API key 输入栏中填入申请的 API key

### 选择模型

将 **主模型**，**Haiku 默认模型**，**Sonnet 默认模型**，**Opus 默认模型** 均设置为 `deepseek-v4-pro[1m]`。

### 点击添加保存配置

## 检查 CC 情况

命令行输入 `Claude`，启动 Claude Code。

输入 `/model` 指令检查模型列表，均为 DeepSeek 则大功告成。

