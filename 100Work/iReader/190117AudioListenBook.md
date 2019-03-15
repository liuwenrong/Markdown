| 功能  | 时间 | 服务器完成时间 |
| --------- | ------------- | ------------- |
| 批量下载页 & 下载管理页 | 4 | 
| 下载管理中心 | 5 | 
| 云书架 & 搜索 | 3 |

# 相关类:
- core.download.Download 
- DOWNLOAD_INFO 下载进度信息
- DownloadManager 下载管理器，提供添加，暂停，删除，移除，暂停所有，保存数据等操作
- core.drm.DRMHelper  DRMUtil DRM校验,时间戳
- Eink.filedownload.FileDownloadManager extends DownloadManager 文件下载管理
- FileDownloadTasker extends Download 文件下载器
- Eink.APP.URL.URLBase 改网址/灰度
- FragmentStoreBookList.createPopFilter  StoreFilterHelper 筛选功能
	- onCreateToolbarMenu 筛选 选集
	- startForResult onFragmentResult   setFramgentResult(RESULT_CODE, data); 返回回调
- 全选功能
- com.zhangyue.iReader.api.FeeApi 计费订单
- FeeResponse 计费后的响应信息
        String url = AudioUrl.getUrlOrderUrl(mBookId, AudioUrl.ORDER_SOURCE.SOURCE_DETAIL, chapterIdList);
