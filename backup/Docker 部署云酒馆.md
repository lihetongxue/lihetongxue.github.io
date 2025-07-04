## 1. 介绍

> [!NOTE]
> SillyTavern 是一个充满激情的项目，由一群热忱的 LLM 爱好者社区倾情打造，并将永远免费开源

### 1.1 SillyTavern 是什么？

**SillyTavern**（俗称酒馆）是一个本地安装的用户界面，允许您与文本生成 LLM、图像生成引擎和 TTS 语音模型进行交互；主要是用来玩角色扮演, 其中提供了非常丰富的自定义选项, 备受NSFW用户喜爱。

而**云酒馆**顾名思义，就是将酒馆部署在云端，不依赖本地，能够在任何有网络的地方进行访问；毛坯房预览如下：

![1750665823211.png](https://lihe.ink/2025/06/23/68590a5d52924.png)

### 1.2 角色卡

**SillyTavern** 围绕`角色卡`的概念构建。角色卡是设定 LLM 行为的 Prompt 集合，是在 SillyTavern 中进行持久对话所必需的。它们的功能类似于 ChatGPT 的 GPTs 或 Poe 的 bots。角色卡的内容可以是任何东西：一个抽象的场景、一个为特定任务量身定制的助手、一个著名人物或一个虚构角色。

### 1.3 主要功能

* **高级文本生成设置：** 包含许多社区制作的预设
* **世界书支持：** 创建丰富的传说或节省角色卡上的 Token
* **群聊：** 多机器人房间，供角色与您或彼此交谈
* **丰富的 UI 自定义选项：** 主题颜色、背景图片、自定义 CSS 等
* **用户角色：** 让 AI 了解一些关于您的信息，以获得更强的沉浸感
* **内置 RAG 支持：** 将文档添加到您的聊天中供 AI 参考
* 广泛的聊天命令子系统和自己的脚本引擎

### 1.4 扩展

1. 角色情绪表达
2. 聊天记录自动摘要
3. 自动 UI 和聊天翻译
4. Stable Diffusion/FLUX/DALL-E 图像生成
5. AI 回复消息的文本转语音（通过 ElevenLabs、Silero 或操作系统的 TTS）
6. 网络搜索功能，为您的 Prompt 添加额外的现实世界背景信息
7. 更多的扩展可从“下载扩展和资源”菜单中下载..

## 2. 部署云酒馆

> [!IMPORTANT]
> 本教程采用 Docker 方式部署，Docker 和 Docker-compose 安装请移步 [Docker 安装](https://blog.lihe.site/post/Docker-an-zhuang.html)

### 2.1 开始部署

**克隆仓库到本地：**

```
git clone https://github.com/SillyTavern/SillyTavern
```

**进入docker目录下并运行容器：**

```
cd SillyTavern/docker

docker compose up -d
```

**配置白名单：**

编辑`config/config.yaml`文件，找到`whitelist`部分内容：

```
whitelist:
    - 127.0.0.1
```

在下面增加一行，如下：

```
whitelist:
    - 127.0.0.1
    - 0.0.0.0/0
```

**设置用户名和密码：**

编辑`config/config.yaml`文件，找到`basicAuthUser:`部分，修改为你的用户名和密码

```
basicAuthUser:
  username: "user" # 修改为你自己的用户名
  password: "password" # 修改为你自己的密码
```

**重启容器：**

```
docker compose restart sillytavern
```

### 2.2 访问云酒馆

> [!NOTE]
> 访问云酒馆需要服务器放行`8000`端口

浏览器访问 `服务器IP:8000`即可看到**sillytavern**加载完成

![1750672088286.png](https://lihe.ink/2025/06/23/685922d3ea072.png)

输入用户名和密码就能进入了

## 3. 使用与配置：

### 3.1 配置API进行聊天

**在 SillyTavern 的顶部栏中，单击 API 连接：**

![1751612053839.png](https://lihe.ink/2025/07/04/68677a959bb76.png)

* **API 连接配置：** 自定义名称

* **API：** 选择聊天补全
 
* **聊天补全来源：** 可选(如Deepseek)
 
* **DeepSeek API 密钥：** 填入你的 API
 
* **DeepSeek 模型：** deepseek-chat

填好后点击`连接`进行验证，显示`有效`即可开始聊天！

### 3.2 用户设置及背景配置

**用户配置：**


![1751613357163.png](https://lihe.ink/2025/07/04/68677fad6dbe8.png)

可自行对相应的配置进行修改即可！

**背景更换：**

![1751613529011.png](https://lihe.ink/2025/07/04/68678059e432b.png)

可更换内置背景或自己上传背景图片！

### 3.3 世界书配置

**什么是世界书？**

![1751613815424.png](https://lihe.ink/2025/07/04/68678177bc7cd.png)

**导入世界书：**

![1751613904834.png](https://lihe.ink/2025/07/04/686781d55a71d.png)

点击右下角导入图标进行世界书的导入，世界书的格式为`世界书.json`；此方法为手动导入，也有角色卡自带世界书！

> [!TIP]
> 你可以下载别人的世界书（[类脑Discord频道](https://discord.com/invite/B7Wr25Z7BZ)），也可以创建自己的世界书

### 3.4 角色卡配置

**下载角色卡文件(图片格式)并导入：**

![1751615486714.png](https://lihe.ink/2025/07/04/686787fe0bee1.png)

点击`文件导入角色`即可导入下载的角色卡文件！

**可从以下地址查找角色卡：**

1. [https://character-tavern.com/character/catalog?tags=NSFW](https://character-tavern.com/character/catalog?tags=NSFW)

2. [https://aicharactercards.com/category/character-cards/nsfw/](https://aicharactercards.com/category/character-cards/nsfw/)

3. [https://chub.ai/tags/NSFW](https://chub.ai/tags/NSFW)

4. [类脑Discord频道](https://discord.com/invite/B7Wr25Z7BZ)

### 3.5 破限教程

**手动破限教程可移步：**

[https://sqivg8d05rm.feishu.cn/wiki/QEZ9wcbF9ilvLZkVMEscHQxwnoh](https://sqivg8d05rm.feishu.cn/wiki/QEZ9wcbF9ilvLZkVMEscHQxwnoh)

## 4. 结尾

**更多的使用教程和配置可以查看官方文档：**

* 酒馆 Github 地址：[https://github.com/SillyTavern/SillyTavern](https://github.com/SillyTavern/SillyTavern)
* 
* 酒馆官方文档：[https://docs.sillytavern.app/](https://docs.sillytavern.app/)

更详细的使用教程(飞书文档)：

[https://sqivg8d05rm.feishu.cn/wiki/QEZ9wcbF9ilvLZkVMEscHQxwnoh](https://sqivg8d05rm.feishu.cn/wiki/QEZ9wcbF9ilvLZkVMEscHQxwnoh)

> [!IMPORTANT]
> 感谢文档的编写者，特在此引用并注明！
