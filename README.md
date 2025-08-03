# 应急响应 #
# windows 入侵检测 #
# 检查系统账号安全 #
我用的是win10专业版，至于密钥，懂的都懂，但是大家支持正版！！！

检查可疑账号

打开 cmd 窗口，输入 `lusrmgr.msc` 命令，查看是否有新增/可疑的账号，如有管理员群组的（Administrators）里的新增账户，如有，请立即禁用或删除掉。 

<img width="712" height="545" alt="image" src="https://github.com/user-attachments/assets/86df6005-7c2c-42a6-96d1-da54d49914d8" />

也可以查看日志

Win+R 打开运行，输入"eventvwr.msc"，回车运行，打开“事件查看器”。

<img width="712" height="347" alt="image" src="https://github.com/user-attachments/assets/65d11a2d-a4b1-48ef-8b94-1de5a6df44e9" />

# 检查异常端口和进程 #

使用`netstat -ano` 命令查看目前的网络连接，定位可疑的 ESTABLISHED

<img width="715" height="380" alt="image" src="https://github.com/user-attachments/assets/e9a6b746-230c-4774-a6eb-a16f55e172ac" />


# 检查启动项 #

单击【开始】>【运行】，输入 regedit，打开注册表，查看开机启动项是否正常，特别注意如下三个注册表项：
```
HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\run
HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Run
HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Runonce
```

<img width="715" height="488" alt="image" src="https://github.com/user-attachments/assets/35e39473-5bf0-4098-9238-cf46f9edb743" />


<img width="713" height="509" alt="image" src="https://github.com/user-attachments/assets/af6b62d8-0298-4a34-8e58-0c7270bf0110" />


<img width="716" height="512" alt="image" src="https://github.com/user-attachments/assets/cf92476a-9e62-41c5-9796-4159aa4a7cc7" />

检查右侧是否有启动异常的项目，如有请删除，并建议安装杀毒软件进行病毒查杀，清除残留病毒或木马。

# windows日志分析 #

# 系统日志 #

记录操作系统组件产生的事件，主要包括驱动程序、系统组件和应用软件的崩溃以及数据丢失错误等。系统日志中记录的时间类型由Windows NT/2000操作系统预先定义。

默认位置： %SystemRoot%\System32\Winevt\Logs\System.evtx


<img width="715" height="348" alt="image" src="https://github.com/user-attachments/assets/8f00e1d3-8597-4910-8b1c-409f843a5d41" />


# 应用程序日志 #

包含由应用程序或系统程序记录的事件，主要记录程序运行方面的事件，例如数据库程序可以在应用程序日志中记录文件错误，程序开发人员可以自行决定监视哪些事件。如果某个应用程序出现崩溃情况，那么我们可以从程序事件日志中找到相应的记录，也许会有助于你解决问题。 

默认位置：%SystemRoot%\System32\Winevt\Logs\Application.evtx

<img width="709" height="341" alt="image" src="https://github.com/user-attachments/assets/4d28ea11-3a23-4412-85f9-81db9f8ffdaf" />


# 安全日志 #

记录系统的安全审计事件，包含各种类型的登录日志、对象访问日志、进程追踪日志、特权使用、帐号管理、策略变更、系统事件。安全日志也是调查取证中最常用到的日志。默认设置下，安全性日志是关闭的，管理员可以使用组策略来启动安全性日志，或者在注册表中设置审核策略，以便当安全性日志满后使系统停止响应。

默认位置：%SystemRoot%\System32\Winevt\Logs\Security.evtx


<img width="717" height="381" alt="image" src="https://github.com/user-attachments/assets/bb216181-531b-42b5-89e4-a77a73dc442d" />


事件分析日志
| 事件ID | 说明                             
| :----- | --------------------------------
| 4624   | 登录成功                         
| 4625   | 登录失败                         
| 4634   | 注销成功                         
| 4647   | 用户启动的注销                   
| 4672   | 使用超级用户（如管理员）进行登录  
| 4720   | 创建用户                         