-     public static void startFeeChapter(int bookId, String url, FeeResult feeResult) {
AudioFeeResponseBody 对应接口文档-->听书订单1（单章、单本、自选）.md  -->转化成 FeeResponse 
- 钱不够 生成二维码,支付成功后?


- 黑三角图标在目录/书签 PopReadMenu & FragmentTextReader.onUIChapList()
- PopReadChapList ChapListAdapter icon_book_chapter_fold.png  icon_book_chapter_unfold.png

1404*1872  dp 936*
task.gradle 将EReader中需要在audio中用到的类方法打成jar包并放到指定目录
EReaderApi 暴露一些方法给插件使用

pg_audio_

    Process: com.zhangyue.iReader.Eink, PID: 2418
    java.lang.IllegalStateException: Fragment FragmentProgramSelection{41d61338} not attached to Activity
        at android.support.v4.app.Fragment.getResources(SourceFile:636)
        at android.support.v4.app.Fragment.getString(SourceFile:658)
        at eink.plugin.audiobook.fragment.FragmentProgramSelection$2.a(SourceFile:190)
        at eink.plugin.audiobook.fragment.FragmentProgramSelection$2.a(SourceFile:168)
        at eink.plugin.audiobook.bt$5.run(SourceFile:308)
        at android.os.Handler.handleCallback(Handler.java:733)
        at android.os.Handler.dispatchMessage(Handler.java:95)
        at android.os.Looper.loop(Looper.java:136)
        at android.app.ActivityThread.main(ActivityThread.java:5017)
        at java.lang.reflect.Method.invokeNative(Native Method)
        at java.lang.reflect.Method.invoke(Method.java:515)
        at com.android.internal.os.ZygoteInit$MethodAndArgsCaller.run(ZygoteInit.java:785)
        at com.android.internal.os.ZygoteInit.main(ZygoteInit.java:601)
        at dalvik.system.NativeStart.main(Native Method)

## 问题:
// 删除数据库错误 
        android.database.sqlite.SQLiteException: near "/": syntax error (code 1): , while compiling: DELETE FROM audioList WHERE programPath=/storage/emulated/0/iReader/ting/files/30010327/2.abk
   文件名 应该 前后加双引号 "" 或者new String[]{} 的方式

http://preparetest.ireader.com/zybook3/app/app.php?ca=Eink_Ting.BillingTemplate&usr=i1900530653&rgt=7&p1=XH%2FDmbLzoHwDAADPntRRuY99&pc=100&p2=109969&p3=876837&p4=501637&p5=19&p7=GAIGBJAAAAAAAAB&p16=R608&zysid=&zysign=&sign=uLIJtqallA2Qgl7RFmm4fwoTXUwJrb70m39d8D8clmIop3ayZTiEZLW8jdgCHnbBpj%2Bs6cMNmCuENBrFh9bygx6PJmGAvA6APHMnQX2%2BPildHTeiQBx2eDV%2BDJMhdYj7eaH%2FyxR52RDw1lhqz79yF2PlIZlfl3FNLgVIPmFEzVE%3D&timestamp=1551966555134&programId=1&autoBuy=0&onlyToken=0&bid=30011996&fromType=down&zysid=&zysign=

{"code":53000,"msg":"book is empty","body":{}}

bookId	string	是	书籍id

bookId | 书籍 /免费章节 
------- | -----
30008909 | 郭德纲剧场..
30010327 | 新闻乱播
30010774 | 钢铁是怎样炼成的 
30009313 | 童年的密码 
30011058 | 隋月唐歌 全免费 限时免费
30010349 | 人性的弱点 10集免费 买了12集
30011798 | <今古传奇> 59集 限时免费
30008905 | 郭德纲对口相声 34集 限时免费


1. 已下载存在数据库?  如何查询该文件是否已下载? 独立于主工程的新的数据库
bookId_programId 1.去数据库查?可能文件被手动删除或破坏 2.去文件中查? 效率?  
正在下载也要存数据库,不然APP退出后找不到
 下载完成更新听书 书籍数量等信息 点开集列表时对比两个表节目数量 KEY_BOOK_NEW_CHAP_COUNT ,查询文件是否存在,不相等则更新数量
1. 存数据库时机,addTask和StartTask 完成时
2. 下载进度,查看文件大小和数据库存的总文件大小, 下载完成存DownloadStatus
2. 参考从数据库恢复下载进度 EBK3DownloadManager.init
2. 下载完成的文件路径和下载时的临时路径fileTmpPath
2. 如何判断是否已下载: 书籍id目录下的abk文件 判断programId是否存在于abk文件中 FileIndex FileIndexItem, PageBookExport, ScanTool, 底层c不包含abk格式  
2. 采用直接读取文件是否存在的方式?

2. 订单计费
2. 多线程,后台服务,不绑定activity startService
3. 节目 全部-如何加载更多  参考 FragmentBookChapList StorePage  BookChapListPresenter
4. 下载路径如何定义 ChapDownloadManager extends DownloadManager   ChapDownload 
去掌阅app查看下载路径,是否在sdcard还是data/data
    public static String getChapDirInternal() {
        return getDataDataRoot() + "iReader/books/.chap/";
    }
        if (isInternalBook(bookID)) {
            chapDir = getChapDirInternal();
        }

        return chapDir + bookID + "_" + chapID + ".chap2";




1. 已下载和下载中： 下载管理 DownloadManagerFragment
下载中-刷新会非常频繁，是否考虑把下载完成后转移至已下载的交互变更为，下载完成的书籍，先保留在“下载中”，显示“下载完成”状态，然后等退出页面后再次进入，再变更至“已下载”列表

2. 下载过程，没有网络，下载队列应全部暂停，而不是跳过当前下载队列的下载中文件；重新联网后，应该自动联网继续下载队列
- BaiduTTSManager.initTTSBroadcastReceiver 监听网络变化

3. 下载管理-选集和全选下载 SelectDownloadFragment ---> FragmentAudioChapList 听书章节列表
下载管理页面：全部和其他选集选择后，页面自动切换为对应的选集列表，此时单击“批量下载”，进入的批量下载页面页应该是对应的选集列表
 批量下载 BatchDownloadFragment

4. 切换已下载和下载中  参考: FragmentUserCloudBook.mTopItemClickListener

 鼠戏 
 ebk3PathName /storage/emulated/0/iReader/audios/.chap/30011798_1
chapPathName:  /storage/emulated/0/iReader/audios/.chap/30011798_1.chap 
/storage/emulated/0/iReader/books/.chap/30011798_1.chap2.tmp
filePathName  /storage/emulated/0/iReader/books/.chap/30011798_1.chap2


preAudioUrl:
http://tingbk.d.ireader.com/group1/M00/1E/54/wKgKFFnsdgyEChgYAAAAAEFFSjY055786196.m4a?v=I5MNzRMi&t=wKgKFFnsdgw.
audioUrl:
 http://d.iting.ireader.com:21000/rt/downloadTing/enc_dl?downInfo=OGMEPMCRZBCUI9w3ZAO0rHRIb3dMjRixyJyq71UM5VsvhDqz1yDoeCJWzv_2WTBBNKYEobvslMJCTzLDErlw3XTARs7EwezEeE6A8UwBsXjx1b0K9a3ojusExu_1zvsWlppWILFczAblVTt5m72lqB8jDd7dejQrDWDstBxLfbvJoLQawb_WeK4jK3HTGz53kTYkFNRzO6lfXYJowPyWOzDQ4vd9QE50ryyrX33Jm7rhVPMPy6WI3O3jJ5GBmC6o&usr=i558082447

EReader/src/main/java/com/zhangyue/iReader/Eink/home/FmContainerOnline.java
             SupportFragment fragment = (SupportFragment) ERouter.with("eink://audiobook/detail").param("key_book_id", 30011798).getFragment();
            loadRootFragment(R.id.fm_root_container, fragment);

02-15 14:25:49.737 7034-7056/com.zhangyue.iReader.Eink E/SQLiteDatabase: Failed to open database '/data/user/0/com.zhangyue.iReader.Eink/databases/iReader.db'.
    android.database.sqlite.SQLiteDatabaseLockedException: database is locked (code 5): , while compiling: PRAGMA journal_mode
        at android.database.sqlite.SQLiteConnection.nativePrepareStatement(Native Method)
        at android.database.sqlite.SQLiteConnection.acquirePreparedStatement(SQLiteConnection.java:889)
        at android.database.sqlite.SQLiteConnection.executeForString(SQLiteConnection.java:634)
        at android.database.sqlite.SQLiteConnection.setJournalMode(SQLiteConnection.java:320)
        at android.database.sqlite.SQLiteConnection.setWalModeFromConfiguration(SQLiteConnection.java:294)
        at android.database.sqlite.SQLiteConnection.open(SQLiteConnection.java:215)
        at android.database.sqlite.SQLiteConnection.open(SQLiteConnection.java:193)
        at android.database.sqlite.SQLiteConnectionPool.openConnectionLocked(SQLiteConnectionPool.java:463)
        at android.database.sqlite.SQLiteConnectionPool.open(SQLiteConnectionPool.java:185)
        at android.database.sqlite.SQLiteConnectionPool.open(SQLiteConnectionPool.java:177)
        at android.database.sqlite.SQLiteDatabase.openInner(SQLiteDatabase.java:804)
        at android.database.sqlite.SQLiteDatabase.open(SQLiteDatabase.java:789)
        at android.database.sqlite.SQLiteDatabase.openDatabase(SQLiteDatabase.java:694)
        at android.app.ContextImpl.openOrCreateDatabase(ContextImpl.java:944)
        at android.content.ContextWrapper.openOrCreateDatabase(ContextWrapper.java:256)
        at android.database.sqlite.SQLiteOpenHelper.getDatabaseLocked(SQLiteOpenHelper.java:224)
        at android.database.sqlite.SQLiteOpenHelper.getWritableDatabase(SQLiteOpenHelper.java:164)
        at com.zhangyue.iReader.DB.DBAdapter.init(DBAdapter.java:249)
        at com.zhangyue.iReader.DB.DBAdapter.getInstance(DBAdapter.java:257)
        at com.zhangyue.iReader.DB.DBHelper.onUpgrade(DBHelper.java:45)
        at android.database.sqlite.SQLiteOpenHelper.getDatabaseLocked(SQLiteOpenHelper.java:257)
        at android.database.sqlite.SQLiteOpenHelper.getWritableDatabase(SQLiteOpenHelper.java:164)
        at com.zhangyue.iReader.DB.DBAdapter.init(DBAdapter.java:249)
        at com.zhangyue.iReader.DB.DBAdapter.getInstance(DBAdapter.java:257)
        at com.zhangyue.iReader.app.APP.initAPPData(APP.java:488)
        at com.zhangyue.iReader.Eink.Activity.ActivityWelcome$2.run(ActivityWelcome.java:75)
        at java.lang.Thread.run(Thread.java:841)

         Finalizing a Cursor that has not been deactivated or closed. database = /data/user/0/com.zhangyue.iReader.Eink/databases/iReader.db, table = booklist, query = SELECT * FROM booklist WHERE isnotebook=1 and (ext_int1 = 0) ORDER BY shelfweight desc ,readlasttime desc
    android.database.sqlite.DatabaseObjectNotClosedException: Application did not close the cursor or database object that was opened here

RuntimeException: Parcelable encountered IOException writing serializable object (name = eink.plugin.audiobook.bean.download.AudioProgramResponse)
        at android.os.Parcel.writeSerializable(Parcel.java:1316)

Caused by: java.io.NotSerializableException: eink.plugin.audiobook.bean.download.AudioProgramResponse$AudioProgram
        at java.io.ObjectOutputStream.writeNewObject(ObjectOutputStream.java:1364)

 注意事项:
 1. 测试下载的数据库,是否正常更新状态,
 2. 书架的数据库增加了讲书人字段,测试数据库升级

 http://59.151.93.132:12345/zybook3/app/app.php?ca=Eink_Ting.BillingTemplate&bid=30011798&programId=9&fromType=down&timestamp=1550556085&zyeid=76dd0519c116405e91bb1d4d879b8c5d&usr=i558082447&rgt=7&p1=W1lrFSiI6BQDAGimdwJMXWWm&pc=100&p2=109969&p3=876837&p4=501637&p5=19&p7=BHAAAAAAAAAAAAA&p16=R1001&zysid=&zysign= 
格式化:
{
    "ca": "Eink_Ting.BillingTemplate",
    "bid": "30011798",
    "programId": "9",
    "fromType": "down",
    "timestamp": "1550556085",
    "zyeid": "76dd0519c116405e91bb1d4d879b8c5d",
    "usr": "i558082447",
    "rgt": "7",
    "p1": "W1lrFSiI6BQDAGimdwJMXWWm",
    "pc": "100",
    "p2": "109969",
    "p3": "876837",
    "p4": "501637",
    "p5": "19",
    "p7": "BHAAAAAAAAAAAAA",
    "p16": "R1001",
    "zysid": "",
    "zysign": " "
}

 https://graytest.ireader.com/zybook3/app/app.php?ca=Eink_Ting.BillingTemplate&zysid=995ac36fadd82aecac1727c37dd3c744&usr=i558082447&rgt=7&p1=W1lrFSiI6BQDAGimdwJMXWWm&pc=100&p2=109969&p3=876837&p4=501637&p5=19&p7=BHAAAAAAAAAAAAA&p16=R1001&zysid=995ac36fadd82aecac1727c37dd3c744&zysign=yh61zNynkcREDF3LJ85Jl9kT%2BC%2BhVv%2BYCe1hW1GyyjVmIpkxNiGRvmjmmA3aSVoNCgPNpb%2FYZA9vjwhImB4ycA%3D%3D&sign=NxFDKYddoLQySlbkgrEiEZx0OQw3VjaCSPGN7m8Z8CVWUcL74vzrUQ6LxaxYmnrTEi1%2BY%2FTdVdxit6aYk%2BsYbyyWGoTxaL0lhewS7CAISWg3Y7UnuodW3phBPIfSNXWpCrHkFO86xOZ0nSQVdiWVZMt2RTYQuVu8WfUjBujxJvE%3D&timestamp=1550721188889&programId=1&autoBuy=0&onlyToken=0&bid=0&fromType=down&zysid=995ac36fadd82aecac1727c37dd3c744&zysign=ssP5GtXoTZRoKZQQnhYgXcodjiv6Kc%2BYqcCyvcmbOXULGToE%2FHNgqLh5ceBvL1AAafCeRyd6rvrf3pJ318Rskg%3D%3D

{
    "ca": "Eink_Ting.BillingTemplate",
    "zysid": "995ac36fadd82aecac1727c37dd3c744",
    "usr": "i558082447",
    "rgt": "7",
    "p1": "W1lrFSiI6BQDAGimdwJMXWWm",
    "pc": "100",
    "p2": "109969",
    "p3": "876837",
    "p4": "501637",
    "p5": "19",
    "p7": "BHAAAAAAAAAAAAA",
    "p16": "R1001",
    "zysign": "ssP5GtXoTZRoKZQQnhYgXcodjiv6Kc+YqcCyvcmbOXULGToE/HNgqLh5ceBvL1AAafCeRyd6rvrf3pJ318Rskg==",
    "sign": "NxFDKYddoLQySlbkgrEiEZx0OQw3VjaCSPGN7m8Z8CVWUcL74vzrUQ6LxaxYmnrTEi1+Y/TdVdxit6aYk+sYbyyWGoTxaL0lhewS7CAISWg3Y7UnuodW3phBPIfSNXWpCrHkFO86xOZ0nSQVdiWVZMt2RTYQuVu8WfUjBujxJvE=",
    "timestamp": "1550721188889",
    "programId": "1",
    "autoBuy": "0",
    "onlyToken": "0",
    "bid": "0",
    "fromType": "down"
}

 {"body":{},"msg":"book is empty","code":53000}

正常:
https://graytest.ireader.com/zybook3/app/app.php?ca=Eink_Ting.BillingTemplate&bid=30010327&programId=1&fromType=down&timestamp=1550721526&zysid=4352a5d1feedbd24c4025cea2d549dd9&usr=i1799784435&rgt=7&p1=W%2FKRNpnVUdkDABWZ9DR4JAFZ&pc=100&p2=109969&p3=876837&p4=501637&p5=19&p7=BAABBICAAAAAAAC&p16=R1001&zysid=4352a5d1feedbd24c4025cea2d549dd9&zysign=yzZuFDbxTTO1fZ3ImCXGPm1fvGtKBGTFdyqHkF3KF%2FFngo6I8Wr2o%2F1tovOrKMjd10n%2FQ%2BVRFumIjRGyVx7%2BjQ%3D%3D
格式化:
{
    "ca": "Eink_Ting.BillingTemplate",
    "bid": "30010327",
    "programId": "1",
    "fromType": "down",
    "timestamp": "1550721526",
    "zysid": "4352a5d1feedbd24c4025cea2d549dd9",
    "usr": "i1799784435",
    "rgt": "7",
    "p1": "W/KRNpnVUdkDABWZ9DR4JAFZ",
    "pc": "100",
    "p2": "109969",
    "p3": "876837",
    "p4": "501637",
    "p5": "19",
    "p7": "BAABBICAAAAAAAC",
    "p16": "R1001",
    "zysign": "yzZuFDbxTTO1fZ3ImCXGPm1fvGtKBGTFdyqHkF3KF/Fngo6I8Wr2o/1tovOrKMjd10n/Q+VRFumIjRGyVx7+jQ=="
}
结果:
{
    "code": 0,
    "msg": "success",
    "body": {
        "status": 2,
        "bookId": 30010327,
        "isPreview": "N",
        "audioDuration": 343000,
        "preAudioUrl": "",
        "preAudioDuration": 0,
        "chapterId": 1,
        "downloadInfo": {
            "code": 20099,
            "msg": "sign.required",
            "token": "",
            "type": "",
            "vipCode": 0,
            "vipStatus": 0,
            "audioUrl": "http://59.151.100.67:21701/rt/downloadTing/enc_dl?downInfo=h21Mt2lRoz-tErBUqspj8LBlYtpSIT_BdW67TQfGQIv3RSIJMS3MAPkUtMDo6sVkI014hhUGgOdtEdpom8GKyhSUTsYxb3JOLdZ23SQJXZJqiyh_MYggUrRux4Xg9uQidqYysYqG6eMlkM-VhnVganh5IRI6jR_eTW9fsFj7wOSRMCXD9hy8t_OC95TW6fC99NBu_WcM05_XnC8UDhX1lmIm0XY_73-Bj3f7_Qh0Lua1gL30wSfiOHSiYPXZBRFQlrU9jZIgjSe-AZEyJu0ZZw&usr=i1799784435",
            "isHighQuality": 0
        },
        "orderInfo": {
            "orderId": "",
            "name": "",
            "price": "",
            "url": ""
        },
        "action": ""
    }
}
http://59.151.100.67:21701/rt/downloadTing/enc_dl?downInfo=h21Mt2lRoz-tErBUqspj8MZHR6DAWSjtgQQR4YEkL-QvhDqz1yDoeCJWzv_2WTBBNKYEobvslMJCTzLDErlw3XTARs7EwezEeE6A8UwBsXh0ToJmgRmyWGGAF0Wqm2J9MUgEGX2V6yW1lYP1wlZhyNMQAFE1_wTy3yMhlpd1AwRDNtgJZm_PA7LONO3ULIwGSUgDEegn0M0BcqGGqkI8tABlMKgZkfxDbDlYF0N6ickgyOvXnt7MKq8mv4eLRjEH2qQUxK_Ae4psd1t63TcXCw&usr=i558082447&zysid=995ac36fadd82aecac1727c37dd3c744&usr=i558082447&rgt=7&p1=W1lrFSiI6BQDAGimdwJMXWWm&pc=100&p2=109969&p3=876837&p4=501637&p5=19&p7=BHAAAAAAAAAAAAA&p16=R1001&zysid=995ac36fadd82aecac1727c37dd3c744&zysign=IaKLmfPbWwptezC%2FWbRWOY6xrKtwMyJbHmVpsUJb7QsGsOiUUuSMS5MF%2FXINj%2FqRFyHUCxfw%2Boa6uG1eeaRPTw%3D%3D

//下载地址 无法访问这个ip  59.151.100.67  电脑网线和yanfa打不开 
http://59.151.100.67:21701/rt/downloadTing/enc_dl?downInfo=h21Mt2lRoz-tErBUqspj8FNlHEVo8gltkXj4tTWdPHMvhDqz1yDoeCJWzv_2WTBBNKYEobvslMJCTzLDErlw3XTARs7EwezEeE6A8UwBsXh0ToJmgRmyWGGAF0Wqm2J9MUgEGX2V6yW1lYP1wlZhyNMQAFE1_wTy3yMhlpd1AwRDNtgJZm_PA7LONO3ULIwGSUgDEegn0M0BcqGGqkI8tABlMKgZkfxDbDlYF0N6ickgyOvXnt7MKq8mv4eLRjEH2qQUxK_Ae4psd1t63TcXCw&usr=i558082447

正常访问的下载地址 d.iting.ireader.com
http://d.iting.ireader.com:21000/rt/downloadTing/enc_dl?downInfo=d4BLLfgQX1zraxV04yauB0pHfIhVXjqcgr6fXrkFr0TzgB3HJtLMgKybg4S44tjaa_Wd5RU2e3oCPEeAoSoWx3TARs7EwezEeE6A8UwBsXjx1b0K9a3ojusExu_1zvsWlppWILFczAblVTt5m72lqB8jDd7dejQrDWDstBxLfbtJyxZJIlNxhu966ZYQVfBvOQbNnVG5bUp-Zo1RQJk2Vy6fWGa2rxhKh-9WGVCZFZ_rYgVpq-rAsRoZetH3hDULuO5D-rOHKU05pLgk603Mbg&usr=i1459430843

30010349
1919418  1924096   7849061
mSql = select sum(downTotalSize) from audioList where bookId=30010349 and downStatus=4

NotSerializableException: java.util.AbstractList$SubAbstractListRandomAccess

最后发现是因为CommodityBean中有个list列表是sublist生成的,
groupInfoBeanList.subList(0, 1)
。这样就无法序列化。。改成
new ArrayList(groupInfoBeanList.subList(0, 1))

 result = <!DOCTYPE>
    <html>
    <head>
    <meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0, minimum-scale=1.0, maximum-scale=1.0,user-scalable=no">
    <style type="text/css">
    @charset "utf-8";
    /*iReader CSS reset @author:gaodongmei;by:2014-4-23*/
    * { -webkit-tap-highlight-color: rgba(0, 0, 0, 0); -webkit-touch-callout:none;}
    body,div,p,blockquote,hr,ul,ol,h1,h2,h3,h4,h5,h6,menu,dd,dl,form,fieldset,input,textarea,caption,select,button,input,pre,hr,td,th,address,section,article,aside,header,footer,nav,dialog,figure,hgroup,label{margin:0px; padding:0}
    html{-webkit-text-size-adjust:100%;}
    html{ height: 100%}
    body {-webkit-text-size-adjust: none; background: #f3f3f3; font-size:14px; color:#555; font-family:Tahoma,"微软雅黑", Geneva, sans-serif; line-height:150%; }
    h1, h2, h3, h4, h5 { font-weight: normal; font-size:14px}
    em,i{ font-style: normal; }
    ul, ol, li { list-style: none }
    img{border: 0;vertical-align: middle;}
    a {text-decoration: none; color: #333333; outline:none}
    input,select,textarea{font-size:100%;border:none;outline: none; }
    select,textarea{-webkit-appearance:none;}
    /*clear float*/
    .clear{clear:both;}
    .clearfix {zoom: 1 }
    .clearfix:after {content: ".";display: block; height: 0; clear: both; visibility: hidden; }
    /*model style*/
    .textL{text-align:left;}
