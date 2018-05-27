# shell 学习
## 编写第一个shell脚本
``` tex
#!/bin/bash
echo "Hello World!"
```

`#!`是一个约定的标记，告诉系统这个脚本要什么解释器来执行

``` bash
chmod +x ./test.sh
sh ./test.sh
```

注意：
一定要写成./test.sh，而不是test.sh，运行其他二进制程序也一样，直接写test.sh，linux系统会去PATH里寻找有没有叫test.sh的，而只有/bin，/sbin，/usr/bin，/usr/sbin等在PATH里，而我的当前目录不在PATH中，所以写成test.sh是会找不到命令的，要用./test.sh告诉系统，就在当前目录中找。

也可以用`/bin/sh test.sh`来执行

## shell变量
### 变量定义
定义变量的时候，变量名不加美元符号：
``` bash
your_name='xxy'
```
**变量名和等号之间不能有空格**
1. 变量名只能用英文字母，数字和下划线，收个字母不能以数字开头
2. 中间不能有空格，可以使用下划线_
3. 不能使用标点符号
4. 不能使用bash里的关键词
### 使用变量
```
echo ${your_name}
```
#### 只读变量
使用`readonly`命令可以将变量直接定义为只读变量，只读变量的值不能被改变
``` text
#!/bin/bash
myname="xxy"
readonly myname
myname="machine"
```
运行脚本结果如下：
```
./test.sh: line 4: myname: readonly variable
```
####  删除变量
使用unset命令可以删除变量
``` bash
unset myname
```
注意：
  变量删除后不能再次使用。unset不能删除只读变量。
  删除变量后，echo将不再有输出

## shell字符串
### 单引号
单引号字符串的显示：
- 单引号里的任何字符都会原样输出，单引号中的变量的是无效的
- 单引号字符串中不能再出现单引号（对单引号使用转义字符也不行）

### 双引号
双引号的优点：
- 双引号里可以有变量
- 双引号里尅可以出现转义字符

### 字符串拼接
``` bash
$ str1="abc"
$ str2="bcd"
$ str3="0"$str1
$ echo $str3
0abc
$ str4="0"$str1$str2
$ echo $str4
0abcbcd
```

### 获取字符串长度
``` bash
$ string="abc"
$ echo ${#string}
3
```

### 提取子串
``` bash
$ string="0123456789"
$ echo ${string:1:4}
1234
```
### 查找子串
``` bash
# 查找字符'i'或's'的位置
$ string="runoob is a great company"
$ echo `expr index "$string" is`
8
# mac 里面没有index https://discussions.apple.com/thread/923299
```

## shell数组
bash支持一维数组（不支持多维数组），并且没有设定数组的大小，起始下标为0
### 读取数组
``` bash
$ my_array=(A B "C" D)
$ echo "第一个元素为: ${my_array[0]}"
第一个元素为: A
```
### 获取数组所有元素
``` bash
$ echo ${my_array[*]}
A B C D
$ echo ${my_array[@]}
A B C D
```

### shell运算符
#### 算术运算符
``` bash
$ val=`expr 2 + 2`
$ echo $val
4
$ a=10
$ b=20
$ echo `expr $a + $b`
30
$ echo `expr $a - $b`
-10
$ echo `expr $a / $b`
0
$ echo `expr $a \* $b`
200
$ echo `expr $a % $b`
10
$ echo `expr $a == $b`
0
$ echo `expr $a != $b`
1
```
注意：
- 表达式和运算符之间要有空格，例如2+2是不对的，必须写成2 + 2
- 完整的表达式要被``包含
- 乘号(*)前边必须加反斜杠(\)才能实现乘法运算

#### 关系运算符
编写test.sh
``` text
#!/bin/bash
# author:xxy
a=10
b=20
if [ $a -eq $b ]
then
    echo "$a -eq $b : a 等于 b"
else
    echo "$a -eq $b : a 不等于 b"
fi

if [ $a -ne $b ]
then
    echo "$a -ne $b : a 不等于 b"
else
    echo "$a -ne $b : a 等于 b"
fi

if [ $a -gt $b ]
then
    echo "$a -gt $b : a 大于 b"
else
    echo "$a -gt $b : a 不大于 b"
fi

if [ $a -lt $b ]
then
    echo "$a -lt $b : a 小于 b"
else
    echo "$a -lt $b : a 不小于 b"
fi

if [ $a -ge $b ]
then
    echo "$a -ge $b : a 大于等于 b"
else
    echo "$a -ge $b : a 小于 b"
fi

if [ $a -le $b ]
then
    echo "$a -le $b : a 小于等于 b"
else
    echo "$a -le $b : a 大于 b"
fi
```
输出：
10 -eq 20: a 不等于 b
10 -ne 20: a 不等于 b
10 -gt 20: a 不大于 b
10 -lt 20: a 小于 b
10 -ge 20: a 小于 b
10 -le 20: a 小于或等于 b

总结：
- `-eq`: 判断两个数是否相等
- `-ne`: 判断两个数是否不相等
- `-gt`: 判断左边的数是否大于右边的数
- `-lt`: 判断左边的数是否小于右边的数
- `-ge`: 判断左边的数是否大于等于右边的数
- `-le`: 判断左边的数是否小于等于右边的数

在if语句中，判断条件[ $a -ne $b ]的空格是严格的
