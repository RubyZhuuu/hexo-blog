title: 移动端IOS上怎么显示1px物理像素的border
tags:
  - 移动端开发
  - CSS
---

# 起因
我在移动端开发的过程中，给很多元素设置了border，在CSS中我是这么写的：
``` CSS
border: solid 1px #eeeeee;
```
在我的认识中，这1px应该就是设计稿上面那1px。结果美工妹子来找我说我的border比设计稿要粗。我觉得奇怪呀明明设置好了1px，结果一看还真是明显要粗一些。
于是我跟她说你的设计稿是基于640px的宽度（以5S为例）设计的，而我开发是基于320px宽度，所以可能要粗一些。美工表示需要让我能显示更细的border。

## 物理像素 VS CSS像素
在PC端开发的时候，由于CSS里面写1px似乎等同于电脑屏幕的1px，因此我们常常误以为1px就是一个绝对的单位，并且是等于一个物理像素。
然而实际上并不是这样。想象你的电脑上有一个div，宽度是100px。你对你的浏览器窗口使用了缩放，放大到了200%。这个时候，你的div看起来宽度是原来的两倍，而它的CSS宽度应该是多少呢？答案还是100px。
这就是CSS像素与物理像素之间的区别，二者并不等价。

## 640px VS 320px
那为什么5S有640px和320px“两个分辨率”呢？
根据[友盟指数](http://www.umindex.com/devices/ios_resolutions) 的数据，5S手机分辨率中的宽度确实是640px没错，这个640px说明，5S的物理分辨率是640px。
在实际开发中，移动端开发通常会在head中加入：
``` HTML
<meta name="viewport" content="width=device-width, initial-scale=1.0,maximum-scale=1.0,uer-scalabe=no">
```
大家都这知道这个meta标签的意思是在你的网页上禁止用户使用缩放，并且让网页的宽度固定为一个让用户阅读良好的宽度。"width=device-width"这里对width的设置可以简单理解为“在这么大的手机窗口中，显示多少像素的内容”。而这里的device-width，对于5S来说，并不是640px，而是320px。这是因为5S采用的retina屏幕的设备像素比率是2，也就是一个逻辑像素（可以简单理解为CSS像素）对应了两个物理像素，以达到更好的显示效果。因此，倘若你把width设置为device-width，initial-scale设置为1.0，那么你的整个屏幕，在逻辑上和CSS中宽度应该是320px。

关于viewport，还有很多内容，[PPK大神在关于viewport介绍的文章里面详细介绍了PC端和移动端关于viewport的各种概念](http://www.quirksmode.org/mobile/viewports.html)，这篇文章非常详细，我觉得我不可能解释的比他更清楚。

## 回答问题，为什么我的1px比设计稿要粗
相信看到这里已经非常清楚为什么我设置border为1px看着比设计稿要粗了。
设计图是按照物理像素的640px进行设计的。而我的1px是CSS中的1个像素，由于我设置了"width=device-width, initial-scale=1.0”它对应着两个物理像素。其实可以这么说，我不可能写出1px物理像素的宽度。

## 解决方法
那么问题来了，既然我已经设置了initial-scale的值，在页面其他地方也写成了很多px的值，倘若我直接将initial-scale改为0.5，虽然能达到让border变细的目的，但是其他地方的元素大小设置都得改变啊，这不现实！不多说了直接上解决方案吧
``` CSS
/* 我要给这个元素设置更细的底边 */
.item {
    position: relative;
}
/* 给这个元素设置:after为元素，并使用scale进行缩放，这样看起来就更细了呀 */
.item:after {
    content: '';
    display: block;
    position: absolute;
    width: 100%;
    left: 0;
    bottom: 0;
    height: 1px;
    background-color: #c8c7cc;
    -webkit-transform: scaleY(0.5);
    transform: scaleY(0.5);
}
```
其他还有一些解决方案，比如使用base64图片等等，但是我觉得这是最简单，也很实用的一种方法，测了5,5S,6，和部分安卓机，也基本都能正确显示。

## 新的问题
这又引入了一个新问题。如果是给一个元素设置的是四边都有边框，而这个元素又是个button，会让button变得不能点击。给after元素使用 pointer-events: none; 就可以解决，一般人我不告诉他

原创文章，转载注明作者[RubyZhuuu](http://rubyzhuuu.github.io/)