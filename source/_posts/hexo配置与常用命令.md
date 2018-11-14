---
title: hexo配置与常用命令
date: 2017-08-02 09:55:04
tags:
- Hexo
categories:
- 编程
toc: true
---
# hexo+github建立个人博客
建站参考[muyunyun](http://www.cnblogs.com/MuYunyun/p/5927491.html)的一篇博客。步骤很详细。

# hexo常用命令

## hexo部署
- `hexo clean` : 清除。需要重新部署时（如修改主题之后），需要运行此命令
- `hexo g` : 生成
- `hexo d` : 发布
- `hexo d -g` : 上两个命令的合成

## 写文章
发布文章有两种方式
1. `hexo new "title"` : 新建一篇博客
2. `hexo new draft "title"` : 新建草稿 <br> `hexo publish "title"` : 将草稿发布

### 文章内容

    title: name #文章标题
    date: 2015-12-25 19:23:00 #写作时间
    description: #文章描述
    categories: #文章分类
    - c1
    tags: #文章标签
    - tag1
    - tag2
    toc: true #生成目录
    author:
    comments:
    original:
    permalink: #指定链接    

# hexo功能扩展

## 支持图片

方法参考[阿祥JOKER的博客](http://www.jianshu.com/p/c2ba9533088a)

首先设置`_config.yml`中`post_asset_folder:true`。
在建立文件时，Hexo会自动建立一个与文章同名的文件夹，可以把与该文章相关的所有资源都放到那个文件夹。

在blog目录下执行：`npm install https://github.com/CodeFalling/hexo-asset-image --save`

文件中使用`![logo](本地图片测试/logo.jpg)`就可以插入本地图片了。

## 支持Tex数学公式

方法参考[ShallowLearner的博客](http://www.jianshu.com/p/7ab21c7f0674)

1. 更换Hexo的markdown渲染引擎

```vim
npm uninstall hexo-renderer-marked --save
npm install hexo-renderer-pandoc --save
```
2. 接下来到博客根目录下，找到`node_modules\kramed\lib\rules\inline.js`，把第11行的`escape`变量和20行的`em`变量的值做相应的修改：

```vim
//  escape: /^\\([\\`*{}\[\]()#$+\-.!_>])/,
  escape: /^\\([`*\[\]()#$+\-.!_>])/

//  em: /^\b_((?:__|[\s\S])+?)_\b|^\*((?:\*\*|[\s\S])+?)\*(?!\*)/,
  em: /^\*((?:\*\*|[\s\S])+?)\*(?!\*)/
```
3. 进入到主题目录，找到_config.yml配置问题，把mathjax默认的false修改为true
```vim
# MathJax Support
mathjax:
  enable: true
  per_page: true
```

4. 在文章的Front-matter里打开mathjax开关，如下：
```vim
---
title: index.html
date: 2016-12-28 21:01:30
tags:
mathjax: true
---
```
## 站内搜索
`npm install hexo-generator-search --save`
