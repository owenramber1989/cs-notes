# 字符串相关操作
***
#在文件中查找字符串
`grep printf path/*.c`
	如果只想看到出现的次数，可以加 -c 参数
	如果不想看次数，只想看到文件名，可以加 -l
`rm -i $(grep -l 'This file is obsolete' *)`
注意\*不会深入子目录, 只会匹配当前目录
#只关心搜索是否成功
`if grep -q findme foo.bar ; then echo yes ; else echo nope ; fi`
-q 指代 quiet , 如果在其后列出多个文件名，那么找到第一处匹配后grep就下班了
如果成功的话，第一处匹配的文件名储存在 `$?`这个shell 变量里面
	如果想要移植性更好或者搜索所有文件，可以
	`if grep findme foo.bar > /dev/null ; then echo yes ; else echo nope ; fi`
	/dev/null 是 位桶 ， 重定向到此处的输出将被丢弃
-i 忽略大小写
#在管道中进行搜索
	记得要将命令的输出重定向到标准输出
	`gcc foo.c 2>&1 | grep -i error`
***
grep 也可以嵌套使用
```shell
grep -i taylor mail/*
!! | grep -i swift
!! | grep -i "the female artist"
```
注意历史运算符 !! 的使用，它可以重复上一条命令
#缩写搜索结果 
`grep -i dec logfile | grep -vi decimal | grep -vi | decimate`
但是搜索12月的日志文件最好用下面的正则表达式
`grep Dec\ [0-9 ][0-9] logfile`
***
# 基本命令
#查找命令
	忘记了命令名的时候可以用 `apropos` 或 `man -k`
#查找文件
`ls -d .v*/` 是一个例子，-d表示筛选，文件名模式以斜线结尾可以只显示匹配模式的目录，而不会显示文件名
#输出
	'' 单引号可以让shell不处理字符串，双引号则还是会进行变量扩展、算术扩展、波浪号扩展和命令替换
#格式控制
```shell
printf '%-10.10s = %4.2f\n' 'Gigahertz' 1.92735
```
对于字符串s，第一个数字是字段的最大宽度，第二个数字是要输出的字符数量
#重定向
`>&`              `&>`              `2>&1` 都是把标准错误重定向到与标准输出一样的地方
#追加
使用 `>>`
#花括号的用法
1. 可以用来将重定向应用于多个命令的输出
	` { pwd; ls; cd ../somewhere; pwd; ls; } > /tmp/all.out`
	当然用圆括号也可以，用花括号则要注意花括号两端要有空格，圆括号是在子shell中运行命令，花括号更像一种便捷写法
	上例如果用圆括号，wd不会改变，但若用花括号则会改变
2. 简化if-else分支结构
```shell
[ $result = 1 ] \
  && { echo "Result is 1." ; exit 0 ; } \
  || { echo "Result isn't 1." ; exit 120; } 
```
#T形接头
将输出作为输入，同时保留其副本
```shell
find / -name '*.c' -print 2>&1 | tee /tmp/all.my.sources
```
这样，输出保留在了指定的文件里，同时也会打印到屏幕上
#STDERR与STDOUT
	标准输出是缓冲式的，标准错误则不是。对于标准输出，只有缓冲区满了或者文件被关闭，才会写入输出，标准错误则是每个字符都单独写入，因次可以立刻看到错误信息
#标准输入
<< 允许我们创建一个临时输入源，然后指定一个终止符，用`<<-`  就可以在每行的开头用tab缩进here-document
```shell
grep $1 <<-'EOF'
	lots of data
	EOF
```
注意EOF后面不能有空白字符
#获取用户输入
1. read
```shell
read 
read -p "what's your name" ANSWER
read -t 3 -p "Tell me your name in 3 sec." ANSWER
read PRE MID POST
```
-p 可以先输出提示，-t设置时间限制
获取多个变量，输入少了后面的设为null，多了的则全部留给最后一个变量
2. 避免屏幕回显
	`read -s -p "password: " PASSWD ; printf "%b" "\n"`
***
#  执行命令
`$PATH` 包含了一个目录列表，bash 使用该变量定位可执行文件， 目录以冒号分隔
注意当前目录，也就是点号目录一定要放在最后面，最好不放
***因为bash是按照目录中列出的目录顺序依次查找命令行上指定的可执行文件，点号目录放在前面就容易被同名命令的恶意版本欺骗***
```shell
bash
cd
touch ls
chmod 755 ls
PATH=".:$PATH"
ls
```
这样的话ls就失效了，但因为第一行命令bash是一个副本，exit该shell就好了，不过删掉该ls亦可
```shell
cd
rm ls
exit
```
因此，***绝对不要把点号目录或可写目录放进root的$PATH变量里***
## 多个命令
```shell
a ; b ; c # 依次执行abc三个命令
a && b && c # 前一个命令成功了才执行下一个命令
a & b & c # 同时执行三个命令，a b 转后台
```

