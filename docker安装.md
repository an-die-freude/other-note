#### 一 Ubuntu安装Dockers

##### 1 添加软件源、GPG key、APT

```shell
apt update
apt install apt-transport-https ca-certificates curl gnupg-agent software-properties-common
```

```shell
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```

```shell
add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
```

##### 2 安装Docker

安装最新版

```shell
apt install docker-ce docker-ce-cli containerd.io
```

安装指定版本

```shell
apt list -a docker-ce
```

```shell
apt install docker-ce=<VERSION> docker-ce-cli=<VERSION> containerd.io
```

通过以下指令来验证安装

```shell
systemctl status docker
```

停止自动更新

```shell
apt-mark hold docker-ce
```

##### 3 运行

运行测试容器

```shell
docker container run hello-world
```

更换国内镜像源并重启服务

```shell
vim /etc/docker/daemon.json
```

```shell
{
"registry-mirrors":[
	"https://hub-mirror.c.163.com",
	"https://ustc-edu-cn.mirror.aliyuncs.com",
	"https://ghcr.io",
	"https://mirror.baidubce.com"]
}
```

```shell
systemctl restart docker
```

##### 4 卸载

停止所有正在运行的容器并移除所有docker对象

```shell
docker container stop $(docker container ls -aq)
docker system prune -a --volumes
```

卸载

```shell
apt purge docker-ce
apt autoremove
```

##### 5 其他

查看容器信息

```shell
docker inspect <containername/id>
```

