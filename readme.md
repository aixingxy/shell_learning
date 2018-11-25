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
  删除变量后，echo将不再有输出

## shell字符串
### 单引号
单引号字符串的显示：
- 单引号里的任何字符都会原样输出，单引号中的变量的是无效的
- 单引号字符串中不能再出现单引号（对单引号使用转义字符也不行）

### 双引号
双引号的优点：
- 双引号里可以有变量
- 双引号里尅可以出现转义字符

### 字符串拼接
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
# 变量赋值格式
变量赋值等号两边不能有空格

$name 是${name}的简化版,在某些情况下必须使用花括号的方式来消除歧义并避免意外结果
```shell
xxy@xxy:~$ VAR="1234"
xxy@xxy:~$ echo $VAR
1234
xxy@xxy:~$ echo $VAR1234  # 将会查找VAR1234这个变量

xxy@xxy:~$ echo ${VAR}1234
12341234
```
# data命令
date命令是显示或设置系统时间与日期
-s<字符串>：根据字符串来设置日期与时间。字符串前后必须加上双引号
<+时间日期格式>：指定显示时候，使用指定的日期时间格式
```shell
xxy@xxy:~$ date +"%Y-%m-%d %H:%M:%S"
2018-11-25 22:28:48
```

# 数学运算expr命令
1. 用空格隔开每个项
2. 用/(反斜杠)放在shell特定的字符前面
3. 对包含空格和其他特殊的字符串要用引号括起来

```shell
# 计算字符串长度
xxy@xxy:~$ expr length "this is a test"
14
# 抓取子串
xxy@xxy:~$ expr substr "this is a test" 3 5
is is
# 抓取第一个字符数字串出现的位置
xxy@xxy:~$ expr index "sarsarsar" a
2
# 整数运算
xxy@xxy:~$ expr 14 % 9
5
xxy@xxy:~$ expr 10 + 10
20
xxy@xxy:~$ expr 10+10  #　注意空格
10+10
```

# sed流编辑器
sed编辑器是一行一行处理文件内容的。正在处理的内容存放在模式空间（缓冲区）内，处理完成以后按照选项的规定进行输出或文件的修改。
sef是一种在线编辑器，它一次处理一行内容。处理时，把当前处理的行存储在临时缓冲区中，称为“模式空间”（pattern sapce），接着用sed命令处理缓冲区中的内容，处理完成后，把缓冲区的内容送往屏幕。
接着处理下一行，这样不断重复，知道文件末尾。文件内容并没有改变，除非你使用重定向存储输出。sed主要用来自动编辑一个或多个文件；简化对文件的反复操作。
sed也支持正则表达式，如果要使用扩展正则表达式加参数-r
sed的执行过程：
1.一次读取一行数据
2.根据提供的规则来匹配相关的数据，比如查找root
3.按照命令修改数据流中的数据，比如替换
4.将结果进行输出
5.重复上面四步

如何使用
语法格式 sed [options] '[commands]' filename

```shell
xxy@xxy:~$ echo "this is apple" | sed 's/apple/dog/'
this is dog
xxy@xxy:~$ echo "this is apple" | sed 's/apple/dog/'
this is dog
xxy@xxy:~$ echo "this is apple" >> a.txt
xxy@xxy:~$ sed 's/apple/dog/' a.txt
this is dog
xxy@xxy:~$ cat a.txt
this is apple
```

sed选项|参数

|options|描述|
|:-:|:-:|
|-a |在当前行插入文件|
|-n |读取下一个输出行，用下一个命令处理新的行而不是用第一个命令|
|-e |执行多个sed指令|
|-f |运行脚本|
|-i |编辑文件内容|
|-i.bak |编辑的同时创建.bak的备份|
|-r |使用扩展的正则表达式|

|commands|描述|
|:-:|:-:|
|i |在当前行上面插入文件|
|c |把选定的行改为新的额指定的文件|
|p |打印|
|d |删除|
|r/R |读取文件/一行|
|W |另存|
|s |查找|
|y |替换|
|h |拷贝模板块的内容到内存中的缓冲区|
|H |追加模板块的内容到缓冲区|
|g |获得内存缓冲区的内容，并替代当前模板中的文本|
|G |获得内存缓冲区的内容，并追加到当前模板块文本后面|
|D |删除\n之前的内容|

