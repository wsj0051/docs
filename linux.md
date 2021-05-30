# linux
## cat命令
### 参数说明
- -n 或 --number：由 1 开始对所有输出的行数编号。
- -b 或 --number-nonblank：和 -n 相似，只不过对于空白行不编号。
- -s 或 --squeeze-blank：当遇到有连续两行以上的空白行，就代换为一行的空白行。
- -v 或 --show-nonprinting：使用 ^ 和 M- 符号，除了 LFD 和 TAB 之外。
- -E 或 --show-ends : 在每行结束处显示 $。
- -T 或 --show-tabs: 将 TAB 字符显示为 ^I。
- -A, --show-all：等价于 -vET。
- -e：等价于"-vE"选项；
- -t：等价于"-vT"选项；

cat命令是linux下的一个文本输出命令，通常是用于观看某个文件的内容的；  
### cat主要有三大功能
1. 一次显示整个文件。
```
$ cat   filename
```
2. 从键盘创建一个文件。只能创建新文件,不能编辑已有文件. 
```
$ cat  >  filename
```
`cat > test.sh << EOF`以EOF作为输入结束，之前存在的内容会被覆盖掉。  
`cat << EOF >> test.sh` 将内容追加到 test.sh 的后面，不会覆盖掉原有的内容  
EOF只是标识，不是固定的
```
cat << HHH > iii.txt
```
输入内容：
```
> sdlkfjksl
> sdkjflk
> asdlfj
> HHH
```
这里的“HHH”就代替了“EOF”的功能。结果是相同的。
引用 `cat iii.txt`可以看到结果
sdlkfjksl  
sdkjflk  
asdlfj  

3. 将几个文件合并为一个文件。
将file1 file合并到file
```
$ cat   file1   file2  > file
```
把 textfile1 的文档内容加上行号后输入 textfile2 这个文档里：
```
cat -n textfile1 > textfile2
```
把 textfile1 和 textfile2 的文档内容加上行号（空白行不加）之后将内容附加到 textfile3 文档里：
```
cat -b textfile1 textfile2 >> textfile3
```

### 实例

清空 /etc/test.txt 文档内容：
```
cat /dev/null > /etc/test.txt
```
cat 也可以用来制作镜像文件。例如要制作软盘的镜像文件，将软盘放好后输入：
```
cat /dev/fd0 > OUTFILE
```
相反的，如果想把 image file 写到软盘，输入：
```
cat IMG_FILE > /dev/fd0
```
注：
1. OUTFILE 指输出的镜像文件名。
2. IMG_FILE 指镜像文件。
3. 若从镜像文件写回 device 时，device 容量需与相当。
4. 通常用制作开机磁片。

## dpkg命令
dpkg命令的英文全称是“Debian package”，dpkg是Debian的一个底层包管理工具，主要用于对已下载到本地和已安装的软件包进行管理。

dpkg这个机制最早由Debian Linux社区所开发出来的，通过dpkg的机制，Debian提供的软件就能够简单的安装起来，同时能提供安装后的软件信息，实在非常不错。只要派生于Debian的其它Linux distributions大多使用dpkg这个机制来管理，包括B2D，Ubuntu等。
### dpkg软件包相关文件介绍

`/etc/dpkg/dpkg.cfg` dpkg包管理软件的配置文件【Configuration file with default options】

`/var/log/dpkg.log`  dpkg包管理软件的日志文件【Default log file (see /etc/dpkg/dpkg.cfg(5) and option --log)】

`/var/lib/dpkg/available`  存放系统所有安装过的软件包信息【List of available packages.】

`/var/lib/dpkg/status`   存放系统现在所有安装软件的状态信息

`/var/lib/dpkg/info`   记安装软件包控制目录的控制信息文件


| 参数| 说明|
| :-------- | :--------| 
|-i	 |安装软件包 |
|-r |	删除软件包 |
|-l |	显示已安装软件包列表 |
|-L |	显示于软件包关联的文件 |
|-c |	显示软件包内文件列表 |
### 参考实例

