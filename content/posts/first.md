---
title: "初始博客"
date: 2024-05-28T00:22:02+08:00
draft: false
---

## 测试
ps:在最优化的课堂上摸鱼终于搭建好网站记录一下第一个blog,后续更新网站搭建的步骤  
考虑到这个主题实在还是太有限制，大概率以后就当做一个过渡图册的记录了
暂时只设置一些日常的记录和图片的记录  
##
##
## hugo博客搭建记录
### 0.前置准备（github上部署）
git包管理，hugo预编译文件， vscode(推荐)    
hugo: https://github.com/gohugoio/hugo/releases
ps: 这三者都需要环境变量的配置    
说明：对于Windows系统，在后续启动Hugo服务器时有报错，找到解决办法是下载扩展版（应该是有些Hugo主题在基础版上不支持），所以建议下载扩展版。即带有extended的版本，尤其是你需要一些看上去比较高级的主题时候。
如果遇到scss类的报错，毫无疑问，更换版本吧      
###  
执行
```cmd
hugo version
```
如果正常显示版本，那么hugo就算安装完毕 
###   
##
### 1.Quick_Start
随后是新建一个文件夹目录，然后开始我们的工作喵
``` cmd
hugo new site your_site_name
```
进入我们刚刚建立好的博客地址里面喵
```cmd
cd your_site_name

```
第一行：把当前目录进行初始化    
第二行：下载anatole主题，并存放在themes文件夹中    
第三行：把主题改为anatole(也可以直接进入config.toml里面结尾添上你需要的主题theme = "anatole" )
```cmd
git init  
# 如果有需要，你可以去皮肤站来获取自己想要的主题
git submodule add https://github.com/lxndrblz/anatole.git themes/anatole  
echo theme = "anatole" >> config.toml
```
接下来写一篇博客看看是否可行
```cmd
# 创建一个life的主页，并写一份first_day的博客
hugo new life/first_day.md
# 运行下博客看是否可行
hugo server -D
```
这个时候如果出现了一下错误，请返回第0步，更换你的hugo版本，选择extend版本
```Error
Error building site: TOCSS: failed to transform "scss/main.scss" (text/x-scss): resource "scss/scss/main.scss_de1a7f5f1c8c46959803c429bb697ff0" not found in file cache
```
不出意外的话就可以访问cmd输出的一个本地的地址了     
生成静态文件，准备进一步托管
```cmd
hugo -D
```
###
##
### 2. 部署到自己的github上
首先是准备一个自己的远程仓库（网上有很多教程）    
然后开始git提交远程仓库连击
```cmd
# 在命令行中进行git 初始化
git init
# 检查是否有改变
git status
# 提交到暂存区
git add .
# 提交到版本库
git commit -m "msg"   
# 创建分支
git branch -M main
git remote add origin https://.....git # 这边改成你的远程仓库地址
# 推送到远程仓库
git push -u origin main
```
#### github pages 部署设置

新建一个文件，在pingfan-blog目录下，名称为.github,然后在.github文件夹下新建一个文件夹workflows，在workflows文件夹下新建一个文件叫gh-pages.yml    

总的路径为pingfan-blog/.github/workflows/gh-pages.yml    

在gh-pages.yml输入以下内容后保存。    
```yml
name: github pages

on:
  push:
    branches:
      - main  # Set a branch to deploy
  pull_request:

jobs:
  deploy:
    runs-on: ubuntu-20.04
    permissions: write-all
    steps:
      - uses: actions/checkout@v2
        with:
          submodules: true  # Fetch Hugo themes (true OR recursive)
          fetch-depth: 0    # Fetch all history for .GitInfo and .Lastmod

      - name: Setup Hugo
        uses: peaceiris/actions-hugo@v2
        with:
          hugo-version: 'latest'
          # extended: true

      - name: Build
        run: hugo --minify

      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3
        if: github.ref == 'refs/heads/main'
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./public
```
注意：如果后面部署过程报错大概率是来自这个文件的问题，这边附上一些参考链接（该文件本人已经修改过，应该问题不大）    
###
Github_Actions入门教程：    
https://www.ruanyifeng.com/blog/2019/09/getting-started-with-github-actions.html    
###

然后再走一遍git提交四连：
```cmd
git status 
git add . 
git commit -m "add yml file" 
git push
```
找到刚刚的github仓库，点击Actions，就可以看到我们的网站部署成功了    
如果没有成功，需要进一步修改 Settings->Actions->General->Workflow permissions 然后勾上第一条Read and write permissions点击Save即可    
###
最后一步在settings->Pages修改branch为gh-pages（这几步没有图文就直接去知乎看吧，结尾给出参考链接）    最后刷新几次界面，会出现一个visit site选项。复制你的网址，回到工作目录的config.toml，替换掉baseURL的默认参数就可以了
最后再走一遍git提交四连：（以后要修改也通过这个方法）
```cmd
git status 
git add . 
git commit -m "add yml file" 
git push
```
###
##
### 3. 丰富网页
这一步其实没什么特别要说的，通过选取的theme，查看对应的说明文档。然后修改config.toml即可    
然后遇到的第二个问题就是图片上传的事情。网上大多数教程是放置在status目录下，然后会转移到public里面。而hugo引用的是public下的内容。不过本人尝试过content和status各种路子。最后还是选择使用图床，图床才是真神（确信）
#
##
## 参考链接：
####
https://www.gohugo.org/    
https://zhuanlan.zhihu.com/p/558804132    
https://blog.csdn.net/qq_38250687/article/details/118488688    
https://www.ruanyifeng.com/blog/2019/09/getting-started-with-github-actions.html
