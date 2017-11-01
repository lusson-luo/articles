---
title: 用Hexo搭建个人博客
date: 2017-11-01 17:50:00
tags:
- 网站搭建
categories:
- '运维'
---

## HEXO安装

### 安装

Hexo安装需要git、 nodejs

**进入git或cmd，下载hexo**

`npm install -g hexo`

hexo必备插件，建议安装

```
$ npm install hexo-generator-index --save #索引生成器
$ npm install hexo-generator-archive --save #归档生成器
$ npm install hexo-generator-category --save #分类生成器
$ npm install hexo-generator-tag --save #标签生成器
$ npm install hexo-server --save #本地服务
$ npm install hexo-deployer-git --save #hexo通过git发布（必装）
$ npm install hexo-renderer-marked@0.2.7--save #渲染器
$ npm install hexo-renderer-stylus@0.3.0 --save #渲染器' 
```

<!-- more -->

### 初始化

创建hexo文件夹，并用git打开

`hexo init`

生成静态页面

`hexo p`

打开服务

`hexo s`

访问测试页面：<http://localhost:4000/2017/10/21/hello-world/>。

### 配置部署页面git

进入hexo文件夹，打开_config.yml

配置

> deploy:
> type: git
> repo: https://github.com/programluo/articles.git
> branch:master 

保存执行发布命令。

`hexo d`

### 部署

hexo通过`hexo d -g`生成部署文件，并上传git。

1. 部署需要**hexo-deployer-git**插件，安装插件语句

   `npm install --save hexo-deployer-git`

2. 部署hexo，部署语句

   ` hexo d -g`

  *注意1，该文件夹必须`git init`，并添加remote*

  *注意2，如果部署后的访问git页面地址加载不了样式，则是url和root那里没有配置项目名，应做如下配置：*

> url: http://lcdsg.com/articles/
> root: /articles

## 使用配置

### 安装皮肤

下载并使用皮肤

在hexo中，皮肤在themes文件夹下，我们在github下载选择好的themes放入themes文件夹下，再在_config.yml中配置theme。下面是使用next皮肤。

在git命令中执行

`it clone https://github.com/iissnan/hexo-theme-next.git themes/next`

再修改_config.yml

> theme:next

### 配置使用本地图片

hexo默认一般使用网络图片，如果能加载本地图片，需要安装插件并配置。

1. 打开_config.yml，查看是否存在**post_asset_folder**这个属性。设置为**true**。

这个属性在html时，会自动建立一个与文章同名的文件夹，可以把与文章相关的所有资源都放到那个文件夹类。

1. 在hexo的目录下执行`npm install https://github.com/CodeFalling/hexo-asset-image --save`
2. 在_post文件下，新建一个文章同名的文件夹，将图片放在文件夹下。
3. 使用`![logo](本地图片测试/logo.jpg)` 就可以插入图片。其中`[]`里面不写文字则没有图片标题。

*关心一下hexo对markdown的生成规则，这样可以更好的使用。*

查看网页生成目录*.deploy_git\2017\10\13\zwpx*，可以发现，图片和页面都放在该文件加下，打开页面源码，图片的地址也是`2017\10\13\zwpx\zwpx.jpg`。

### 语言设置

hexo的语言默认是英文，可以配置成中文

> 打开*_config.yml*，搜索language，配置成`zh-Hans`

### front-matter

front-matter是文件最上方以`---`分隔的区域，用于指定个别文件的变量，举例来说

```
title: Hello World
date: 2013/7/13 20:46:25
--- 
```

front-matter属性有

| 参数         | 描述         | 默认值    |
| :--------- | ---------- | ------ |
| layout     | 布局         |        |
| title      | 标题         |        |
| date       | 建立日期       | 文件建立日期 |
| updated    | 更新日期       | 文件更新日期 |
| comments   | 开启文章的评论功能  | true   |
| tags       | 标签(不适用于分页) |        |
| categories | 分类(不适用于分页) |        |
| permalink  | 覆盖文章网址     |        |

