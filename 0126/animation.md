# 核心动画Core Animation/核心动画和UIView动画
# 核心动画
* Core Animation可以用在Mac OS X和iOS平台
* 动画执行过程都是在后台操作的,不会阻塞主线程.
* 直接作用在CALayer上

* 核心提供的效果有很多,具体看 **PPT 20 核心动画**

![](/0126/images/WX20170814-102228.png)

## 属性
    duration：动画的持续时间
    repeatCount：重复次数，无限循环可以设置HUGE_VALF或者MAXFLOAT
    repeatDuration：重复时间
    removedOnCompletion：默认为YES，代表动画执行完毕后就从图层上移除，图形会恢复到动画执行前的状态。如果想让图层保持显示动画执行后的状态，那就设置为NO，不过还要设置fillMode为kCAFillModeForwards
    fillMode：决定当前对象在非active时间段的行为。比如动画开始之前或者动画结束之后
    beginTime：可以用来设置动画延迟执行时间，若想延迟2s，就设置为CACurrentMediaTime()+2，CACurrentMediaTime()为图层的当前时间
    timingFunction：速度控制函数，控制动画运行的节奏
    delegate：动画代理
    
fillMode（要想fillMode有效，最好设置removedOnCompletion = NO）

    kCAFillModeRemoved 这个是默认值，也就是说当动画开始前和动画结束后，动画对layer都没有影响，动画结束后，layer会恢复到之前的状态
    kCAFillModeForwards 当动画结束后，layer会一直保持着动画最后的状态
    kCAFillModeBackwards 在动画开始前，只需要将动画加入了一个layer，layer便立即进入动画的初始状态并等待动画开始。
    kCAFillModeBoth 这个其实就是上面两个的合成.动画加入后开始之前，layer便处于动画初始状态，动画结束后layer保持动画最后的状态

## CABasicAnimation - 单值
```objectivec
-(void)touchesBegan:(NSSet<UITouch *> *)touches withEvent:(UIEvent *)event {
    //1.创建动画对象(设置layer的属性值.)
    CABasicAnimation *anim = [CABasicAnimation animation];
    //2.设置属性值
    anim.keyPath = @"position.x";
    anim.toValue = @300;
    //动画完成时, 会自动删除动画
    anim.removedOnCompletion = NO;
    anim.fillMode = @"forwards";
    //3.添加动画
    [self.redView.layer addAnimation:anim forKey:nil];   
}
//心跳效果
-(void)touchesBegan:(NSSet<UITouch *> *)touches withEvent:(UIEvent *)event {
    //创建动画对象
    CABasicAnimation *anim = [CABasicAnimation animation];
    //设置属性值
    anim.keyPath = @"transform.scale";
    anim.toValue = @0;
    //设置动画执行次数
    anim.repeatCount = MAXFLOAT;
    //设置动画执行时长
    anim.duration = 1;
    //自动反转(怎么样去 怎么样回来)
    anim.autoreverses = YES;
    //添加动画
    [self.imageV.layer addAnimation:anim forKey:nil];
}
```
## CAKeyframeAnimation - 多值
```objectivec
//图标抖动
- (void)icon {
    //1.创建动画对象
    CAKeyframeAnimation *anim = [CAKeyframeAnimation animation];
    //2.设置属性值
    anim.keyPath = @"transform.rotation";
    anim.values = @[@(angle2Rad(-3)),@(angle2Rad(3)),@(angle2Rad(-3))];
    //@[@(angle2Rad(-3)),@(angle2Rad(3))]
    //3.设置动画执行次数
    anim.repeatCount =  MAXFLOAT;
    anim.duration = 0.2;
    //anim.autoreverses = YES;
    [self.imageV.layer addAnimation:anim forKey:nil];
}
//路径动画
-(void)touchesBegan:(NSSet<UITouch *> *)touches withEvent:(UIEvent *)event {
    CAKeyframeAnimation *anim = [CAKeyframeAnimation animation];
    anim.duration = 2;
    UIBezierPath *path = [UIBezierPath bezierPath];
    [path moveToPoint:CGPointMake(50, 50)];
    [path addLineToPoint:CGPointMake(300, 50)];
    [path addLineToPoint:CGPointMake(300, 400)];
    anim.keyPath =  @"position";
    //根据路径来开始动画
    anim.path = path.CGPath;
    [self.imageV.layer addAnimation:anim forKey:nil];
}
```
## CATransition - 转场动画
* 苹果为我们提供了很多的转场效果

