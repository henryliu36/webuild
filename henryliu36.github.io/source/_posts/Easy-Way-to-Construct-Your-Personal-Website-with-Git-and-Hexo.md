---
title: postA
date: 2018-07-28 23:45:13
tags:
---
---
title: How to construct a personal website with Git and Hexo
date: 2018-07-28 19:31:41
categories:
- Learning experience
tags:
```
#博客编辑完成之后在中端执行部署命令：

#hexod-g
```
---
# How to construct a personal website with Git and Hexo

本帖是Henry作为小白上路的学习构建博客以及其他网络工具的第一篇帖子

其中的内容便是本博客的由来。虽然按照了多个教程一步一步跟进，但是仍然遇到了一些大坑，这里会着重说明新手如我可能遇到的一些问题。

开门见山，搭建步骤如下
======
1. 注册Github，了解建repository的流程
>* Hexo也是Github的开源内容
>* 本方法利用了Github的免费空间，使得用户暂时不用去担心构建服务器的问题。
>* Github构建网页的域名统一为Github.io，但是用户可以后续通过购买域名以及新建CNAME方法更改为你在阿里云或者GoDaddy所购买的域名
2. 下载必备工具Git
3. 下载必备工Node.js
4. 本地测试所有程序成功安装
5. 通过npm下载Hexo
> Hexo是一款基于Node.js的静态博客框架，通过Hexo便可以实现类似Wordpress的功能实现一键以及后续的修修补补实现一个看得过去的网页
6. 本地测试网页
7. 通过在Github中添加秘钥SSH，使本地主机可以被Github所访问
8. 网络测试
9. 后续优化



---------------
**Let us begin from the literal basis!**

## 1.注册
和其他论坛一样，以你的邮箱来申请Github的账号，希望在此之前你已经学习了Github贴心的easy tutorial。这之后请你建立一个以自己的未来网址为名的仓库。
> 比如我的是```Henryliu36.github.io```
>> **这里是第一个大坑，你之后所建立的本地建站文件夹要和这个一模一样。**不要个性化的随意建立一个文件夹和你的repo（repository)名字 e.g. ```Henryliublog.github.io. ```

## 2. Git installation
很简单，进入官网后点Download，网页会自动识别你的电脑版本以适配你的下载。
之后有一个exe文件叫做Git Bash相当于Git自带的命令行文件，之后我们的指令可以全部在其中实现。
Git Bash 和window的cmd shell不同之处在于
* 没有粘贴功能，你输入的ctrl+V 会显示为^v, 十分的尴尬，只能一个一个手打进去要copy的东西。但是你可以复制git bash中的东西到其他地方。
* 其中的密码不会显示，比如在设置SSH的密码时候，不要误认为你没有输入东西。
* Git Bash 可以直接右击文件夹而产生的菜单中直接选择，代表你操作的Git Bash的路径。（即cd，**我们所有的建站操作必须在这个路径里，此为第二个大坑**）

