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
