# chapter 5 Linux的文件权限与目录配置

## 5.2 Linux 文件权限

查看文件

`ls -al`

文件类型权限

|-|rwx|rwx|rwx|
|---|---|---|---|
|1|234|567|890|
|为目录或文件|拥有者权限|同用户组的用户权限|其他用户权限|


修改文件权限
```
chgrp //修改文件所属用户组
chown //修改文件拥有者,包括子目录 -R
chmod //修改文件权限，SUID,SGID,SBIT等属性
```

* `chmod`

    `chmod [-R] xyz path`
     
     xyz : number
     r:4 w:2 x:1 -:0
    
    `chmod [u/g/o/a][+/-/=][r/w/x] path`
    
    u:user g: group o:other +:add -:remove =:set 
    
    eg: `chmod a+r path`
    
## 5.3 FHS

||shareable|unshareable|
|-|-|-|
|static|/usr(软件存放)|/etc(配置文件)|
||/opt(第三方辅助软件)|/boot(启动与内核文件)|
|variable|/var/mail|/var/run(程序相关)|
||/var/spool/news|/var/lock(程序相关)|

* /
* /usr
* /var

重要目录
* /etc 
* /bin
* /dev
* /lib
* /sbin

相对路径与绝对路径
* `.` 当前目录，或 `./`
* `..` 上一层目录， 或 `../`

# chapter 6 Linux 文件与目录管理

## 6.1

### 6.1.2 目录的相关操作
```
. //当前目录
.. //此层目录
- //上一层目录
~ //前一个工作目录
~account //account的家目录
```

command 
* cd
* pwd [-P] 

  -p : 显示出真正路径
* mkdir [-mp]
  
  -m : 设置文件权限

  -p : 创建递归 
* rmdir [-p]
  
  -p : 连通上层空目录删除

## 6.2

### 6.2.1 ls
```
-a 显示所有文件及目录 (. 开头的隐藏文件也会列出)
-A 同 -a ，但不列出 "." (目前目录) 及 ".." (父目录)
-l 除文件名称外，亦将文件型态、权限、拥有者、文件大小等资讯详细列出
-r 将文件以相反次序显示(原定依英文字母次序)
-t 将文件依建立时间之先后次序列出
-h 将文件容量以易读的方式列出
-F 在列出的文件名称后加一符号；例如可执行档则加 "*", 目录则加 "/"
-R 若目录下有文件，则以下之文件亦皆依序列出
```

### 6.2.2 cp rm mv

* cp
```
-a：此选项通常在复制目录时使用，它保留链接、文件属性，并复制目录下的所有内容。其作用等于dpR参数组合。
-d：复制时保留链接。这里所说的链接相当于 Windows 系统中的快捷方式。
-f：覆盖已经存在的目标文件而不给出提示。
-i：与 -f 选项相反，在覆盖目标文件之前给出提示，要求用户确认是否覆盖，回答 y 时目标文件将被覆盖。
-p：除复制文件的内容外，还把修改时间和访问权限也复制到新文件中。
-r：若给出的源文件是一个目录文件，此时将复制该目录下所有的子目录和文件。
-l：不复制文件，只是生成链接文件。
```

* rm
```
-i 删除前逐一询问确认。
-f 即使原档案属性设为唯读，亦直接删除，无需逐一确认。
-r 将目录及以下之档案亦逐一删除。
```

* mv
```
-b: 当目标文件或目录存在时，在执行覆盖前，会为其创建一个备份。
-i: 如果指定移动的源目录或文件与目标的目录或文件同名，则会先询问是否覆盖旧文件，输入 y 表示直接覆盖，输入 n 表示取消该操作。
-f: 如果指定移动的源目录或文件与目标的目录或文件同名，不会询问，直接覆盖旧文件。
-n: 不要覆盖任何已存在的文件或目录。
-u：当源文件比目标文件新或者目标文件不存在时，才执行移动操作。
```

## 6.3 文件内容查看
* cat 由第一行开始显示文件内容
* tac 由最后一行开始显示文件内容
* nl 同时输出行号
* more 一页一页地显示文件内容
* less 可以往前翻页
* head 只看前几行
* tail 只看后几行
* od 以二进制的方式读取文件内容

### 6.3.5 修改文件时间或创建新文件

`touch [-acdmt] file`

-a : 仅自定义access time

-c ：仅修改文件的时间，若该文件不存在则不建立新的文件

-d ：后面可以接欲自定义的日期而不是当前日期

