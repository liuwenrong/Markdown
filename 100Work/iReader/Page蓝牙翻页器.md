### 翻页界面
- StorePageLinearLayout 
- PageLinearLayout
- 
1. Delegate
- EinkSupportFragment
	- onPreHandleKeyEvent

- LinearLayoutDelegate
	- dispatchKeyEvent
		- onDelegateKeyEvent

    private boolean onDelegateKeyEvent(KeyEvent event) {
        int keyCode = event.getKeyCode();
        return mDelegateView != null && (keyCode == KeyEvent.KEYCODE_VOLUME_DOWN || keyCode == KeyEvent.KEYCODE_VOLUME_UP);
    }
   	return mDelegateView.dispatchKeyEvent(event); 
- AutoPageTextLayout  
    - dispatchKeyEvent
- PageLinearLayout
	- dispatchKeyEvent //翻页的具体实现

蓝牙翻页器排期:
1. 测试连接蓝牙,测试下发按键事件,熟悉具体需求 12.20-12.20(1天)
2. 处理按键事件,翻页,看能否在基类中处理,不能的话只能在需要翻页的页面去实现翻页功能 12.21-12.24(2天)

按键	| Game/APP模式 - scanCode 	| Key/Reader模式 - scanCode
---	| 	---------	| ----------
上	| KEYCODE_DPAD_UP 19 -103 | KEYCODE_DPAD_RIGHT 22 -106	|
下	| KEYCODE_DPAD_DOWN 20 -108	| KEYCODE_DPAD_LEFT 21 -105
左	| KEYCODE_VOLUME_UP 24 -115	| KEYCODE_DPAD_UP 19 -103
右	| KEYCODE_VOLUME_DOWN 25 -114	| KEYCODE_DPAD_DOWN 20 -108
X	| 小米监测不到,打开系统多任务菜单	| KEYCODE_MENU 82 -127
Y	| KEYCODE_ESCAPE 111 -1	| KEYCODE_ESCAPE 111 -1 
A 	| KEYCODE_PAGE_DOWN 93 -109	| KEYCODE_PAGE_DOWN 93 -109
B 	| 	| KEYCODE_ENTER 66 -0
切换	| KEYCODE_MEDIA_EJECT 129 -161	|

上下用来切换书籍


Activity 能获取事件, View也能监听物理按钮事件, 是否需要获取焦点?
onKeyDown
dispatchKeyEvent

ActivityRead
- registerOnKeyListener
- EinkKeyHandleFilter
	- OnEinkKeyListener
	- isNeedHandlePageKey  控制阅读页的是否需要处理的按键


问题: 
1. 连蓝牙输入设备,导致不能弹出输入法
2. 蓝牙果断时间会自动断开,大概锁屏一段时间
 
翻页跳转指定的item
@Override
    public boolean dispatchKeyEvent(KeyEvent event) {

    //1、通过判断ListView当前被选择item的position，如果是最后一条就setSelection(0);可达到循环的目的
        if (event.getAction() == KeyEvent.ACTION_DOWN) {
            if (KeyEvent.KEYCODE_DPAD_DOWN == event.getKeyCode()) {
                setSelection(getNextPosition());
                return true;
            }
        }
        //KeyEvent.ACTION_UP

        return super.dispatchKeyEvent(event);
    }

    //2、down事件到dispatchKeyEvent时，找下一个否符合要求item的position，再setSelection()
    //   达到"略过一些item，跳到指定item的效果"
    private int getNextPosition() {

//        for->
//            getSelectedItemPosition()
//            getAdapter().getCount()
//            getAdapter().getItemViewType()
//            return 符合要求返回;

        return getSelectedItemPosition() + 2 < getAdapter().getCount()
                ? getSelectedItemPosition() + 2
                : getAdapter().getCount() - 1;
    }

# 蓝牙翻页器功能提测
## 影响模块
1. 所有能够翻页的页面,如书架,书城,网页

## 注意事项
1. 需要找朱文志刷蓝牙翻页器相关的版本
2. 连上BOOX蓝牙
3. Y键对应 KEYCODE_ESCAPE keyCode=111,代表返回当前页面,可用adb shell input keyevent 111模拟按键事件
4. B键对应 KEYCODE_ENTER keyCode=66,代表确定,可用于打开书架书架/笔记本
5. X键对应 KEYCODE_MENU keyCode=82, 代表菜单,因为目前没用到,所以用来书架/笔记本切换选择下一本 
6. A键对应 KEYCODE_PAGE_DOWN keyCode=93,代表翻页,可用于能翻页的页面翻到下一页

## 测试建议
暂无

固件 我给apk,朱工把蓝牙相关的系统修改烧成固件