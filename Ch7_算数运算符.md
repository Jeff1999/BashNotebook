# Ch.7 算数表达式

算数表达式：`((...))`语法可以进行整数算数运算，出现非整数运算则报错。`Bash`自动忽略内部的空格。表达式支持嵌套结构。

```bash
>>> echo $((1 + 1))
2

>>> echo $(1+1)
2

>>> echo $((1.5 + 1))
-bash: 1.5 + 1: syntax error: invalid arithmetic operator (error token is ".5 + 1")

>>> echo $(($((5**2)) * 3))
75
```

语法不返回计算结果，通过`$?`命令可查看算术表达式计算结果是否错误，或计算结果是否为零。`0`表示表达式计算正确或结果不为零，`1`表示表达式计算错误或结果为零。

```bash
>>> ((1 + 1 == 2)); echo $?
0

>>> ((1 - 1 == 3)); echo $?
1

>>> ((1 - 1)); echo $?
1

>>> ((1 + 1)); echo $?
0
```

算数运算符：

- `+`：加法
- `-`：减法
- `*`：乘法
- `/`：除法（整除）
- `%`：余数
- `**`：指数
- `++`：自增运算（前缀或后缀）
- `--`：自减运算（前缀或后缀）
- `(...)`：改变运算优先级

`Bash`自动将表达式内的字符串当作变量解释，不需要使用美元符号`$`。空变量会被当成`0`解释。

```bash
>>> number=1; echo $((number + 1))
2

>>> number=1; echo $(($number + 1))
2

>>> number=; echo $((number + 1))
1

>>> var=; number=var; echo $((number + 1))
1
```

----------------

数值的进制：`Bash`默认十进制，可通过以下语法使用其他进制。

- `number`：没有任何特殊表示法的数字是十进制数（以10为底）。
- `0number`：八进制数。
- `0xnumber`：十六进制数。
- `base#number`：`base`进制的数。

```bash
>>> echo $((0xff))
255

>>> echo $((2#11111111))
255
```

-------------------------

位运算

- `<<`：位左移运算，把一个数字的所有位向左移动指定的位。
- `>>`：位右移运算，把一个数字的所有位向右移动指定的位。
- `&`：位的“与”运算，对两个数字的所有位执行一个`AND`操作。
- `|`：位的“或”运算，对两个数字的所有位执行一个`OR`操作。
- `~`：位的“否”运算，对一个数字的所有位取反。
- `^`：位的异或运算（exclusive or），对两个数字的所有位执行一个异或操作。

例如：

```bash
>>> echo $((2<<1))
4

>>> echo $((2>>1))
1

>>> echo $((5&2))
0

>>> echo $((5|2))
7

>>> echo $((~0))
-1

>>> echo $((2^1))
3
```

---------------------------

逻辑运算：逻辑表达式为真，返回`1`，否则返回`0`。

- `<`：小于
- `>`：大于
- `<=`：小于或相等
- `>=`：大于或相等
- `==`：相等
- `!=`：不相等
- `&&`：逻辑与
- `||`：逻辑或
- `!`：逻辑否
- `expr1?expr2:expr3`：三元条件运算符。若表达式`expr1`的计算结果为非零值（算术真），则执行表达式`expr2`，否则执行表达式`expr3`。

例如：

```bash
>>> echo $((2 > 1))
1

>>> echo $(((3 <= 2) && (1 != 2)))
0

>>> echo $((1<2 ? 1 : 0))
1
```

---------------------

赋值运算

- `parameter = value`：简单赋值。
- `parameter += value`：等价于`parameter = parameter + value`。
- `parameter -= value`：等价于`parameter = parameter – value`。
- `parameter *= value`：等价于`parameter = parameter * value`。
- `parameter /= value`：等价于`parameter = parameter / value`。
- `parameter %= value`：等价于`parameter = parameter % value`。
- `parameter <<= value`：等价于`parameter = parameter << value`。
- `parameter >>= value`：等价于`parameter = parameter >> value`。
- `parameter &= value`：等价于`parameter = parameter & value`。
- `parameter |= value`：等价于`parameter = parameter | value`。
- `parameter ^= value`：等价于`parameter = parameter ^ value`。

例如：

```bash
>>> a=1; echo $((a+=1))
2
```

表达式内部复制需要放在圆括号中，否则报错。

```bash
>>> a=1; echo $((a<2 ? (a+=1) : (a-=1)))
2
```

---------------

求值运算：`,`执行前后两个表达式，并返回后一个表达式的值。

```bash
>>> echo $((a = 1 + 1, 2 + 2))
4

>>> echo $a
2
```

-----------------

`expr`命令：支持整数算数运算和变量替换，不支持非整数运算。

```bash
>>> a=1; expr $a + 1
2
```

-----------------------

`let`命令：用于将算数运算结果赋值给变量，表达式中不能含有空格。

```bash
>>> let a=1+1; echo $a
2
```
