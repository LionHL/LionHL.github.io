---
title: Hexo博客部署在GitHub上
date: 2016-10-01 12:50:02
tags: [前端,Hexo,GitHub]
---

## 在GitHub上的操作
### 新建一个Repository
在 Repository name 下填写 yourname.github.io
我的GitHub账号是LionHL，那么我的Repository name就是 LionHL.github.io
<img src='/./images/img01.png' width='600' height='500' style='border: 1px solid #ccc;margin: 10px 0;'/>

## 在笔记本上操作
### 安装Hexo
使用npm命令安装即可

```npm
  npm install -g hexo
```

### 创建 Hexo 文件夹
在你本地的Hexo文件夹下操作，安装Hexo。Hexo 即会自动在目标文件夹建立网站所需要的所有文件

```npm
  hexo init
```

### 安装依赖包

```npm
  npm install
```

### 本地查看
完成以上步骤，你就可以查看本地的hexo了。执行 `hexo generate`  `hexo server` ，然后在浏览器中打开localhost:4000。

```npm
  hexo generate
  hexo server
```

### 为 Hexo 安装 Git 插件
安装 `hexo-deployer-git` 。否则当你把本地的部署到github上时会报 `ERROR Deployer not found: git ` 的错误，如图。

<img src='/./images/img02.png' width='300' height='40' style='border: 1px solid #ccc;margin: 10px 0;'/>
```npm 
  npm install hexo-deployer-git
```

### 修改你的 _config.yml 
找到根目录下的 _config.yml 文件，修改你的 _config.yml 如下：

<img src='/./images/img03.png' width='500' height='200'  style='border: 1px solid #ccc;margin: 10px 0;'/>
```npm
  # Deployment
  ## Docs: https://hexo.io/docs/deployment.html
  deploy:
    type: git
    repo: git@github.com:LionHL/LionHL.github.io.git
    branch: master
```
注意这里选择的是 ssh 地址   
   
### 生成静态文件并部署到 GitHub 上
执行以下命令

```npm 
  hexo g
  hexo d
```
完成以上步骤，我们就搭建好自己的博客并部署到 GitHub 上了，我们可以在浏览器里打开 LionHL.github.io 查看自己的博客。第一次访问的时候可能访问不了，你可以过几分钟再进行访问。


### Hexo 常见操作命令
hexo new "postName" 新建文章   
hexo new page "pageName" 新建页面   
hexo generate 生成静态页面至public目录   
hexo server 开启预览访问端口（默认端口4000，'ctrl + c'关闭server）   
hexo deploy 将.deploy目录部署到GitHub   
hexo help   查看帮助   
hexo version  查看Hexo的版本


### 关于 themes
Hexo 提供了很多 theme 供你选择，在这我就不多做介绍了，自己可以去搜索。   
我目前用的是 <a href='https://github.com/litten/hexo-theme-yilia' target='_blank'>hexo-theme-yilia</a>   
简单介绍一下我安装的步骤吧。   
在本地的 hexo 文件夹下执行

```npm 
  git clone git@github.com:litten/hexo-theme-yilia.git themes/yalia
```
完成后你可以在本地themes下看见 yilia(自己定其他的名字) 主题包。然后在 _config.yml 文件里主题修改为 `theme: yilia`
#### 修改头像
打开 themes/yalia/_config.yml 文件，在 `#你的头像url  avatar: `后面添加一个url就行了。   
（一些其他的设置也在这个文件里面修改，具体的可以自己查看）  
在执行部署提交 `hexo g` `hexo d`
   
<br><br>
## 关于管理
在完成以上步骤后，就完成了Hexo博客部署在GitHub上的操作了，这时会发现一个问题，如果我们更换了电脑，怎么更新管理博客呢？难道要将以上的所有步骤在新的电脑上再执行一遍？
不用这么麻烦，其实，Hexo生成的文件里面有.gitignore，它的本意应该也是放在GitHub上的。
所以我们可以考虑使用分支，创建一个分支hexo，来存放blog的原始文件。

### 流程
1、新建一个Repository， LionHL.github.io
2、创建两个分支，master和hexo，并将hexo设置为默认分支（因为我们要管理这个Hexo网站文件）
3、克隆仓库 git clone git@github.com:LionHL/LionHL.github.io.git
4、在本地文件夹下执行安装Hexo操作（如上在笔记本上操作，在这里我就不重复了，这个时候当前显示的分支应该是hexo）
5、修改生成的.gitignore文件，并上传github。（使用git add . ,git commit -m '...', git push origin hexo 等操作）
6、生成静态文件并部署到 GitHub 上（hexo g, hexo d）

这样我们就算资料丢失或者更换电脑也不用害怕了，直接克隆仓库就可以了。
注意，在本地拷贝LionHL.github.io.git之后，执行安装Hexo操作时不需要`hexo init`这个命令了。