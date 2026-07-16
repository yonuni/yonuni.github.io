+++
date = '2026-07-16T17:11:30+08:00'
draft = false
title = 'Hugo部署'
+++
hugo部署流程
一、前置说明
1.1 整体架构
本方案采用「本地写作 + GitHub 云端自动构建部署」的模式，整体流程如下：

Windows 本地
（写文章 + hugo server 预览）	→	GitHub 仓库
（git push 源码）	→	GitHub Pages
（自动构建 + HTTPS 发布）

1.2 需要安装的软件
软件	用途	下载地址
Git	版本管理 + 推送代码到 GitHub	https://git-scm.com/download/win
Hugo Extended	静态站点生成器（必须 Extended 版）	https://github.com/gohugoio/hugo/releases
（可选）VS Code	编辑 Markdown 文章和配置文件	https://code.visualstudio.com/

⚠️ baseURL 末尾必须带 /，否则 CSS 和 JS 会 404，页面样式全乱。
✅ 写作和本地预览全程离线，只有 git push 和首次拉主题时需要联网。


二、安装 Git
2.1 下载与安装
1.打开浏览器，访问 https://git-scm.com/download/win
2.下载 64-bit Git for Windows Setup（约 50MB）
3.双击安装包，一路点击 Next，全部保持默认选项即可
4.安装完成后，在任意文件夹右键，能看到「Git Bash Here」即表示安装成功

2.2 验证安装
打开命令提示符（CMD）或 PowerShell，输入：
git --version
如果看到类似 git version 2.xx.x 的输出，说明安装成功。

2.3 配置 Git 用户信息
Git 提交时需要知道你是谁，执行以下两条命令（替换为你的信息）：
git config --global user.name "你的用户名"
git config --global user.email "你的邮箱@example.com"
💡 用户名和邮箱不一定要和 GitHub 一致，但建议保持一致方便管理。


三、安装 Hugo（Extended 版）
3.1 为什么必须用 Extended 版
Hugo 有两个版本：Standard 和 Extended。大多数主流主题（PaperMod、Stack、Even 等）都使用 SCSS 样式预处理，只有 Extended 版才包含 SCSS 编译器。装了 Standard 版在构建时会报类似「SCSS not supported」的错误。

3.2 下载
5.打开浏览器，访问 https://github.com/gohugoio/hugo/releases
6.找到最新版本的 Release，往下翻找到 Assets 区域
7.下载文件名类似 hugo_extended_0.xx.x_windows-amd64.zip 的文件（注意带 extended 字样）
8.解压到 C:\Hugo 目录（没有就新建）

3.3 配置环境变量 Path
把 Hugo 所在目录加入系统 Path，这样在任意位置都能直接敲 hugo 命令。
9.右键「此电脑」→ 属性 → 高级系统设置 → 环境变量
10.在「系统变量」区域找到 Path → 选中 → 编辑
11.点击「新建」，输入 C:\Hugo（即 hugo.exe 所在目录）
12.一路确定保存
13.关闭所有已打开的 CMD/PowerShell 窗口（重要！环境变量不会自动刷新）

3.4 验证安装
重新打开 CMD 或 PowerShell，输入：
hugo version
正确输出示例：
hugo v0.147.8+extended windows/amd64 BuildDate=...
✅ 看到 +extended 字样就说明装对了。如果没有，检查 Path 配置或重新打开终端。


四、创建 Hugo 站点
4.1 新建站点
选一个你喜欢的目录放博客项目，比如 D:\MyBlogs。在 CMD 中执行：
cd D:\MyBlogs
hugo new site myblog
cd myblog
执行后 myblog 文件夹内会生成以下结构：
myblog/
├── content/          # 文章存放目录
├── static/           # 图片等静态资源
├── themes/           # 主题目录
├── hugo.toml         # 站点配置文件（重点）
├── archetypes/       # 文章模板
├── layouts/          # 自定义布局（高级）
└── data/             # 数据文件（高级）

4.2 安装主题（PaperMod）
PaperMod 是目前最流行的 Hugo 主题之一：轻量、快速、支持中文、文档完善。
方法 A：Git Submodule（推荐）
在 myblog 目录下执行：
git init
git submodule add https://github.com/adityatelange/hugo-PaperMod.git themes/PaperMod
💡 submodule 方式方便日后更新主题，执行 git submodule update --remote 即可。
方法 B：下载 ZIP（网络不好时用）
14.访问 https://github.com/adityatelange/hugo-PaperMod
15.点击绿色 Code 按钮 → Download ZIP
16.解压后将文件夹重命名为 PaperMod，放入 myblog/themes/ 目录

4.3 修改 hugo.toml 基础配置
用记事本或 VS Code 打开 myblog/hugo.toml，替换为以下内容：
baseURL = "https://你的用户名.github.io/"
languageCode = "zh-cn"
title = "我的博客"
theme = "PaperMod"

