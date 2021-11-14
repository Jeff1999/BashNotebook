# Ch.5 `Bash`变量

环境变量：`Bash`环境自带变量。系统定义变量，可直接使用。可由父`Shell`传入子`Shell`。

通过`env`或`printenv`查看所有环境变量：

```bash
>>> env
SHELL=/bin/bash
CONDA_EXE=/home/ubuntu/anaconda3/bin/conda
_CE_M=
HISTSIZE=1000
LANGUAGE=en_US.utf8:
HISTTIMEFORMAT=%F %T 
PWD=/home/ubuntu
LOGNAME=ubuntu
XDG_SESSION_TYPE=tty
CONDA_PREFIX=/home/ubuntu/anaconda3
MOTD_SHOWN=pam
HOME=/home/ubuntu
LANG=en_US.utf8
LS_COLORS=rs=0:di=01;34:ln=01;36:mh=00:pi=40;33:so=01;35:do=01;35:bd=40;33;01:cd=40;33;01:or=40;31;01:mi=00:su=37;41:sg=30;43:ca=30;41:tw=30;42:ow=34;42:st=37;44:ex=01;32:*.tar=01;31:*.tgz=01;31:*.arc=01;31:*.arj=01;31:*.taz=01;31:*.lha=01;31:*.lz4=01;31:*.lzh=01;31:*.lzma=01;31:*.tlz=01;31:*.txz=01;31:*.tzo=01;31:*.t7z=01;31:*.zip=01;31:*.z=01;31:*.dz=01;31:*.gz=01;31:*.lrz=01;31:*.lz=01;31:*.lzo=01;31:*.xz=01;31:*.zst=01;31:*.tzst=01;31:*.bz2=01;31:*.bz=01;31:*.tbz=01;31:*.tbz2=01;31:*.tz=01;31:*.deb=01;31:*.rpm=01;31:*.jar=01;31:*.war=01;31:*.ear=01;31:*.sar=01;31:*.rar=01;31:*.alz=01;31:*.ace=01;31:*.zoo=01;31:*.cpio=01;31:*.7z=01;31:*.rz=01;31:*.cab=01;31:*.wim=01;31:*.swm=01;31:*.dwm=01;31:*.esd=01;31:*.jpg=01;35:*.jpeg=01;35:*.mjpg=01;35:*.mjpeg=01;35:*.gif=01;35:*.bmp=01;35:*.pbm=01;35:*.pgm=01;35:*.ppm=01;35:*.tga=01;35:*.xbm=01;35:*.xpm=01;35:*.tif=01;35:*.tiff=01;35:*.png=01;35:*.svg=01;35:*.svgz=01;35:*.mng=01;35:*.pcx=01;35:*.mov=01;35:*.mpg=01;35:*.mpeg=01;35:*.m2v=01;35:*.mkv=01;35:*.webm=01;35:*.ogm=01;35:*.mp4=01;35:*.m4v=01;35:*.mp4v=01;35:*.vob=01;35:*.qt=01;35:*.nuv=01;35:*.wmv=01;35:*.asf=01;35:*.rm=01;35:*.rmvb=01;35:*.flc=01;35:*.avi=01;35:*.fli=01;35:*.flv=01;35:*.gl=01;35:*.dl=01;35:*.xcf=01;35:*.xwd=01;35:*.yuv=01;35:*.cgm=01;35:*.emf=01;35:*.ogv=01;35:*.ogx=01;35:*.aac=00;36:*.au=00;36:*.flac=00;36:*.m4a=00;36:*.mid=00;36:*.midi=00;36:*.mka=00;36:*.mp3=00;36:*.mpc=00;36:*.ogg=00;36:*.ra=00;36:*.wav=00;36:*.oga=00;36:*.opus=00;36:*.spx=00;36:*.xspf=00;36:
CONDA_PROMPT_MODIFIER=(base) 
PROMPT_COMMAND=history -a; 
SSH_CONNECTION=112.49.233.4 56891 10.0.8.3 22
LESSCLOSE=/usr/bin/lesspipe %s %s
XDG_SESSION_CLASS=user
TERM=xterm
_CE_CONDA=
LESSOPEN=| /usr/bin/lesspipe %s
USER=ubuntu
CONDA_SHLVL=1
DISPLAY=localhost:11.0
SHLVL=1
XDG_SESSION_ID=135755
CONDA_PYTHON_EXE=/home/ubuntu/anaconda3/bin/python
XDG_RUNTIME_DIR=/run/user/1000
SSH_CLIENT=112.49.233.4 56891 22
CONDA_DEFAULT_ENV=base
PATH=/home/ubuntu/anaconda3/bin:/home/ubuntu/anaconda3/condabin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin
DBUS_SESSION_BUS_ADDRESS=unix:path=/run/user/1000/bus
SSH_TTY=/dev/pts/2
_=/usr/bin/env

>>> printenv
...
```