每个成功登录的事件都会标记一个登录类型，不同登录类型代表不同的方式：

| 登录类型 | 描述                            | 说明                                             
| :------- | ------------------------------- | ------------------------------------------------ 
| 2        | 交互式登录（Interactive）       | 用户在本地进行登录。                             
| 3        | 网络（Network）                 | 最常见的情况就是连接到共享文件夹或共享打印机时。 
| 4        | 批处理（Batch）                 | 通常表明某计划任务启动。                         
| 5        | 服务（Service）                 | 每种服务都被配置在某个特定的用户账号下运行。     
| 7        | 解锁（Unlock）                  | 屏保解锁。                                       
| 8        | 网络明文（NetworkCleartext）    | 登录的密码在网络上是通过明文传输的，如FTP。      
| 9        | 新凭证（NewCredentials）        | 使用带/Netonly参数的RUNAS命令运行一个程序。      
| 10       | 远程交互，（RemoteInteractive） | 通过终端服务、远程桌面或远程协助访问计算机。     
| 11       | 缓存交互（CachedInteractive）   | 以一个域用户登录而又没有域控制器可用             




# linux的入侵排查 #

# 账号安全 #

```
1、用户信息文件 /etc/passwd
root:x:0:0:root:/root:/bin/bash
account:password:UID:GID:GECOS:directory:shell
用户名：密码：用户ID：组ID：用户说明：家目录：登陆之后的 shell
注意：无密码只允许本机登陆，远程不允许登陆

2、影子文件 /etc/shadow
root:$6$oGs1PqhL2p3ZetrE$X7o7bzoouHQVSEmSgsYN5UD4.kMHx6qgbTqwNVC5oOAouXvcjQSt.Ft7ql1WpkopY0UV9ajBwUt1DpYxTCVvI/:16809:0:99999:7:::
用户名：加密密码：密码最后一次修改日期：两次密码的修改时间间隔：密码有效期：密码修改到期到的警告天数：密码过期之后的宽限天数：账号失效时间：保留

who     查看当前登录用户（tty 本地登陆  pts 远程登录）
w       查看系统信息，想知道某一时刻用户的行为
uptime  查看登陆多久、多少用户，负载状态
```

<img width="718" height="147" alt="image" src="https://github.com/user-attachments/assets/7109b336-eef4-4a8b-9ddf-9e0bd8aeb2ed" />

查询特权用户


<img width="711" height="43" alt="image" src="https://github.com/user-attachments/assets/353b7a19-8b1e-4ccb-9cc8-805be90ffc7d" />

查询可以远程登录的账号信息


<img width="714" height="37" alt="image" src="https://github.com/user-attachments/assets/1e35a67b-396c-4b4f-a3c2-3ff62b1de5fe" />

除root帐号外，其他帐号是否存在sudo权限。如非管理需要，普通帐号应删除sudo权限


<img width="724" height="35" alt="image" src="https://github.com/user-attachments/assets/cbfc770a-e629-4845-9268-2b605230731d" />


# 历史命令#

root用户的历史命令


<img width="716" height="179" alt="image" src="https://github.com/user-attachments/assets/9d62923d-f35f-40cb-8fde-5678c13b474d" />


打开 /home 各帐号目录下的 .bash_history，查看普通帐号执行的历史命令。
为历史的命令增加登录的 IP 地址、执行命令时间等信息：
1.保存1万条命令


<img width="723" height="24" alt="image" src="https://github.com/user-attachments/assets/de9f5726-3495-4f20-a112-587d1eca1292" />

```
2）在/etc/profile的文件尾部添加如下行数配置信息：
######jiagu history xianshi#########
USER_IP=`who -u am i 2>/dev/null | awk '{print $NF}' | sed -e 's/[()]//g'`
if [ "$USER_IP" = "" ]
then
USER_IP=`hostname`cd ..
fi
export HISTTIMEFORMAT="%F %T $USER_IP `whoami` "
shopt -s histappend
export PROMPT_COMMAND="history -a"
######### jiagu history xianshi ##########

3）source /etc/profile 让配置生效
生成效果： 1  2018-07-10 19:45:39 192.168.204.1 root source /etc/profile

3、历史操作命令的清除：history -c
但此命令并不会清除保存在文件中的记录，因此需要手动删除 .bash_profile 文件中的记录。
```

