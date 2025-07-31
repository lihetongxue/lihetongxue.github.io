#### 前言

Hugo 是由 Go 编写的快速现代静态网站生成器，所以速度比 Hexo 更快，Hugo 适用于那些想要手动编码自己的网站而又不想担心设置复杂的运行时、依赖和数据库的人。

那么 Hexo 有什么优点呢，Hexo插件功能提供了高扩展性和灵活性，两者的部署几乎一模一样，这里我选择 Hugo 举例；至于为什么，我只能说 Hexo 教程实在太多了，不需要我来丰富。

Github Pages 能够免费部署 Hugo，并通过免费域名访问；由于 Hugo 没有后台界面，可以接入 Qexo 后台，Qexo 能够通过 Vercel 部署；这样，Hugo + Github pages + Qexo 就能愉快使用你的免费博客了！

#### 1. Hugo 创建

> 开始前请安装 [GIT](https://git-scm.com/downloads/win)，这里默认使用Git Bash 来操作后续内容，当然使用Power Shell 也是可以的

**安装 Hugo**

Winget 是 Microsoft 的官方免费且开源的 Windows 包管理器。这将安装 Hugo 的扩展版：

```
winget install Hugo.Hugo.Extended
```

当然 Windows 下的其他安装方法也可以，其他安装参考：[https://hugo.opendocs.io/installation/windows/](https://hugo.opendocs.io/installation/windows/)

验证 Hugo 安装，输出版本号则安装成功：

```
hugo version
```

**创建网站**

安装 Hugo 成功后，可以通过 hugo new site 命令创建博客：

```
hugo new site blog
```


![1753873394780.png](https://lihe.ink/2025/07/30/6889fbf6b40c1.png)

**主题配置**

初始化本地仓库：

```
git init
```

下载主题，这里以 [Neillo](https://github.com/guangmean/Niello) 主题举例：

```
git clone https://github.com/guangmean/Niello.git themes/Niello
```

初始化主题配置：

```
cp -rf themes/Niello/exampleSite/* ./
```


接下来可以通过配置 `hugo.toml` 来进行站点细节配置，具体配置方法可以查看 [Niello](https://github.com/guangmean/Niello) 配置文档。

完成后可以通过 hugo new 命令来发布新文章，这里先不创建，因为我们可以通过 Qexo 后台来发布新文章和页面。

**本地查看站点**

Hugo 可以通过 hugo server 命令生成静态网页，给我们提供预览：

```
hugo server
```

![1753874812197.png](https://lihe.ink/2025/07/30/688a017f82e6a.png)

现在我们可以通过浏览器 h`ttp://localhost:1313` 地址访问我们的本地预览网页，网页正常显示的话，`Ctrl + C` 停止服务。

接下来就是将本地仓库 `PUSH` 到你的 GitHub 仓库中，通过 Github pages 来访问博客。

#### 2. Github Pages 配置

**创建名为 `用户名.github.io` 特殊格式仓库**

![1753875194596.png](https://lihe.ink/2025/07/30/688a02fdd7a7b.png)

仓库名填入后点击创建即可。

**本地仓库生成静态网页文件**

```
hugo
```

Hugo 默认会将生成的静态网页文件存放在 public/ 目录下, 可以看到本地博客目录下生成了public文件夹。

**推送到 Github**

配置用户名和邮箱，用户名为 GitHub 账号，邮箱为注册 GitHub 的邮箱：

```
git config --global user.name "用户名"
git config --global user.email "邮箱"
```

配置 Github 公钥：

```
ssh-keygen -t rsa -C "邮箱"
```

![1753875957675.png](https://lihe.ink/2025/07/30/688a05f949aca.png)

这里一路回车就行，根据信息找到 `id_rsa.pub` 文件所在路径，复制里面的内容，再去 Github 右上角主页 `Settings` - `SSH and GPG keys` 创建 SSH keys 并将复制的内容粘贴进去保存即可。

测试连接：

```
ssh -T git@github.com
```

如果出现提示输入 `yes` 即可，如果连接成功，则会输出 `You've successfully`。

添加远程仓库：

```
git remote add origin git@github.com:username/username.github.io
```

这里的 `username` 换成你自己的用户名就行了。

推送到GitHub：

```
git branch -M main
git add .
git commit -m "add test"
git push origin main
```

现在本地仓库已经推送到 Github 了。

**创建 Workflows**
 
在仓库 `Actions` 下 创建 `New workflow` ，文件名随意，文件内容如下：

```
# 工作流名称
name: Deploy Hugo site to gh-pages

# 触发条件
on:
  # 当向 main 分支 push 代码时触发
  push:
    branches:
      - main
  # 允许手动触发
  workflow_dispatch:

# 设置 GITHUB_TOKEN 的权限
permissions:
  contents: write # 允许向仓库写入内容

# 定义任务
jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    steps:
      # 第一步：拉取源码
      # 使用你当前的 main 分支作为源码
      - name: Checkout
        uses: actions/checkout@v4
        with:
          submodules: recursive  # 拉取主题子模块
          fetch-depth: 0        # 获取所有历史，以便 Hugo 生成 lastmod

      # 第二步：设置 Hugo 环境
      - name: Setup Hugo
        uses: peaceiris/actions-hugo@v3
        with:
          hugo-version: 'latest'
          extended: true

      # 第三步：构建网站
      # Hugo 会将生成的网站文件放在 ./public 目录
      - name: Build
        run: hugo --minify

      # 第四步：部署到 gh-pages 分支
      # 这个 Action 会自动处理好一切
      - name: Deploy
        uses: peaceiris/actions-gh-pages@v4
        with:
          # 这个 token 是 GitHub 自动提供的，不需要你手动设置 secret
          # 它有权限向当前仓库推送代码
          github_token: ${{ secrets.GITHUB_TOKEN }}
          # 要将哪个文件夹的内容推送到 gh-pages 分支
          publish_dir: ./public
```


点击保存提交后，会自动触发任务，任务完成后即可更新博客网站内容；接下来在仓库 `Settings` -> `Pages` 下，将分支名切换到 `gh-pages` ，点击 `save` 保存即可。

![1753944502226.png](https://lihe.ink/2025/07/31/688b11b7cc081.png)

#### 3. 搭建并配置 Qexo

这里通过 Vercel 部署Qexo，直接采用官方文档的教程即可：[https://oplog.cn/qexo/start/build.html#vercel-%E9%83%A8%E7%BD%B2-postgresql-vercel](https://oplog.cn/qexo/start/build.html#vercel-%E9%83%A8%E7%BD%B2-postgresql-vercel)

部署完成后，访问提供的域名进行初始化配置，用户配置自行填写即可，博客配置项中配置如下：

* **服务商：** 这里填写`github`即可

* **使用配置：** 这里选择 `hugo` 

* **github 密钥：** 这个密钥去 Github 右上角个人账号下 `Settings` -> `Developer Settings` -> `Personal access tokens (classic)` 下创建一个新的 token，将这个 token 填入此处即可

* **github 仓库：** 这里填写仓库名，`username/username.github.io` 这里 `username` 换成你的 github 用户名

* **项目分支：** 这里填写 `gh-pages` 即可

* **博客路径：** 这里留空即可

Vercel 配置：

这里的配置根据官方文档即可：[https://oplog.cn/qexo/configs/vercel.html](https://oplog.cn/qexo/configs/vercel.html)

填写完成后，如果没问题将进入 Qexo 界面，所有的文章和页面等信息都将显示出来：

![1753946519134.png](https://lihe.ink/2025/07/31/688b199ac830c.png)

最后，你就可以在 Qexo 进行文章和页面的编辑和发布了，发布之后，会自动触发 Actions，完成后博客页面就会有相应的变更了，博客主题配置文件也可以更改，保存后同样会触发更新，这样就不用每次都从本地更改后推送到 Github 进行更新了。

#### 结语

这些免费的服务给我们提供了大大的方便，同时免费的也许是最贵的，免费的东西往往需要我们花费更多的精力去维护，这样一个免费的博客很炫酷，但是可能访问的速度慢一些，不过可以通过自定义域名来加速访问，对于喜欢折腾的人来说，这很好！