#### 整体流程
- 1 整体UI的搭建
- 2 给图片VIEW添加动画
- 3 进行播放操作
- 4 进度条的设置
- 5 使用的播放器AVAudioPlayer
- 6 歌词的实现
- 7 歌词的染色 
- 8 手机后台播放 a 后台模式 b AppDelegate
- 9 自动播放下一首，代理方法


####  1 修改APP的 状态栏颜色
- 1 单个控制器修改

```
- (UIStatusBarStyle)preferredStatusBarStyle
{
    return UIStatusBarStyleLightContent;
}
```
####  2 毛玻璃效果

- 1 美工
- 2 第三方框架
- 3 CoreImage：图片的高斯模糊
- 4 UIToolBar

```
    UIToolbar *toolBar = [[UIToolbar alloc] init];
    [toolBar setBarStyle:UIBarStyleBlack];
    [self.albumView addSubview:toolBar];
    toolBar.translatesAutoresizingMaskIntoConstraints = NO;
```
#### 3 进度界面的更新
- 要添加定时器，进行时间的刷新，更新和移除
###### 这里需要的是在停止的时候要重新给timer 赋值为nil
- silder 的事件处理，拖动停止完成时候更新，还有一个change的监听

```
//点击的时候时间改变，然后进行更新
- (IBAction)sliderClick:(UITapGestureRecognizer *)sender {
    // 1.获取点击的位置
    CGPoint point = [sender locationInView:sender.view];
    
    // 2.获取点击的在slider长度中占据的比例
    CGFloat ratio = point.x / self.progressSlider.bounds.size.width;
    
    // 3.改变歌曲播放的时间
    self.currentPlayer.currentTime = ratio * self.currentPlayer.duration;
    
    // 4.更新进度信息
    [self updateProgressInfo];
}
```

#### 4 后台播放

```
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
    
    // 1.获取音频回话
    AVAudioSession *session = [AVAudioSession sharedInstance];
    
    // 2.设置后台播放类别
    [session setCategory:AVAudioSessionCategoryPlayback error:nil];
    
    // 3.激活回话
    [session setActive:YES error:nil];
    
    return YES;
}


```
#### 5 自动下一首

```
#pragma mark - AVAudioplayer的代理方法
- (void)audioPlayerDidFinishPlaying:(AVAudioPlayer *)player successfully:(BOOL)flag
{
    if (flag) {
        [self next];
    }
}


```
