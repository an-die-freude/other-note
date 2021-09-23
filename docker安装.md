#### 一 Ubuntu安装Dockers

##### 1 添加软件源、GPG key、APT

```shell
sudo apt update
sudo apt install apt-transport-https ca-certificates curl gnupg-agent software-properties-common
```

```shell
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```

```shell
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"

```

##### 2 安装Docker

安装最新版

```shell
sudo apt install docker-ce docker-ce-cli containerd.io
```

安装指定版本

```shell
apt list -a docker-ce
```

```shell
sudo apt install docker-ce=<VERSION> docker-ce-cli=<VERSION> containerd.io
```

通过以下指令来验证安装

```shell
sudo systemctl status docker
```

停止自动更新

```shell
sudo apt-mark hold docker-ce
```

##### 3 运行

如果是非root用户

```shell
sudo usermod -aG docker $USER
```

运行测试容器

```shell
docker container run hello-world
```

##### 4 卸载

停止所有正在运行的容器并移除所有docker对象

```shell
docker container stop $(docker container ls -aq)
docker system prune -a --volumes
```

卸载

```shell
sudo apt purge docker-ce
sudo apt autoremove
```

