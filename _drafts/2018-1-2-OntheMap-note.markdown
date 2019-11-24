---
layout: post
title:  "OntheMap 研发笔记"
date:   2018-1-1
---

## 设置 UIButton 为圆角矩形：

1. 纯代码：
2. 在 storyboard 中，选中 UIButton，打开 Identity inspector，在 User Defined Runtime Attributes 中添加相应的属性值：Key Path:layer.cornerRadius Type:Number Valus:5。

## 设置 UIImageView 长宽比为 1:1(最后没有设置 1:1 ，发现没必要)

建立 ImageView 对应其本身的约束，选择 Aspect Ratio，然后设置为 1:1

## 设置 UIButton 中的文本为不同的颜色：

在 UIBUtton 的 Attributes Inspector 中设置 Title 为“Attributed”，然后设置不同文本的颜色。

## 为 UIButton 设置超链接：

1. 先为该 Button 建一个 IBAction；
2. 然后在函数中添加
```
UIApplication.shared.open(URL(string: "https://cn.udacity.com/signup")!, options: [:], completionHandler: nil)
```