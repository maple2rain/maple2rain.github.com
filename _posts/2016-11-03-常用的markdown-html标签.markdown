title: 常用的markdown-html标签
date: 2016-11-03 15:09:03
updated: 2017-02-21 16:17:09
tags:
- Html
categories:
- Life
---
# 1 插入图片

当想为*Markdown*文章添加图片并设置格式控制时，可以不使用*Markdown*的语法，而使用*HTML*标签，如以下代码所示，其中

- `align`表示**对齐属性**，其可选参数为`left`、`right`、`center`，分别表示居左、居右、居中。
- `img`表示标签为**图片**
- `width`和`height`表示宽度和高度，在此表示图片宽高，可以以像素`px`为单位，也可以用百分比的方式表示，如`width=50%`表示占原图片的50%；若不加这一选项则自动放大铺满或为原尺寸
- `title`表示当鼠标滑过图片时显示的文字，用于提示

```css
/*实际编写时应去掉注释及换行*/
<div align=center>  //对齐属性
<img    
    src="http://i.imgur.com/6CD0Erz.jpg"//图片URL
    width=256px height=256px            //图片宽高
    title="图片标签"                    //鼠标滑过图片时显示的文字
/>
</div>
```
例图如下所示：
<div align=center>
<img src="http://i.imgur.com/6CD0Erz.jpg" width=256px height=256px title="图片标签"/>
</div>

# 2 插入音乐或视频

当想为自己的文章添加润色时，可以考虑插入音频及视频，但*Markdown*本身是不支持插入音频视频的，所以可以考虑插入*HTML*标签的方法。此时要用到的标签为`iframe`，代码如下所示，其中

- `div`用于控制格式，若无则默认为居左
- `frameborder`用于规定是否显示框架周围的边框，1为是，0为否
- `marginwidth`及`marginheight`表示距离边缘的像素大小
- `width`及`height`表示播放条的长度和宽度
- `src`为播放链接，可以在如网易云音乐的`生成外链播放器`获取该链接，同时也获得以下代码，并可以自行更改；也可将音频链接改为视频链接，从而播放视频
- 值得注意的是，音频和视频在默认情况下是会自动循环播放的，可以修改链接的值进行修改
    - 在`src`域中，`auto`值表示是否自动播放，当值为1时为自动播放，0则不是
    - 在`src`域中，有些链接会附有`height`或`width`值，其表示表示播放框的基本宽高，可以更换其值以获得想要的播放框大小，此时可以不用填写外部的`width`及`height`

```css
/*实际编写时应去掉注释及换行*/
<div align=center>  //对齐属性
<iframe 
frameborder="no" 
marginwidth="0" 
marginheight="0" 
width=330 height=86     //音乐播放链接宽高
src="http://music.163.com/outchain/player?type=2&id=28285910&auto=1&height=66">     //音乐链接
</iframe>
</div>
```

以下添加网易云音乐上金玟崎的《岁月神偷》为例
<div align=center>
<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=330 src="http://music.163.com/outchain/player?type=2&id=28285910&auto=1&height=158">
</iframe>
</div>

# 3 设置首行缩进

由于*markdown*本身不支持`首行缩进`，故需要让`div`标签来配合，主要是用到了如下代码所示的`style`，此后，将文本放置于`p`标签内即可实现首行缩进。

```css
<div style="text-indent:2em">
<p>段落一</p>
<p>段落二</p>
</div>
```