入侵排查：
```
进入用户目录下，导出历史命令。
cat .bash_history >> history.txt
```

检查异常端口


<img width="718" height="236" alt="image" src="https://github.com/user-attachments/assets/2b5a24e5-2a00-4c2a-8a26-c937d3fbba3f" />


netstat -antlp | more

查看下 pid 所对应的进程文件路径，
运行 ls -l /proc/$PID/exe 或 file /proc/$PID/exe（$PID 为对应的 pid 号）


检查异常进程


<img width="726" height="107" alt="image" src="https://github.com/user-attachments/assets/d92ffa32-48dd-465e-a270-ddcef85871db" />

检查开机启动项

| 运行级别 |                           含义                            
| :------: | :-------------------------------------------------------: 
|    0     |                           关机                            
|    1     | 单用户模式，可以想象为windows的安全模式，主要用于系统修复 
|    2     |              不完全的命令行模式，不含NFS服务              
|    3     |            完全的命令行模式，就是标准字符界面             
|    4     |                         系统保留                          
|    5     |                         图形模式                          
|    6     |                          重启动                           


查看运行级别命令 `runlevel`

系统默认允许级别
```
vi  /etc/inittab
id=3：initdefault  #系统开机后直接进入哪个运行级别
```

开机启动配置文件

```
/etc/rc.local
/etc/rc.d/rc[0~6].d
```

例子：当我们需要开机启动自己的脚本时，只需要将可执行脚本丢在 /etc/init.d 目录下，然后在 /etc/rc.d/rc*.d 文件中建立软链接即可。


注：此中的 * 代表 0,1,2,3,4,5,6 这七个等级
```
  root@localhost ~]# ln -s /etc/init.d/sshd /etc/rc.d/rc3.d/S100ssh
```
此处sshd是具体服务的脚本文件，S100ssh是其软链接，S开头代表加载时自启动；如果是K开头的脚本文件，代表运行级别加载时需要关闭的。

**入侵排查：**

启动项文件：

```
more /etc/rc.local
/etc/rc.d/rc[0~6].d
ls -l /etc/rc.d/rc3.d/
```



检查服务

服务自启动

第一种修改方法：
```
chkconfig [--level 运行级别] [独立服务名] [on|off]
chkconfig –level  2345 httpd on  开启自启动
chkconfig httpd on （默认level是2345）
```

第二种修改方法：
```
修改 /etc/re.d/rc.local 文件  
加入 /etc/init.d/httpd start
```
第三种修改方法：

使用 ntsysv 命令管理自启动，可以管理独立服务和 xinetd 服务。

入侵排查

1、查询已安装的服务：

RPM 包安装的服务
```
	chkconfig  --list  查看服务自启动状态，可以看到所有的RPM包安装的服务
	ps aux | grep crond 查看当前服务
	
	系统在3与5级别下的启动项 
	中文环境
	chkconfig --list | grep "3:启用\|5:启用"
	英文环境
	chkconfig --list | grep "3:on\|5:on"

```
源码包安装的服务
```
	查看服务安装位置 ，一般是在/user/local/
	service httpd start
	搜索/etc/rc.d/init.d/  查看是否存在

```

检查异常文件

1、查看敏感目录，如/tmp目录下的文件，同时注意隐藏文件夹，以“..”为名的文件夹具有隐藏属性

2、得到发现WEBSHELL、远控木马的创建时间，如何找出同一时间范围内创建的文件？

​	可以使用find命令来查找，如  find /opt -iname "*" -atime 1 -type f 找出 /opt 下一天前访问过的文件

3、针对可疑文件可以使用 stat 进行创建修改时间。



检查系统日志

