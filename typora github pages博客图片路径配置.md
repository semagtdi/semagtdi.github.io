github博客图片路径配置

* * *


通常我们在使用markdown时，插入图片会使用相对位置，这在本地和github repository中浏览时都行得通，但是一到博客中就不行了，这一度让我很苦恼，今天花了点时间研究一下，总算解决了问题。不想看踩坑过程可以直接看4。

## 1 寻找问题

首先要弄清楚为什么会出现这个问题呢。现在假设我们的.md文件和图片放置的位置如下：

> C:\\HUSTHuangKai.github.io\\\_posts\\2019-11-23-github博客图片路径配置.md
> 
> C:\\HUSTHuangKai.github.io\\\_posts\\2019-11-23-github博客图片路径配置.assert\\test.png

那么插入图片的相对路径就是这样写

> ”./2019-11-23-github博客图片路径配置.assert/test.png”

”./”表示当前文件所在目录，这样写在本地和github repository中都是没问题的，但是到博客中就会出问题，为什么呢？

我们在博客中访问一下这篇文章，会发现url变成了如下：

> https://husthuangkai.github.io/2019/11/23/github博客图片路径配置问题/

按照现在这个路径，相对路径就变成了：

> https://husthuangkai.github.io/2019/11/23/github博客图片路径配置问题/2019-11-23-github博客图片路径配置.assert/test.png

没错！就是这样，它的”./“表示当前页面的路径。再加上jekyll会把“2019-11-23”自动解析成”2019/11/23/“，最后就变成了这个鬼路径。那我们的图片在哪呢？理论上应该在这个位置：

> https://husthuangkai.github.io/\_post/github博客图片路径配置问题.assert/test.png

## 2 解决问题

1.  解决jekyll会自动改url的问题。
    
    在_config.yml文件中有一行这样的配置：
    
    > permalink: /:year/:month/:day/:title/
    
    这就是罪魁祸首啦，只要把它改成这个亚子：
    
    > permalink: /:year-:month-:day-:title/
    
    上面的url就会解析成：
    
    > https://husthuangkai.github.io/2019-11-23-github博客图片路径配置问题/
    
    这样看起来舒服多了，现在图片的相对路径会解析成
    
    > https://husthuangkai.github.io/2019-11-23-github博客图片路径配置问题/2019-11-23-github博客图片路径配置.assert/test.png
    
    和我们想要的路径还有差距。怎么办呢？
    
2.  修改相对路径的写法
    
    把相对路径写成这样：
    
    > ”../\_posts/2019-11-23-github博客图片路径配置.assert/test.png”
    
    这样在本地是可以找到的，在浏览器中路径会解析为：
    
    > https://husthuangkai.github.io/\_posts/2019-11-23-github博客图片路径配置.assert/test.png
    
    好像就是我们要的亚子。
    
    但是，对不起，咱又一次踩坑了。经过jekyll解析之后**\_posts这个文件夹在url中已经访问不到了**。怎么办呢？
    
3.  修改图片存放的位置。
    
    在_posts的同级目录下，也就是husthuangkai.github.io这个文件夹中新建一个文件夹images（**注意：不要命名为”\_images”，以”\_“开头又是一个坑，不要问我怎么知道的**），把图片放在这里。相对路径改为：
    
    > ”../images/2019-11-23-github博客图片路径配置.assert/test.png”
    
    这样在本地和博客中都可以访问到了。
    

## 3 Typora图片设置

使用Typora编辑器的童鞋可以看一下，在Typora中点 文件——偏好设置——图像，设置成如下：

<img width="795" height="363" src="resources/fc456cdb4b1848f4b426647d30b46fb8.png"/>

这样在写markdown时截图直接粘贴到文档中，它就会自动在将图片放到images文件夹下一个名为filename.assets的文件夹中啦。比如上面那张图片的路径就会自动生成为：

> ../images/2019-11-23-github%E5%8D%9A%E5%AE%A2%E5%9B%BE%E7%89%87%E8%B7%AF%E5%BE%84%E9%85%8D%E7%BD%AE%E9%97%AE%E9%A2%98.assets/image-20191123194447055.png

**爽吧！！！**

## 4 总结

步骤如下：

1.  修改_config.yml文件中的
    
    permalink: /:year/:month/:day/:title/
    
    为
    
    permalink: /:year-:month-:day-:title/
    
2.  将.md文件都放到
    
    xxxxxxx.github.io\\\_posts\\
    
    注意，一定是这个路径，如果在_posts目录下再建一级目录就不行了。
    
3.  将图片到放到
    
    xxxxxxx.github.io\\images\\
    
    注意，images命名可以修改，但是不要以”\_“开头。
    
4.  相对路径写为如下形式
    
    ../images/xxx.png
    
5.  使用typora的同学请看第3节。
    

* * *