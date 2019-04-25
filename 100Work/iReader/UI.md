- R6002_608 ['122e11d4ea9e4867']  1448x1072 6寸 1801 每寸300px  xhdpi 
DisplayMetrics{density=1.5, width=1072, height=1376, scaledDensity=1.5, xdpi=213.0, ydpi=213.0}
- R1001 --> 新版Smart 19年上市 1872x1404 10寸 hdpi 对角线 2340 每寸234px

- ActivityHome 主界面 4个Tab
 public static final Class TAB_CLASS_SHELF       = FmContainerShelf.class;  // 书架  
 public static final Class TAB_CLASS_STORE       = FmContainerOnline.class; // 书城  FragmentStoreMainNew  FragmentStoreMainChannel 
 public static final Class TAB_CLASS_LISTEN      = FmContainerAudioBook.class; // 听书 FragmentStoreMainAudio FragmentAudioBookModule FragmentAudioStoreChannel 频道是啥
 public static final Class TAB_CLASS_ACCOUNT     = FmContainerAccount.class; // 我的



AlertDialog 在UiUtil.showAlertDialog()
FragmentUserCloudBook 下过的书
FragmentUserConsumptionRecord 消费记录

# 适配
PageItemCount  控制不同设备不通条目个数
页面 | 10寸智能线 | 6寸-阅读线
---- | --------	| ------
下载管理-书籍 | 7 | 5
下载中 | 7 | 5
选集和批量下载 | 10 | 7
集管理 | 10 | 7
flavor 



BaseFragmentExport listPop没有点击动画
layout_setting_switch_tips_item 实验室列表item布局
下拉弹框:
TextView + ListPopupWindow
ToolBar+MenuItem + ListPopupWindow
空心 下拉图标 icon_menu_arrow_down
实心 下拉图标 icon_down_arrow  

EinkWebView 墨水屏的WebView
改高度测试是否能翻页? 加上显示页码的功能

createKeyEventDelagateView 代理类view
mKeyEventDispatchRootView
        setKeyEventDelegateView(createKeyEventDelagateView()); //处理keyEvent

- 生词本是自定义的生词本 DialogVocabularyDetail,自定义的layout.dialog_vocabulary_detail_layout
onUIHighLigth 显示弹出高亮功能块,阅读页长按 PopHighLightMenu layout.read_highlight_menu 阅读页的布局 注意横屏竖屏land
fragment_dictresult_layout

ShelfView 根据手势监听类点击位置判断属于哪个item 

drawListSelectedStatus 列表-画编辑时的选中状态
drawGridItemSelectedStatus 九宫格-
updateItemBookIsSelected 局部更新选中状态
drawGridItemFolder 画文件夹 
drawItemCover 画书籍图片 
