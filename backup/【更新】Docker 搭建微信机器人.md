## 前言

> 由于 Gewechat 项目不再维护及可用，现采用 WeChatPadPro 方法替代

之前介绍的 [Docker 搭建微信机器人](https://blog.lihe.site/post/Docker%20-da-jian-wei-xin-ji-qi-ren.html) 教程现将微信接入方式更新为 WeChatPadPro，替换标题 5 的内容，其他部分不变

## 5. 部署消息平台

### 5.1 通过 WeChatPadPro 接入微信

> [!NOTE]
> 1. 需要 AstrBot 版本 >= 3.5.10
> 2. 这个接入方式不受微信官方支持，使用的是 [WeChatPadPro](https://github.com/luolin-ai/WeChatPadPro)。请注意风险。如果要使用官方支持的方式，> 请使用企业微信的方式接入
> 3. 请控制聊天频率。如果过于频繁使用（同一时间发送消息次数过多），可能会导致更高的风控风险，请注意使用频率

> [!WARNING]
> 1. 仅支持微信个人号
> 2. 微信限制，需要手动扫码登录
> 3. 微信限制一个微信号必须有一台手机在线才能登录其他端。因此，你需要有一台手机登录该微信，才能使用该接入方式。请尽量保证手机和部署地设备处于同一城市。

#### 5.1.1 部署 WeChatPadPro

> [!NOTE]
> 1. 这里仅通过 Docker 部署，且仅在 Linux 上测试了部署的成功性
> 2. 不支持 `arm64` 架构的设备

**克隆 WeChatPadPro-Docker 到本地并进入：**

```
 git clone https://github.com/bclz-wyz/WeChatPadPro-Docker --depth 1
 
 cd WeChatPadPro-Docker
```

**创建 main 和 mysql 目录：**

```
mkdir main && mkdir mysql
```

**进入 main 目录并克隆 WeChatPadPro 到当前目录：**

```
cd main

git clone https://github.com/luolin-ai/WeChatPadPro --depth 1
```

**将 WeChatPadPro 内所有文件放到 main 文件夹下：**

```
mv WeChatPadPro/* .
```

**释出 stay 可执行文件：**

```
unzip 859_service_linux.zip
```

**覆盖 WeChatPadPro 原来的 setting.json 文件：**

```
cd .. # 回到 WeChatPadPro-Docker 目录

mv main/assets/setting.json setting.json
```

> [!TIP]
> 你可以根据你的需要，编辑 `main/assets/setting.json` 文件，也可以不编辑。建议将 `adminKey` 修改为更安全的密码

**运行容器并查看 WeChatPadPro 日志：**

```
docker compose up -d

docker logs wechatpadpro
```

**正常情况下，会出现如下日志：**

```shell
版本号: v20250515.00
WeChatPadPro
🚀 基于 WeChat Pad 协议的高级微信管理工具 🚀
...
...
如有任何问题或建议，欢迎通过 GitHub Issues 或邮件与我们联系。
======== ADMIN_KEY === ****** ========
connect MySQL success
auto create MySQL tables success
connect Redis success
updateApiVersion success
``` 

**如果发现报错：**

1. MySQL 容器还没有启动成功

![image](https://lihe.ink/2025/06/09/6846bd067c5ac.png)

确认 `wx_mysql` 容器是为 healthy，再重启 `wechatpadpro` 容器

2. Redis 容器启动失败

![image](https://lihe.ink/2025/06/09/6846be3adbd50.png)

编辑 `./WeChatPadPro-Docker/redis/conf/redis.conf` 文件，删除或注释以下内容：

```shell
redis_max_conn_lifetime 30
redis_idle_timeout 3000
redis_connect_timeout 1000
redis_max_idle 10
redis_max_active 20
``` 
重启 Redis 容器，确保两个容器 `wx_redis` 和 `wx_mysql` 都为 healthy 后，再重启 `wechatpadpro` 容器

#### 5.1.2 在 AstrBot 中配置  `wechatpadpro`  适配器

**在 AstrBot 的管理面板中，打开消息平台 -> 新增适配器 -> wechatpadpro(微信)，如下图所示：**

![image](https://lihe.ink/2025/06/09/6846c1e2771c2.png)

* **admin_key：** 上面步骤中，setting.json 下的 adminKey 的值

* **host：** 填写部署 WeChatPadPro 项目的机器的主机地址，这里直接填你的服务器IP，不要填 localhost

* **port：** Docker 部署默认填 `38849`，如果没修改填这个即可

> [!NOTE]
> 填写完成后，保存；由于 host 填的是服务器 IP,  服务器需要放行 `38849` 端口

**进入`控制台`，查看日志，如果连接成功，会出现如下日志：**

```shell
 [12:50:16] [Core] [INFO] [wechatpadpro.wechatpadpro_adapter:69]: WeChatPadPro 设备不在线或无可用凭据，开始扫码登录流程。 
 [12:50:16] [Core] [INFO] [wechatpadpro.wechatpadpro_adapter:189]: 成功获取授权码 
 [12:50:16] [Core] [INFO] [wechatpadpro.wechatpadpro_adapter:81]: 请扫描以下二维码登录: https://api.pwmqr.com/qrcode/create/?url=http://weixin.qq.com/x/xxx
 [12:50:16] [Core] [INFO] [wechatpadpro.wechatpadpro_adapter:251]: 请在 30 秒内扫码登录！！！
``` 

请打开微信扫码即可，如果超出了 30 秒，重新关开这个平台适配器即可

> [!WARNING]
> **在第一次掉线之后请务必执行以下工作流程，否则将会严重提高风控风险** 
> 
> 1. 进入 WechatPadPro 的 API 接口界面，将你刚刚扫码登录的账号的 token 填入 “TOKEN_KEY”，在 ADMIN_KEY 处输入自己设置的密钥。然后点击“连接”
>
> 2. 然后往下翻找到“登录”——>”唤醒登录（只限扫码登陆）“，点击 ”Try it out“，再点击 ”Execute“。此时，您扫码的那台机器会弹出登录界面。这个时候就能实现老设备登录不会触发风控
> 
> Token Key 即授权码，获取方式如下：
>
> 打开 AstrBot 数据目录（AstrBot/data 目录），找到 `wechatpadpro_credentials.json` 文件，打开之后，其中的 `auth_key` 即为对应 `wxid` 的授权码

#### 5.1.3 接入大模型服务

在**管理面板**->**服务提供商**->**+****新增服务提供商** 处配置各种大语言模型服务

![image](https://lihe.ink/2025/03/04/67c6723684470.png)
![image](https://lihe.ink/2025/03/04/67c672f4096bf.png)

选择想要接入的大模型，例如`Deepseek`,填入获取的 **API key** 保存即可

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
