---
title: FileBrowser文件管理器竞品分析
tags: Yota,Markdown,Roger
grammar_cjkRuby: true
---

# 文件管理的基础功能
- 列表显示文件夹和文件
- 剪切，复制，粘帖功能
- 创建文件夹功能

# 谷歌的Files Go


# 华为手机的文件管理器功能
- 分类功能 :图片,音频,视频,文档,压缩包,应用,收藏
- 按来源浏览:相机,微信,截屏,浏览器,音乐...
- 最近30天 一个列表
- 压缩
- 保密柜
- 云收藏
- 桌面快捷方式

![华为8.1](./images/huaweiFileBrowser8.png "huaweiFileBrowser8")

![enter description here](./images/huaweiFileBrowser8_editFile.png "huaweiFileBrowser8_editFile")

# ES文件浏览器
- 分析:大文件,清理
- 回收站
- 投屏,远程管理,应用锁,压缩管理,快传

![enter description here](./images/ESFileBrowser.png "ESFileBrowser")

![enter description here](./images/ESFileBrowser_EditFile.png "ESFileBrowser_EditFile")

# 源生界面和功能
- 默认有勾选框

# 其他功能
- 针对30多种不同文件类型显示不同的图标
- 显示或者隐藏文件
- 支持列表方式进行文件浏览 
- 支持显示 APK 图标

# zeusis众思的FileBrowser
依赖的commonui控件:
搜索commonui,搜到了194项
```java
import com.zeusis.commonui.widget.dialog.ZeusisDialog;
import com.zeusis.commonui.widget.listview.ZeusisListView;
import com.zeusis.commonui.widget.pulltorefresh.PullToRefreshLayout;
```
但其中代码使用了的控件不好计算,估计差不多有一千处

搜索Zeusis,搜到了314项 