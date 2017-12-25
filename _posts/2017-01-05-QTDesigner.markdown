---
layout: post
author: LPF
title: QT Designer
date: 2017-01-05 13:44:22
updated: 2017-01-05 16:00:57
tags:
- QT
categories:
- Study
- Computer
- Graphics
- QT
---
# 1 使用Designer

在使用QT设计界面时，一开始使用的是手动编码的方式，这种做法其实是很费功夫的。由于前期考虑不是很周全，在后期想要添加一些组件的时候，总是会影响一些布局，使得处理麻烦。并且那么多的组件，为其命名并且记得其在哪里也是比较麻烦的。所以，最好是使用`Designer`，这样可以使得程序员专心于编写逻辑程序，而界面设计则交与设计师来制作。

# 2 使用流程

## 2.1 创建Designer

- 为项目添加新文件

    
<p>
    选择QT->QT 设计师界面类
</p>

<div align=center>
<img src="../post_img/586ddf60ab6441236e0038ab" title="新建Designer"/>
</div>

<p>
    选择界面模板，在此可选择Widget
</p>

<div align=center>
<img src="../post_img/586de3b3ab6441209e003a7c" title="选择模板"/>
</div>

<p>
    设置文件细节
</p>
<div align=center>
<img src="../post_img/586ddf60ab6441236e0038ac" title="设置文件细节"/>
</div>

<p>
    点击完成，此后将添加`widget.ui`文件
</p>
<div align=center>
<img src="../post_img/586de3b3ab6441209e003a7b" title="完成"/>
</div>

## 2.2 添加部件

<p>
    在选择刚刚被创建的`widget.ui`文件，双击或以`QT Designer`方式打开时，将会出现以下界面(示例中已完成组件的布局，新文件则为空)。
    其中，`组件选择区域`用于选择需要的组件，可将其拖动至`界面布局区域`以完成布局；`对象区域`用于显示组件间父子结构及修改属性，如类名等；而`组件属性区域`则用于为组件添加相关的属性。
    
</p>
<div align=center>
<img src=../post_img/586de3b3ab6441209e003a7a" title="QT Designer界面"/>
</div>

## 2.3 继承类

在完成组件的拖放之后，编译后会生成一个`ui_filename.h`文件，其名字由设计文件决定，如在此例中为`ui_widget.h`。为了使用该界面，还需创建一个相应的`Widget`类来继承它。
(注：使用Widget的原因是上文中创建的该设计界面为Widget的原因，若为其他则选择其他)。
创建该类之后，将类的声明改为如下方式：

```c++
#include "ui_widget.h" //include the header file
namespace Ui {
class Widget;
}

class Widget : public QWidget, public Ui::Widget// inherit that designer class
{
    Q_OBJECT
public:
    explicit Widget(QWidget *parent = 0);
    ~Widget();
}
```

此时不需要成员`ui`，而在构造函数中，修改为如下：

```c++
Widget::Widget(QWidget *parent) :
    QWidget(parent)
{
    setupUi(this);
    ...//other operation
}
```

设置`Ui`为自身，原因是`Ui::Widget`类已经重载了`setupUi`函数，并显示之前拖放的组件内容。
到了这一步就完成了基本的操作，便可以进行下一步的逻辑处理了。