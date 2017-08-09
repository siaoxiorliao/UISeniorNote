# 问问

# hitTest
* 默认做法

```objectivec
//作用:寻找最适合的view.
//调用:当一个事件传递给当前view的时候调用,会调用两次
//返回值:返回谁,谁就是最适合的view,然后会调用最合适view的touch方法
- (UIView *)hitTest:(CGPoint)point withEvent:(UIEvent *)event{
    UIView *fitView = [super hitTest:point withEvent:event];
    NSLog(@"%@",fitView);
    return fitView;
}
```
## hitTest作用
* 拦截用户触摸事件

```objectivec
UIWindow.m
//重写UIWindow的hitTest事件,返回它子控件为最适合的view,
//则事件响应链条为 UIWindow->UIViewController->WhiteView,
//WhiteView则会调用touchs方法(如果向上传递了事件,则会调用其他响应者的touchs方法)
//如果是默认做法,则调用whiteView的hitTest方法.
- (UIView *)hitTest:(CGPoint)point withEvent:(UIEvent *)event{
//    UIView *fitView = [super hitTest:point withEvent:event];
//    NSLog(@"%@",fitView);
    return self.subviews[0];
}
```