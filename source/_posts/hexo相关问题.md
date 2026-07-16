---
title: hexo相关问题
date: 2018-04-15 10:55:26
tags: [博客,hexo,yilia]
---

hexo相关的一些问题和yilia主题配置

<!--more-->

参考：

https://blog.csdn.net/xdfwsl/article/details/102587821

https://www.jianshu.com/p/10a12ec243a6

http://dongshuyan.com/2019/05/24/hexo%E5%8D%9A%E5%AE%A2%E6%B3%A8%E6%84%8F%E4%BA%8B%E9%A1%B9/

#### 去除yilia主题中失效的统计访问

使用yilia主题打开网站会有个 `https://litten.me:9005/badjs/?id=1&uin=xxxxxx` 的访问一直转圈，主题作者最后维护已经是N年前，这个统计链接也早已失效
查找了一下，代码在`themes/yilia/source-src/js/report.js`，但这里是源码，实际使用的是编译后的文件，所以更改这里并不能解决问题。



##### 解决方法

文件：`themes/yilia/source/main.0cf68a.js`

查找

```js
192:function(e,t,n){"use strict";function o(e){var t=new RegExp("(^|&)"+e+"=([^&]*)(&|$)","i"),n=window.location.search.substr(1).match(t);return null!=n?unescape(n[2]):null}var r=n(388);if(n(197),window.BJ_REPORT){BJ_REPORT.init({id:1}),BJ_REPORT.init({id:1,uin:window.location.origin,combo:0,delay:1e3,url:"//litten.me:9005/badjs/",ignore:[/Script error/i],random:1,repeat:5e5,onReport:function(e,t){},ext:{}});var i=window.location.host,a=top===window,u=!(/localhost/i.test(i)||/127.0.0.1/i.test(i)||/0.0.0.0/i.test(i));a&&u&&BJ_REPORT.report("yilia-"+window.location.host);var l=o("f"),c="yilia-from";l?(a&&BJ_REPORT.report("from-"+l),r.set(c,l)):document.referrer.indexOf(window.location.host)>=0?(l=r.get(c),l&&a&&BJ_REPORT.report("from-"+l)):r.remove(c)}e.exports={init:function(){}}},
```

替换为

```js
192:function(e,t,n){},
```

#### 鼠标点击小红心的设置

在文件夹 `hexo/themes/yilia/source`目录下添加文件`love.js`。

love.js内容：

```JS
!function(e,t,a){function r(){for(var e=0;e<s.length;e++)s[e].alpha<=0?(t.body.removeChild(s[e].el),s.splice(e,1)):(s[e].y--,s[e].scale+=.004,s[e].alpha-=.013,s[e].el.style.cssText="left:"+s[e].x+"px;top:"+s[e].y+"px;opacity:"+s[e].alpha+";transform:scale("+s[e].scale+","+s[e].scale+") rotate(45deg);background:"+s[e].color+";z-index:99999");requestAnimationFrame(r)}function n(){var t="function"==typeof e.onclick&&e.onclick;e.onclick=function(e){t&&t(),o(e)}}function o(e){var a=t.createElement("div");a.className="heart",s.push({el:a,x:e.clientX-5,y:e.clientY-5,scale:1,alpha:1,color:c()}),t.body.appendChild(a)}function i(e){var a=t.createElement("style");a.type="text/css";try{a.appendChild(t.createTextNode(e))}catch(t){a.styleSheet.cssText=e}t.getElementsByTagName("head")[0].appendChild(a)}function c(){return"rgb("+~~(255*Math.random())+","+~~(255*Math.random())+","+~~(255*Math.random())+")"}var s=[];e.requestAnimationFrame=e.requestAnimationFrame||e.webkitRequestAnimationFrame||e.mozRequestAnimationFrame||e.oRequestAnimationFrame||e.msRequestAnimationFrame||function(e){setTimeout(e,1e3/60)},i(".heart{width: 10px;height: 10px;position: fixed;background: #f00;transform: rotate(45deg);-webkit-transform: rotate(45deg);-moz-transform: rotate(45deg);}.heart:after,.heart:before{content: '';width: inherit;height: inherit;background: inherit;border-radius: 50%;-webkit-border-radius: 50%;-moz-border-radius: 50%;position: fixed;}.heart:after{top: -5px;}.heart:before{left: -5px;}"),n(),r()}(window,document);
```

在`hexo/themes/yilia/layout/_partial/footer.ejs`文件最后添加以下代码：

```html
<!--页面点击小红心-->
<script type="text/javascript" src="/love.js"></script>
```





> #### 常用命令

```sh
hexo n "postName" # 新建文章，文章路径为 source/_posts
hexo new draft "draftName"  # 新建草稿，不会发布至你的网站，文章路径为 source/_drafts
hexo new page "pageName"  # 新建页面，文章路径为 source
hexo publish draft "draftName"  # 将草稿进行发布
hexo clean  # 清除缓存，建议每次部署时先执行该命令，再生成静态页面
hexo g  # 生成静态页面至 public 目录
hexo s  # 开启预览访问端口(默认端口 4000，‘ctrl+c’ 关闭 server)
hexo d  # 将文件进行部署
hexo help # 查看帮助
```



> #### 设置文章模板

每次新建文章都会调用文章模板，其位于 `scaffolds/post.md` 文件，因此，我们可以对其进行必要的修改，提供便利。例如，我的修改如下：

