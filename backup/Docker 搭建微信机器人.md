## 1. 什么是 AstrBot？

![image](https://lihe.ink/2025/03/03/67c5be982ca9b.png)

AstrBot 是一个易于上手的多平台聊天机器人及开发框架。通过它，你能够在多种消息平台上部署一个支持大语言模型（LLM）的聊天机器人。并以此实现但不限于 AI 知识库问答、角色扮演、群聊管理、LLM Agent 等功能。它有如下特性：

*  **松耦合：** AstrBot 历经 3 次大代码重构。每一次都在向着松耦合、模块化的方向迈进。目前，AstrBot 采用了事件总线和消息事件流水线的架构设计，实现近乎完全的模块化。

*  **异步：** AstrBot 采用了异步编程模型，使得 AstrBot 在处理多个消息平台的消息时，能够更加高效。

*  **多消息平台部署：** AstrBot 默认支持接入 QQ、QQ频道、微信。通过插件，还可以接入 Telegram 等任何消息平台。

*  **完善的插件系统：** AstrBot 提供了完善、及其易于上手的插件系统，你可以通过插件实现自己的功能。开发一个插件，只需要几行代码。

## 2. 它是如何实现的？

> 下面的拓扑图简述了 AstrBot 与插件、消息平台适配器、语言模型提供商、消息协议实现端的关系。

![image](https://lihe.ink/2025/03/03/67c5c1056b7fe.png)

## 3. 相关地址

**github地址：** [http://github.com/Soulter/AstrBot](http://github.com/Soulter/AstrBot)

**官网地址：** [https://astrbot.app/](https://astrbot.app/)

## 4. 部署 AstrBot

> [!NOTE]
> 此教程默认使用服务器部署，服务器请放行`6180-6200`和`11451`端口，若需其他方式部署，可查看相关[文档](https://astrbot.app/deploy/astrbot/docker.html)

### 4.1 通过 Docker-compose 部署

> Docker及Docker-compose安装方法请移步[Docker安装](https://blog.lihe.site/post/Docker-an-zhuang.html)

**首先，需要 Clone AstrBot 仓库到本地：**

```
git clone https://github.com/Soulter/AstrBot
cd AstrBot
```

**然后，运行 Compose：**

```
docker compose up -d
```

**查看 AstrBot 日志：**

```
docker-compose logs #当前目录下
```

### 4.2 通过 Docker 部署

**创建目录并执行docker run：**

```
mkdir astrbot
docker run -itd -p 6180-6200:6180-6200 -p 11451:11451 -v $PWD/data:/AstrBot/data --name astrbot soulter/astrbot:latest
```

**查看 AstrBot 的日志：**

```
docker logs -f astrbot
```

### 4.3 管理面板

> AstrBot 管理面板具有管理插件、查看日志、可视化配置、查看统计信息等功能

#### 4.3.1 访问管理面板

> 当启动 AstrBot 之后，你可以通过浏览器访问 http://服务器IP:6185 来访问管理面板

![image](https://lihe.ink/2025/03/04/67c674ac7376b.png)

#### 4.3.2 登录

默认用户名和密码是 `astrbot` 和 `astrbot`，登录后可进行修改密码

#### 4.3.3 可视化配置

> 在管理面板中，你可以通过可视化配置来配置 AstrBot 的插件。点击左栏 `配置` 即可进入配置页面

![image](https://lihe.ink/2025/03/04/67c6757c81b7b.png)

* 顶部的两个按钮可以切换`可视化编辑配置`和`代码编辑配置`

* 在`可视化编辑配置`中，当修改完配置后，需要点击右下角保存按钮来保存配置

* 在`代码编辑配置`中，你可以直接编辑配置文件，编辑完后首先点击`应用此配置`，此时配置将应用到可视化配置中，然后再点击右下角`保存`按钮来保存配置

![image](https://lihe.ink/2025/03/04/67c6763a39660.png)

> [!WARNING]
> 请注意，当你在`代码编辑配置`中编辑配置文件时，如果你不点击`应用此配置`，那么你的修改将不会生效

#### 4.3.4 更新管理面板

1. 在 AstrBot 启动时，会自动检查管理面板是否需要更新，如果需要，第一条日志（黄色）会提示

2. 使用 `/dashboard_update` 命令可以手动更新管理面板（管理员指令）

3. 管理面板文件在 `data/dist` 目录下。如果需要手动替换，请在 [https://github.com/Soulter/AstrBot/releases/](https://github.com/Soulter/AstrBot/releases/) 下载 dist.zip 然后解压到 data 目录下

## 5. 部署消息平台

### 5.1 通过 Gewechat 接入微信

> [!NOTE]
> 1. 这个接入方式不受微信官方支持，使用的是 [Devo919/Gewechat](https://github.com/Devo919/Gewechat)。请注意风险。如果要使用官方支持的方式，> 请使用企业微信的方式接入
> 2. 请控制聊天频率。如果过于频繁使用（同一时间发送消息次数过多），可能会导致更高的风控风险，请注意使用频率

> [!WARNING]
> 1. 仅支持微信个人号
> 2. 微信限制，需要手动扫码登录
> 3. 微信限制一个微信号必须**有一台手机在线**才能登录其他端。而 Gewechat 是一个 IPAD 微信客户端。因此，你需要有一台手机登录该微信，才能使用 Gewechat

#### 5.1.1 部署 Gewechat

**拉取镜像：**

```
 docker pull registry.cn-hangzhou.aliyuncs.com/gewe/gewe:latest
 
 docker tag registry.cn-hangzhou.aliyuncs.com/gewe/gewe gewe
```

**运行容器：**

```
mkdir -p /root/temp
docker run -itd -v /root/temp:/root/temp -p 2531:2531 -p 2532:2532 --privileged=true --name=gewe gewe /usr/sbin/init
```

**设置开机自启：**

```
docker update --restart=always gewe
```

**查看日志：**

```
docker-compose logs
```

#### 5.1.2 在 AstrBot 中配置 Gewechat 适配器

在 AstrBot 的管理面板中，选择左边栏的 **配置**，然后在右边的界面中，点击 **消息平台** 选项卡。点击  **+** 号，选择 **gewechat**，会出现 gewechat 的相关配置项，如下图所示：

![image](https://lihe.ink/2025/03/04/67c66a14e6e52.png)

* **nickname** 请随便填一个具有辨识度的英文名,不需要是微信用户名

* **base_url** 连接到 Gewechat 后端的 API 地址

* **host** 回调地址主机，即 gewechat 下发事件到 AstrBot 的地址，同一个服务器则默认为localhost

* **port** 回调地址端口，可不修改，默认11451

> [!NOTE]
> 对于 Linux 使用 Docker 部署 AstrBot 部署的用户，请通过 docker inspect gewe 查看 gewechat 容器网络的 IP 地址，然后`http://ip地址:2531`；如果有 公网ip，也可以是公网 ip，但是需要放行 2531 端口

#### 5.1.3 接入大模型服务

在**管理面板**->**服务提供商**->**+****新增服务提供商** 处配置各种大语言模型服务

![image](https://lihe.ink/2025/03/04/67c6723684470.png)
![image](https://lihe.ink/2025/03/04/67c672f4096bf.png)

选择想要接入的大模型，例如`Deepseek`,填入获取的 **API key** 保存即可

#### 5.1.4 扫码登录

**查看日志：**

![image](https://lihe.ink/2025/03/04/67c66e1e7f233.png)

查看 AstrBot 的终端日志输出，会出现相关引导提示，打开提示的二维码链接扫码登录即可

> [!IMPORTANT]
> 如果扫码登录失败，可以将手机语言设置为英文，然后再次扫码即可登录

#### 5.1.5 食用方法

**查看日志确认登录成功：**

在控制台查看日志，若登录成功，则会显示 `成功登录绿泡泡及回调信息等`

**在微信测试：**

可以私发消息给机器人微信，如`/help`可以查看内置指令

可将机器人加入群聊，通过`@机器人`可以进行唤醒对话，例如`@机器人 请帮我写一首关于爱情的诗`

**开启插件：**

可以在管理面板的**插件**添加插件开启或禁止，插件的使用可以点击插件的**帮助**来查看使用方法

![image](https://lihe.ink/2025/03/04/67c670d489976.png)

#### 5.1.6 注意事项

一旦登录成功，请牢记在配置时配置的 username，如果更换，则相当于使用新设备登录。频繁新设备登录容易触发风控

## 6. 结尾

> [!TIP]
> 觉得不错的可以去[Github](https://github.com/Soulter/AstrBot)给项目点个`Star`

在使用的过程中如果遇到问题，可以先自行查看文档，解决不了可以去`Github`提交issues

更多的使用方法以及功能可以参考[官网文档](https://astrbot.app/what-is-astrbot.html)







