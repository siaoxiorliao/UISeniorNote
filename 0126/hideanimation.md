# 隐式动画
* UIView.layer是根层Root Layer
* 非根层都可以创建隐式动画,对这些非根层属性进行更改就会自动创建隐式冬瓜.这些属性即称可动画属性(bounds,position,backgroundColor)
* 可以通过设置动画事务(CATransaction)关闭默认的隐式动画效果
* 默认是有动画效果的(0.5秒?),只是代码隐藏了

```objectivec
//只有非根层才有隐式动画,(自己手动创建的图片)
    [CATransaction begin];
    [CATransaction setDisableActions:YES];//关闭动画效果
    self.layer.backgroundColor = [UIColor greenColor].CGColor;
    [CATransaction commit];
    //有动画效果
    [CATransaction setAnimationDuration:5];//动画时长
    self.layer.position = CGPointMake(100, 400);
```

# 时钟示例