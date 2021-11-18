# Ch.3 `Bash`的模式扩展

`Shell`在执行用于的指令前，会根据空格将用户的输入拆分成词元（token），并扩展词元中的特殊字符。扩展完成后才会调用相应的命令。这种对于特殊字符的扩展，称为模式扩展（globbing），其中用到通配符的扩展又称为通配符扩展（wildcard expansion）。

`Bash`提供了以下八种扩展：

- 波浪线`~`扩展
- 问号`?`扩展
- 星号`*`扩展
- 方括号`[...]`扩展
- 大括号`{...}`扩展
- 变量`${...}`扩展
- 子命令`$(...)`扩展
- 算术`$((...))`扩展

命令本身不存在参数扩展，`Bash`会原样执行用户输入的参数。

模式扩展先于正则表达式出现，可以看作是原始的正则表达式。

`Bash`允许用户通过以下两种方式关闭扩展：

```bash
>>> set -o noglob

>>> set -f
```

通过以下两种方式重新打开扩展：

```bash
>>> set +o noglob

>>> set +f
```

------------

波浪线`~`扩展：扩展当前用户的主目录。

```bash
>>> echo ~
/home/ubuntu
```

`~user`表示扩展成用户`user`的主目录：

```bash
>>> echo ~root
/root

>>> echo ~ubuntu
/home/ubuntu
```

当`user`不存在时，不会发生扩展。

`~+`表示扩展当前所在目录，等效于`pwd`：

```bash
>>> echo ~+
/home/ubuntu

>>> pwd
/home/ubuntu
```

------------------

问号`?`扩展：`?`代表文件路径中的任意单个字符（不包括空字符），例如`data???`匹配所有`data`后面跟随三个字符的文件名。

匹配单个字符时：

```bash
>>> ls data_?.csv
data_1.csv  data_2.csv
```

匹配多个字符时：

```bash
>>> ls data_???.csv
data_001.csv  data_002.csv
```

当文件不存在时，不会发生扩展。

----------

星号`*`扩展：`*`代表文件路径中的任意数量的任意字符（包括空字符）。

```bash
>>> ls *.csv
data_1.csv  data_2.cs
```

匹配空字符：

```bash
>>> ls data_1*.csv
data_10.csv  data_1.csv
```

`*`默认不会匹配隐藏文件。当匹配隐藏文件时，在星号前添加`.`：

```bash
>>> echo .*
. .. .astropy .bash_history .bash_logout .bashrc .cache .conda .condarc .config .ipynb_checkpoints .ipython .jetbrains_rplugin_helpers .jupyter .lesshst .local .pip .profile .pycharm_helpers .pydistutils.cfg .python_history .ssh .sudo_as_admin_successful .viminfo .vimrc .Xauthority
```

使用`.[!.]*`可以去掉特殊文件`.`和`..`：

```bash
>>> echo .[!.]*
.astropy .bash_history .bash_logout .bashrc .cache .conda .condarc .config .ipynb_checkpoints .ipython .jetbrains_rplugin_helpers .jupyter .lesshst .local .pip .profile .pycharm_helpers .pydistutils.cfg .python_history .ssh .sudo_as_admin_successful .viminfo .vimrc .Xauthority
```

当文件不存在时，不会发生扩展。

匹配子目录文件时，通过控制`*/`数量来控制匹配层数：

```bash
>>> echo */*.sh
downloads/Anaconda3-2021.05-Linux-x86_64.sh

>>> echo */*/*.sh
anaconda3/lib/tclConfig.sh anaconda3/lib/tclooConfig.sh anaconda3/lib/tkConfig.sh anaconda3/lib/xml2Conf.sh anaconda3/lib/xsltConf.sh githubRepo/mtag/024.gwas.mtag.sh githubRepo/mtag/mytest.sh
```

通过`shopot`设置`globstar`参数，允许通过`**`来匹配零个或多个子目录。

------------

方括号`[...]`扩展：表示匹配方括号中的任意一个字符。当文件不存在时，不会发生扩展。

```bash
>>> touch data_1.csv data_2.csv

>>> ls data_[12].csv
data_1.csv  data_2.csv

>>> ls data_[134].csv
data_1.csv
```

当匹配`[`和`]`字符时，可放在方括号中，例如：`[[123]`或`[123]]`；

当匹配`-`字符时，只能放在开头或结尾，例如：`[-123]`或`[123-]`。

当匹配不在方括号中的字符时，可使用`[^...]`和`[!...]`两种等效的方法：

```bash
>>> ls data_[^234].csv
data_1.csv

>>> ls data_[!134].csv
data_2.csv
```

`[start-end]`简写形式：表示匹配一个连续的范围。例如：`[a-c]`等效于`[abc]`，`[0-5]`等效于`[012345]`，`[!x-z]`等效于`[!xyz]`。

```bash
>>> ls data_[0-9].csv
data_1.csv  data_2.csv
```

常用简写形式：