安装包：
```
dpkg -i package.deb 
```
删除包：
```
dpkg -r package.deb  
```
`dpkg -r package-name`  # --remove， 移除软件包，但保留其配置文件  
`dpkg -P package-name ` # --purge， 清除软件包的所有文件（removes everything, including conffiles）

列出当前已安装的包：
```
dpkg -l 
```
每条记录对应一个软件包，注意每条记录的第一、二、三个字符，这就是软件包的状态标识，后边依此是软件包名称、版本号和简单描述。

1. 第一字符为期望值(Desired=Unknown/Install/Remove/Purge/Hold)，它包括：  
  u  Unknown状态未知,这意味着软件包未安装,并且用户也未发出安装请求.  
  i  Install用户请求安装软件包.  
  r  Remove用户请求卸载软件包.  
  p  Purge用户请求清除软件包.  
  h  Hold用户请求保持软件包版本锁定.  

2. 第二列,是软件包的当前状态(Status=Not/Inst/Conf-files/Unpacked/halF-conf/Half-inst/trig-aWait/Trig-pend)  
  n  Not软件包未安装.  
  i  Inst软件包安装并完成配置.  
  c  Conf-files软件包以前安装过,现在删除了,但是它的配置文件还留在系统中.  
  u  Unpacked软件包被解包,但还未配置.  
  f  halF-conf试图配置软件包,但是失败了.  
  h  Half-inst软件包安装,但是但是没有成功.  
  w trig-aWait触发器等待  
  t Trig-pend触发器未决  

3. 第三列标识错误状态,第一种状态标识没有问题,为空. 其它符号则标识相应问题（Err?=(none)/Reinst-required (Status,Err: uppercase=bad)）  
  h  软件包被强制保持,因为有其它软件包依赖需求,无法升级.  
  r  Reinst-required，软件包被破坏,可能需要重新安装才能正常使用(包括删除).  
  x  软包件被破坏,并且被强制保持.  

4. 案例说明：
  ii —— 表示系统正常安装了该软件  
  pn —— 表示安装了该软件，后来又清除了  
  un —— 表示从未安装过该软件  
  iu —— 表示安装了该软件，但是未配置  
  rc —— 该软件已被删除，但配置文件仍在  
5. dpkg子命令
为了方便用户使用，dpkg不仅提供了大量的参数选项, 同时也提供了许多子命令。比如：  
`dpkg-deb、dpkg-divert、dpkg-query、dpkg-split、dpkg-statoverride、start-stop-daemon`

列出deb包的内容：
```
[root@linuxcool ~]# dpkg -c package.deb 
```
配置：
```
[root@linuxcool ~]# dpkg --configure package 
```
查询
```
dpkg -l package-name-pattern  # --list, 查看系统中软件包名符合pattern模式的软件包
dpkg -L package-name  # --listfiles, 查看package-name对应的软件包安装的文件及目录
dpkg -p package-name  # --print-avail, 显示包的具体信息
dpkg -s package-name  # --status, 查看package-name（已安装）对应的软件包信息
dpkg -S filename-search-pattern  # --search, 从已经安装的软件包中查找包含filename的软件包名称
```
更多dpkg的使用方法可在命令行里使用`man dpkg`来查阅 或直接使用`dpkg --help`。
## sed命令
sed是一种流编辑器，它是文本处理中非常好的工具，能够完美的配合正则表达式使用，功能不同凡响。
处理时，把当前处理的行存储在临时缓冲区中，称为“模式空间”（pattern space），接着用sed命令处
理缓冲区中的内容，处理完成后，把缓冲区的内容送往屏幕。接着处理下一行，这样不断重复，直到文
件末尾。文件内容并没有改变，除非你使用重定向存储输出。Sed主要用来自动编辑一个或多个文件，
可以将数据行进行替换、删除、新增、选取等特定工作，简化对文件的反复操作，编写转换程序等。
### 命令格式
sed的命令格式：sed [options] 'command' file(s);  
sed的脚本格式：sed [options] -f scriptfile file(s);
### 选项
 -e ：直接在命令行模式上进行sed动作编辑，此为默认选项;

 -f ：将sed的动作写在一个文件内，用–f filename 执行filename内的sed动作;

 -i ：直接修改文件内容;

 -n ：只打印模式匹配的行；

 -r ：支持扩展表达式;

 -h或--help：显示帮助；

 -V或--version：显示版本信息。