|     日志文件     |                             说明                             
| :--------------: | :----------------------------------------------------------: 
|  /var/log/cron   |                 记录了系统定时任务相关的日志                 
|  /var/log/cups   |                      记录打印信息的日志                      
|  /var/log/dmesg  | 记录了系统在开机时内核自检的信息，也可以使用dmesg命令直接查看内核自检信息 
| /var/log/mailog  |                         记录邮件信息                         
| /var/log/message | 记录系统重要信息的日志。这个日志文件中会记录Linux系统的绝大多数重要信息，如果系统出现问题时，首先要检查的就应该是这个日志文件 
|  /var/log/btmp   | 记录错误登录日志，这个文件是二进制文件，不能直接vi查看，而要使用lastb命令查看 
| /var/log/lastlog | 记录系统中所有用户最后一次登录时间的日志，这个文件是二进制文件，不能直接vi，而要使用lastlog命令查看 
|  /var/log/wtmp   | 永久记录所有用户的登录、注销信息，同时记录系统的启动、重启、关机事件。同样这个文件也是一个二进制文件，不能直接vi，而需要使用last命令来查看 
|  /var/log/utmp   | 记录当前已经登录的用户信息，这个文件会随着用户的登录和注销不断变化，只记录当前登录用户的信息。同样这个文件不能直接vi，而要使用w,who,users等命令来查询 
| /var/log/secure  | 记录验证和授权方面的信息，只要涉及账号和密码的程序都会记录，比如SSH登录，su切换用户，sudo授权，甚至添加用户和修改用户密码都会记录在这个日志文件中 


比较重要的几个日志：
	登录失败记录：/var/log/btmp     //lastb
	最后一次登录：/var/log/lastlog  //lastlog
	登录成功记录: /var/log/wtmp     //last
	登录日志记录：/var/log/secure   

​	目前登录用户信息：/var/run/utmp  //w、who、users

​	历史命令记录：history
​	仅清理当前用户： history -c

# linux的日志分析 #

常用的shell命令

Linux下常用的shell命令如：find、grep 、egrep、awk、sed

小技巧：

1、grep显示前后几行信息:

```
	标准unix/linux下的grep通过下面參数控制上下文：
​	grep -C 5 foo file 显示file文件里匹配foo字串那行以及上下5行
​	grep -B 5 foo file 显示foo及前5行
​	grep -A 5 foo file 显示foo及后5行
​	查看grep版本号的方法是
​	grep -V
```

2、grep 查找含有某字符串的所有文件

```
	grep -rn "hello,world!" 
	* : 表示当前目录所有文件，也可以是某个文件名
	-r 是递归查找
	-n 是显示行号
	-R 查找所有文件包含子目录
	-i 忽略大小写
```

3、如何显示一个文件的某几行：

```
	cat input_file | tail -n +1000 | head -n 2000
	#从第1000行开始，显示2000行。即显示1000~2999行
```


4、find /etc -name init 

	//在目录/etc中查找文件init

5、只是显示/etc/passwd的账户

	`cat /etc/passwd |awk  -F ':'  '{print $1}'`  
	//awk -F指定域分隔符为':'，将记录按指定的域分隔符划分域，填充域，​$0则表示所有域,$1表示第一个域,​$n表示第n个域。

6、sed -i '153,$d' .bash_history

	删除历史操作记录，只保留前153行

日志分析技巧

A、/var/log/secure



1、定位有多少IP在爆破主机的root帐号：    
```
grep "Failed password for root" /var/log/secure | awk '{print $11}' | sort | uniq -c | sort -nr | more
```
定位有哪些IP在爆破：
```
grep "Failed password" /var/log/secure|grep -E -o "(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)"|uniq -c
```
爆破用户名字典是什么？
```
grep "Failed password" /var/log/secure|perl -e 'while($_=<>){ /for(.*?) from/; print "$1\n";}'|uniq -c|sort -nr
 ```


2、登录成功的IP有哪些： 	
```
grep "Accepted " /var/log/secure | awk '{print $11}' | sort | uniq -c | sort -nr | more
```
登录成功的日期、用户名、IP：
```
grep "Accepted " /var/log/secure | awk '{print $1,$2,$3,$9,$11}' 
```


