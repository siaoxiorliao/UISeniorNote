##



# 触摸监听劣势
* 通过touches监听必须自定义控件,
* 监听方法只能在内部进行,外界无法监听
* 无法区别具体的手势行为

# 手势识别器UIGestureRecognizer
* 苹果推出了手势识别功能 Gesture Recognizer
* UIGestureRecognizer是一个抽象类,定义了基本的手势行为,使用它的子类才能处理具体手势,默认提供手势 : 

 1. UITapGestureRecognizer (敲击,点按)
 2. UIPinchGestureRecognizer(捏合,缩放)
 3. UIPanGestureRecognizer(拖拽)
 4. UISwipeGestureRecognizer(轻扫)
 5. UIRotationGestureRecognizer(旋转)
 6. UILongPressGestureRecognizer(长按)
 
## 使用手势
```objectivec
 UITapGestureRecognizer *tap = [[UITapGestureRecognizer alloc] init];
 // 连续敲击2次
 tap.numberOfTapsRequired = 2;
 // 需要2根手指一起敲击
 tap.numberOfTouchesRequired = 2;
 [self.iconView addGestureRecognizer:tap];
 [tap addTarget:self action:@selector(tapIconView:)];
 ```
 * 可根据手势状态和UIGestureRecognizerDelegate进行相应的逻辑实现
 
```objectivec
 //-(BOOL)gestureRecognizer:(UIGestureRecognizer *)gestureRecognizer shouldReceiveTouch:(UITouch *)touch;
 - (void)longP:(UILongPressGestureRecognizer *)longP{
    NSLog(@"%s",__func__);
    //判断手势的状态
    if (longP.state == UIGestureRecognizerStateBegan) {
        NSLog(@"开始长按");
    }else if(longP.state == UIGestureRecognizerStateChanged){
         NSLog(@"长按时移动");
    }else if(longP.state == UIGestureRecognizerStateEnded){
        NSLog(@"手指离开");
    }
}
```