### sed常用命令

 a\ 在当前行下面插入文本;

 i\ 在当前行上面插入文本;

 c\ 把选定的行改为新的文本;

 d 删除，删除选择的行;

 D 删除模板块的第一行;

 s 替换指定字符;

 h 拷贝模板块的内容到内存中的缓冲区;

 H 追加模板块的内容到内存中的缓冲区;

 g 获得内存缓冲区的内容，并替代当前模板块中的文本;

 G 获得内存缓冲区的内容，并追加到当前模板块文本的后面;

 l 列表不能打印字符的清单;

 n 读取下一个输入行，用下一个命令处理新的行而不是用第一个命令;

 N 追加下一个输入行到模板块后面并在二者间嵌入一个新行，改变当前行号码;

 p 打印模板块的行。 P(大写) 打印模板块的第一行;

 q 退出Sed;

 b lable 分支到脚本中带有标记的地方，如果分支不存在则分支到脚本的末尾;

 r file 从file中读行;

 t label if分支，从最后一行开始，条件一旦满足或者T，t命令，将导致分支到带有标号的命令处，或者到脚本的末尾;

 T label 错误分支，从最后一行开始，一旦发生错误或者T，t命令，将导致分支到带有标号的命令处，或者到脚本的末尾;

 w file 写并追加模板块到file末尾;

 W file 写并追加模板块的第一行到file末尾;

 ! 表示后面的命令对所有没有被选定的行发生作用;

 = 打印当前行号;

### sed替换标记

 g 表示行内全面替换;

 p 表示打印行;

 w 表示把行写入一个文件;

 x 表示互换模板块中的文本和缓冲区中的文本;

 y 表示把一个字符翻译为另外的字符（但是不用于正则表达式）;
 
 \1 子串匹配标记;

 & 已匹配字符串标记;
### 实例
替换文本中的字符串：

```
 sed 's/book/books/' file
```
`-n`选项和`p`命令一起使用表示只打印那些发生替换的行：

```
sed -n 's/test/TEST/p' file
```
直接编辑文件选项-i，会匹配file文件中每一行的第一个book替换为books
```
 sed -i 's/book/books/g' file
```
### 全面替换标记g

使用后缀 /g 标记会替换每一行中的所有匹配：
```
 sed 's/book/books/g' file
```
当需要从第N处匹配开始替换时，可以使用 /Ng：
```
 echo sksksksksksk | sed 's/sk/SK/2g' 
 skSKSKSKSKSK
 echo sksksksksksk | sed 's/sk/SK/3g'
 skskSKSKSKSK  
 echo sksksksksksk | sed 's/sk/SK/4g'
 skskskSKSKSK 
```
### 定界符

以上命令中字符 / 在sed中作为定界符使用，也可以使用任意的定界符
```
 sed 's:test:TEXT:g' 
 sed 's|test|TEXT|g' 
```
定界符出现在样式内部时，需要进行转义：
```
 sed 's/\/bin/\/usr\/local\/bin/g'
```
### 删除操作：d命令

