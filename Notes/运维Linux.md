## Linux目录结构

![image-20250429193117819](C:\Users\Tismin\Documents\GitHub\MyNotes\Pictures\image-20250429193117819.png)

- 相对路径
- 绝对路径

### etc重要配置文件

- /etc/sysconfig/netword-scripts/ifcfg-ens33 用于配置网络接口ens33的参数
- /etc/resolv.conf 用于配置域名解析的相关信息，例如指定DNS服务器地址
- /etc/hostname 用于设置系统的主机名，标识计算机的名称
- /etc/hosts
- /etc/motd 设置登录后的欢迎文字
- /etc/os-release 用于记录Linux发行版本信息
- /etc/passwd 用户user的配置文件，记录用户的各种信息
- /etc/group 组的配置文件，记录组信息
- /etc/shadow 口令的配置文件，类似记录用户的密码



## Linux基本概念

### 用户和用户组

- /etc/passwd

  用户的配置文件，记录用户的各种信息

  ~~~
  用户名:口令:用户标识号:组标识号:注释性描述:主目录:登录shell
  ~~~

- /etc/shadow

  口令的配置文件

  ~~~
  登录名:加密口令:最后一次修改时间:最小时间间隔:最大时间间隔:警告时间:不活动时间:失效时间:标志
  ~~~

- /etc/group

  组的配置文件，记录Linux包含的组信息

  ~~~
  组名:口令:组标识号:组内用户列表
  ~~~

- /etc/login.defs

- /etc/skel

  此目录存放新用户所需要的基础环境变量文件，添加新用户的时候，这个目录下所有文件自动被复制到新家目录下，且默认是隐藏文件，以点开头

  ~~~
  root@Ubuntu-test1:~# ls -al /etc/skel/
  total 20
  drwxr-xr-x   2 root root 4096 Apr 25 12:01 .
  drwxr-xr-x 114 root root 4096 May  8 13:14 ..
  -rw-r--r--   1 root root  220 Feb 25  2020 .bash_logout
  -rw-r--r--   1 root root 3771 Feb 25  2020 .bashrc
  -rw-r--r--   1 root root  807 Feb 25  2020 .profile
  ~~~

  

### 运行级别

| 运行级别 | 说明                             |
| -------- | -------------------------------- |
| 0        | 关机                             |
| 1        | 单用户，可用来找回丢失密码       |
| 2        | 多用户状态没有网络服务           |
| 3        | 多用户状态有网络服务，**最常用** |
| 4        | 系统未使用，保留给用户           |
| 5        | 图形界面                         |
| 6        | 系统重启                         |

常用的运行级别是**3**和**5**

CentOS中 systemctl get-default检查默认运行级别；systemctl set-default [.target]修改默认运行级别

### 通配符

#### · 星号 *

~~~js
#用于匹配任何字符，包括数字、字母和符号

#例如
		"test*"		#以test开头的文件或目录
		"a*.c"		#以a开头.c结尾的文件或目录
		"*.txt"		#以.txt结尾的文件或目录
~~~

#### · 问号 ?

~~~js
#用于匹配单个字符

#例如
		"a?c*x?"	#匹配所有以“a”为第一个字母、第三个字母为“c”以及倒数第二个字母是小写字母“x”的文件
        ls ?????	#列出所有文件名为5个字符的文件
~~~

#### · 方括号 []

~~~js
#用于匹配指定字符集范围中的一个字符，如果需要匹配一小段字符集范围，可以使用该通配符

#例如
		ls [ab]*	#列出以a或b开头的文件
		ls [0-9]*	#列出以数字开头的文件
~~~

#### · 花括号 {}

~~~js
#提供一种在Linux中生成文件名的方法。若文件名中有几个不同的选项，就可以使用此通配符

#例如
		ls file{Hebei,Wuhan}.txt		#展开为 fileHebei.txt、fileWuhan.txt并列出这些文件（如果存在）
		touch file{a..z}.txt	#生成从 filea.txt 到 filez.txt 的多个文件。
		echo {1..10}			#输出 1 2 3 4 5 6 7 8 9 10
~~~

#### · 反斜杠 \

~~~js
#用于转义特殊字符，使其失去特殊含义

