### 安装docker（非大陆服务器）
```
wget -qO- get.docker.com | bash
```
### 查看docker版本
```
docker -v
```
### 设置开机自启动
```
systemctl enable docker
```
### 安装docker-compose（非大陆服务器）
```
sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```
### 加执行权限
```
sudo chmod +x /usr/local/bin/docker-compose
```
### 查看docker-compose版本
```
docker-compose --version
```

### 安装docker（国内服务器）
```
curl -sSL https://get.daocloud.io/docker | sh
```
### 查看docker版本
```
docker -v
```
### 设置开机自启动
```
systemctl enable docker
```
### 安装docker-compose（国内服务器）
```
curl -L https://get.daocloud.io/docker/compose/releases/download/v2.1.1/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose

chmod +x /usr/local/bin/docker-compose  #加执行权限

docker-compose --version  #查看 docker-compose 版本
```

### 修改docker配置

> [!NOTE]
> 以下配置会增加一段自定义内网 IPv6 地址，开启容器的 IPv6 功能，以及限制日志文件大小，防止 Docker 日志塞满硬盘（泪的教训）：
```
cat > /etc/docker/daemon.json <<EOF
{
    "log-driver": "json-file",
    "log-opts": {
        "max-size": "20m",
        "max-file": "3"
    },
    "ipv6": true,
    "fixed-cidr-v6": "fd00:dead:beef:c0::/80",
    "experimental":true,
    "ip6tables":true
}
EOF
```
**重启docker服务**
```
systemctl restart docker

```
