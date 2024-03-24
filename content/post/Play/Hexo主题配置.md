---
title : Ocean 主題配置
date: 2021-10-30
updated : 2021-11-02
categories: ["玩机"]
tags: ["hexo", "博客", "主题"]
---

Ocean 是基于 Hexo 默认主题 landscape 的功能，设计的一款支持移动设备的主题，并且集成了 Gitalk 与 Valine 评论功能。

如果你喜欢 Ocean 可以从 [GitHub](https://github.com/zhwangart/hexo-theme-ocean) 下载，主题默认使用 Logo 是 Hexo 的 Logo，更换 Logo 路径在 ocean/source/images/hexo.svg ，注意它还有一个反色版 hexo-inverted.svg ，如果你更改了文件名，那么还需要在 ocean/_config.yml 做对应的修改。

详细文档见 [Ocean 文档](https://zhwangart.com/2018/11/30/Ocean/)，以下是个人的需要的配置

<!--more-->


# 1. 安装

下载安装
~~~shell
$ git clone https://github.com/zhwangart/hexo-theme-ocean.git themes/ocean
~~~

更新
~~~
$ cd themes/ocean
$ git pull
~~~

# 2. 配置

## 2.1 添加页面

Ocean 默认在主题配置文件中配置相册和关于的链接， 但是主题中实际上并没有此页面，点击会提示 404 。可以按照需要进行删除和安装。

### 2.1.1 安装存在格式的页面
~~~
hexo new page gallery    // 创建相册页面
hexo new page about      // 创建关于页面
hexo new page tags       // 创建标签页面
hexo new page categories // 创建分类页面
hexo new page favorites  // 创建收藏页面
~~~

主题会根据不同的页面定义不同的模板(theme/layout/), 需要在 index.md 文件中的 Front-matter 区域标注 layout， 以 Tags 页面为例：

1，位置处在 source/tag/index.md

2，Markdown
~~~Markdown
---
title: 标签
date: 2021-10-31
type: tags
layout: "tags"
---
~~~

3，配置后需要在 themes/ocean/_config.yml 文件中的 menu 下新增页面， 如 about 。
~~~Markdown
menu:
    关于: about/
~~~

#### 2.1.1.1 增加收藏页面

1. 新增一个收藏页面

2. 修改 navbar.styl 文件中的 favorites 展示图标，注意顺序。

3. 编辑 favorites/index.md 文件

~~~
### 样机 Mockups

<div class="card-quote">

![Graphics](/images/logos/lstoreLogo.svg)
#### Graphics
高质量的样机素材
[https://www.ls.graphics](https://www.ls.graphics)

![sketchsheets](/images/logos/sketchLogo.svg)
#### Sketchsheets
Sketch 稿件的样机素材
[https://sketchsheets.com](https://sketchsheets.com)

</div>

~~~

## 2.2  插件

### 2.2.1 开启插件

#### 2.2.1.1  本地检索 [hexo-generator-search](https://github.com/wzpan/hexo-generator-search)

1，安装

~~~
npm install hexo-generator-searchdb --save
~~~

2，配置

2.1，Hexo 的配置文件 _config.yml 添加插件配置
~~~yaml
# 本地搜索
search:
  path: search.xml
  field: post
  content: true
~~~

#### 2.2.1.2 博客文章置顶 [hexo-generator-index-pin-top](https://github.com/netcan/hexo-generator-index-pin-top)

1，安装
~~~
npm uninstall hexo-generator-index --save
npm install hexo-generator-index-pin-top --save
~~~

2，在需要置顶文章的 Front-matter 区域加上 top: ture， 即可开启文章置顶
~~~
---
title : Ocean 配置
top: ture
---
~~~

### 2.2.2 修改配置文件

#### 2.2.2.1 文章封面图

1，添加
~~~
---
title:  Ocean 配置
photos: [
        ["/images/相机.jpg"], // themes/ocean/source/images 目录下
        ["img_url"] // 使用 http 链接
        ]
---
~~~
*设置 Hexo 博客的封面图，而不是博客的配图*


#### 2.2.2.2 Toc 文章目录

1，配置文件路径为 ocean/_config.yml
~~~ymal
# Toc 文章目录
toc: true
~~~

2，開啓 Toc 后, 在文章 Front-matter 中設置 toc 進行部分關閉。
~~~markdown
---
title: Toc 文章目录
toc: false
---
~~~
*使用 [Tocbot](http://tscanlin.github.io/tocbot/) 解析内容中的标题标签（h1~h6）并插入目录。*  


#### 2.2.2.3 修改導航欄圖標

1，修改主页、相册等图标，需要修改 css 文件，其所在位置在目录 source/css/_partial/navbar.styl 中， 修改时需要注意对应的顺序
2，修改 icon，需要替换 themes/ocean/source/favicon.ico 图片文件

#### 2.2.2.4 刪除鏈接文字的下劃綫

1，配置文件的路勁為 ocean/source/css/style.styl。

~~~css
a
  color link-color
  &:hover
    color link-hover-color
  &:active
    color link-active-color
  &.disabled
    color disabled-color

//  为

a
  color link-color
  text-decoration none
  &:hover
    color link-hover-color
    text-decoration none
  &:active
    color link-active-color
  &.disabled
    color disabled-color

~~~

#### 2.2.2.5 配置語言

1，配置文件的目錄為 _config.yml。

+ language 的值为空时默认为 en
+ language 的配置文件位置为 themes/ocean/languages/

*若本主題的導航文字，需要直接修改主题配置文件（_config.yml）中的 menu*

#### 2.2.2.6 取消文章的分享鏈接

1，配置文件的路勁為 themes\ocean\layout\_partial\footer.ejs。注釋以下代碼

~~~html
<!-- 显示分享按钮
      <a data-url="<%- post.permalink %>" data-id="<%= post._id %>" class="article-share-link">
        <%= __('share') %>
      </a>
-->
~~~




