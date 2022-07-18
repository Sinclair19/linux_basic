# chapter 5

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