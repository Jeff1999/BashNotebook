# Ch.1 `Shell`与`Bash`简介

- `Shell`是用户与内核（kernel）交互的对话界面。
- `Shell`提供给用户与操作系统对话的环境——命令行环境（Command Line Interface, CLI）。`Shell`接收用户输入的命令，并将其送入操作系统中执行，最后将结果返回给用户。
- `Shell`是一个命令解释器，解释用户输入的命令（例如变量，条件判断，循环等）。用户可通过编写无需编译的脚本（script），并交由`Shell`解释执行。
- `Shell`是一个工具箱，供用户使用操作系统的功能。

`Bash`是目前最常用的一种`Shell`，除此之外还有`Bourne Shell`, `C Shell`, `Z Shell`等。Linux 系统允许每个用户使用不同的`Shell`。

终端模拟器（terminal emulator）：用户进入图形环境后，需要自己启动终端模拟器，才能进入命令行环境。

命令行提示符：普通用户的提示符以美元符号`$`结尾，`root`用户以井号`#`结尾。命令行提示符可自定义。

- 查看当前使用的`Shell`：

  ```bash
  >>> echo $SHELL
  /bin/bash
  ```

- 查看当前系统中的所有`Shell`：

  ```bash
  >>> cat /etc/shells
  # /etc/shells: valid login shells
  /bin/sh
  /bin/bash
  /usr/bin/bash
  /bin/rbash
  /usr/bin/rbash
  /bin/dash
  /usr/bin/dash
  /usr/bin/tmux
  /usr/bin/screen
  ```

- 启动`Bash`：

  ```bash
  >>> bash
  ```

- 退出`Bash`：

  ```bash
  >>> exit
  ```

- 查看`Bash`版本：

  ```bash
  >>> bash --version
  GNU bash, version 5.0.17(1)-release (x86_64-pc-linux-gnu)
  Copyright (C) 2019 Free Software Foundation, Inc.
  License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>
  
  This is free software; you are free to change and redistribute it.
  There is NO WARRANTY, to the extent permitted by law.
  
  >>> echo $BASH_VERSION
  5.0.17(1)-release
  ```