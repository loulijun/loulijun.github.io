---
layout: post
title: Android中iconfont的应用
date: 2016-11-21 19:30:24.000000000 +09:00
---

### iconfont是什么

iconfont翻译过来就是矢量字体图标，通过字体文件取代图片来展示图标，特殊字体等元素的方法，并且，该技术使用也比较普遍了，可以应用在web、Android、iOS中，比如[阿里妈妈](http://www.iconfont.cn)将iconfont整合并公开了很多字体图标库，可以直接拿来使用，当然如果要自己制作iconfont需要先制作svg图片然后转为字体，或者有相应的网站直接生成均可。由于近期在Android中使用到，所以记录之，以下只总结在Android中应用遇到的问题。
![Image By Dustin Wallace](http://7xla3m.com1.z0.glb.clouddn.com/20160601-iconfont-banner.png) 

<!-- more -->

### 使用iconfont的优缺点
优点

* 体积比图片小很多
* 矢量，所以拉伸不会变形；颜色大小可以更换，所以后期更易维护（Android中可能提供很多套不同尺寸的图片，iconfont只需要一套即可）
* 风格统一，使用方便
* 可以添加一些视觉效果如：阴影、旋转、透明度

缺点

* 制作iconfont门槛稍高，首先需要提供svg资源，通过工具转换为对应的字库文件
* 只能是纯色，如果图标是颜色很多的就不适合了
* 在Android中部分应用场景受限（如TextView的drawableLeft， ImageView的placeholder等）

### 在Android中使用
step1：将字体库iconfont.ttf放到assets目录下（我放到了arrets/iconfont/iconfont.ttf），ttf字体库可以在相应的字体库多选打包

step2：封装IconFontTextView，代码如下

```
public class IconFontTextView extends TextView {
    public IconFontTextView(Context context) {
        super(context);
        initIconFontTypeFace(context);
    }

    public IconFontTextView(Context context, AttributeSet attrs) {
        super(context, attrs);
        initIconFontTypeFace(context);
    }

    private void initIconFontTypeFace(Context context) {
        Typeface iconfont = null;
        try {
            iconfont = Typeface.createFromAsset(context.getAssets(), "iconfont/iconfont.ttf");
            setTypeface(iconfont);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}

```

step3：找到需要替换的图标及iconfont对应的图标的unicode，例如替换一个礼包的图标，需要知道我要替换的图标大小（大小可以自己指定，一般都是UE指定），颜色，以及对应的字体的unicode。

![iconfont demo](http://7xla3m.com1.z0.glb.clouddn.com/20160601-iconfont-demo.png)

在strings.xml中将值定义，将对应的图标的unicode填入string.xml中。

```
 <string name="iconfont_gift">&#xf3d8;</string>
```
step4：最后在代码中使用

```
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical" android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:gravity="center"
    android:background="@android:color/white">
    <com.loulijun.text.IconFontTextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:textSize="160sp"
        android:textColor="#FF0000"
        android:text="@string/iconfont_gift"/>
</LinearLayout>
```

得到的效果如下：

![MacDown Screenshot](http://7xla3m.com1.z0.glb.clouddn.com/20160601-iconfont-show.png)