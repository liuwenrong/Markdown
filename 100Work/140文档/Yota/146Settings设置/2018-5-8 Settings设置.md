---
title: 2018-5-8 Settings设置
tags: Yota,Markdown,Roger
grammar_cjkRuby: true
---


欢迎使用 **{小书匠}(xiao shu jiang)编辑器**，您可以通过==设置==里的修改模板来改变新建文章的内容。

# 简介

源码地址
> packages/apps/Settings

> 首页

- com.android.settings.Settings extends SettingsActivity
	- BluetoothSettingsActivity 
	- WirelessSettingsActivity
	- ...

[Android 4.4Settings 源码阅读记录-1407](https://blog.csdn.net/adayabetter/article/details/43409955)


# **布局修改**
-关于手机
	-内核版本
string
```xml
<string name="kernel_version" msgid="9192574954196167602">"内核版本"</string>
```
device_info_settings.xml
```xml
        <!-- Device Kernel version -->
        <Preference
                android:key="kernel_version"
                android:title="@string/kernel_version"
                android:summary="@string/summary_placeholder"/>

```

- 无障碍
	- 放大功能
	- 

```
    <string name="accessibility_screen_magnification_title" msgid="6001128808776506021">"放大功能"</string>

```
accessibility_settings.xml
```
        <Preference
            android:fragment="com.android.settings.accessibility.MagnificationPreferenceFragment"
            android:key="magnification_preference_screen"
            android:title="@string/accessibility_screen_magnification_title"
            android:icon="@mipmap/ic_accessibility_magnification" />

```

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