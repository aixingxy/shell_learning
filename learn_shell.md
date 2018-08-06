# 变量的显示：echo
```
xxy@xxy:~$ echo $PATH
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin
xxy@xxy:~$ echo ${PATH}
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin
```
# 变量的设置规则
1. 变量与变量内容以一个等号“=”来连接。
2. 等号两边不能直接接空格符号。
3. 变量名称只能是英文字母与数字，但是开头字符不能是数字。
4. 变量内容若有空格符可使用双引号“"”或单引号“'”将变量的内容结合起来，但是
+ 双引号内的特殊字符如$等，可以保佑原本的特性，例如：
```
xxy@xxy:~$ var="lang is $LANG"
xxy@xxy:~$ echo $var
lang is zh_CN.UTF-8
```
+ 单引号内的特殊字符则仅为一般字符（纯文本），例如:
```
xxy@xxy:~$ var="lang is $LANG"
xxy@xxy:~$ echo $var
lang is zh_CN.UTF-8
```
5. 可用转义字符”\“将特殊符号变成一般字符
6. 在一串命令中，还需要通过其他命令提供的信息，可以使用反单引号”`命令`“或”$(命令)“，例如：
```
xxy@xxy:~$ version=$(uname -r)
xxy@xxy:~$ echo $version
4.4.0-130-generic
xxy@xxy:~$ version=`uname -r`
xxy@xxy:~$ echo $version
4.4.0-130-generic
```
7. 若该变量为了增加变量内容时，则可用"$变量名称"或${变量}累加到内容，例如：
```
PATH="$PATH:/home/bin"
```
8. 若该变量需要在其他子进程执行，则需要以export来使用变量变成环境变量
```
export PATH
```
9. 通常大写字符位系统默认变量，自行设置变量可以使用小写变量，方便判断。
10. 取消变量的方法为使用“unset变量名称”，例如取消version的设置
```
xxy@xxy:~$ version=$(uname -r)
xxy@xxy:~$ echo $version
4.4.0-130-generic
xxy@xxy:~$ version=`uname -r`
xxy@xxy:~$ echo $version
4.4.0-130-generic
xxy@xxy:~$ unset version
xxy@xxy:~$ echo $version

```
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

#### 布尔运算符
- `!`: 非运算符，表达式为ture则返回false，否则返回true
- `-o`: 或运算符，有一个表达式为true，则返回ture
- `-a`: 与运算符，有两个表达式都为ture才返回true


#### 逻辑运算符
- `&&`: 逻辑的AND
- `||`: 逻辑的OR

#### 字符串运算符
- `=`: 检测两个字符串是否相等，相等返回true
- `!=`: 检测两个字符串是否不相等，不相等返回true
- `-z`: 检测字符串长度是否为0，为0返回true [ -z $a ] 返回 false
- `n`: 检测字符串长度是否为0，不为0返回true [ -n $a ] 返回 true
- `str`: 检测字符串是否为空，不为空返回ture [ $a ] 返回 true


#### 输出重定向
重定向一般是通过在命令之间插入特定的符号来实现
``` bash
 command1 > file1
```
先执行command1，然后将输出的内容存入到file1，file1中的内容将被新的内容替换。如果要将新的内容追加到文件末尾，使用>>操作符。

#### 输入重定向
将本来需要从键盘获取输入的命令会转移到从文件获取输入
统计users文件的行数
wc -l < users

##### 重定向深入讲解
一般情况下，每个Unix/Linux命令运行时会打开三个文件：
 标准输入文件（stdin）：stdin的文件描述符为0
 标准输出文件（stdout）：stdout的文件描述符为1
 标准错误文件（stderr）：stderr的文件描述符为2
默认情况下，`command > file` 将stdout重定向到file，`command < file`将stdin重定向到file

将stderr重定向到file，可以用`command 2 > file`
将stdout和stderr合并后重定向到file，`command > file 2>&1` 或者 `command >> file 2>&1`

##### /dev/null文件
如果希望执行某个命令，但又不希望在屏幕上显示输出结果，那么可以将输出重定向到/dev/null:
``` bash
command > /dev/null
```
/dev/null是一个特殊的文件，写入到它的内容都会被丢弃；如果尝试从该文件读取内容，那什么也读取不到。但是/dev/null文件非常有用，将命令的输出重定向到它，会起到“禁止输出”的效果。
如果希望屏蔽stdout和stderr，可以这样写
``` bash
command > /dev/null 2>&1
```
