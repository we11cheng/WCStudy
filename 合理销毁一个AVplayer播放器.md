### 合理销毁一个AVplayer播放器

```
@property (nonatomic, strong) AVPlayer *player;          //播放器
@property (nonatomic, strong) AVPlayerItem *playerItem;

//销毁播放器
- (void)destroyPlayer{
	 //移除监听对象
    [self removeObserver];
    //暂停
    [_player pause];
    //清除
    [self.player setRate:0];
    [self.player replaceCurrentItemWithPlayerItem:nil];
    self.playerItem = nil;
    self.player = nil;
}
-(void)removeObserver{
    [[NSNotificationCenter defaultCenter] removeObserver:self name:AVPlayerItemDidPlayToEndTimeNotification object:_playerItem];
    [_playerItem removeObserver:self forKeyPath:@"status"];
    [_playerItem removeObserver:self forKeyPath:@"loadedTimeRanges"];
    [_playerItem removeObserver:self forKeyPath:@"playbackBufferEmpty"];
    [_playerItem removeObserver:self forKeyPath:@"playbackLikelyToKeepUp"];
}
```