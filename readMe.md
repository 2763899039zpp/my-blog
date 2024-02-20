Welcome to [Hexo](https://hexo.io/)! This is your very first post. Check [documentation](https://hexo.io/docs/) for more info. If you get any problems when using Hexo, you can find the answer in [troubleshooting](https://hexo.io/docs/troubleshooting.html) or you can ask me on [GitHub](https://github.com/hexojs/hexo/issues).

## Quick Start

### Create a new post 创建一个文章

```bash
$ hexo new "My New Post"
```

More info: [Writing](https://hexo.io/docs/writing.html)

### Run server

```bash
$ hexo server
```

More info: [Server](https://hexo.io/docs/server.html)

### Generate static files

```bash
$ hexo generate
```

More info: [Generating](https://hexo.io/docs/generating.html)

### Deploy to remote sites

```bash
$ hexo deploy
```

More info: [Deployment](https://hexo.io/docs/one-command-deployment.html)

## 如何生成文章

```
hexo clean   //清理
hexo generate
hexo deploy
```

输入 hexo g (完整命令为 hexo generate)，用于生成静态文件；
然后输入 hexo s(完整命令为 hexo server)，用于启动服务器，主要用来本地预览；完成后 打开浏览器输入 http://localhost:4000，会发现多了你刚写的那篇博客;
最后输入 hexo d(hexo deploy)，用于将本地文件发布到 github 等 git 仓库上；
