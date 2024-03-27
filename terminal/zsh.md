# Zsh

![zsh__banner.png](../static/images/terminal/zsh__banner.png)

## 什么是 Zsh？

Zsh（Z Shell）是一种命令行解释器。

类似于 Bash，是 Bash 的替代品，具有更强大的功能和更灵活的配置选项，提供了更多的功能和改进，包括更好的自动补全、更强大的通配符扩展、更好的历史命令管理等等。

Zsh 是许多 Linux 和 macOS 系统的默认 Shell，也被广泛用于开发人员和系统管理员的工作中。

## 和 Bash 的对比

Bash（Bourne Again Shell）和 Zsh（Z Shell）都是常见的命令行解释器，用于在 Unix-like 系统（如 Linux 和
macOS）中执行命令和脚本。

### 功能和扩展性

Bash 是一个功能强大的 Shell，具有广泛的功能，但相对于 Zsh 来说，它的扩展性可能较弱。

Zsh 在功能和扩展性方面更加强大，提供了更多的功能和选项，例如更好的自动补全、更强大的通配符扩展、更好的历史命令管理等。

### 默认情况下的使用

在许多 Unix-like 系统中，默认的 Shell 是 Bash，包括大多数 Linux 发行版和 macOS。

虽然 Zsh 不是默认的 Shell，但许多用户选择将其作为替代 Shell，并根据自己的需求进行配置。

### 目标人群

Bash 是一种功能强大且广泛使用的 Shell，适合大多数用户的基本需求。

而 Zsh 则提供了更多的功能和灵活性，适合那些希望定制和扩展命令行体验的用户。

### 历史

Bash 是 Unix Shell 的一种扩展，是 Bourne Shell（sh）的改进版本，最初由 Brian Fox 开发。

Zsh 是一个独立的 Shell，最初由 Paul Falstad 开发。它是从 Bourne Shell 衍生出来的，并在功能和用户体验方面进行了扩展和改进。


## 安装配置

https://github.com/ohmyzsh/ohmyzsh/wiki/Installing-ZSH

### 安装

```shell
sudo apt install zsh
```

### 查看版本

```shell
zsh --version
```

### 修改系统默认登录 Shell

```shell
chsh -s $(which zsh)
```

### 配置`.zshrc`

在 `~` 目录下打开 `.zshrc` 文件（可能需要新建）。

```shell
vim ~/.zshrc
```

输入以下内容

```shell
# 配置命令行颜色打开
export CLICOLOR=1
# 配置命令行颜色主题
export LSCOLORS=FxGxFxdaCxDaDahbadeche

# 加载 colors 模块并启用颜色支持
autoload -U colors && colors

# 配置终端提示符颜色
PROMPT="%{$fg_bold[red]%}➜ %{$fg_bold[cyan]%}%m %{$fg_bold[green]%}%1~ %{$fg_bold[red]%}% %{$reset_color%}> "

# 配置 ls 颜色 
if [ -x /usr/bin/dircolors ]; then
    test -r ~/.dircolors && eval "$(dircolors -b ~/.dircolors)" || eval "$(dircolors -b)"
    alias ls='ls --color=auto'
    #alias dir='dir --color=auto'
    #alias vdir='vdir --color=auto'

    alias grep='grep --color=auto'
    alias fgrep='fgrep --color=auto'
    alias egrep='egrep --color=auto'
fi

# 配置常用别名
alias ll='ls -l'
alias la='ls -A'
alias l='ls -CF'
```
