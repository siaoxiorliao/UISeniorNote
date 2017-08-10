# 位图上下文
* 系统默认没有开启位图上下文,需手动开启,开启多大,图片就是多大

```objectivec
- (void)viewDidLoad {
    [super viewDidLoad];
    //之前view上下文是在drawRect系统自动生成自己获取的,这里不需要,自己生成自己获取,还要自己关闭
    //0.加载图片
    UIImage *image = [UIImage imageNamed:@"阿狸头像"];
    //1.开启一个跟图片原始大小的上下文
    //opaque:不透明度,scale: 0是根据当前设备率设置上下文分辨率
    UIGraphicsBeginImageContextWithOptions(image.size, NO, 0);
    //2.把图片绘制到上下文当中
    [image drawAtPoint:CGPointZero];
    //3.把文字绘制到上下文当中
    NSString *str = @"小码哥";
    [str drawAtPoint:CGPointMake(10, 20) withAttributes:nil];
    //4.从上下文当中生成一张图片.(把上下文当中绘制的所有内容,生成一张图片)
    UIImage *newImage = UIGraphicsGetImageFromCurrentImageContext();
    //5.关闭上下文.
    UIGraphicsEndImageContext();
    self.imageV.image = newImage;
}
```