# 图片折叠效果
* 用两张图片
* 一张图片的一部分表示固定不变的,另一部分做绕x轴的旋转动作

## 根据手势偏移做出旋转,并且有透视的效果

```objectivec
- (IBAction)pan:(UIPanGestureRecognizer *)sender {
    //获取手势移动偏移量
    CGPoint transP = [sender translationInView:sender.view];
    CGFloat angle = transP.y/200 * M_PI;//最大值则是pai
//    self.topImageView.layer.transform = CATransform3DMakeRotation(-angle, 1, 0, 0);
    //近大远小的透视效果m34
    CATransform3D transform = CATransform3DIdentity;
    //-1除以的数就是眼睛离屏幕的距离
    transform.m34 = -1 / 300.0;
    self.topImageView.layer.transform = CATransform3DRotate(transform, -angle, 1, 0, 0);
}
```

## 渐变层
* 渐变
```objectivec
    CAGradientLayer *gradientL = [CAGradientLayer layer];
    gradientL.frame = self.bottomImageView.bounds;
    //渐变颜色
    gradientL.colors = @[(id)[UIColor clearColor].CGColor, (id)[UIColor blackColor].CGColor];
    //渐变起点和终点
    gradientL.startPoint = CGPointMake(0.5, 0);
    gradientL.endPoint = CGPointMake(0.5, 1);
    gradientL.opacity = 0;
    //一个颜色到另一个颜色
//    gradientL.locations = @[@0.2,@0.6];
    [self.bottomImageView.layer addSublayer:gradientL];
    self.gradientL = gradientL;
```
* 弹性复位操作
```objectivec
    if(sender.state == UIGestureRecognizerStateEnded){
        //复位
        //弹性效果
        self.gradientL.opacity = 0;
        [UIView animateWithDuration:0.5 delay:0 usingSpringWithDamping:0.1 initialSpringVelocity:0 options:UIViewAnimationOptionCurveEaseInOut animations:^{
            self.topImageView.layer.transform = CATransform3DIdentity;
        } completion:nil];
    }
```

