# burpsuite pro 的破解与使用 #

# 简介 #
Burp Suite 是一款由英国 PortSwigger Web Security 公司开发的 Web 应用安全测试工具，被广泛用于网络安全领域，尤其是 Web 渗透测试和漏洞评估。它集成了多个功能模块，能帮助安全测试人员、渗透测试工程师或开发者检测 Web 应用中的安全漏洞，如 SQL 注入、跨站脚本（XSS）、 CSRF（跨站请求伪造）等。

因为github上传文件不能大于25m，我的文件里有的已经大于25m所以这里用百度网盘分享，链接会一直有效

通过网盘分享的文件：BurpSuitePro.zip
链接: https://pan.baidu.com/s/1Iinaci0HRgcNuEZRKvNwNw?pwd=tk4i 提取码: tk4i 



# 第一步下载 #
下载Burp Suite Pro 按照常规步骤安装

下载BurpLoaderKeygen 注册机，将其放入到 Burp Suite Jar 包的同级目录下。


# 第二步破解 #

打开命令行窗口，先带着注册机运行一下 BurpSuite （注意先使用cd /d 命令将目录切换到BurpSuite的安装目录之下）
也可以在文件搜索框输入cmd
<img width="838" height="435" alt="ba1c4f83789ee03f69d94da43fccac5" src="https://github.com/user-attachments/assets/0a40e69b-5b20-4e98-9d9e-f201f09a4b4f" />
<img width="1667" height="853" alt="image" src="https://github.com/user-attachments/assets/e8f93f2d-7173-4729-9a8a-ec12136827f0" />

```
E:\burpsuite\BurpSuitePro\jre\bin\java.exe --add-opens=java.base/java.lang=ALL-UNNAMED --add-opens=java.base/jdk.internal.org.objectweb.asm=ALL-UNNAMED --add-opens=java.base/jdk.internal.org.objectweb.asm.tree=ALL-UNNAMED --add-opens=java.base/jdk.internal.org.objectweb.asm.Opcodes=ALL-UNNAMED -javaagent:BurpLoaderKeygen.jar -jar E:\burpsuite\BurpSuitePro\burpsuite_pro.jar
```


运行后，会弹出一个窗口，提示Enter License Key不管放一边


新开命令行窗口，运行：
```
E:\burpsuite\BurpSuitePro\jre\bin\java.exe -jar E:\burpsuite\BurpSuitePro\BurpLoaderKeygen.jar
```


将窗口中的License中的内容复制粘贴到激活页面的Enter License key中，点击 Next
<img width="1152" height="547" alt="image" src="https://github.com/user-attachments/assets/14be532d-0348-41e1-9f0b-f8cce6c16bc3" />
<img width="1153" height="643" alt="image" src="https://github.com/user-attachments/assets/2edf6ee5-8719-4f1b-bf7e-54fe4b664d2c" />


点击Manual activation进行手动激活
<img width="511" height="431" alt="6fcdcec2935b84495e29c1d58419982" src="https://github.com/user-attachments/assets/76e53da2-9404-4100-b7ce-92daa6c7d80d" />



点击Copy Request将其复制粘贴到左侧窗口的Activation Request中后，自动生成Activation Response
<img width="1160" height="339" alt="image" src="https://github.com/user-attachments/assets/12a39554-6f3d-4fd1-8202-68bc9585deb2" />


