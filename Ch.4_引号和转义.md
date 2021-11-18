# Ch.3 引号和转义

转义（escape）：在特殊符号（例如`$`, `&`, `*`）前加上反斜杠`\`，使其变成普通字符。

```bash
>>> echo \$USER
$USER

>>> echo $USER
ubuntu

>>> echo \\
\
```

反斜杠可用于其他不可打印的字符：

- `\a`：响铃
- `\b`：退格
- `\n`：换行
- `\r`：回车
- `\t`：制表符

通过使用`echo`的`-e`参数打印转义字符：

```bash
>>> echo -e "Hello\tWorld"
Hello	World

>>> echo "Hello\tWorld"
Hello\tWorld
```

反斜杠常用于将一条长命令拆分成多行：

```bash
>>> mv \
/home/ubuntu/before \
/home/ubuntu/after
```

等同于：

```bash
>>> mv /home/ubuntu/before /home/ubuntu/after
```

原理：换行符`\n`用于表示命令的结束，`Bash`在接受到换行符后，对输入的命令进行解释执行。在换行符前面加上反斜杠转义，使得换行符成为一个普通字符，在`Bash`中被当作空格处理。

-------------

单引号：用于保留字符的字面含义。特殊字符在单引号中变为普通字符。

```bash
>>> echo '$((1 + 1))'
$((1 + 1))
```

单引号使得`Bash`扩展、变量引用、算数运算和子命令失效。

----------------------

双引号：同单引号。三个特殊字符除外：美元符号、反引号和反斜杠。保留特殊含义，会被`Bash`自动扩展。美元符号用于引用变量，反引号用于执行子命令。

```bash
>>> echo "$SHELL"
/bin/bash

>>> echo "`date`"
Mon 15 Nov 2021 08:30:54 PM CST

>>> echo -e "He says: \"Hello!\""
He says "Hello!"

>>> echo "\\"
\
```

换行符在双引号中不再作为命令结束符，只是作为普通的转换符：

```bash
>>> echo "Hello
World"
Hello
World
```

当文件名包含空格时，使用双引号或单引号输出文件名：

```bash
>>> touch "hello    world.txt"; ls "hello    world.txt" 
'hello    world.txt'

>>> echo "there  are   some      blanks"
there  are   some      blanks
```

双引号用于保留原始命令的输出格式：

```bash
>>> echo "$(cal)"
   November 2021      
Su Mo Tu We Th Fr Sa  
    1  2  3  4  5  6  
 7  8  9 10 11 12 13  
14 15 16 17 18 19 20  
21 22 23 24 25 26 27  
28 29 30

>>> echo $(cal)
November 2021 Su Mo Tu We Th Fr Sa 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19 20 21 22 23 24 25 26 27 28 29 30
```

_________

`Here`文档

`Here`文档（Here document）是一种输入多行字符串的方法，格式如下：

```bash
<< token
document
token
```

`<< token`：开始标记符。`token`常用文档的名称。

`token`：结束标记符。

`Here`文档内部发生变量替换，支持反斜杠转义，不支持通配符扩展，双引号和单引号变成普通字符。

```bash
>>> var="Hello Bash"

>>> cat << __example__
> $var
> "$var"
> '$var'
> __example__
Hello Bash
"Hello Bash"
'Hello Bash'
```

若在`token`处加上单引号，则变量替换失效：

```bash
>>> cat << '__example__'
> $var
> "$var"
> '$var'
> __example__
$var
"$var"
'$var'
```

`Here`文档的本质是重定向，它将字符串重定向输出给每个命令，等效于`echo`命令：

```bash
>>> cat << __example__
> Hello Bash
> __example__
Hello Bash

>>> echo "Hello Bash" | cat
Hello Bash
```

`Here`文档只适用于接受以标准输入作为参数的命令，例如`echo`命令无法接受标准输入作为参数，则`Here`文档对其无效，`echo`命令不会有任何输出：

```bash
>>> echo << __example__
> Hello Bash
> __example__

```

`Here`文档不能作为变量的值，只能用于接受标准输入作为参数的命令的参数。

-----------------

`Here`字符串（Here string），用三个小于号`<<<`表示，格式如下：

```bash
<<< string
```

`Here`字符串将字符串通过标准输入传递给命令，只适用于接受标准输入作为参数的命令。

```bash
>>> cat <<< "Hello Bash"
Hello Bash

>>> echo "Hello Bash" | cat
Hello Bash

>>> md5sum <<< 'Hello Bash'
18e38bf51006df9890a7b4694fc6b45c  -

>>> echo "Hello Bash" | md5sum
18e38bf51006df9890a7b4694fc6b45c  -
```
