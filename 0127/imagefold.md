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