分页、分类的写法

```
categories:
- Diary
tags:
- PS3
- Games
```

### 基本配置

#### 网站

|          参数 |      描述 |  默认值 |
| ----------: | ------: | ---: |
|       title |    网站标题 |      |
|    subtitle |   网站副标题 |      |
| description |    网站描述 |      |
|      author |      名字 |      |
|    language | 网站使用的语言 |      |

其中，`description`主要用于SEO，告诉搜索引擎一个关于站点的描述，建议加入关键字。

#### 网址

|   参数 |    描述 |  默认值 |
| ---: | ----: | ---: |
|  url |    网站 |      |
| root | 网站根目录 |  `/` |

### github page域名

github page是github下用于域名解析的服务，使用github绑定域名步骤如下

1. 将项目名改为**programluo.github.io**，**program**是用户名。使用github page项目名必须是这样的。接着访问programluo.github.io，如果能正确跳到项目页面就成功。cmd中执行`ping programluo.github.io`，获得IP地址

2. 使用网址解析，将网址解析成IP。

3. 在项目根目录(hexo中的resource)下，新建文件CNAME，值就是自己的网址。

   *原理，ping programluo.github.io的ip是github的官方ip，github page服务会根据CNAME中的网站记录，找到对应的项目*

**解析二级域名**

解析二级域名与一级相同

**注意，修改github page的域名后，所有项目的url都会发生改变，成为 '个人域名'+'/项目名称'的形式，修改成个人域名后，github原先的‘用户名.github.io’的域名就不可再用，一个github只能绑定一个域名，如带'www'和不带'www'是两个域名，他们两个是不能同时绑定的。**

### pages平台和域名绑定

- github：github page是最官方的平台，但是防火长城的原因，github常常无法访问，可以抛弃。
- coding：coding的pages服务不错，可以CNAME重订访问。
- 码云code：码云code不行，不能CNAME绑定个性化域名。

## next主题配置

### 首页分段显示

next首页的文章默认显示全文，如果需要只显示预览，可做如下配置。

- 进入主题文件加下`themes/next`下
- 打开*config.yml*文件
- 搜索`auto_excerpt`，找到如下代码段

> \# Automatically Excerpt. Not recommand.
> \# Please use <!-- more --> in the post to control excerpt accurately.
> auto_excerpt:
> enable: false
> length: 150

- 把`enable: false` 改为 `enable: true`

### next修改主题尾缀

在coding中，如果要关闭中途的跳转页，必须在页面首页加上by coding这类的东西，我于是加在底部

方法如下：

1. 在themes/next文件夹下，搜索footer.swig文件，打开。

2. 找到`<div class="theme-info"></div>`标签，删除标签内的内容，并插入coding的要求后缀。

   ### 标签和分类

   1. 在`themes/next/_config.yml`中，搜索`menu`，开启`tags`和`categories`。


   2. 使用hexo命令新增`tags`和`categories`页面

      ```
      hexo new page tags
      hexo new page categories
      ```

   3. 在每篇博客的Front-matter中写上对应的`tags`和`categories`。

      ```
      categories:
      - Diary
      tags:
      - PS3
      - Games
      ```

## next插件

next本身支持许多插件，在`themes/next/_config.yml`中，搜索`Third Party`下面的都是支持的第三方插件，有评论、访问量等

### 访问量量统计

阅读统计用的是**不蒜子**，用法也很简单，找到next/_config.yml文件，搜索`busuanzi_count`，将`enable: false`改为`enable: true`

### 评论

评论用的是leancloud的valine，配置如下

> valine:
>  enable: true
>  appid:  你的appid
>  appkey:  你的appkey

详细教程：https://ioliu.cn/2017/add-valine-comments-to-your-blog/

### 分享

可以分享空间、微信等，搜索`needmoreshare2`，将`false`改为`true`。