删除空白行：
```
sed '/^$/d' file
```
删除文件的第2行：
```
 sed '2d' file
``` 
删除文件的第2行到末尾所有行：
```
 sed '2,$d' file
``` 
删除文件最后一行：
```
 sed '$d' file
``` 
删除文件中所有开头是test的行：
```
 sed '/^test/'d file
``` 
已匹配字符串标记&
正则表达式 \w\+ 匹配每一个单词，使用 [&] 替换它，& 对应于之前所匹配到的单词：
```
echo this is a test line | sed 's/\w\+/[&]/g'
[this] [is] [a] [test] [line]
```
所有以192.168.0.1开头的行都会被替换成它自已加localhost：
```
sed 's/^192.168.0.1/&localhost/' file
192.168.0.1localhost
```
### 子串匹配标记\1
匹配给定样式的其中一部分：
```
echo this is digit 7 in a number | sed 's/digit \([0-9]\)/\1/'
this is 7 in a number
```
命令中 digit 7，被替换成了 7。样式匹配到的子串是 7，\(..\) 用于匹配子串，对于匹配到的第一个子串就标记为 \1，依此类推匹配到的第二个结果就是 \2，例如：
```
echo aaa BBB | sed 's/\([a-z]\+\) \([A-Z]\+\)/\2 \1/'
BBB aaa
```
love被标记为1，所有loveable会被替换成lovers，并打印出来：
```
sed -n 's/\(love\)able/\1rs/p' file
```
### 选定行的范围：,（逗号）
所有在模板test和check所确定的范围内的行都被打印：
```
sed -n '/test/,/check/p' file
```
打印从第5行开始到第一个包含以test开始的行之间的所有行：
```
sed -n '5,/^test/p' file
```
对于模板test和west之间的行，每行的末尾用字符串aaa bbb替换：
```
sed '/test/,/west/s/$/aaa bbb/' file
```
### 多点编辑：e命令
`-e`选项允许在同一行里执行多条命令：
```
sed -e '1,5d' -e 's/test/check/' file
```
上面sed表达式的第一条命令删除1至5行，第二条命令用check替换test。命令的执行顺序对结果有影响。如果两个命令都是替换命令，那么第一个替换命令将影响第二个替换命令的结果。

和 -e 等价的命令是 --expression：
```
sed --expression='s/test/check/' --expression='/love/d' file
```

[更多详情](https://man.linuxde.net/sed)
## useradd命令详解
[原文链接](https://www.runoob.com/linux/linux-comm-useradd.html)

### 语法
```
useradd [-mMnr][-c <备注>][-d <登入目录>][-e <有效期限>][-f <缓冲天数>][-g <群组>][-G <群组>][-s <shell>][-u <uid>][用户帐号]
```
或
```
useradd -D [-b][-e <有效期限>][-f <缓冲天数>][-g <群组>][-G <群组>][-s <shell>]
```
### 参数说明
- -c<备注> 　加上备注文字。备注文字会保存在passwd的备注栏位中。
- -d<登入目录> 　指定用户登入时的起始目录。
- -D 　变更预设值．
- -e<有效期限> 　指定帐号的有效期限。
- -f<缓冲天数> 　指定在密码过期后多少天即关闭该帐号。
- -g<群组> 　指定用户所属的群组。
- -G<群组> 　指定用户所属的附加群组。
- -m 　自动建立用户的登入目录。
- -M 　不要自动建立用户的登入目录。
- -n 　取消建立以用户名称为名的群组．
- -r 　建立系统帐号。
- -s<shell>　 　指定用户登入后所使用的shell。
- -u<uid> 　指定用户ID。

### 实例
添加一般用户
```
useradd tt
```
为添加的用户指定相应的用户组
```
useradd -g root tt
```
创建一个系统用户
```
useradd -r tt
```
为新添加的用户指定home目录
```
useradd -d /home/myd tt
```
建立用户且制定ID
```
useradd caojh -u 544
```
禁止用户wsj0051登录
```
 usermod -s /sbin/nologin wsj0051
```
恢复用户wsj0051登录
```
usermod -s /bin/bash wsj0051

```
其他

![笔记](https://gitee.com/wsj0051/pic/raw/master/img/usermod.png)
