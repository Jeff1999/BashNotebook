# Ch.2 `Bash`基本语法

`Shell`的命令格式如下：

```bash
>>> command [ arg1 ... [ argN ]]
```

上述`command`是具体的命令或一个可执行文件，`arg1 ... argN`是传递给命令的可选参数。参数的短形式常手动输入；长形式常用于脚本中，增加可读性。两种形式的作用完全相同。

```bash
>>> ls -r /home
ubuntu  lighthouse

>>> ls --reverse
ubuntu  lighthouse
```

- 分号`;`：命令结束符。位于同一行的前一个命令执行结束后（无论执行成功或失败），再执行下一个命令。

  ```bash
  >>> echo Hello; echo Bash!
  Hello
  Bash!
  ```

- 命令组合符`&&`和`||`：控制多个命令之间的继发关系。

  ```bash
  >>> expr 1 + 1 == 2 && echo correct
  1
  correct
  ```

  `&&`表示如果前一个命令运行成功，则继续运行下一个命令。

  ```bash
  >>> expr 1 + 1 == 100 || echo not correct
  1
  not correct
  
  >>> expr 1 + 1 == 2 || echo correct
  1
  ```

  `||`表示如果前一个命令运行失败，则继续运行下一个命令。

---------------------------------------------

`echo`：在屏幕输出文本，可以将该命令的参数原样输出。

```bash
>>> echo Hello Bash!
Hello Bash!

>>> echo Hello \
> Bash!
Hello Bash!
```

反斜杠`\`用于多行编辑，`Bash`将下一行与当前行放在一起解释。

```bash
>>> echo Hello		       Bash!
Hello Bash!
```

`Bash`使用空格区分不同的参数。上述`Hello`和`Bash!`是两个参数，`Bash`会自动忽略多余的空格。

- 参数`-e`：激活双引号`“”`和单引号`‘’`中的转义字符。

  ```bash
  >>> echo -e "Hello Bash,\nthis is Jeff."
  Hello Bash,
  this is Jeff.
  
  >>> echo -e 'HELLO BASH,\nTHIS IS JEFF.'
  HELLO BASH,
  THIS IS JEFF.
  
  >>> echo "Hello Bash,\nthis is Jeff."
  Hello Bash,\nthis is Jeff.
  ```

- 参数`-n`：取消本次输出末尾的换行符`\n`，使下一个提示符紧跟在本次输出文本后面。

  ```bash
  >>> echo -n Hello Bash!
  Hello Bash!>>>
  
  >>> echo -n Hello; echo Bash
  HelloBash
  
  >>> echo Hello; echo Bash
  Hello
  Bash
  ```

--------------------------------------------------------

`type`：用于判断命令来源。

```bash
>>> type echo
echo is a shell builtin

>>> type expr
expr is hashed (/usr/bin/expr)
```

上述`echo`为内部命令，`expr`为外部程序。

- 参数`-a`：查看命令的所有定义。

  ```bash
  >>> type -a echo
  echo is a shell builtin
  echo is /usr/bin/echo
  echo is /bin/echo
  ```

- 参数 `-t`：返回一个命令的类型，例如别名（alias），关键词（keyword），函数（function），内置命令（builtin）和文件（file）。

  ```bash
  >>> type -t bash
  file
  
  >>> type -t if
  keyword
  ```

---------------------------------------------

常用快捷键：

- `Ctrl + L`：清楚屏幕并将光标移动到页面顶部。
- `Ctrl + C`：中止当前正在执行的命令。
- `Shift + PageUp / PageDown`：向上/下滚动。
- `Crtl + U`：从光标当前位置删除到行首。
- `Ctrl + K`：从光标当前位置删除到行尾。
- `Ctrl + D`：关闭`Shell`对话。
- `↑ / ↓`：浏览已执行命令的历史记录。
- `Tab`：自动补全。