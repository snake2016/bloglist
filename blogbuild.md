##  5分钟 搭建免费个人博客

[转自简书](http://www.jianshu.com/p/4eaddcbe4d12?utm_campaign=hugo&utm_medium=reader_share&utm_content=note&utm_source=qq)

##  step1: 创建Github 域名和空间
注册一个Github账号，创建仓库，域名将会是 username.github.io 。Respository name 中的username.github.io 的username 一定与前面的Owner 一致

##  step2: 安装
#### 1、安装Git
```
 // 如果已安装HomeBrew 无需执行此行
 $ /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"

$ brew install git   // 安装Git
```

#### 2、安装Nodejs
先安装nvm，可以轻松切换Nodejs版本。两种方式安装。如果使用curl的方式安装，安装完成之后一定要重启终端。
```
 1. Homebrew 安装方式，此安装方式无需重启
 $ brew install nvm  
 $ mkdir ~/.nvm
 $ export NVM_DIR=~/.nvm
 $ . $(brew --prefix nvm)/nvm.sh

 2. curl安装方式
 $ curl https://raw.github.com/creationix/nvm/master/install.sh | sh
```
安装完成后，重启终端 并执行下列命令即可安装 Node.js。
```
 $ nvm install 4
```
#### 3、安装Hexo
```
 $ sudo npm install hexo-cli -g
```

 ## step3：编写，发布
接下来我们需要用Hexo初始化一个博客，然后更改一些自定义的配置，或者加上自己喜欢的主题，写上第一篇文章，然后发布到自己的个人Github网站(username.github.io)。

####  1、创建博客
将下面的 username 替换成你自己的username(其实也无所谓，作者强迫症)，执行成功后，会创建出一个名为 username.github.io 的文件夹。
```
$ hexo init username.github.io
```
####  2、更改配置
**主题安装**
为了使博客不太难看，我们需要安装一个主题，切换至刚刚生成的Hexo 目录，安装主题
```
$ cd username.github.io
$ git clone https://github.com/iissnan/hexo-theme-next themes/next
```
这里选了一个极简的主题，也是Hexo众多主题中最受欢迎的一个。上面出现的喵神的主题 在[这里](https://github.com/monniya/hexo-theme-new-vno)。Hexo也有[更多主题](https://hexo.io/themes/)供你选择。

**基础配置**：打开文件位置username.github.io/`_config.yml`修改几个键值对，下面把几个必须设置的列出来按需求修改，记得保存， 还有注意配置的键值之间一定要有**空格**。更多设置...
```
 title: dimsky 的 9 维空间    //你博客的名字
 author: dimsky  //你的名字
 language: zh-Hans    //语言 中文
 theme: next   //刚刚安装的主题名称
 deploy:
   type: git    //使用Git 发布
   repo: https://github.com/username/username.github.io.git    // 刚创建的Github仓库
```
**主题配置**：
主题配置文件在username.github.io/themes/next/`_config.yml`中修改，这里略过。[设置详情](http://theme-next.iissnan.com/getting-started.html#theme-settings)

####  3、写文章
所有基础框架都已经创建完成，接下来可以开始写你的第一篇博客了
在username.github.io/source/_posts下创建你的第一个博客吧，例如，创建一个名为FirstNight.md的文件，用Markdown大肆发挥吧，注意保存。

####  4、测试
```
$ hexo s
```
测试服务启动，你可以在浏览器中输入https://localhost:4000 访问了。

####  5、安装hexo-deployer-git自动部署发布工具
```
 $ npm install hexo-deployer-git --save
```
####  6、发布
测试没问题后，我们就生成静态网页文件发布至我们的Github pages 中。
```
$ hexo clean && hexo g && hexo d
```
如果这是你的第一次，终端会让你输入Github 的邮箱和密码，正确输入后，骚等片刻，就会把你的博客上传至Github 了。以后在每次把博客写完后，执行一下这个命令就可以直接发布了，灰常苏胡。

####  7、5分钟应该快到了
是不是很快，恭喜你能走到这一步，你的博客已经完成了，在浏览器中输入 http://dimsky.github.io 就能够访问了。

### 后续增加文章
1. 找到对应目录  cd  folder
2. hexo init  :初始化
3. hexo s ：测试 https://localhost:4000 查看并修改
4. $ hexo clean && hexo g && hexo d ：发布