常见环境变量：

- `BASHPID`：Bash 进程的进程 ID。
- `BASHOPTS`：当前 Shell 的参数，可以用`shopt`命令修改。
- `DISPLAY`：图形环境的显示器名字，通常是`:0`，表示 X Server 的第一个显示器。
- `EDITOR`：默认的文本编辑器。
- `HOME`：用户的主目录。
- `HOST`：当前主机的名称。
- `IFS`：词与词之间的分隔符，默认为空格。
- `LANG`：字符集以及语言编码，比如`zh_CN.UTF-8`。
- `PATH`：由冒号分开的目录列表，当输入可执行程序名后，会搜索这个目录列表。
- `PS1`：Shell 提示符。
- `PS2`： 输入多行命令时，次要的 Shell 提示符。
- `PWD`：当前工作目录。
- `RANDOM`：返回一个0到32767之间的随机数。
- `SHELL`：Shell 的名字。
- `SHELLOPTS`：启动当前 Shell 的`set`命令的参数，参见《set 命令》一章。
- `TERM`：终端类型名，即终端仿真器所用的协议。
- `UID`：当前用户的 ID 编号。
- `USER`：当前用户的用户名。

`Bash`区分大小写。如果用于自定义一个常量，常使用全部大写的变量名。

查看单个环境变量的值：

```bash
>>> printenv HOME
/home/ubuntu

>>> echo $HOME
/home/ubuntu
```

---------------

自定义变量：用户在当前`Shell`中定义的变量。仅限在当前`Shell`使用，退出后被清空。

`set`命令用于显示所有变量（环境变量和自定义变量），以及所有的`Bash`函数。

```bash
>>> set
...
```

变量命名规则：

- 字母、数字和下划线字符组成。
- 第一个字符必须是一个字母或一个下划线，不能是数字。
- 不允许出现空格和标点符号。

命名语法：

```bash
>>> variable=value
```

通过使用分号`;`可在同一行定义多个变量：

```bash
>>> a=1; b=2
```

`Bash`的所有变量类型均为字符串，通过引号可将空格、转义字符、其他变量值、命令执行结果和数学运算结果等包含在变量中：

```bash
>>> var1=hellobash

>>> var2="hello bash"

>>> var3="$var2, this is jeff."

>>> var4="hello\nbash"

>>> var5=$(ls -l /home/)

>>> var6=$((1 + 1))
```

当变量重复赋值会覆盖原先赋值。

----------------

读取变量：在变量名之前加`$`。当变量不存在时，`Bash`输出空字符。

花括号`{}`常用于变量名与其它字符串连用的情况：

```bash
>>> echo ${var1}_file
hellobash_file

>>> echo $var1_file

```

如果变量的值是另一个变量，可使用`${!variable}`读取最终的值：

```bash
>>> var7=USER

>>> echo ${!var7}
ubuntu
```

如果变量值包含连续空格（或制表符和换行符），常使用双引号读取：

```bash
>>> var8="1     2     3"; echo "$var8"
1     2     3

>>> var9="1     2     3"; echo $var9
1 2 3
```

------------

删除变量：`unset`命令用于删除变量。

```bash
>>> unset variable
```

由于在`Bash`中不存在的变量为空字符串，所以即使删除变量后仍可读取，其值为空字符串。等效于：

```bash
>>> variable=""

>>> variable=
```

-----------------

输出变量：`export`用于将在父`Shell`中的自定义变量传递给子`Shell`。

```bash
>>> export var10="hello world"

>>> bash

>>> echo $var10
hello world

>>> var10="HELLO WORLD"

>>> echo$var10
HELLO WORLD

>>> exit

>>> echo $var10
hello world
```

在退出子`Shell`后，被修改的变量值在父`Shell`中不会受影响。

------------

特殊变量

----------------------

变量默认值

----------------

`declare`命令

--------------

`readonly`命令

--------------

`let`命令