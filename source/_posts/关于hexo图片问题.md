---
title: 关于hexo图片问题
date: 2020-08-22 19:56:56
tags:
- HEXO
- BLOG
- bug
categories:
- 学习
- BLOG
---
# Bug：
使用hexo写博客时，在文章里面插入图片，最后hexo g的时候总会在图片路径前面插一个.top。导致图片无法展示

# Debug：
    一开始怀疑是配置文件
{% asset_img 1.jpg 关于hexo图片问题 %}

`可是修改后无法解决`  
`百度后发现是hexo-asset-image这个插件的bug`

# 解决方法1
**我他妈删爆**
直接卸载hexo-asset-image，选择使用hexo 3.0方式的标签插件进行图片插入。  

    好
    耶  
    ！    
    大  
    成  
    功

# 解决方法2    
**网站url的方式与3.0以下有些不同，照着文章修改hexo.js的index.js就行**
1. 先卸载 hexo-asset-image  
2. 重新下载 `npm install https://github.com/CodeFallin... --save`
3. 修改js,一定要改，不然这个bug解决了，还会有别的bug出现，所以打开文件`/node_modules/hexo-asset-image/index.js`，把下面这段复制进去就ok了。
```js
'use strict';
var cheerio = require('cheerio');

function getPosition(str, m, i) {
return str.split(m, i).join(m).length;
}

var version = String(hexo.version).split('.');
hexo.extend.filter.register('after_post_render', function(data){
    var config = hexo.config;
if(config.post_asset_folder){
    var link = data.permalink;
if(version.length > 0 && Number(version[0]) == 3)
   var beginPos = getPosition(link, '/', 1) + 1;
else
   var beginPos = getPosition(link, '/', 3) + 1;
// In hexo 3.1.1, the permalink of "about" page is like ".../about/index.html".
var endPos = link.lastIndexOf('/') + 1;
link = link.substring(beginPos, endPos);

var toprocess = ['excerpt', 'more', 'content'];
for(var i = 0; i < toprocess.length; i++){
  var key = toprocess[i];

  var $ = cheerio.load(data[key], {
    ignoreWhitespace: false,
    xmlMode: false,
    lowerCaseTags: false,
    decodeEntities: false
  });

  $('img').each(function(){
    if ($(this).attr('src')){
        // For windows style path, we replace '\' to '/'.
        var src = $(this).attr('src').replace('\\', '/');
        if(!/http[s]*.*|\/\/.*/.test(src) &&
           !/^\s*\//.test(src)) {
          // For "about" page, the first part of "src" can't be removed.
          // In addition, to support multi-level local directory.
          var linkArray = link.split('/').filter(function(elem){
            return elem != '';
          });
          var srcArray = src.split('/').filter(function(elem){
            return elem != '' && elem != '.';
          });
          if(srcArray.length > 1)
            srcArray.shift();
          src = srcArray.join('/');
          $(this).attr('src', config.root + link + src);
          console.info&&console.info("update link as:-->"+config.root + link + src);
        }
    }else{
        console.info&&console.info("no src attr, skipped...");
        console.info&&console.info($(this));
    }
  });
  data[key] = $.html();
    }
  }
});
```
