# study-way #
sjszhdg的学习渗透的过程


# linux ubantu docker搭建 #
首先要有linux先装好ubantu，我这里用的是24.04版本，20.04以上都行。
Docker 是目前最流行的容器化技术，它可以帮助开发者快速部署和运行应用程序。以后会经常用到，漏洞环境拉取等等。
这里推荐一个网站https://vulhub.org/zh是一个开源的漏洞环境，它上面会有一个简单的教程我下面提供另一种方法。


<img width="2292" height="994" alt="image" src="https://github.com/user-attachments/assets/ade3037d-42c9-499e-bb22-9d4e859a5903" />

<img width="2143" height="1241" alt="image" src="https://github.com/user-attachments/assets/46167a1e-9a0b-4abd-9d5f-7a1f5a17ecd5" />



# vulhub的教程 #
Linux使用便捷脚本安装Docker
```
curl -fsSL https://get.docker.com | sh
```
验证Docker是否正确安装：
```
docker version
```

还要验证Docker Compose是否可用：
```
docker compose version
```


克隆仓库
```
git clone --depth 1 https://github.com/vulhub/vulhub.git
```

选择漏洞环境
浏览仓库并选择您想要探索的漏洞。每个目录代表一个不同的漏洞应用程序。
```
cd vulhub/spring/CVE-2022-22947
```


启动环境
使用Docker Compose构建并启动漏洞环境。
```
docker compose up -d
```


# 另一个方法
两者选其一即可
添加 Docker 官方 GPG key （可能国内现在访问会存在问题）
```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
```

 阿里源（推荐使用阿里的gpg KEY）
```
curl -fsSL https://mirrors.aliyun.com/docker-ce/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
```

两者选其一
添加 apt 源:
Docker官方源
```
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

阿里apt源
```
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://mirrors.aliyun.com/docker-ce/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

更新源
```
sudo apt update
sudo apt-get update
```

安装最新版本的Docker
```
sudo apt install docker-ce docker-ce-cli containerd.io
```
等待安装完成

查看Docker版本
```
sudo docker version
```
查看Docker运行状态
```
sudo systemctl status docker
```

我在安装过程中xshell意外连接不上了没找到是什么原因但是我编辑一下/etc/ssh/sshd_config文件：
```
sudo vim /etc/ssh/sshd_config
```
将 PermitRootLogin改为yes：
<img width="620" height="609" alt="image" src="https://github.com/user-attachments/assets/8975db8d-9fa0-4cc1-a125-3d7e5c91d007" />

然后重启一下ssh就行了

另外ssh连接不上可能是主机虚拟适配器VMware没有连接到VMnet8
在VMware编辑里找到虚拟网络编辑器下面就能看到将两个勾上就行了


# 问题 #
我在拉镜像是遇到了报错Get "https://registry-1.docker.io/v2/": net/http: request canceled while waiting for connection (Client.Timeout exceeded while awaiting headers)
大概就是请求超时，是网络问题导致镜像没拉下来


配置一个加速地址
```
vim /etc/docker/daemon.json

添加下面内容
{
	"registry-mirrors": [
	 				"https://docker.211678.top",
					"https://docker.1panel.live",
					"https://hub.rat.dev",
					"https://docker.m.daocloud.io",
					"https://do.nark.eu.org",
					"https://dockerpull.com",
					"https://dockerproxy.cn",
					"https://docker.awsl9527.cn"
	]
}
```

然后保存
重启
```
systemctl daemon-reload
systemctl restart  docker
```

我完成配置后是成功完成了第一个方法
