# Ch.6 字符串操作

- 字符串的长度

获取长度时，必须使用`${#string}`的语法结构来表示字符串长度。如果没有大括号，`Bash`会把`$#`解释成参数个数，而`string`会被当作字符串直接输出。

例如：

```bash
>>> var="Hello World"

>>> echo ${#var}
11

>>> echo $#var
0var
```

----------------------

- 子字符串

字符串可通过`${string:offset:length}`的语法来提取子串，`offset`表示起始位置（下标从`0`开始），`length`表示子串的长度。省略`length`时，输出从`offset`位置开始的剩余子串。例如：

```bash
>>> string="HelloWorld"

>>> echo ${string:5:5}
World

>>> echo ${string:2}
llo_World
```

当`offset`为负值时，表示从字符串的末尾开始计算。负数前必须有一个空格，防止与`$variable:-world`的变量设置默认值语法混淆。当`length`为负值时，表示子串截至到倒数第`abs(length)`个字符（不包含倒数第`abs(length)`个字符）：

```bash
>>> string="Hello World Hello Linux"

>>> echo ${string: -5}
Linux

>>> echo ${string: -11:-6}
Hello
```

其中，`string`本身必须是变量，而不能是字符串：

```bash
>>> echo ${"HelloWorld":5:5}
-bash: ${"HelloWorld":5:5}: bad substitution
```

-----------

- 搜索和替换

`Bash`提供了字符串搜索和替换的多种方法，并支持通配符的使用。

字符串头部模式匹配：匹配成功则删除匹配部分，返回剩余部分。否则返回原始字符串。原变量不会发生变化。

`${string#pattern}`非贪婪匹配，删除从头部开始的最短匹配部分，返回剩余部分；

`${string##pattern}`贪婪匹配，删除从头部开始的最长匹配部分，返回剩余部分：

```bash
>>> path=/home/yifan/test.txt

>>> echo ${path#/*/}
yifan/test.txt

>>> echo ${path##/*/}
test.txt
```

头部替换：`${string/#pattern/new}`完成非贪婪匹配后，将被匹配部分替换后并返回；

```bash
>>> pic=/home/yifan/test.jpg

>>> echo ${pic/#.jpg/.png}
/home/yifan/test.jpg
```



字符串尾部模式匹配：匹配成功则删除匹配部分，返回剩余部分。否则返回原始字符串。原变量不会发生变化。

`${string%pattern}`非贪婪匹配，删除从尾部开始的最短匹配部分，返回剩余部分；

`${string%%pattern}`贪婪匹配，删除从尾部开始的最长匹配部分，返回剩余部分：

```bash
>>> path=/home/yifan/test.1.txt

>>> echo ${path%.*}
/home/yifan/test.1

>>> echo ${path%%.*}
/home/yifan/test
```

尾部替换：`${string/%pattern/new}`

```bash
>>> path=/home/yifan/test.1.txt

>>> echo ${path/%[0-9].*/md}
/home/yifan/test.md
```



任意位置的模式匹配与替换：匹配成功则删除匹配部分，返回剩余部分。否则返回原始字符串。原变量不会发生变化。

`${string/pattern/new}`非贪婪匹配，删除符合`pattern`的最短匹配部分，替换后返回剩余部分；

`${string//pattern/new}`贪婪匹配，删除符合`pattern`的最长匹配部分，替换后返回剩余部分。

当`new`为空时，等效于删除功能：

```bash
>>> path=/home/yifan/test.1.txt

>>> echo ${path/yifan/jeff}
/home/jeff/test.1.txt

>>> echo ${path//.*/.md}
/home/yifan/test.md

>>> echo ${path/test*/}
/home/yifan
```

-------------------------

- 改变大小写

`${string^^}`将字符串改为大写；`${string,,}`将字符串改为小写。

```bash
>>> string="hElLo WoRlD"

>>> echo ${string^^}
HELLO WORLD

>>> echo ${string,,}
hello world
```