3、增加一个用户kali日志：
```
Jul 10 00:12:15 localhost useradd[2382]: new group: name=kali, GID=1001
Jul 10 00:12:15 localhost useradd[2382]: new user: name=kali, UID=1001, GID=1001, home=/home/kali
, shell=/bin/bash
Jul 10 00:12:58 localhost passwd: pam_unix(passwd:chauthtok): password changed for kali
#grep "useradd" /var/log/secure 
```



4、删除用户kali日志：
```
Jul 10 00:14:17 localhost userdel[2393]: delete user 'kali'
Jul 10 00:14:17 localhost userdel[2393]: removed group 'kali' owned by 'kali'
Jul 10 00:14:17 localhost userdel[2393]: removed shadow group 'kali' owned by 'kali'
# grep "userdel" /var/log/secure
```






5、su切换用户：
```
Jul 10 00:38:13 localhost su: pam_unix(su-l:session): session opened for user good by root(uid=0)

sudo授权执行:
sudo -l
Jul 10 00:43:09 localhost sudo:    good : TTY=pts/4 ; PWD=/home/good ; USER=root ; COMMAND=/sbin/shutdown -r now

2、/var/log/yum.log

软件安装升级卸载日志：

yum install gcc
yum install gcc

[root@bogon ~]# more /var/log/yum.log

Jul 10 00:18:23 Updated: cpp-4.8.5-28.el7_5.1.x86_64
Jul 10 00:18:24 Updated: libgcc-4.8.5-28.el7_5.1.x86_64
Jul 10 00:18:24 Updated: libgomp-4.8.5-28.el7_5.1.x86_64
Jul 10 00:18:28 Updated: gcc-4.8.5-28.el7_5.1.x86_64
Jul 10 00:18:28 Updated: libgcc-4.8.5-28.el7_5.1.i686

```

kail端口扫描和ubuntu封禁


<img width="717" height="631" alt="image" src="https://github.com/user-attachments/assets/cb7b31b2-8b41-4102-a9e0-6e697ac11dc9" />
```
-R  #继续从上一次进度接着破解。
-S  #采用SSL链接。
-s  #PORT 可通过这个参数指定非默认端口。
-l  #LOGIN 指定破解的用户，对特定用户破解。
-L  #FILE 指定用户名字典。
-p  #PASS 小写，指定密码破解，少用，一般是采用密码字典。
-P  #FILE 大写，指定密码字典。
-e  #ns 可选选项，n：空密码试探，s：使用指定用户和密码试探。
-C  #FILE 使用冒号分割格式，例如“登录名:密码”来代替-L/-P参数。
-M  #FILE 指定目标列表文件一行一条。
-o  #FILE 指定结果输出文件。
-f  #在使用-M参数以后，找到第一对登录名或者密码的时候中止破解。
-t  #TASKS 同时运行的线程数，默认为16。
-w  #TIME 设置最大超时的时间，单位秒，默认是30s。
-v/-V #显示详细过程。
server #目标ip
service #指定服务名
OPT #可选项
```
我们用kail自带的九头蛇

九头蛇有自带的字典

<img width="715" height="150" alt="image" src="https://github.com/user-attachments/assets/9dcd2055-bbb7-48d6-b44d-26b213b4856e" />

上面用kail自带的九头蛇给我们的靶机ubuntu爆破了一下


<img width="730" height="184" alt="image" src="https://github.com/user-attachments/assets/73c9d2a0-1b6b-4c4d-8ef2-76a59e64e7fa" />


我们去ubuntu上看一下，爆破情况


<img width="731" height="62" alt="image" src="https://github.com/user-attachments/assets/b3431f73-f06b-4784-b32a-787723f4123a" />

这里可以看到kail的ip爆破失败了503次

再看一下登录成功的ip有哪些


<img width="718" height="41" alt="image" src="https://github.com/user-attachments/assets/7d062088-1c2b-4db6-b605-9a090dae7970" />

这里就显示一个，不是kail的

