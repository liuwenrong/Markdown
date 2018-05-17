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

其中Preference是v7包下的 android.support.v7.preference.Preference

去找安卓8.1的v7包源码在frameworks\support\v7\preference\src\android\support\v7\preference\
在Preference的构造方法中
```
	final TypedArray a = context.obtainStyledAttributes(
                attrs, R.styleable.Preference, defStyleAttr, defStyleRes);
				//...
	//8.1新增的 疑似设置项左边的图标 图标空间是否预留
        mIconSpaceReserved = TypedArrayUtils.getBoolean(a, R.styleable.Preference_iconSpaceReserved,
                R.styleable.Preference_android_iconSpaceReserved, false);
				
```

可以通过下面这句话控制不可见还是gone
```
final ImageView imageView = (ImageView) holder.findViewById(android.R.id.icon);
imageView.setVisibility(mIconSpaceReserved ? View.INVISIBLE : View.GONE);

       View imageFrame = holder.findViewById(R.id.icon_frame);
	   imageFrame.setVisibility(mIconSpaceReserved ? View.INVISIBLE : View.GONE);
```
在frameworks\support\v7\preference\res\layout\preference.xml 找到icon和icon_frame
frameworks\support\v7\preference\res\values找到attrs.xml
```xml
    <!-- Base attributes available to Preference. -->
    <declare-styleable name="Preference">
		...
	<!-- Whether the space for the preference icon view will be reserved. If set to true, the
             preference will be offset as if it would have the icon and thus aligned with other
             preferences having icons. By default, preference icon view visibility will be set to
             GONE when there is no icon provided, so the default value of this attribute is false.
             -->
        <attr name="iconSpaceReserved" format="boolean" />
        <attr name="android:iconSpaceReserved" />
    </declare-styleable>
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

R.xml.device_info_settings.xml
```xml
<PreferenceScreen xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:settings="http://schemas.android.com/apk/res-auto"
    android:title="@string/about_settings" >

    <!-- System update settings - launches activity -->
    <PreferenceScreen
        android:key="system_update_settings"
        android:summary="@string/system_update_settings_list_item_summary"
        android:title="@string/system_update_settings_list_item_title" >
        <intent android:action="android.settings.SYSTEM_UPDATE_SETTINGS" />
    </PreferenceScreen>

    <!-- Device hardware model -->
    <Preference
        android:enabled="false"
        android:key="device_name"
        android:shouldDisableView="false"
        android:summary="@string/device_info_default"
        android:title="@string/softap_equipments_name" />

    <!-- Device hardware model -->
    <Preference
        android:enabled="false"
        android:key="device_model"
        android:shouldDisableView="false"
        android:summary="@string/device_info_default"
        android:title="@string/model_number" />
...
```

其中 Preferencer是import android.support.v7.preference.Preference;
众思还自定义了一个com.android.settings.SettingsPreference extends Preference
Android.mk中引用了
```
LOCAL_STATIC_JAVA_LIBRARIES := \
    libcore \
    android-support-v4 \
    android-support-v13 \
    android-support-v7-recyclerview \
    android-support-v7-preference \

```