#例如
		ls \*.txt		#列出当前目录下以 *.txt 为名称的文件，而不是匹配所有以 .txt 结尾的文件。
~~~

#### · 否定 ^ or !

~~~js
#匹配不在指定范围或集合中的字符

#例如
		ls [^JFM]*		#列出当前目录下所有不以 J、F 或 M 开头的文件。
		ls [!a-z]*		#列出当前目录下所有不以小写字母开头的文件。
~~~

### 文件属性

~~~js
TIsmin@Ubuntu-test1:/home/server$ ls -alhSi
531215 -rw-rw-r--  1 TIsmin TIsmin 210M May  2 12:22 'SD for server 1.20.1 v1.0.zip'
535658 -rw-r--r--  1 root   root    46M Jun 22  2024  server.jar
535435 -rw-r--r--  1 root   root   450K Jun  1  2024  fabric-installer-1.0.1.exe
531214 drwxrwxrwx 11 root   root   4.0K May  2 12:38  .

#inode号|文件类型和权限|硬链接数|属主|属组|文件大小|修改时间|文件名
	
~~~



## Linux命令

### 别名Alias

~~~python
#Synopsis
		alias name=‘command’	#为command设置[name]的别名
		unalias name			#删除[name]的别名

#永久设置别名
		echo "alias ll='ls -alF'" >> ~/.bashrc
		source ~/.bashrc
~~~

### ==vi和vim==

- 一般模式

| 用途         | 快捷键                           |
| ------------ | -------------------------------- |
| 拷贝         | [num] + yy + p                   |
| 删除         | [num] + dd + p                   |
| 查找字符     | /[string] (n表示查找下一个)      |
| 显示行号     | :set nu                          |
| 隐藏行号     | :set nonu                        |
| 定位到最末行 | G                                |
| 定位到最首行 | gg                               |
| 定位到某一行 | [num] + shift+g或者:[num] +enter |
| 撤销动作     | u                                |

- 编辑模式

| 命令 | 用途                 |
| ---- | -------------------- |
| i    | 编辑                 |
| I    | 从首行开始编辑       |
| o    | 在后添加新的一行开始 |
| O    | 在前添加新的一行开始 |

- 命令行模式 write quit **!表示强制操作**

| 命令 | 用途       |
| :--- | :--------- |
| :wq  | 保存并退出 |
| :q   | 退出       |
| :q!  | 强制退出   |

### 关机、重启、用户注销登录

| 命令            | 用途                 |
| --------------- | -------------------- |
| shutdown -h now | 立即关机             |
| shutdown -h 1   | 一分钟之后关机       |
| shutdown -r now | 现在立即重启计算机   |
| halt            | 关机                 |
| reboot          | 重新启动计算机       |
| sync            | 将内存数据同步到磁盘 |

- ==**sync将内存数据保存后再关机，小心驶得万年船**==

| 命令      | 用途           |
| --------- | -------------- |
| su - root | 登录到root用户 |
| logout    | 登出当前账户   |

###  帮助指令

| 命令                 | 用途                        |
| -------------------- | --------------------------- |
| man [命令或配置文件] | 获取查看帮助信息            |
| help [命令]          | 获取shell内置命令的帮助信息 |
|                      |                             |

### 命令行常用快捷键

| 快捷键   | 功能                 |
| -------- | -------------------- |
| Ctrl + c | cancel取消当前操作   |
| Ctrl + l | 清空屏幕内容         |
| Ctrl + d | 退出当前用户         |
| Ctrl + a | 光标移动到行首       |
| Ctrl + e | 光标移动到行尾       |
| Ctrl + u | 删除光标到行首的内容 |

**echo $PATH**

### 文件目录指令

#### · pwd

  ~~~python
  #Name
  		Print name of current/working directory
  
  #Synopsis
  		pwd [OPTION]...
  
  #Description
  		-P			avoid all symlinks
  ~~~

#### · ls

~~~python
  #Name
  		List directory contents
  #Synopsis
  		ls [OPTION]...[FILE]
  		
  #Description
  		-a, --all	do not ignore entries starting with .
  		-l     		use a long listing format
  		-h			with -l and -s, print sizes like 1K 234M 2G etc
        --full-time	like -l --time-style=full-iso
        -F			append indicator (one of */=>@|) to entries
          			#以/结尾的为文件夹
          			#以*结尾的为可执行文件
                  	#以@结尾的为软链接，类似快捷方式
                      #普通文件类型结尾是空的
  		-d			list directories themselves, not their contents
        -r			reverse order while sorting
        -S			sort by file size, largest first
        -i			print the index number of each file 