这里给ubuntu加一个自动封禁ip的脚本


<img width="718" height="477" alt="image" src="https://github.com/user-attachments/assets/6da536c5-ce17-44df-9020-fab101f9f417" />

添加执行权限
```
sudo chmod +x /usr/local/bin/block_ssh_shell.sh
```

测试一下

运行脚本


<img width="717" height="29" alt="image" src="https://github.com/user-attachments/assets/242b9863-50e3-4aa2-9df8-39ca4fc7df4b" />


kail的九头蛇攻击


<img width="720" height="164" alt="image" src="https://github.com/user-attachments/assets/473584ca-917a-447f-8426-f98206e64d60" />


ubuntu检测到攻击


<img width="742" height="243" alt="image" src="https://github.com/user-attachments/assets/ca9819a6-befb-478e-bef9-73005060c6f3" />

显示kail的ip被封禁


看一下shell脚本代码
```
这段脚本的作用是：
 **自动检测并封禁**在过去一分钟内对 Ubuntu 系统的 **SSH（22端口）暴力破解尝试的IP**。
```bash
#!/bin/bash
```

- 指定使用 Bash 解释器。

------

```bash
LOGFILE="/var/log/auth.log"
THRESHOLD=10
BAN_PORT=22
```

- `LOGFILE`：登录日志文件，Ubuntu 默认是 `/var/log/auth.log`。
- `THRESHOLD`：阈值，超过 10 次失败就封禁。
- `BAN_PORT`：要封禁的端口，这里是 SSH 默认端口 22。

------

```bash
TIME_WINDOW=$(date -d "1 minute ago" +"%b %e %H:%M")
```

- 获取当前时间前 1 分钟的时间戳（格式例如：`Aug  3 23:30`），用于筛选日志。


------

```bash
TMPFILE="/tmp/ssh_banlist.txt"
> $TMPFILE
```

- `TMPFILE` 是临时存放超出阈值的 IP 的文件。
- `> $TMPFILE` 表示清空（或创建）该文件。

------

```bash
grep "Failed password" "$LOGFILE" | tail -n 100 /var/log/auth.log | \
awk '{print $(NF-3)}' | grep -Eo '([0-9]{1,3}\.){3}[0-9]{1,3}' | \
sort | uniq -c | awk -v t=$THRESHOLD '$1 > t {print $2}' > "$TMPFILE"
```

这串命令的作用是：

| 步骤                                      | 说明                                                         |
| ----------------------------------------- | ------------------------------------------------------------ |
| `grep "Failed password"`                  | 从日志中筛出登录失败记录                                     |
| `tail -n 100`                             | 只取最后 100 行（模拟“1分钟”窗口）⚠️ 实际不等价于一分钟内的数据 |
| `awk '{print $(NF-3)}'`                   | 获取 IP 地址字段                                             |
| `grep -Eo ...`                            | 进一步提取合法 IP 格式                                       |
| `sort                                     | uniq -c`                                                     |
| `awk -v t=$THRESHOLD '$1 > t {print $2}'` | 找出超过阈值的 IP                                            |
| `> "$TMPFILE"`                            | 把结果写入临时文件                                           |

------

```bash
for IP in $(cat $TMPFILE); do
```

- 遍历每个需要封禁的 IP。

------

```bash
  if ! iptables -C INPUT -s "$IP" -p tcp --dport $BAN_PORT -j DROP &>/dev/null; then
```

- 检查该 IP 是否已经在防火墙规则中。
- `iptables -C` 用于检查规则是否存在，如果没有（即返回非0），就执行 `DROP`。

------

```bash
    echo "[+] Detected brute-force from $IP - Blocking"
    iptables -I INPUT -s "$IP" -p tcp --dport $BAN_PORT -j DROP
```

- 插入一条规则：封禁此 IP 访问端口 22。

------

```bash
  else
    echo "[-] $IP already blocked"
  fi
done
```

- 如果 IP 已封禁，跳过。
- 否则添加规则。

------

```bash
rm -f "$TMPFILE"
```
- 脚本最后删除临时