- `[a-z]`：所有小写字母。
- `[a-zA-Z]`：所有小写字母与大写字母。
- `[a-zA-Z0-9]`：所有小写字母、大写字母与数字。
- `[abc]*`：所有以`a`、`b`、`c`字符之一开头的文件名。
- `program.[co]`：文件`program.c`与文件`program.o`。
- `BACKUP.[0-9][0-9][0-9]`：所有以`BACKUP.`开头，后面是三个数字的文件名。

----------------

大括号`{...}`扩展：表示分别扩展成大括号中的所有值，各值之间使用逗号分隔（没有空格）。

```bash
>>> echo {1,2,3}
1 2 3

>>> echo data_{A,B,C}
data_A data_B data_C

>>> echo file{,x,y,z}
file filex filey filez
```

大括号扩展不是文件名扩展，无论是否有对应文件存在都会扩展成给定值。

大括号与其他扩展符连用时，总是先于其他扩展符进行扩展：

```bash
>>> touch data_1.csv data_2.csv

>>> echo data_{1,2,[!1]}.csv
data_1.csv data_2.csv data_2.csv
```

`{start..end..step}`简写形式：表示扩展成一个连续序列，例如`{a..c}`等效于`{a,b,c}`。

```bash
>>> echo {a..z}
a b c d e f g h i j k l m n o p q r s t u v w x y z

>>> echo {3..1}
3 2 1

>>> echo Hello-Bash{1..4}.0
Hello-Bash1.0 Hello-Bash2.0 Hello-Bash3.0 Hello-Bash4.0

>>> echo {001..5}
001 002 003 004 005

>>> echo {a..c}{1..3}
a1 a2 a3 b1 b2 b3 c1 c2 c3

>>> echo {0..6..2}
0 2 4 6
```

常用于创建目录：

```bash
>>> mkdir 20{19,20,21}_{01..12}

>>> mkdir {a..c}{1..3}
```

常用于`for`循环：

```bash
>>> for i in {1..3}
> do
> echo $i
> done
1
2
3
```

--------------

变量`${...}`扩展：使用`$`或`${...}`将词元扩展为变量值。

```bash
>>> echo $USER
ubuntu

>>> echo ${USER}
ubuntu
```

`${!string*}`或`${!string@}`返回所有包含字符串`string`的变量名。

```bash
>>> echo ${!U*}
UID USER

>>> echo ${!SH@}
SHELL SHELLOPTS SHLVL
```

------------------

子命令`$(...)`扩展：使用`$(...)`或\`…\`将词元扩展成另一个命令的运行结果，该命令的所有输出都会作为返回值。

```bash
>>> echo $(date)
Thu 18 Nov 2021 09:00:21 PM CST

>>> echo `expr 1 + 1`
2
```

子命令嵌套扩展：

```bash
>>> echo $(ls $(find data_[0-9].csv))
data_1.csv data_2.csv
```

--------

算术`$((...))`扩展：扩展成整数运算的结果。

```bash
>>> echo $((1 + 1))
2
```

------------------------

字符类：`[[:class:]]`扩展成某一类特定字符之中的一个。常用的字符类如下。

- `[[:alnum:]]`：匹配任意英文字母与数字
- `[[:alpha:]]`：匹配任意英文字母
- `[[:blank:]]`：空格和 Tab 键。
- `[[:cntrl:]]`：ASCII 码 0-31 的不可打印字符。
- `[[:digit:]]`：匹配任意数字 0-9。
- `[[:graph:]]`：A-Z、a-z、0-9 和标点符号。
- `[[:lower:]]`：匹配任意小写字母 a-z。
- `[[:print:]]`：ASCII 码 32-127 的可打印字符。
- `[[:punct:]]`：标点符号（除了 A-Z、a-z、0-9 的可打印字符）。
- `[[:space:]]`：空格、Tab、LF（10）、VT（11）、FF（12）、CR（13）。
- `[[:upper:]]`：匹配任意大写字母 A-Z。
- `[[:xdigit:]]`：16进制字符（A-F、a-f、0-9）。

```bash
>>> echo data_[[:digit:]].csv
data_1.csv data_2.csv

>>> echo [[:upper:]]*
R
```

-----------------------

使用注意点：

- 通配符先解释，再执行。

  `Bash`在接受到命令后，先对通配符进行扩展，再执行命令。

- 文件名扩展不匹配时，原样输出。

  大括号扩展`{...}`不属于文件名扩展。

- 只适用于单层路径。

  所有文件名扩展只能匹配单层路径，`?`和`*`不能匹配路径分隔符`/`。

  在`Bash 4.0`中，通过`shopt`设置`globstar`参数，允许`**`匹配零个或多个子目录。

- 文件名可以使用通配符。

  引用文件名时，需要使用单引号或双引号：

  ```bash
  >>> touch "data*.txt": ls data?.txt
  'data*.txt'
  ```

--------------------

量词语法：

---------------------------------

`shopt`命令：