|替换标记|描述|
|:-:|:-:|
|数字|表明新文本将替换第几处模式匹配的地方
|g|表示新文本将会替换所有匹配的文本
|\1|子穿匹配标记，前面搜索可以用元字符集\(//\)
|&|保留搜索到的字符串用来替换其他字符

|sed匹配字符集|描述|
|:-:|:-:|
|^|匹配行开始，如：/^sed/匹配所有以sed开头的行
|$|匹配行结束，如：/sed$匹配所有以sed结尾的行
|.|匹配一个非换行符的任意字符，如：/s.d/匹配s后接一个任意字符，最后是d
|*|匹配0个或多个字符，如：/*sed/匹配所有模板是一个或多个空格后紧跟sed的行



例1：只替换第一个匹配到的字符，将passwd中root用户替换成xuegod
```shell
xxy@xxy:~$ sed 's/root/xuegod/' /etc/passwd
例2：全面替换标记g
xxy@xxy:~$ sed 's/root/xuegod/g' /etc/passwd
例3：将sed中默认的/定界符改成#号
xxy@xxy:~$ sed 's#/bin/bash#/sbin/nologin#' /etc/passwd
如果非得用，加转义符号
xxy@xxy:~$ sed 's/\/bin\/bash/\/sbin\/nologin/' /etc/passwd
```
（2）按行查找替换
写法如下：
用数字表示范围；$表示行尾
用文本模式配置来过滤
例1：单行替换，将第2行中第一个bin替换成xuegod
```shell
xxy@xxy:~$ sed '2s/bin/xuegod/' /etc/passwd
xxy@xxy:~$ sed '2s/bin/xuegod/g' /etc/passwd  # 将第二行中所有bin都替换成xuegod
xxy@xxy:~$ sed '2,$s/bin/xuegod/g' /etc/passwd  # 将第二行到最后一行的所有bin都替换成xuegod
```
（3）d 删除第二行到第4行
```shell
xxy@xxy:~$ cat a.txt
this is apple
line1
line2
line3
line4
line5
xxy@xxy:~$ sed '2,4d' a.txt
this is apple
line4
line5
```

（4）添加行
命令i(insert)，在当前行前面插入一行 i\
命令a(append)，在当前行后面添加一行 a\
```shell
xxy@xxy:~$ sed '1i\append' a.txt  # 在第一行前插入
append
this is apple
line1
line2
line3
line4
line5
xxy@xxy:~$ sed '1a\append' a.txt  # 在第1行后面追加
this is apple
append
line1
line2
line3
line4
line5
xxy@xxy:~$ sed '2,4a\append' a.txt # 2,3,4行后面追加append
this is apple
line1
append
line2
append
line3
append
line4
line5
xxy@xxy:~$ sed 'a\append' a.txt  # 每一行都追加
this is apple
append
line1
append
line2
append
line3
append
line4
append
line5
append
xxy@xxy:~$ sed '$a\append' a.txt  # 在最后一行追加
this is apple
line1
line2
line3
line4
line5
append
```
（5）修改行命令c(change) c\
修改第4行内容
```shell
xxy@xxy:~$ sed '1c\line0' a.txt
line0
line1
line2
line3
line4
line5
xxy@xxy:~$ sed '1,3c\line0' a.txt
line0
line3
line4
line5
```
将line的内容都改成LINE
```shell
xxy@xxy:~$ sed '/line0/c\LINE0' a.txt
this is apple
line1
line2
line3
line4
line5
```
（6）打印，直接输入文件中的内容
```shell
xxy@xxy:~$ sed -n '1p' a.txt
this is apple
```

（7）将修改过或过滤出来的内容保存到另一个文件中
```shell
xxy@xxy:~$ sed -n '/root/w c.txt' /etc/passwd
xxy@xxy:~$ cat c.txt
root:x:0:0:root:/root:/bin/bash
```

（8）-i对源文件修改
```shell
xxy@xxy:~$ sed -i 's/line/LINE/' a.txt
xxy@xxy:~$ cat a.txt
this is apple
LINE1
LINE2
LINE3
LINE4
LINE5
```
（9）查找字符串
```shell
xxy@xxy:~$ cat a.txt
per_cpu:0
speakid:maohui 9101-0,9102-0

speakid:sangzhujuan 9201-1,9202-1
xxy@xxy:~$ cat c.sh
echo "重启端口号：$1"
confile=a.txt
port=$1
gpuid=`grep $port $confile | sed -r "s#(speakid:)(.*) (.*)($port)-([0-9])(.*)#\5#"`
echo "gpu: $gpuid"

speakid=`grep $port $confile | sed -r "s#(speakid:)(.*) (.*)($port)-([0-9])(.*)#\2#"`
echo "speakid: $speakid"
xxy@xxy:~$ sh c.sh 9101
重启端口号：9101
gpu: 0
speakid: maohui

```