* 切换图片转场:

```objectivec
static int _i = 1;
-(void)touchesBegan:(NSSet<UITouch *> *)touches withEvent:(UIEvent *)event {
    //转场代码与转场动画必须得在同一个方法当中.
    //转场代码
    _i++;
    if (_i == 4) {
        _i = 1;
    }
    NSString *imageName = [NSString stringWithFormat:@"%d",_i];
    self.imageV.image = [UIImage imageNamed:imageName];
    //添加转场动画
    CATransition *anim = [CATransition animation];
    anim.duration  = 1;
    //设置转场的类型
    anim.type = @"pageCurl";
    //设置动画的起始位置
    anim.startProgress = 0.3;
    //设置动画的结束位置
    anim.endProgress = 0.8;
    [self.imageV.layer addAnimation:anim forKey:nil];
    
    //第二种
    [UIView transitionWithView:self.imageV duration:0.5 options:UIViewAnimationOptionTransitionCurlUp animations:^{
        //转场 代码
        _i++;
        if (_i == 4) {
            _i = 1;
        }
        NSString *imageName = [NSString stringWithFormat:@"%d",_i];
        self.imageV.image = [UIImage imageNamed:imageName];
    } completion:^(BOOL finished) {   
    }];
}
```

## 动画组 CAAnimationGroup
* 是CAAnimation的子类，可以保存一组动画对象，将CAAnimationGroup对象加入层后，组中所有动画对象可以同时并发运行

```objectivec
-(void)touchesBegan:(NSSet<UITouch *> *)touches withEvent:(UIEvent *)event {
    CABasicAnimation *anim = [CABasicAnimation animation];
    anim.keyPath = @"position.y";
    anim.toValue = @400;
//    anim.removedOnCompletion = NO;
//    anim.fillMode = kCAFillModeForwards;
//    [self.redView.layer addAnimation:anim forKey:nil];
    
    CABasicAnimation *anim2 = [CABasicAnimation animation];
    anim2.keyPath = @"transform.scale";
    anim2.toValue = @0.5;
//    anim2.removedOnCompletion = NO;
//    anim2.fillMode = kCAFillModeForwards;
//    [self.redView.layer addAnimation:anim2 forKey:nil];
    CAAnimationGroup *group = [CAAnimationGroup animation];
    //会自动执行animations数组当中所有的动画对象,统一操作;
    group.animations = @[anim,anim2];
    group.removedOnCompletion = NO;
    group.fillMode = kCAFillModeForwards;
    [self.redView.layer addAnimation:group forKey:nil];
}
```

## CAAnimationDelegate
* 可以监听动画执行过程

> 
//动画开始时执行
-(void)animationDidStart:(CAAnimation *)anim 
//动画完成时执行
-(void)animationDidStop:(CAAnimation *)anim finished:(BOOL)flag


# 核心动画和UIView动画的区别
   1.核心动画只作用在layer.
   2.核心动画看到的都是假像,它并没有去修改UIView的真实位置.
   > (当你点击使用核心动画后的view是触发不了touchBegan的,但是点击原来的位置就可以触发.用UIView则不用担心动画前和动画后)
   
# 什么时候使用核心动画?
   1.当不需要与用户进行交互,使用核心动画
   2.当要根据路径做动画时,使用核心动画
   3.当做转场动画时, 使用核心动画 (核心动画转场类型比较多)

