# 位图上下文 - 图片水印
* 利用位图上下文可以为图片添加水印
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

# 裁剪圆形头像

```objectivec
- (void)viewDidLoad {
    [super viewDidLoad];
    //0.加载图片
    UIImage *image = [UIImage imageNamed:@"阿狸头像"];
    //1.开启跟原始图片一样大小的上下文
    UIGraphicsBeginImageContextWithOptions(image.size, NO, 0);
    //2.设置一个圆形裁剪区域
    //2.1绘制一个圆形
    UIBezierPath *path = [UIBezierPath bezierPathWithOvalInRect:CGRectMake(0, 0, image.size.width, image.size.height)];
    //2.2.把圆形的路径设置成裁剪区域
    [path addClip];//超过裁剪区域以外的内容都给裁剪掉
    //3.把图片绘制到上下文当中(超过裁剪区域以外的内容都给裁剪掉)
    [image drawAtPoint:CGPointZero];
    //4.从上下文当中取出图片
    UIImage *newImage =  UIGraphicsGetImageFromCurrentImageContext();
    //5.关闭上下文
    UIGraphicsEndImageContext();
    self.imageV.image = newImage;
}
```
## 带边框的裁剪
* 绘制比图片大的大圆和裁剪图片的小圆,渲染大圆大小的上下文即可

```objectivec
+ (UIImage *)imageWithBorder:(CGFloat)borderW color:(UIColor *)borderColor image:(UIImage *)image{
    //0.加载图片
    //UIImage *image = [UIImage imageNamed:@"阿狸头像"];
    //1.确定边框宽度
    //CGFloat borderW = 5;
    //2.开启一个上下文
    CGSize size = CGSizeMake(image.size.width + 2 * borderW, image.size.height + 2 * borderW);
    UIGraphicsBeginImageContextWithOptions(size, NO, 0);
    //3.绘制大圆,显示出来
    UIBezierPath *path = [UIBezierPath bezierPathWithOvalInRect:CGRectMake(0, 0, size.width, size.height)];
    [borderColor set];
    [path fill];
    //4.绘制一个小圆,把小圆设置成裁剪区域
    UIBezierPath *clipPath = [UIBezierPath bezierPathWithOvalInRect:CGRectMake(borderW, borderW, image.size.width, image.size.height)];
    [clipPath addClip];
    //5.把图片绘制到上下文当中
    [image drawAtPoint:CGPointMake(borderW, borderW)];
    //6.从上下文当中取出图片
    UIImage *newImage = UIGraphicsGetImageFromCurrentImageContext();
    //7.关闭上下文
    UIGraphicsEndImageContext();
    return newImage;
}
```

# 屏幕截图
* 对控制器的view截图然后生成一张图片

```objectivec
-(void)touchesBegan:(NSSet<UITouch *> *)touches withEvent:(UIEvent *)event {
    //把控制器的View生成一张图片
    //1.开启一个位图上下文(跟当前控制器View一样大小的尺寸)
    UIGraphicsBeginImageContextWithOptions(self.view.bounds.size, NO, 0);
    //把把控制器的View绘制到上下文当中.
    //2.想要把UIView上面的东西给绘制到上下文当中,必须得要使用渲染的方式.
    CGContextRef ctx = UIGraphicsGetCurrentContext();
    [self.view.layer renderInContext:ctx];
    //[self.view.layer drawInContext:ctx];
    //3.从上下文当中生成一张图片
    UIImage *newImage = UIGraphicsGetImageFromCurrentImageContext();
    //4.关闭上下文
    UIGraphicsEndImageContext();
    //把生成的图片写入到桌面(文件方式进行传输:二进制流NSData)
    //把图片转成二进制流NSData
    //NSData *data = UIImageJPEGRepresentation(newImage, 1);
     NSData *data = UIImagePNGRepresentation(newImage);
    [data writeToFile:@"/Users/siaoxiorliao/Desktop/newImage.png" atomically:YES];
}
```
