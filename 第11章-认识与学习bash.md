# 1. 认识bash这个shell
管理整个计算机硬件的是操作系统的内核，内核是需要被保护的，一般用户只能通过shell和内核进行通信

## 1.1 硬件、内核与shell
内核管理和控制硬件工作，shell接受来自用户的指令，操作内核

## 1.2 为何要学命令行界面的shell
* 命令行界面的shell都是一样的，具有通用性，例如：ubuntu和mac的命令是一样的
* 远程管理：命令行比较快
* linux的任督二脉

## 1.3 系统的合法shell与/etc/shells功能
* shell有多个版本
* /etc/shells文件中都是可以使用的shell
 
        MacBook-Pro:~ zhanghuamao$ cat /etc/shells 
        # List of acceptable shells for chpass(1).
        # Ftpd will not allow users to connect who are not using
        # one of these shells.
        
        /bin/bash
        /bin/csh
        /bin/ksh
        /bin/sh
        /bin/tcsh
        /bin/zsh
    
* 查看默认bash

        MacBook-Pro:~ zhanghuamao$ echo $SHELL
        /bin/bash

## 1.4 bash shell的功能
* 命令记忆能力
    1. 记录在主文件夹內的.bash_history中，记录的是前一次登录以前所执行过的命令
    2. 查看默认记忆条数：
    
            MacBook-Pro:~ zhanghuamao$ echo $HISTSIZE
            500
    
* 命令与文件补全
* 命令行别名设置

        MacBook-Pro:java zhanghuamao$ alias la='ls -al'
        MacBook-Pro:java zhanghuamao$ la
        total 4693032
        drwxr-xr-x   7 zhanghuamao  staff         238  2 17 00:11 .
        drwxr-xr-x+ 51 zhanghuamao  staff        1734  2 17 00:11 ..
        -rw-r--r--@  1 zhanghuamao  staff        8196  2  6 15:53 .DS_Store
        drwxr-xr-x   4 zhanghuamao  staff         136  5 27  2018 code
        -rw-r--r--   1 zhanghuamao  staff           0  2 17 00:11 ls.txt
        MacBook-Pro:java zhanghuamao$ alias 
        alias la='ls -al'
* 作业控制、前台、后台控制
* 程序脚本（shell script）
    1. 将经常执行的连续命令写成脚本
* 通配符
    1. bash支持通配符查询与执行命令

## 1.5 bash shell的内置命令：type
* bash中会内置一些命令，例如：cd，通过man bash查看bash的帮助文档，可以发现里面有cd的说明

* 通过type命令，可以查看某条命令是否为bash内置命令

        MacBook-Pro:code zhanghuamao$ type ls
        ls is hashed (/bin/ls)
        MacBook-Pro:code zhanghuamao$ type cd
        cd is a shell builtin
        MacBook-Pro:code zhanghuamao$ type type
        type is a shell builtin
        MacBook-Pro:code zhanghuamao$ type la
        la is aliased to `ls -al'

## 1.6 命令行执行
* 命令太长时，可以通过"\[Enter]"将[Enter]进行转义，让[Enter]按键不再具有“开始执行”的功能

        MacBook-Pro:code zhanghuamao$ ls \
        > 


# 2. shell的变量功能
linux是多用户、多任务的环境，每个人登录系统都能获取一个bash，每个都能使用bash执行命令来收取自己的邮件，bash是如何知道当前用户使用的邮箱是啥？这就是变量的作用

## 2.1 什么是变量
以y = ax + b为例，等号左边的y是变量，等号右边的ax + b是变量的内容

* 变量的可变性与方便性

    使用MAIL这个变量保存邮箱地址，当用户A登录时，他得到的MAIL变量值为/var/spool/mail/usera，当用户B登录时，他得到的MAIL变量值为/var/spool/mail/userb。
    
    程序员在编写代码时，直接去MAIL内容就好了，不需要将/var/spool/mail/usera或者/var/spool/mail/userb写死到代码中
    
* 影响bash环境操作的变量

    影响bash环境操作的变量叫环境变量。例如，PATH变量中记录的是执行命令的路径，之所以可以执行
    anaconda这个命令，是因为在PATH路径配置了anaconda的路径
    
      MacBook-Pro:~ zhanghuamao$ echo $PATH
      /opt/local/bin:/opt/local/sbin:/Users/zhanghuamao/anaconda3/bin
          
      MacBook-Pro:~ zhanghuamao$ anaconda -V
      anaconda Command line client (version 1.6.5)

* 脚本程序设计的好帮手

    在写shell脚本时，有些数据在不同用户下会有不同的值，可以将这些值定义为变量。例如，配置myname，如果是用户a登录myname就是a，如果是用户b登录，myname就是b

## 2.2 变量的显示与设置

* 变量的显示：echo $变量名 或者 echo ${变量名}
    
      MacBook-Pro:~ zhanghuamao$ echo $PATH
      /opt/local/bin:/opt/local/sbin:/Users/zhanghuamao/anaconda3/bin

      MacBook-Pro:~ zhanghuamao$ echo ${PATH}
      /opt/local/bin:/opt/local/sbin:/Users/zhanghuamao/anaconda3/bin

    没有设置的变量默认值为空，通过等号(=)可以进行复制
    
      MacBook-Pro:~ zhanghuamao$ echo $myname
      MacBook-Pro:~ zhanghuamao$ myname=usera
      MacBook-Pro:~ zhanghuamao$ echo $myname
      usera
      
* 变量设置原则
    * 变量和变量内容使用等号连结
    * 等号两边不能使用空格
    
          MacBook-Pro:~ zhanghuamao$ myname = userb
          -bash: myname: command not found

    * 变量名称只能是英文字母与数字，但是开头字符不能是数字
    * 