-m ：仅修改mtime

-t ：后可接自定义日期而不是当前时间 [YYYYMMDDhhmm]


## 6.4 文件与目录的默认权限和隐藏权限

### 6.4.1 文件默认权限 umask

unmask 目前用户在建立文件或目录时候的权限默认值

unmask xyz 为减掉的权限

### 6.4.2 文件隐藏属性
* chattr 配置文件隐藏属性（只支持在传统linux文件系统上完整生效 ext2,3,4
* lsattr 显示文件隐藏属性

### 6.4.3 文件特殊权限 SUID SGID SBIT

- Set UID
  - SUID权限仅对二进制程序有效
  - 执行者对于该程序需要具有 x 的可执行权限
  - 本权限仅在执行程序的过程中有效
  - 执行者将具有该程序拥有者的权限
- Set GID
  - SGID对二进制程序有用
  - 程序执行者对于该程序来说，需要具备 x 的权限
  - 执行者在执行的过程中将会获得该程序用户组的支持
- Sticky Bit
  - 当用户对于此目录具有 w，x 权限，即具有写入的权限
  - 当用户在该目录下建立目录或文件时，仅有自己与 root 才有权力删除该文件

- SUID/SGID/SBIT 权限设置
  - SUID 4
  - SGID 2
  - SBIT 1

### 6.4.4 观察文件类型 file

## 6.5 命令与文件的查找

### 6.5.1 脚本文件的查找

`which [-a] command`

### 6.5.2 文件的查找
- whereis(由一些特定目录中查找文件)
- locate/ updatedb
- find

# chapter 7 Linux 磁盘与文件系统管理

# chapter 8 文件与文件系统的压缩

## 8.2 Linux 系统常见的压缩命令

### 8.2.1 gzip,zcat/zmore/zless/zgrep
`gzip [-cdtv#] filename`
`zcat filename.gz`

### 8.2.2 bzip2,bzcat/bzmore/bzless/bzgrep
`bzip2 [-cdkzv#] filename`
`bzcat filename.bz2`

### 8.2.3 xz,xzcat/xzmore/xzless/xzgrep
`xz [-dtlkc#] filename`
`xcat filename.xz`

## 8.3 打包命令 tar
- 压缩 `tar -Jcv -f filename.tar.xz`
- 查询 `tar -Jtv -f filename.tar.xz`
- 解压缩 `tar -Jxv -f filename.tar.xz`
- -z gzip
- -j bzip2
- -J xz

## 8.4 XFS文件系统的备份与还原

### 8.4.1 xfsdump

## 8.6 其他压缩与备份工具

### 8.6.1 dd

`dd if = "input_file" of = "output_file" bs = "block_size" count="number"`

### 8.6.2 cpio

备份 `find / | cpio -ocvB > /dev/st0`

还原 `cpio -idvc < /dev/st0

# chapter 9 vim程序编辑器

## 9.1 vi and vim

## 9.2 vi

- command mode
  - 可移动光标，删除字符，删除整行，复制粘贴
- insert mode
  - 按下 i,l,o,O,a,A,r,a,R 等
- command-line-mode
  - :/?

### 9.2.1
### 9.2.2 按键说明
|快捷键|说明|
|---|---|
|h,j,k,l|左下上右|
|Ctrl+f/b|向下移动，向上移动|
|0 [Home]|移动到这一行最前面字符处|
|$/[End]|移动到这一行最后面字符|
|G|移动到文件最后一行|
|gg|移动到第一行|
|x X|del backspace|
|dd|删除(剪切)光标所在的一整行|
|yy|赋值光标所在的那一行|
|u|恢复前一个操作|
|ctrl+r|重做上一个操作|
|.|重复前一个操作|
|i,l|从目前光标处插入，在目前所在行的第一个非空格符处插入|
|a,A|从目前光标所在处的下一个字符|
|o,O|在目前光标所在处下一行插入新一行，在目前光标处上一行插入新一行|


### 9.3.1 visual block

|||
|---|---|
|v|字符选择|
|V|行选择|
|ctrl+v|可视区块，矩阵方式选择|
|y|将反白地方复制|
|d|将反白地方删除|
|q|粘贴|

### 9.3.2 多文件编辑
|||
|---|---|
|:n|编辑下一个文件|
|:N|编辑上一个文件|
|:files|列出目前这个vim开启的所有文件|

### 9.3.3 多窗口功能

|||
|---|---|
|:sp [filename]||