~~~

#### · cd

~~~python
#Name
		Change the shell workign directory
    
#几个特殊的目录
		.	当前工作目录
		..	上一级的工作目录
		-	上一次的工作目录
		~	当前系统登录的用户家目录
        
#Synopsis
		cd [OPTION] [DIR]
    
#Description
		
~~~

#### · mkdir

~~~python
#Name
		Make directories
    
#Synopsis
		mkdir [OPTION]... {DIR,DIR,...}
    
#Description
    	-p, --parents
              no error if existing, make parent directories as needed
        
#Bash
		mkdir [DIR]{num1..num2} 批量创建dir[num1]~dir[num2]的文件夹
~~~

#### · touch

~~~python
#Name
		Change file timestamps or create a new file

#Synopsis
		touch [OPTION]... FILE...
		
#Description
		-t			use [[CC]YY]MMDDhhmm[.ss] instead of current time
		-c			do not create any files
		-r,--reference=FILE			
					use this file's times instead of current time
#Bash
		touch {num1..num2}
		touch {string1..string2}
		
~~~

#### · cp

~~~python
#Name
		Copy files and directories

#Synopsis
		cp [OPTION]... [-T] SOURCE DEST
        cp [OPTION]... SOURCE... DIRECTORY
        cp [OPTION]... -t DIRECTORY SOURCE...
        
#Description
		-r		copy directories recursively
		-d		same as --no-dereference --preserve=links
        -p		same as --preserve=mode,ownership,timestamps
        -i		
        
~~~

~~~python
#Examples
		1.复制普通文件
    	cp 你想复制的文件 复制后的文件名
        
        2.复制普通文件，且改名，放入到另一个文件夹中
        cp xxx.txt ./xxxx/  #复制放入其他文件夹，保留源文件名
        cp xxx.txt ./xxxx/yyy.txt #复制放入其他文件夹，且修改文件名
        
        3.一次性复制多个文件，放入另一个文件夹中
        cp xxx.txt xxx.gif ./xxx/
        
        4.复制整个文件夹，加上-r参数
        cp -r xxx xxx2 #递归参数复制xxx及其子目录和文件，复制为xxx2
        
        5.拷贝软链接时，保持连接属性不变
        cp -d link_luffy link_luffy3
        
        6.-i参数的用法，覆盖文件提示
        cp -i 文件1 文件2  #如果文件2已经存在，则会覆盖，-i会让用户进行输入y确认覆盖
~~~

#### · rm

~~~python
#Name
		remove files or directories
		
#Synopsis
		rm [OPTION]... [FILE]...
		
#Description
		-f, --force
				gnore nonexistent files and arguments, never prompt
       	-i		prompt before every removal
       	-r, -R, --recursive
              	remove directories and their contents recursively
        -d, --dir
              remove empty directories
        -v, --verbose
              explain what is being done

