# xss.PwnFunction #

准备访问xss.pwnfunction.com时发现访问不了，开vpn也不行
这是原作者的项目地址
https://github.com/PwnFunction/xss.pwnfunction.com?tab=readme-ov-file

然后可以将他部署在ubantu上

下载完压缩包后通过xftp传到ubantu上，我的是ubantu24.04

<img width="1292" height="672" alt="image" src="https://github.com/user-attachments/assets/f3f7711c-99fa-410f-819a-4739f35f7b70" />

我这里放在了bin文件下
<img width="1072" height="169" alt="image" src="https://github.com/user-attachments/assets/d96a9e9c-d79d-4f60-bed2-e855f0617f20" />

进入吗命令行，先下载hugo服务
```
apt install hugo
```
<img width="1260" height="643" alt="image" src="https://github.com/user-attachments/assets/1f80cbac-57ed-4772-af16-2c078f733e06" />


由于我们下载的后缀是zip所以我们解压的时候命令就是unzip
```
unzip xss.pwnfunction.com-main.zip
```
<img width="1128" height="357" alt="image" src="https://github.com/user-attachments/assets/5517cbe1-9a01-4dd3-98a3-5abb1c2f0bd1" />

进入hugo目录
```
cd xss.pwnfunction.com-main/hugo
```

将可以访问的ip改掉，这里0.0.0.0 指的是所有ip都可以访问
```
hugo server --bind 0.0.0.0
```

<img width="1185" height="536" alt="image" src="https://github.com/user-attachments/assets/e65a593c-a145-4240-92d7-6086c0fed951" />


然后就可以在我们的浏览器上访问了
```
http://<ubantu的ip>:1313
```
<img width="2247" height="1309" alt="image" src="https://github.com/user-attachments/assets/aa185bfc-c9da-45c8-af73-ae2ba0494775" />