## 3. Node.js installation
[Node.Js下载地址](https://nodejs.org/en/download/)，没什么好说的, 一路Next，注意这里会包括安装工具npm（后续会用到这个命令）以及环境变量的安装。


## 4. Installation Examination
在Window命令行或者Git bash中
``` node -v
``` git -v
``` git --version
```
这些命令可以检查你的软件的版本信息，若全部正常显示可以进行下一步操作


## 5. Hexo installation
在安装之前，你需要自己在电脑常里创建一个文件夹，命名yourusername.github.io之后的所有操作必须在此路径完成。Hexo框架与以后你自己发布的网页都在这个文件夹中，因此务必好好管理。创建好后，如之前所述，右键——菜单——Git Bash.
```
npm install -g hexo-cli   #or#   npm install -g hexo
```
## 6. SSH setting
1. 进入Git Bash设置user.name和user.email配置信息：
```
git config --global user.name +"你的GitHub用户名"（无需引号）
git config --global user.email +"你的GitHub注册邮箱"
```
2. 生成ssh密钥文件：
```
ssh-keygen -t rsa -C "你的GitHub注册邮箱"  （这里应该是在C盘建立ssh文件夹）
```
接下来三次回车确定，密码可设置可不设置，如果回车过去，则是无密码。
找到生成的.ssh的文件夹中的id_rsa.pub密钥，将内容全部复制（notepad等text程序打开）

3. 进入Github setting项，新建new SSH Key
* Title无所谓，输入刚才复制的SSH内容生成一个token
> 其后每次hexo clean   hexo g   hexo d 的时候都要输入ssh密码
* 在Git Bash中检测GitHub公钥设置是否成功：

```
$ ssh -T git@github.com  （不要修改任何一个字）
```
得到
```
The authenticity of host 'GitHub.com (207.97.227.239)' can't be established.
RSA key fingerprint is 16:27:ac:a5:76:28:2d:36:63:1b:56:4d:eb:df:a6:48.
Are you sure you want to continue connecting (yes/no)
```
回车
```
Enter passphrase for key '/c/Users/lenovo/.ssh/id_rsa':
```
输入密码完成测试



> 这里之所以设置GitHub密钥原因是，通过非对称加密的公钥与私钥来完成加密，公钥放置在GitHub上，私钥放置在自己的电脑里。GitHub要求每次推送代码都是合法用户，所以每次推送都需要输入账号密码验证推送用户是否是合法用户，为了省去每次输入密码的步骤，采用了ssh，当你推送的时候，git就会匹配你的私钥跟GitHub上面的公钥是否是配对的，若是匹配就认为你是合法用户，则允许推送。这样可以保证每次的推送都是正确合法的。


4. 在设置之前，我们需要知道在本地根目录里的_config.yml文件称为站点配置文件，里面包含了你的网页的基本设置。

本地根目录中有一个yml文件，可以修改主题, 在最末的地方
```
deploy:
  type: git
  repo: 这里填入你之前在GitHub上创建仓库的SSH完整路径，在你的repo那里可以copy，注意不要选择HTML格式
  branch: master参考如下：
```




## 6. Local test

1. 然后初始化博客文件
```
hexo init                #其中大部分文件都是这一步产生的
```
2. 安装依赖包dependencies
```
$ npm install
```
3. 确保部署,保存站点配置文件。

> 其实就是给hexo d 这个命令做相应的配置，让hexo知道你要把blog部署在哪个位置，很显然，我们部署在我们GitHub的仓库里。最后安装Git部署插件，输入命令：
```
npm install hexo-deployer-git --save
```
4.为了检验雏形，可以在本地打开
```
hexo g #hexo g 每次进行相应改动都要hexo g 生成一下
hexo s #启动服务预览
```

如果可以hexo s显示

那就代表本地测试成功，可以在浏览器打出localhost:4000
<br>下一步是如何同步到Github上。
Ctrl+C取消本地模拟


> 下面是一些其他npm和Hexo常见指令
```
3.0.0版本执行npm install hexo-cli -g     之后执行    npm install hexo -g       #安装Hexo
npm update hexo -g #升级
3.0.0版本执行npm uninstall hexo-cli -g   npm uninstall hexo -g
hexo init #初始化博客
```

```
#生成网页的操作

hexo n "title" == hexo new "title" #新建文章（title后面自己在source/_post中自己修改也可以）
#每次更新网页都要进行这些操作
hexo clean #清除缓存，若是网页正常情况下可以忽略这条命令
hexo g == hexo generate #生成
hexo s == hexo server #启动服务预览
hexo d == hexo deploy #部署
```
**大坑3， hexo c不是hexo clean**
```
#在本地预览
hexo server #Hexo会监视文件变动并自动更新，无须重启服务器
hexo server -s #静态模式
hexo server -p 5000 #更改端口
hexo server -i 192.168.1.1 #自定义 IP
hexo clean #清除缓存，若是网页正常情况下可以忽略这条命令
```




## 8. Test on Internet

```
hexo clean
hexo g
hexo d
```
###此时不出意外的话你可以在浏览器输入yourusername.github.io来看到你的网页了
###每次更新网页都要进行这些操作
----------------


## 9.Theme Change

我们也可以通过下载其他主题来丰富化我们的网页，通过添加各种有趣，便捷的元素来实现这一目的。在theme的文件夹下面也包括一个yml文件，我们可以细化我们的各种设置。

1. 找主题
<br>Hexo的Default模板是Landscape，明显无法满足各位少年的需求。因此我们可以去搜罗各种风骚的themes去玩转自己的网页。

    [知乎：有哪些好看的 Hexo 主题？](https://www.zhihu.com/question/24422335/answer/46357100)

2. 找到它所在的 Github Repository

3. 找到之后通过git命令下载

4. 在主题的repository点击clone 复制一下那个地址
5. git bash中
```
$ git clone +复制的地址+themes/next(以next为例，会下载到themes文件夹中的next主题中)
```
6. 启用主题去根目录下的yml中文件的Extension那里把原主题名Landscape改成next
> 可以自己添加plugin： 来添加新的插件
7. git bash中执行主题替换
```
$ cd themes/主题名
$ git pull
```

8. hexo s查看并调试
```
$ hexo g #生成
$ hexo s #启动本地服务，进行文章预览调试，退出服务用Ctrl+c
```


## 9. Optimisition


-------------

hexo -g便是生成各种文件包括CSS样式，index.html等文件的操作
添加新文章的时候，只能先通过命令行添加，再修改。不可以直接在文件夹添加。









Henry (Hangrui Liu)
Ph.D Candidate at Macquarie University
Email: hrliu36@gmail.com
I look forward to talking with you.
WeCHat: ![WeChat](https://i.loli.net/2018/07/28/5b5c620308f08.jpg)