将左侧窗口的Activation Response的值复制粘贴到激活页面的第三个输入框内，点击Next
![e641e0029fbcc691b5a467c127f446d](https://github.com/user-attachments/assets/db300b33-5691-46bc-bd40-b6d7acdfa644)

然后就激活成功


# 说明 #
Target(目标)——显示目标目录结构的的一个功能。

Proxy(代理)——拦截HTTP/S的代理服务器，作为一个在浏览器和目标应用程序之间的中间人，允许你拦截，查看，修改在两个方向上的原始数据流。

Spider(蜘蛛)——应用智能感应的网络爬虫，它能完整的枚举应用程序的内容和功能。

Scanner(扫描器)——高级工具，执行后，它能自动地发现web 应用程序的安全漏洞。

Intruder(入侵)——一个定制的高度可配置的工具，对web应用程序进行自动化攻击，如：枚举标识符，收集有用的数据，以及使用fuzzing 技术探测常规漏洞。

Repeater(中继器)——一个靠手动操作来触发单独的HTTP 请求，并分析应用程序响应的工具。

Sequencer(会话)——用来分析那些不可预知的应用程序会话令牌和重要数据项的随机性的工具。

Decoder(解码器)——进行手动执行或对应用程序数据者智能解码编码的工具。

Comparer(对比)——通常是通过一些相关的请求和响应得到两项数据的一个可视化的“差异”。

Extender(扩展)——可以让你加载Burp Suite的扩展，使用你自己的或第三方代码来扩展Burp Suit的功能。

Options(设置)——对Burp Suite的一些设置。

# Dashboard模块的介绍 #

<img width="1583" height="1002" alt="image" src="https://github.com/user-attachments/assets/e89da382-5052-4785-a83e-893d961baeab" />

<img width="1708" height="948" alt="image" src="https://github.com/user-attachments/assets/b13f7784-1d18-4bfa-be16-f668e016c2ca" />

其他按照默认的配置，可以设置自己的UA头

<img width="1397" height="952" alt="image" src="https://github.com/user-attachments/assets/5696a094-91b2-4f96-b15f-97aa5abcd35f" />

设置账户密码访问

<img width="1161" height="707" alt="image" src="https://github.com/user-attachments/assets/db742fed-9c23-40b0-87aa-48ecb072a199" />

扫描结果：更详细可以点击项目的右下角view模块，看到爬取的url信息与扫描结果

<img width="1736" height="911" alt="image" src="https://github.com/user-attachments/assets/4998a5fd-5f04-43a1-802c-aba671a1cb59" />

更详细的模块查看你（Target模块）

<img width="1742" height="879" alt="image" src="https://github.com/user-attachments/assets/7f242597-22be-4a27-8e1d-e1b6ef70f930" />

# New live task 模块的使用 #

<img width="1693" height="963" alt="image" src="https://github.com/user-attachments/assets/66e454aa-9b75-419f-aec5-71af68008f78" />


# 常用模块 #
# proxy 模块的使用 #

<img width="1112" height="391" alt="image" src="https://github.com/user-attachments/assets/75256185-9df3-4a88-87a3-66306bccd923" />

proxy中的intercept模块详解
可以审计当前的请求，或者存储和发送到其他的页面进行其他的操作等

<img width="1357" height="859" alt="image" src="https://github.com/user-attachments/assets/2e8f161c-bba1-4144-a72c-7bdbe8fff642" />

在其他地方导入其他的请求数据包

<img width="1061" height="848" alt="image" src="https://github.com/user-attachments/assets/dc15b4d3-bf3f-4590-8fd2-51bc620d78d7" />

拦截当前数据包的 返回包，可以进行伪造验证等操作，在之后的渗透中会使用到

<img width="1167" height="923" alt="image" src="https://github.com/user-attachments/assets/6675189f-ed4d-44fc-9f08-377fd1dd3520" />

对当前输入时候的对象进行urlcode编码，一些特殊的符号会自动转换成url编码

<img width="900" height="928" alt="image" src="https://github.com/user-attachments/assets/e7316e63-89a0-4101-8cbb-69e5d18e4705" />

proxy中的http hitsory模块 记录请求日志功能


<img width="1408" height="967" alt="image" src="https://github.com/user-attachments/assets/9df32298-6299-4be4-b29b-86b283c59662" />

proxy----options模块 监听配置
拦截的代理设置，proxy就是拦截当前设置的IP与port

<img width="1022" height="503" alt="image" src="https://github.com/user-attachments/assets/81c6355f-8859-41f6-8524-bf28526a048e" />

添加请求拦截规则设置模块–intercept client requests
可以设置不拦截某些条件，例如不拦截请求地址后缀为jpg等，proxy会自动放行此数据包

<img width="1110" height="610" alt="image" src="https://github.com/user-attachments/assets/38cc48c1-9704-4390-b21b-88c5e643136c" />

设置：只拦截post请求

<img width="1055" height="333" alt="image" src="https://github.com/user-attachments/assets/a81bbb3d-cd78-48b8-9d08-1462d6a52972" />

下面一个模块针对返回包的拦截条件，目前用的不是太多

<img width="903" height="629" alt="image" src="https://github.com/user-attachments/assets/205d1763-3ad5-430e-9f2a-1f6d9824d54d" />

自动将请求包的指定内容替换成某些内容—match and replace

<img width="1081" height="780" alt="image" src="https://github.com/user-attachments/assets/26fd8265-0fe8-4942-97a8-de1f84b06d7a" />

# Target模块使用介绍 #
抓取流量后，本模块起到一个辅助的作用，可以观察捕获的页面内的链接等

<img width="1761" height="881" alt="image" src="https://github.com/user-attachments/assets/9db02cc6-2ec6-4ae6-a615-598bf9c8137f" />

# Repeater模块使用介绍 #
将数据发到sitemap 模块

<img width="716" height="922" alt="image" src="https://github.com/user-attachments/assets/34c63d09-2cab-4ad1-9168-a331a91308c7" />

数据的加解密编码等

<img width="1053" height="916" alt="image" src="https://github.com/user-attachments/assets/e53308fe-39f2-4afe-871c-37e07040d910" />

数据的hex值修改，常用于渗透绕过等

<img width="721" height="828" alt="image" src="https://github.com/user-attachments/assets/1b9e4944-3e81-4e68-95fc-625726c9608e" />

右侧的更改数据包同等选项的使用，涉及到对数据包的字段的增加修改等，不用担心格式问题的错误

<img width="1600" height="812" alt="image" src="https://github.com/user-attachments/assets/d4247ca7-894e-47b1-b2ce-e4c9bdce986e" />

# intruder模块使用介绍 #
这个模块是经常是使用到的，这里将常用的模块列举出来
爆破的四种模式
Sniper：狙击手单点模式，将数据逐一填充到指定位置
Battering ram：将数据同时填充到多个指定位置，例A字典的数据同时填充到两个位置
Pitchfork：将每个字典逐一对称匹配，例如A字典的1号位与B字典的1号位匹配，绝不相交匹配
Cluster bomb：将每个字典逐一交叉匹配，例如A字典的所有位与B字典的所有位都匹配，

<img width="1450" height="588" alt="image" src="https://github.com/user-attachments/assets/e64d00f1-435c-4110-9ce0-6b91a0b19d73" />

inturder payload 组合破解爆破模式

<img width="1198" height="797" alt="image" src="https://github.com/user-attachments/assets/aa8d44cf-3f33-4381-9a53-d8f524f5ead3" />

# Decoder 加解密模块 #

<img width="1584" height="851" alt="image" src="https://github.com/user-attachments/assets/8465699f-dcb7-41c2-8fde-e19a1f1aafbf" />

# Comparer 数据对比模块 #

<img width="1666" height="966" alt="image" src="https://github.com/user-attachments/assets/ceb29e83-a85d-4694-8248-0e94ea6bf0f4" />

其他模块就不做过多介绍了，其中用到最多的就是proxy模块，intruder模块，repeater模块
burpsuite还可以和proxifier联动抓取微信小程序的流量
这个我们后面在做介绍。

