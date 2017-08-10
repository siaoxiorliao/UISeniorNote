# UIKit

# 利用UIKit进行绘图
* 实际上UIKit只是封装了一下 Quartz2D

```objectivec
//画文字
- (void)drawText{
    NSString  *str = @"小码哥小码哥小码哥小码哥小码哥小码哥";
    NSMutableDictionary *dict = [NSMutableDictionary dictionary];
    //字体大小
    dict[NSFontAttributeName] = [UIFont systemFontOfSize:30];
    //设置颜色
    dict[NSForegroundColorAttributeName] = [UIColor redColor];
    //设置描边
    dict[NSStrokeColorAttributeName] = [UIColor greenColor];
    dict[NSStrokeWidthAttributeName] = @2;
    //设置阴影
    NSShadow *shaw = [[NSShadow alloc] init];
    shaw.shadowColor = [UIColor blueColor];
    //设置阴影的偏移量
    shaw.shadowOffset = CGSizeMake(1, 1);
    shaw.shadowBlurRadius = 2;
    dict[NSShadowAttributeName] = shaw;
    [str drawAtPoint:CGPointZero withAttributes:dict];
    //[str drawInRect:rect withAttributes:dict];
    //用drawInRect:rect会自动换行.用drawAtPoint不会自动换行.
}

//画图片
- (void)drawRect:(CGRect)rect {
    UIImage *image = [UIImage imageNamed:@"001"];
    //drawAtPoint绘制的是原始图片的大小
    //[image drawAtPoint:CGPointZero];
    //把要绘制的图片给填充到给定的区域当中.
    //[image drawInRect:rect];
    //裁剪(超过裁剪区域以外的内容,都会被自动裁剪掉)
    //设置裁剪区域一定要在绘制之前进行设置
    //UIRectClip(CGRectMake(0, 0, 50, 50));
    //平铺
    [image drawAsPatternInRect:self.bounds];
    
    //绘制矩形
//    UIRectFill(CGRectMake(50, 50, 50, 50));
}
```