#Example
		1.删除普通文件
		rm xxx.txt
		
		2.一次性删除多个文件
		rm xxx1.txt xxx2.txt	#删除多个文件，写入多个文件名，用空格分割
		
		3.删除文件夹，添加-r参数，默认rm只能删除文件
		rm -r test				#删除test文件夹及其内部的文件
		
		4.-d参数只能用于删除空文件夹
		rm -d test
		
		5，强制删除文件且不提示
		rm -f xxx*				#强制删除以xxx开头的文件，文件夹无法删除
		
		6.强制删除所有文件和文件夹
		rm -rf ./*				#注意文件目录，命令有很大的危险性
        
        7.-v显示删除的详细内容和过程
~~~

#### · mv

~~~python
#Name
		Move (rename) files
		
#Synopsis
		mv [OPTION]... [-T] SOURCE DEST
		mv [OPTION]... SOURCE... DIRECTORY
  		mv [OPTION]... -t DIRECTORY SOURCE...
  		
#Description
		-f, --force
              do not prompt before overwriting
        -i, --interactive
              prompt before overwrite

#Example
		1.移动文件到另一个文件夹
		mv ./mjj.jj ./oldboy	#把当前的mjj.jj文件移动到oldboy文件夹当中去
		
		2.移动多个文件，放到另一个文件夹中
		mv luffy* ./oldboy		#将当前目录所有以luffy开头的文件，都移动到oldboy文件夹中去
		
		3.重命名的用法
		mv 旧的文件名 新的文件名
		
		4.-i参数的用法，覆盖前询问
        
        5.-f强制性覆盖，不询问
~~~

### 文件管理

#### 重定向符号

| 符号    | 功能           |
| ------- | -------------- |
|         |                |
| >       | 输出覆盖重定向 |
| >>      | 输出追加重定向 |
| < or << | 标准输入重定向 |

~~~javascript
#Example
		1.读取文件内容，并写入到另一个文件中
		cat douyin.txt > kuaishou.txt
		
		2.追加写入文件内容
		cat douyin.txt >> kuaishou.txt
		
		3.重定向写入符
		cat < douyin.txt	#把文件中的数据，发送给cat读取
		
		4.
		cat >> gushi.txt <<EOF
		
~~~

#### · cat

~~~python
#Name
		Concatenate files and print on the standard output

#Synopsis
		cat [OPTION]... [FILE]...
		
#Description
		-A, --show-all
              equivalent to -vET
       -b, --number-nonblank
              number nonempty output lines, overrides -n
       -e     equivalent to -vE
       -E, --show-ends
              display $ at end of each line
       -n, --number
              number all output lines
       -s, --squeeze-blank
              suppress repeated empty output lines
       -t     equivalent to -vT
       -T, --show-tabs
              display TAB characters as ^I
       -u     (ignored)
       -v, --show-nonprinting
              use ^ and M- notation, except for LFD and TAB
 
#Example
		1.查看文本内容，以及参数功能
    	cat xxx.txt
        
        2.对非空行显示行号
        cat -b xxx.txt
        
        3.对所有行显示行号
        cat -n xxx.txt
        
        4.显示的每行末尾加上$符
        cat -n -E xxx.txt
        
        5.减少显示的空行数量至一个
        cat -s xxx.txt
        
        6.合并多个文件内容写入到新的文件中
        cat xxx1.txt xxx2.txt ./add12.txt
        
        7.非交互式写入文件内容信息
        cat >> add12.txt <<EOF
        
        8.清空文件，留下一个空行
        echo > xxx.txt
        
        9.直接清空文件内容
        > xxx.txt
        
        10.利用cat读取一个黑洞文件再重定向写入
        cat /dev/null > xxx.txt
~~~

#### · more

~~~
more 文件名 #分屏显示文件内容

按下enter是下一行
空格是向下滚动一个屏幕大小
=显示当前行号
按下q退出more
~~~

#### · less

~~~~

~~~~

#### · head

~~~~
head -5 文件名
head 文件名	#head默认显示前10行

-c 参数，指定字符数量，显示字符数
head -c 5 文件名	#输出这个文件头5个字符
~~~~

#### · tail

~~~
tail -5 文件 -5 文件名
tail 文件名	#tail默认显示前10行

-c 参数，指定字符数量，显示字符数
tail -c 5 文件名	#输出这个文件头5个字符

-f 实时刷新文件内容的变化
tail -f xxx.txt
~~~

#### · cut

~~~~python
#Name
		Remove sections from each line of files
		
#Synopsis
		cut OPTION... [FILE]...
		
#Description
		-b	以字节为单位切割
		-n	取消分割多字节字符，与-b一起使用
		-c	以字符为单位
		-d	自定义分隔符，默认以tab为分隔符
		-f	与-d一起使用，指定显示哪个区域
		N	第N个字节，字符或字段
		N-	从第N个字节，字符或字段到行尾
		N-M	从第N个到第M个字节，字符或字段
		-M	从第1个到第M个字节，字符或字段
		
#Example
		1.截取每一行的第4个字符
		cut -c 4 alex.txt
		
		2.截取4-6个字符
		cut -c 4-6 alex.txt
		
		3.截取第5和第7的字符
		cut -c 5,7 alex.txt
		
		4.截取一个范围的字符
		cut -c 4- alex.txt
		
		5.指定分隔符号
		cut -d ":" -f 区域范围 alex.txt
			#区域范围也可以是-N;N-;N-M等范围
          
        6.和管道符的配合使用
        cut -d ":" -f -4 alex.txt | head -5
        	#先按cut进行过滤再将过滤后的内容输出前五行
~~~~

#### · sort

~~~python
#Name
		Sort lines of text files
		
#Synopsis
		sort [OPTION]... [FILE]...
       	sort [OPTION]... --files0-from=F
       	
#Description
		-b, --ignore-leading-blanks
              ignore leading blanks
        -n, --numeric-sort
              compare according to string numerical value
        -r, --reverse
              reverse the result of comparisons
        -k, --key=KEYDEF
              sort via a key; KEYDEF gives location and type
        -t, --field-separator=SEP
              use SEP instead of non-blank to blank transition
        u, --unique
              with -c, check for strict ordering; without -c, output only the first of an equal run
              
#Example
		1.对文件的第一个字符进行排序，默认从小到大
		sort —n flie.txt
		
		2.倒序排序输出
		sort -n -r flie.txt
		
		3.对排序结果去重
		sort -u flie.txt
		
		4.指定分割符号，指定区域进行排序
		sort -n -t "." -k 4 ip.txt
~~~

#### · uniq

~~~python
#Name
		Report or omit repeated lines
		
#Synopsis
		uniq [OPTION]... [INPUT [OUTPUT]]
		
#Description
		-c, --count
              prefix lines by the number of occurrences
        -d, --repeated
              only print duplicate lines, one for each group
        -u, --unique
              only print unique lines
            
#Example
		1.去除连续的重复行
		uniq file.txt	#仅去除连续重复行，间隔重复行不会去除
		
		2.结合sort使用去重更精准
		sort -n file.txt | uniq
		
		3.统计重复次数
		sort -n file.txt | uniq -c
		
		4.只找出重复行且统计出现次数
		sort -n file.txt | uniq -c -d
		
		5.找出只出现过一次的行
		sort -n file.txt | uniq -c -u
~~~

#### · wc

~~~python
#Name
		用于计算字数
#Synopsis
		wc [-clw][--help][--version][文件...]
		
#Description
		c --bytes --chars	只显示Bytes数。
		-l --lines 			显示行数。
		-w --words 			只显示字数。
		--help 				在线帮助。
		--version 			显示版本信息。
		
#Example
		1.显示信息
		wc testfile
		
		2.只显示字数
		wc -l testfile
		
		3.echo
		echo "Hello" | wc -l	#会显示6，因为完整内容是Hello$
		可用echo "Hello" | cat -E 查看
~~~

#### · tr

~~~python
#Name
		Translate or delete characters
		
#Synopsis
		tr [-cdst][--help][--version][第一字符集][第二字符集]  
		tr [OPTION]…SET1[SET2] 

#Description
		-c, --complement	
				反选设定字符。也就是符合 SET1 的部份不做处理，不符合的剩余部份才进行转换
		-d, --delete
				删除指令字符
		-s, --squeeze-repeats
				缩减连续重复的字符成指定的单个字符
		-t, --truncate-set1
				削减 SET1 指定范围，使之与 SET2 设定长度相等

#Example
        \NNN 八进制值的字符 NNN (1 to 3 为八进制值的字符)
        \\ 反斜杠
        \a Ctrl-G 铃声
        \b Ctrl-H 退格符
        \f Ctrl-L 走行换页
        \n Ctrl-J 新行
        \r Ctrl-M 回车
        \t Ctrl-I tab键
        \v Ctrl-X 水平制表符
        CHAR1-CHAR2 ：字符范围从 CHAR1 到 CHAR2 的指定，范围的指定以 ASCII 码的次序为基础，只能由小到大，不能由大到小。
        [CHAR*] ：这是 SET2 专用的设定，功能是重复指定的字符到与 SET1 相同长度为止
        [CHAR*REPEAT] ：这也是 SET2 专用的设定，功能是重复指定的字符到设定的 REPEAT 次数为止(REPEAT 的数字采 8 进位制计算，以 0 为开始)
        [:alnum:] ：所有字母字符与数字
        [:alpha:] ：所有字母字符
        [:blank:] ：所有水平空格
        [:cntrl:] ：所有控制字符
        [:digit:] ：所有数字
        [:graph:] ：所有可打印的字符(不包含空格符)
        [:lower:] ：所有小写字母
        [:print:] ：所有可打印的字符(包含空格符)
        [:punct:] ：所有标点字符[:space:] ：所有水平与垂直空格符
        [:upper:] ：所有大写字母
        [:xdigit:] ：所有 16 进位制的数字
        [=CHAR=] ：所有符合指定的字符(等号里的 CHAR，代表你可自订的字符)
~~~

#### · stat

~~~python
以文字的格式来显示 inode 的内容

stat [文件或目录]

# stat testfile                #输入命令
  File: `testfile'
  Size: 102             Blocks: 8          IO Block: 4096   regular file
Device: 807h/2055d      Inode: 1265161     Links: 1
Access: (0644/-rw-r--r--)  Uid: (    0/    root)   Gid: (    0/    root)
Access: 2014-08-13 14:07:20.000000000 +0800
Modify: 2014-08-13 14:07:07.000000000 +0800
Change: 2014-08-13 14:07:07.000000000 +0800
~~~

#### · find

~~~~python
#Name
		Search for files in a directory hierarchy
		
#Synopsis
		find [路径] [匹配条件] [动作]
		
#Description
        路径 是要查找的目录路径，可以是一个目录或文件名，也可以是多个路径，多个路径之间用空格分隔，如果未指定路径，则默认为当前目录。
        expression 是可选参数，用于指定查找的条件，可以是文件名、文件类型、文件大小等等。

        匹配条件 中可使用的选项有二三十个之多，以下列出最常用的部份：

        -name pattern
        		按文件名查找，支持使用通配符 * 和 ?。
        -type type
        		按文件类型查找，可以是 f（普通文件）、d（目录）、l（符号链接）等。
        -size [+-]size[cwbkMG]
        		按文件大小查找，支持使用 + 或 - 表示大于或小于指定大小，单位可以是 c（字节）、w（字数）、b（块数）、						k（KB）、M（MB）或 G（GB）。
        -mtime days
        		按修改时间查找，支持使用 + 或 - 表示在指定天数前或后，days 是一个整数表示天数。
        -user username
        		按文件所有者查找。
        -group groupname
        		按文件所属组查找。
        动作: 可选的，用于对匹配到的文件执行操作，比如删除、复制等。

        find 命令中用于时间的参数如下：

        -amin n		查找在 n 分钟内被访问过的文件。
        -atime n	查找在 n*24 小时内被访问过的文件。
        -cmin n		查找在 n 分钟内状态发生变化的文件（例如权限）。
        -ctime n	查找在 n*24 小时内状态发生变化的文件（例如权限）。
        -mmin n		查找在 n 分钟内被修改过的文件。
        -mtime n	查找在 n*24 小时内被修改过的文件。
        	在这些参数中，n 可以是一个正数、负数或零。正数表示在指定的时间内修改或访问过的文件，负数表示在指定的时间之前修改或访				问过的文件，零表示在当前时间点上修改或访问过的文件。

        正数应该表示时间之前，负数表示时间之内。

        例如：-mtime 0 表示查找今天修改过的文件，-mtime -7 表示查找一周以前修改过的文件。

        关于时间 n 参数的说明：

        +n：查找比 n 天前更早的文件或目录。

        -n：查找在 n 天内更改过属性的文件或目录。

        n：查找在 n 天前（指定那一天）更改过属性的文件或目录。
        
        #Example
~~~~

#### · xargs

~~~
#Name
		build and execute command lines from standard input
		
#Synopsis
		xargs [options] [command [initial-arguments]]
~~~

#### · tar

~~~python
#Name
	An archiving utility
	
#Synopsis
	tar [选项] [参数]
	
#Description
	-A或--catenate
			#新增文件到以存在的备份文件
    -B		#设置区块大小
	-c或--create
			#建立新的备份文件
	-C<目录> #这个选项用在解压缩，若要在特定目录解压缩，可以使用这个选项
	-d		#记录文件的差别
	-x或--extract或--get
			#从备份文件中还原文件;
    -t或--list#列出备份文件的内容
	-z或--gzip或--ungzip
			#通过gzip指令处理备份文件
	-Z或--compress或--uncompress
			#通过compress指令处理备份文件
	-f<备份文件>或--file=<备份文件>
			#指定备份文件
	-v或--verbose
			#显示指令执行过程
	-r		#添加文件到已经压缩的文件
	-u		#添加改变了和现有的文件到已经存在的压缩文件
	-j		#支持bzip2解压文件
	-1		#文件系统边界设置
	-k		#保留原有文件不覆盖
	-m		#保留文件不被覆盖
    -t		#查看打包文件中的文件信息ls
    
#Example
	1.对tmp下所有内容打包，生成alltmp.tar
		tar -cvf alltmp.tar ./*
	
	2.拆开备份文件，拆开快递
		tar -xvf ../alltmp.tar ./
        
    3.对tmp下所有文件，进行打包且压缩，节省磁盘空间
    	tar -czvf alltmp2.tar.gz ./*
        
    4.对打包后的压缩包进行解压缩
    	tar -zxvf ../alltmp2.tar.gz ./*
        
    5.查看打包文件中的文件信息ls   
    	tar -ztvf alltmp2.tar.gz
        
    6.指定解压缩文件路径
    	tar -zxvf alltmp2.tar.gz -C ./alltmp/
        
    7.排除文件解压缩
    	tar -zxvf alltmp2.tar.gz --exclude alex.txt
        
    8.tar命令压缩快捷方式的源文件，而不是压缩快捷方式
    	tar -zchf kx2.tar.gz ./kx.txt
~~~

#### · gzip

~~~
#Name
	Compress or expand files
	#gzip无法压缩文件夹，必须先使用tar对文件夹打包后，才可以gzip压缩
	
#Synopsis
	-a或——ascii
			使⽤ASCII⽂字模式；
	-c或--stdout或--to-stdout 
			把解压后的⽂件输出到标准输出设备。
	-d或--decompress或----uncompress
			解开压缩⽂件；
	-f或——force
			强⾏压缩⽂件。不理会⽂件名称或硬连接是否存在以及该⽂件是否为符号连接
	-h或——help
			在线帮助；
	-l或——list
			列出压缩⽂件的相关信息；
	-L或——license
			显示版本与版权信息；
	-n或--no-name
			压缩⽂件时，不保存原来的⽂件名称及时间戳记；
	-N或——name
			压缩⽂件时，保存原来的⽂件名称及时间戳记；
	-q或——quiet
			不显示警告信息；
	-r或——recursive
			递归处理，将指定⽬录下的所有⽂件及⼦⽬录⼀并处理；
	-S或<压缩字尾字符串>或----suffix<压缩字尾字符串>
			更改压缩字尾字符串；
	-t或——test
			测试压缩⽂件是否正确⽆误；
	-v或——verbose
			显示指令执⾏过程；
	-V或——version
			显示版本信息；
	-<压缩效率>
			压缩效率是⼀个介于1~9的数值，预设值为“6”，指定愈⼤的数值，压缩效率就会愈高
	--best
			此参数的效果和指定“-9”参数相同；
	--fast
			此参数的效果和指定“-1”参数相同。


#Example
	1.对当前所有txt文件进行gzip压缩
		gzip ./*.txt
		
	2.
~~~

#### ·zip和unzip

~~~
	zip alltmp.zip ./*
	
	unzip alltmp.zip 
~~~

### 用户管理

| 命令                                | 用途                                                         |
| ----------------------------------- | ------------------------------------------------------------ |
| useradd [-d] [/home/...] [username] | 在指定路径下添加用户，若无参数则默认在/home                  |
| passwd [username]                   | 给某用户设置密码                                             |
| userdel  [username]                 | 删除某用户                                                   |
| userdel -r [username]               | 删除某用户及其家目录下所有目录和文件                         |
| id [username]                       | 查询用户id等信息                                             |
| su - [username]                     | 切换用户视图（不加-仍在原来用户工作目录下，加-工作目录会被切换） |
| whoami                              | 查看当前用户信息                                             |
| groupadd [gpname]                   | 添加组                                                       |
| groupdel [gpname]                   | 删除组                                                       |
| useradd -g [gpname] [username]      | 将指定的用户添加到指定的组当中                               |
| usermod -g [gpname] [username]      | 更改指定用户的组                                             |

