---
title: 2018-5-8 Settings设置
tags: Yota,Markdown,Roger
grammar_cjkRuby: true
---


欢迎使用 **{小书匠}(xiao shu jiang)编辑器**，您可以通过==设置==里的修改模板来改变新建文章的内容。

# 简介
> 首页

- com.android.settings.Settings extends SettingsActivity
	- BluetoothSettingsActivity 
	- WirelessSettingsActivity
	- ...

# 添加设置流程
> 例:添加手势截屏页面

在 SettingsActivity 添加如下代码
```java
            /* BaoliYota begin, add */
            /* GESTURE_SCREENSHOT */
            /* Add switch control gesture screenshot, renxu@coolpad.com, 2017.12.22 */
            GestureScreenshotSettings.class.getName(),
            /* BaoliYota end */
```