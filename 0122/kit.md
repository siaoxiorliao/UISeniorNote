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

# 屏幕刷新定时器

* setNeedsDisplay不是立马调用drawRect而是屏幕刷新后调用
* CADisplayLink 屏幕每刷新一次立马调用一次
```objectivec
-(void)awakeFromNib {
    //[NSTimer scheduledTimerWithTimeInterval:0.1 target:self selector:@selector(changeY) userInfo:nil repeats:YES];
    CADisplayLink *link = [CADisplayLink displayLinkWithTarget:self selector:@selector(changeY)];
    //想要让CADisplayLink让它工作,必须得要把它添加到主运行循环当中.
    //当每一次屏幕刷新的时候就会调用指定的方法(屏幕每一秒刷新60次)
    [link addToRunLoop:[NSRunLoop mainRunLoop] forMode:NSDefaultRunLoopMode];
    //setNeedsDisplay会调用drawRect:,但是它并不是立马调用.只是设了一个标志.当下一次屏幕刷新的时候才去调用drawRect
}
```