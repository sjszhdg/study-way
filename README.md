# xss.PwnFunction #

准备访问xss.pwnfunction.com时发现访问不了，开vpn也不行
这是原作者的项目地址
https://github.com/PwnFunction/xss.pwnfunction.com?tab=readme-ov-file

然后可以将他部署在ubantu上

我的是ubantu24.04


直接用proxy代理把项目拉下来
```
proxychains git clone https://github.com/PwnFunction/xss.pwnfunction.com.git
```

进入吗命令行，先下载hugo服务
```
apt install hugo
```
<img width="1260" height="643" alt="image" src="https://github.com/user-attachments/assets/1f80cbac-57ed-4772-af16-2c078f733e06" />

进入hugo目录
```
cd xss.pwnfunction.com-main/hugo··
```

将可以访问的ip改掉，这里0.0.0.0 指的是所有ip都可以访问
```
hugo server --bind 0.0.0.0 --baseURL http://192.168.137.135/
```

<img width="1185" height="536" alt="image" src="https://github.com/user-attachments/assets/e65a593c-a145-4240-92d7-6086c0fed951" />


然后就可以在我们的浏览器上访问了
```
http://<ubantu的ip>:1313
```
<img width="2247" height="1309" alt="image" src="https://github.com/user-attachments/assets/aa185bfc-c9da-45c8-af73-ae2ba0494775" />



