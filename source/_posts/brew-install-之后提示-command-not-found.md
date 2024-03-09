---
title: brew install 之后提示 command not found
date: 2024-03-09 18:21:35
tags:
  - MacOS
  - 软件
category:
  - 软件工具
---

使用 Mac 的同学大部分应该都用过 [homebrew](https://brew.sh/)，安装软件相当的方便，不需要我们处理软件的依赖关系。但是有时候在安装完后执行命令却提示 `command not found`，昨天就遇到了这个问题，在 `brew install qcachegrind` 之后执行 `qcachegrind` 提示 `command not found`，调查了一番后顺便记录下来。

### brew 安装的目录是否在 $PATH 中

brew 安装后的软件都会生成一个软链接，M1 的 MacOS 会在 `/opt/homebrew/bin`, 这个目录下你可以看到所有使用 brew 安装后的提供了命令行的软件，比如我安装了 qcachegrind, 这个就会有一个软链接 `lrwxr-xr-x@ qcachegrind -> /opt/homebrew/Cellar/qcachegrind/24.02.0/qcachegrind.app/Contents/MacOS/qCachegrind`，所以可以检查一下 `opt/homebrew/bin` 是否在 $PATH 中，可以在 terminal 中执行 `echo $PATH` 看一下，如果没有可以把这个目录加到 $PATH 中，不过一般使用 homebrew 官网提供的命令安装的 homebrew 都会默认添加到 $PATH 中。

### 检查软链接是否正确，可能因为系统大小写敏感导致软链接无效

检查完上一步之后可以确定环境变量没有问题，但是当我执行 qcachegrind 仍然提示 `command not found`，这时可以检查一下软链接是否正确。拿这个软件举例，软链接配置的是 `/opt/homebrew/Cellar/qcachegrind/24.02.0/qcachegrind.app/Contents/MacOS/qCachegrind`，可以 cd 到这个目录 `/opt/homebrew/Cellar/qcachegrind/24.02.0/qcachegrind.app/Contents/MacOS` 中看看，结果发现目录中可执行的二进制是 QCachegrind, 再加上笔者的系统是大小写敏感的，自然就会提示找不到对应的 command,可以重新创建软链接即可，在 terminal 中执行 `ln -sf /opt/homebrew/Cellar/qcachegrind/24.02.0/qcachegrind.app/Contents/MacOS/QCachegrind /opt/homebrew/bin/qcachegrind` 就可以了。