[params]
  author = "你的名字"
  description = "这是我的个人博客"
  ShowCodeCopyButtons = true
  ShowToc = true
  TocOpen = true
⚠️ baseURL 必须替换成你自己的 GitHub 用户名对应的 Pages 地址，末尾的 / 不能省略。


五、写第一篇文章
5.1 创建文章
在 myblog 目录下执行：
hugo new posts/hello-world.md
这会在 content/posts/ 下生成 hello-world.md 文件。

5.2 编辑文章
用记事本或 VS Code 打开 content/posts/hello-world.md：
---
title: "Hello World"
date: 2025-07-15T10:00:00+08:00
draft: true          # 改成 false 才会发布
tags: ["入门", "测试"]
categories: ["博客"]
---

这是我的第一篇博客文章！

## 二级标题

正文用 Markdown 格式书写，支持 **加粗**、*斜体*、`代码` 等。
⚠️ draft: true 表示草稿，Hugo 不会把它发布到网站上。写完必须改为 draft: false。

5.3 本地预览
在 myblog 目录下执行：
hugo server -D
看到类似下面的输出说明启动成功：
Web Server is available at http://localhost:1313/ (bind address 127.0.0.1)
Press Ctrl+C to stop
打开浏览器访问 http://localhost:1313 即可看到你的博客。修改文章后页面会自动刷新。按 Ctrl+C 停止服务器。


六、创建 GitHub 仓库
6.1 注册 GitHub 账号
•访问 https://github.com 注册账号（记住用户名，后面要用）
•验证邮箱

6.2 创建仓库
17.登录 GitHub，点击右上角 + 号 → New repository
18.Repository name 填写：你的用户名.github.io（必须完全一致）
19.设为 Public（私有仓库 GitHub Pages 需要付费）
20.不要勾选任何初始化选项（README、.gitignore、license 都不要勾）
21.点击 Create repository

💡 示例：用户名为 zhangsan，则仓库名为 zhangsan.github.io，最终访问地址为 https://zhangsan.github.io

6.3 配置 Git 远程仓库地址
回到 CMD，在 myblog 目录下执行（替换用户名）：
git add .
git commit -m "初始提交"
git remote add origin https://github.com/你的用户名/你的用户名.github.io.git
git push -u origin main

6.4 关于认证（PAT Token）
2021 年起 GitHub 不再支持密码认证，push 时需要用 Personal Access Token（PAT）代替密码。
22.打开 GitHub → Settings → Developer settings → Personal access tokens → Tokens (classic)
23.点击 Generate new token (classic)
24.勾选 repo 权限（全选 repo 下的子项）
25.设置过期时间（建议 90 天或 No expiration）
26.点击 Generate token，复制生成的 token（只显示一次！）
27.push 时用户名填 GitHub 用户名，密码粘贴这个 token
✅ 推荐配置 SSH 密钥对，以后 push 再也不用输密码。详见第九章。


七、配置 GitHub Actions 自动部署
7.1 开启 GitHub Pages
28.进入仓库页面 → Settings → Pages
29.Source 选择 GitHub Actions（不要选 Deploy from a branch）
30.无需保存，切换即生效

7.2 创建工作流文件
在 myblog 项目根目录下，新建文件夹和文件：
.github/workflows/hugo-deploy.yml
用记事本或 VS Code 打开 hugo-deploy.yml，粘贴以下内容：
name: Deploy Hugo to GitHub Pages

on:
  push:
    branches: ["main"]
  workflow_dispatch:

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: pages
  cancel-in-progress: false

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4
        with:
          submodules: recursive
          fetch-depth: 0

      - name: Setup Hugo
        uses: peaceiris/actions-hugo@v3
        with:
          hugo-version: 'latest'
          extended: true

      - name: Build
        run: hugo --minify

      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: ./public

  deploy:
    needs: build
    runs-on: ubuntu-latest
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    steps:
      - name: Deploy
        id: deployment
        uses: actions/deploy-pages@v4

7.3 推送工作流文件
回到 CMD，执行：
git add .github
git commit -m "添加 GitHub Actions 工作流"
git push

7.4 查看部署状态
31.打开 GitHub 仓库页面 → 点击 Actions 标签页
32.看到一条名为 Deploy Hugo to GitHub Pages 的工作流正在运行
33.等 30 秒 ~ 2 分钟，变绿色 ✅ 表示部署成功
34.浏览器访问 https://你的用户名.github.io 即可看到博客

⚠️ 如果 Actions 变红报错，点击进去看日志。最常见错误：submodule 拉不到主题（网络问题）、baseURL 写错。


八、日常写作与更新流程
8.1 写新文章
cd D:\MyBlogs\myblog
hugo new posts/文章标题.md
# 用编辑器打开文件，写内容，draft 改为 false
hugo server -D    # 本地预览