```
---
title: {{ title }}
date: {{ date }}
categories:
tags:
comments: true
mathjax: true
---
```

其中 `comments` 用于控制是否开启评论，`mathjax` 用于控制是否公式的加载，对于不需要渲染公式的文章，关闭 mathjax 可提高页面的加载速度。

> #### SEO优化

搜索引擎优化（英语：search engine optimization，缩写为 SEO），是一种通过了解搜索引擎的运作规则来调整网站，以及提高目的网站在有关搜索引擎内排名的方式。由于不少研究发现，搜索引擎的用户往往只会留意搜索结果最前面的几个条目，所以不少网站都希望通过各种形式来影响搜索引擎的排序，让自己的网站可以有优秀的搜索排名。当中尤以各种依靠广告维生的网站为甚。
 所谓 “针对搜索引擎作最优化的处理”，是指为了要让网站更容易被搜索引擎接受。搜索引擎会将网站彼此间的内容做一些相关性的数据比对，然后再由浏览器将这些内容以最快速且接近最完整的方式，呈现给搜索者。搜索引擎优化就是通过搜索引擎的规则进行优化，为用户打造更好的用户体验，最终的目的就是做好用户体验。
 对于任何一个网站来说，要想在网站推广中获取成功，搜索引擎优化都是至为关键的一项任务。同时，随着搜索引擎不断变换它们的搜索排名算法规则，每次算法上的改变都会让一些排名很好的网站在一夜之间名落孙山，而失去排名的直接后果就是失去了网站固有的可观访问流量。所以每次搜索引擎算演法的改变都会在网站之中引起不小的骚动和焦虑。可以说，搜索引擎优化是一个愈来愈复杂的任务。——[维基百科](https://links.jianshu.com/go?to=https%3A%2F%2Fzh.wikipedia.org%2Fwiki%2F%E6%90%9C%E5%B0%8B%E5%BC%95%E6%93%8E%E6%9C%80%E4%BD%B3%E5%8C%96)

> #### 给站点添加 sitemap

首先需要安装两个插件来生成 sitemap 文件，前一个是传统的 sitemap，后一个是百度的 sitemap。

```
npm install hexo-generator-sitemap --save
npm install hexo-generator-baidu-sitemap --save
```

然后需要修改站点配置文件 `_config.yml` ，配置 `sitemap` 插件。

```yaml
Plugins: # 在该区域添加两个插件名称
  - hexo-generator-sitemap
  - hexo-generator-baidu-sitemap

# Sitemap
sitemap:
  path: sitemap.xml

# baidusitemap
baidusitemap:
  path: baidusitemap.xml
```

安装完成后执行 `hexo g` 即会在站点 `public` 目录下生成 `sitemap.xml` 和 `baidusitemap.xml` 。

> #### 添加 robots.txt

在站点 `source` 文件夹下新建 `robots.txt` 文件，文件内容如下：

```
User-agent: *
Allow: /
Allow: /archives/
Allow: /categories
Allow: /tags/

Disallow: /vendors/
Disallow: /js/
Disallow: /css/
Disallow: /fonts/
Disallow: /vendors/
Disallow: /fancybox/

Sitemap: https://kivenckl.github.io/sitemap.xml
Sitemap: https://kivenckl.github.io/baidusitemap.xml
Sitemap: http://kivenckl.coding.me/sitemap.xml
Sitemap: http://kivenckl.coding.me/baidusitemap.xml
```

`Allow` 字段的值即为允许搜索引擎爬取的内容，可以对应到主题配置文件中的 menu 目录配置，如果菜单栏还有其他选项都可以按照格式自行添加。

需要将下方的域名改成自己的域名。

#### 添加RSS

- ##### 什么是RSS？

RSS也就是订阅功能，你可以理解为类似与订阅公众号的功能，来订阅各种博客，杂志等等。

- ##### 为什么要用RSS？

就如同订阅公众号一样，你对某个公众号感兴趣，你总不可能一直时不时搜索这个公众号来看它的文章吧。博客也是一样，如果你喜欢某个博主，或者某个平台的内容，你可以通过RSS订阅它们，然后在RSS阅读器上可以实时推送这些消息。现在网上的垃圾消息太多了，如果你每一天都在看这些消息中度过，漫无目的的浏览，只会让你的时间一点一点的流逝，太不值得了。如果你关注的博主每次都发的消息都是精华，而且不是每一天十几条几十条的轰炸你，那么这个博主就值得你的关注，你就可以通过RSS订阅他。

在我的理解中，如果你不想每天都被那些没有质量的消息轰炸，只想安安静静的关注几个博主，每天看一些有质量的内容也不用太多，那么RSS订阅值得你的拥有。

- ##### 添加RSS功能

安装RSS插件

```
npm i hexo-generator-feed
```

而后在你整个项目的`_config.yml`中找到Extensions，添加：

```
# Extensions
## Plugins: https://hexo.io/plugins/
#RSS订阅
plugin:
- hexo-generator-feed
#Feed Atom
feed:
  type: atom
  path: atom.xml
  limit: 20
```

这个时候你的RSS链接就是 域名`/atom.xml`了。

所以，在主题配置文件中的这个`social links`，开启RSS的页面功能，这样你网站上就有那个像wifi一样符号的RSS logo了，注意空格。

```
rss: /atom.xml
```

官网地址：https://hexo.io/zh-cn/docs/asset-folders

#### hexo修改端口号：

找到hexo安装目录的node_modules\hexo-server\index.js，修改即可。





