# 问问

# 如何找到最合适的view
* 所有操作都是从响应链最开端开始(UIApplication->UIWindow->UIView->....)

1> 自己是否接受触摸事件
2> 触摸点是否在自己身上
3> 从后往前遍历自己的子控件,重复上面两个步骤
(如果自己满足12,则遍历自己的子控件,如果子控件不满足,则自己就是最适合view,若满足,则遍历子控件的子控件,直到找到最适合的view)

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

# pointInside
* 判断当前事件的点是否在自己本身上
* 它是在hitTest上调用的,相当于2操作
* 以point点来判断,point点以调用者坐标系为基准来判断.

```objectivec
- (BOOL)pointInside:(CGPoint)point withEvent:(UIEvent *)event{
    return YES;
}
```

# hitTest底层实现
```objectivec
-(UIView *)hitTest:(CGPoint)point withEvent:(UIEvent *)event{
    //1.判断自己能否接收事件
    if(self.userInteractionEnabled == NO || self.hidden == YES || self.alpha <= 0.01) {
        return nil;
    }
    //2.判断当前点在不在当前View.
    if (![self pointInside:point withEvent:event]) {
        return nil;
    }
    //3.从后往前遍历自己的子控件.让子控件重复前两步操作,(把事件传递给,让子控件调用hitTest)
    int count = (int)self.subviews.count;
    for (int i = count - 1; i >= 0; i--) {
        //取出每一个子控件
        UIView *chileV =  self.subviews[i];
        //把当前的点转换成子控件从标系上的点.
        CGPoint childP = [self convertPoint:point toView:chileV];
        //把事件传递给,让子控件调用hitTest
        UIView *fitView = [chileV hitTest:childP withEvent:event];
        //判断有没有找到最适合的View
        if(fitView){
            return fitView;
        }
    }
    //4.没有找到比它自己更适合的View.那么它自己就是最适合的View
    return self;
}
```

* 利用触摸事件的传递和响应可以做出很好的效果