8.2 发布到线上
git add .
git commit -m "新增文章：文章标题"
git push          # 自动触发构建部署
push 后等 1 分钟左右，刷新网站就能看到新文章。

8.3 修改配置后更新
修改 hugo.toml 后流程完全一样：
hugo server -D    # 先本地预览确认没问题
git add hugo.toml
git commit -m "更新配置"
git push

8.4 更新主题
如果用的是 submodule 方式安装的主题：
git submodule update --remote themes/PaperMod


九、配置 SSH 密钥（免密推送）
9.1 生成密钥对
打开 PowerShell 或 Git Bash，输入：
ssh-keygen -t ed25519 -C "你的邮箱@example.com"
35.提示 Enter file in which to save the key → 直接回车（默认路径）
36.提示 Enter passphrase → 直接回车两次（不设密码短语）
37.生成成功后，在 C:\Users\你的用户名\.ssh\ 下有两个文件：
•id_ed25519 → 私钥，永远不要给别人看
•id_ed25519.pub → 公钥，要贴到 GitHub

9.2 复制公钥内容
在 PowerShell 中执行：
Get-Content C:\Users\你的用户名\.ssh\id_ed25519.pub
复制输出的整行内容（以 ssh-ed25519 开头，以你的邮箱结尾）。

9.3 添加到 GitHub
38.登录 GitHub → 右上角头像 → Settings
39.左侧菜单 SSH and GPG keys → New SSH key
40.Title 随便写（如「我的 Windows 电脑」）
41.Key type 选 Authentication Key
42.把刚才复制的公钥内容粘贴到 Key 框中
43.点击 Add SSH key

9.4 验证 + 切换远程地址
# 验证是否通了
ssh -T git@github.com
# 看到 Hi 你的用户名! You've successfully authenticated 就成功

# 切换远程地址为 SSH（以后 push 不再弹密码框）
git remote set-url origin git@github.com:你的用户名/你的用户名.github.io.git

# 确认
git remote -v
✅ 配置 SSH 后，git push 再也不需要输入用户名和密码，直接推送。


十、常见问题排查
问题现象	可能原因	解决方法
页面样式全乱/CSS 404	baseURL 末尾没加 /	hugo.toml 中 baseURL 末尾加上 /
hugo server 白页	theme 名称写错或主题未下载	检查 themes/ 目录和 hugo.toml 的 theme 字段
文章不显示	draft 还是 true	改为 draft: false 后重新构建
Actions 报错红	submodule 拉取失败	检查主题用 submodule 还是 zip，或手动 commit 主题文件
push 被拒/认证失败	GitHub 不支持密码登录	用 PAT Token 或配置 SSH 密钥
线上没更新	push 到非 main 分支 / 缓存	确认推到 main 分支，Ctrl+F5 强刷
hugo version 报命令不存在	Path 环境变量未配置或终端未重启	检查 Path，重启 CMD/PowerShell


十一、进阶配置建议
11.1 自定义域名
•购买域名后，在仓库 Settings → Pages → Custom domain 填入
•在域名服务商处添加 CNAME 记录指向 你的用户名.github.io
•GitHub 会自动签发 HTTPS 证书（Let's Encrypt）

11.2 评论系统
GitHub Pages 是纯静态，无法自己存评论，可选方案：
•Giscus（基于 GitHub Discussions，免费，推荐）
•Utterances（基于 GitHub Issues，免费）
•Disqus（老牌第三方，有广告）

11.3 网站访问统计
•Google Analytics 4（GA4）—— 功能强大，需翻墙看数据
•Umami —— 开源、隐私友好、可自托管
•百度统计 —— 国内访问快，中文界面

11.4 多作者协作
•A 和 B 都 clone 同一个仓库，文章分目录存放（content/a/ 和 content/b/）
•每篇文章 front matter 加 author 字段区分作者
•push 前先 git pull 拉最新，避免覆盖对方内容

11.5 给网站加访问密码
纯静态站没有后端，无法做真正的登录验证，可选方案：
•Cloudflare Access（推荐）：域名接入 CF 后，Zero Trust 里加 Policy，访问前需邮箱/Google 验证
•Staticrypt：AES 加密 HTML，浏览器输密码后 JS 解密，适合私人内容
⚠️ 不要在前端 JS 里写明文密码做伪登录，F12 一看就穿帮。


附录：常用命令速查表
命令	说明
hugo new site 项目名	创建新的 Hugo 站点
hugo new posts/文章名.md	新建一篇文章
hugo server -D	本地启动开发服务器（含草稿）
hugo --minify	构建生产版本到 public/（CI 自动做）
git add .	暂存所有改动
git commit -m "说明"	提交到本地仓库
git push	推送到 GitHub 触发自动部署
git pull	拉取远程最新代码（协作时必做）
git submodule update --remote	更新主题到最新版
ssh -T git@github.com	测试 SSH 连接是否通
