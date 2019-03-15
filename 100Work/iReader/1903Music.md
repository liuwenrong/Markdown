package com.android.music

- MusicBrowserActivity 音乐浏览 首页
- TabWidget  音乐人 | 专辑 | 歌曲 | 播放列表 
- MusicPicker R.layout.media_picker_activity
- TrackListAdapter 
	- bindView 更新List的UI
- ArtistAlbumBrowserActivity 艺术相册 艺人/音乐人浏览 R.layout.media_picker_activity_expanding  点击专辑,再点击 进入 TrackBrowserActivity
- AlbumBrowserActivity 相册/专辑浏览器 R.layout.media_picker_activity
- TrackBrowserActivity 歌曲  Intent PLAYBACK_VIEWER R.layout.media_picker_activity 
            if (intent.getBooleanExtra("withtabs", false)) { //是否显示顶部tabs
                requestWindowFeature(Window.FEATURE_NO_TITLE);
            }
            setOnCreateContextMenuListener  长按点击事件
            onListItemClick

- PlaylistBrowserActivity extend ListActivity 播放列表  R.layout.media_picker_activity
	- onListItemClick 
- MediaPlaybackActivity 媒体播放页面 R.layout.audio_player  audio_player_common  加了顶部返回 底部的操作栏seekBar
layout-port-finger-854x480/audio_player.xml  居然是这个
- AudioPreview 音频预览
- 歌曲列表item  track_list_item_common

- PlayerPresenter 发Msg 内含PlayInfo 提供音乐的信息,如名字,id
- TingMediaPlayer 听书的播放器对象  -- > IMediaPlaybackService
- PlayInfo ---> TrackItem 
 * @param pos The position to seek to, in milliseconds  单位毫秒
     */
    public long seek(long pos) {
	}

位置和时长换算 
		pos = mService.position();
        int progress = (int) (1000 * pos / mDuration);
		mService.duration(); 
        mCurrentMediaPlayer.start();  需要 android.permission.WAKE_LOCK

       问题 
调用 MediaPlaybackActivity.refreshNow()
   service 没空,里面的player空了 
onUnbind() --->  stopSelf(mServiceStartId); -->  MediaPlaybackService.onDestroy() --> mPlayer = null;
?? MediaPlaybackActivity.onStop() 没调用,没给mService置空

封装 MultiPlayer 
需要哪些方法 play  stop 
MediaCompletionListener  MediaPrepareListener 

    public final static void startActivityMusic(Activity act) {
//        Intent intent = ERouter.with("eink://music/browser").getIntent();
        Intent intent = ERouter.with("eink://music/player").getIntent();
        act.startActivity(intent);
    }


PlayerDataFetcher  可以从目录扫描播放列表 
怎么更新playInfo的信息

问题:
1. 听书后,播放音乐,听书没有停止 
2. 全局控制器退出后,应该被stop-->recycle了, 点击音乐没法启动  √
3. 录音无法播放,调用start就播放完成了
4. 啥时候停止计时器更新 ?? 

Fragment.onCreateView()  --> mPlayerPresenter.onPlay(mPlayingInfo);  -->   AudioController.getInstance().play(playInfo);
--> AudioController.play(mPlayingInfo.mFilePath, null, 0f);  -->  MusicMediaPlayer.play(path);
--> MusicMediaPlayer.start() --> IMusicMedia.start()

1. 删除一并把系统音乐数据库删除
2. 删除文件要更新AudioController的列表??
3. 录音点列表崩溃(音乐插件未安装)
4. 录音列表隐藏 音乐人 未知
5. 重命名后更新AudioController
