#DGDownloadManager
![image](https://github.com/liudiange/DGDownloadManager/blob/master/DGDownloadManager/DGDownloadManager/1.png)

下载管理器，只要传递下载的url来进行下载，支持ios 和macos      （Download Manager, as long as the URL passed to download download, support ios and macos）
 
## 下载管理器实现的功能：(Download Manager to achieve the function:)
- 支持断点续传  （Support HTTP）
- 支持最大的下载任务个数 （Support the largest number of download tasks）
- 传递url进行下载  （Pass url to download）
- 可以暂停全部  （ Pause all）
- 可以暂停某一链接  （Pause a link）
- 可以取消全部  （Cancel all）
- 可以取消某一链接 （Cancel a link）
- 可以恢复下载一链接 （Resume downloading a link）
- 可以恢复全部下载 （Resume all downloads）
## 思路（idea）
![image](https://github.com/liudiange/DGDownloadManager/blob/master/DGDownloadManager/DGDownloadManager/2.png)

##### 解释（explain）：
- 其中DGDownloadManager负责全局的管理，创建一个下载队列管理的数组，需要下载的添加进数组，下载完成的从数组中移除，并且发送通知下载已经完成。（DGDownload Manager is responsible for global management, creating an array of download queue management, adding downloads to the array, removing downloads from the array, and sending notifications that downloads have been completed）
- DGDownloaditem是每一个下载的模型。它提供了下载的相关的属性以及下载进度的block。（DGDownloaditem is a model for each download. It provides the relevant attributes of the download and the block of the download progress）
- DGHttpConfig存储发送通知的key（Storage key for sending notifications）。
## 安装 （install）
- 需要将 DGDownloadManagers拖入工程中  （need to drag DGDownloadManagers into the project）
- pod 'DGDownloadManager', ‘~>0.0.1'
- 注意mac Os 暂时不支持cocoapod  （Note that macos does not currently support cocoapod）
## 使用 （use）
- 开始下载(单一) （Start downloading (single))
````objc
// 指定最大任务数量 需要在下载歌曲前指定否则不起作用,默认最大任务是3
[DGDownloadManager shareManager].DG_MaxTaskCount = 2;
// 其中小不点.mp3为自定义的名字，可以为nil（Which 小不点.mp3 is a custom name, can be nil）
[[DGDownloadManager shareManager] DG_DownloadWithUrl:TEST_URL withCustomCacheName:@"小不点.mp3"];

````
- 批量下载 (Batch download)
````objc
   // 指定最大任务数量 需要在下载歌曲前指定否则不起作用,默认最大任务是3
    [DGDownloadManager shareManager].DG_MaxTaskCount = 2;
     NSArray *customCacheName = @[@"上海滩.mp3",@"大长今.mp3",@"日子.mp3",@"想一想.mp3",@"哈哈.mp3"];
    [self.dataArray removeAllObjects];
    NSArray *list = @[
                      @"http://218.200.160.29/rdp2/test/mac/listen.do?contentid=6990539Z0K8&ua=Mac_sst&version=1.0",
                      @"http://218.200.160.29/rdp2/test/mac/listen.do?contentid=63880300430&ua=Mac_sst&version=1.0",
                      @"http://218.200.160.29/rdp2/test/mac/listen.do?contentid=6005970S6G0&ua=Mac_sst&version=1.0",
                      @"http://218.200.160.29/rdp2/test/mac/listen.do?contentid=63273401896&ua=Mac_sst&version=1.0",
                      @"http://218.200.160.29/rdp2/test/mac/listen.do?contentid=69906300114&ua=Mac_sst&version=1.0"
                      ];
    // 指定最大任务数量
    [DGDownloadManager shareManager].DG_MaxTaskCount = 4;
    for (NSInteger index = 0; index < list.count; index ++) {
        [[DGDownloadManager shareManager] DG_DownloadWithUrl:list[index] withCustomCacheName:customCacheName[index]];
    }
````
- 暂停全部 (Pause all)
````objc

 [[DGDownloadManager shareManager] DG_SuspendAllRequest];
 
````
- 暂停某一链接 (Pause a link)
````objc

[[DGDownloadManager shareManager] DG_SuspendWithUrl:@"http://218.200.160.29/rdp2/test/mac/listen.do?contentid=69906300114&ua=Mac_sst&version=1.0"];

````
- 取消全部 (Cancel all)
````objc

  [[DGDownloadManager shareManager] DG_CancelAllRequest];
  
````
- 取消某一链接 (Cancel a link)
````objc

 [[DGDownloadManager shareManager] DG_CancelWithUrl:@"http://218.200.160.29/rdp2/test/mac/listen.do?contentid=69906300114&ua=Mac_sst&version=1.0"];
 
````
- 恢复所有 (Restore all)
````objc

 [[DGDownloadManager shareManager] DG_ResumeAllRequest];
 
````
- 恢复一首链接 (Restore a link)
````objc

 [[DGDownloadManager shareManager] DG_ResumeWithUrl:@"http://218.200.160.29/rdp2/test/mac/listen.do?contentid=69906300114&ua=Mac_sst&version=1.0"];
 
```` 
- 修改最大的下载的任务数量 (Modify the maximum number of downloaded tasks)
````objc

    // 指定最大任务数量 需要在下载歌曲前指定否则不起作用,默认最大任务是3
    [DGDownloadManager shareManager].DG_MaxTaskCount = 2;
    
````
- 修改默认的最大下载的任务数量 (Modify default the maximum number of downloaded tasks)
````objc

     search MAXTASK_COUNT ,update it
     
````
- 获取下载进度(Get download progress)

````objc

   // 需要自己获取当前下载的item(Need to get the current download item)
   
  item.DGDownloadProgressBlock = ^(CGFloat progress) {
        dispatch_async(dispatch_get_main_queue(), ^{
            weakSelf.progressView.progress = progress;
            weakSelf.progressLable.text = [NSString stringWithFormat:@"%d%%",(int)(progress *100)];
        });
    };
    
````
- 获取下载的状态(Get the status of the download)

````objc

  // 需要自己获取当前下载的item(Need to get the current download item)
   item.downloadStatus
   
````


## 期待(hope)
- 有什么bug或者我不满足的需求，欢迎 Issues我(There are any bugs or not satisfied, welcome to issues me)
- 请大神给我指正或者建议，我将不胜感激。(Please give me advice or suggestions, I will be honored)

 








