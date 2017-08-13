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

```objectivec
#import "ViewController.h"
//每个时间单位转多少度
#define secA 6
#define minA 6
#define hourA 30
#define hourMinA 0.5
#define angleToRad(angle) ((angle) / 180 * M_PI)//角度转弧度

@interface ViewController ()
@property (weak, nonatomic) IBOutlet UIImageView *clockView;
@property (weak, nonatomic) CALayer *secL;
@property (weak, nonatomic) CALayer *minL;
@property (weak, nonatomic) CALayer *hourL;
@end
@implementation ViewController

- (void)viewDidLoad {
    [super viewDidLoad];
    [self setPointer];
    [self timeChange];//进来就先转到当前时间
    [NSTimer scheduledTimerWithTimeInterval:1 target:self selector:@selector(timeChange) userInfo:nil repeats:YES];
}

- (void)timeChange{
    NSCalendar *calendar = [NSCalendar currentCalendar];
    NSDateComponents *cmp = [calendar components:NSCalendarUnitSecond | NSCalendarUnitMinute | NSCalendarUnitHour fromDate:[NSDate date]];
    CGFloat secAngle = cmp.second * secA;// angle = 当前多少秒 * 每秒转的度数
    self.secL.transform = CATransform3DMakeRotation(angleToRad(secAngle), 0, 0, 1);
    CGFloat minAngle = cmp.minute * minA;// angle = 当前多少分 * 每分转的度数
    self.minL.transform = CATransform3DMakeRotation(angleToRad(minAngle), 0, 0, 1);
    CGFloat hourAngle = cmp.hour * hourA + cmp.minute * hourMinA;// angle = 当前多少时 * 每时转的度数 +
    self.hourL.transform = CATransform3DMakeRotation(angleToRad(hourAngle), 0, 0, 1);
}

- (void)setPointer{
    CALayer *hourL = [CALayer layer];
    hourL.bounds = CGRectMake(0, 0, 6, 40);
    hourL.backgroundColor = [UIColor blackColor].CGColor;
    hourL.cornerRadius = 3;
    hourL.anchorPoint = CGPointMake(0.5, 1);
    hourL.position = CGPointMake(self.clockView.bounds.size.width * 0.5, self.clockView.bounds.size.height * 0.5);
    [self.clockView.layer addSublayer:hourL];
    self.hourL = hourL;
    
    CALayer *minL = [CALayer layer];
    minL.bounds = CGRectMake(0, 0, 4, 70);
    minL.backgroundColor = [UIColor blackColor].CGColor;
    minL.cornerRadius = 2;
    minL.anchorPoint = CGPointMake(0.5, 1);
    minL.position = CGPointMake(self.clockView.bounds.size.width * 0.5, self.clockView.bounds.size.height * 0.5);
    [self.clockView.layer addSublayer:minL];
    self.minL = minL;
    
    CALayer *secL = [CALayer layer];
    secL.bounds = CGRectMake(0, 0, 2, 80);
    secL.backgroundColor = [UIColor redColor].CGColor;
    secL.cornerRadius = 1;
    secL.anchorPoint = CGPointMake(0.5, 1);
    secL.position = CGPointMake(self.clockView.bounds.size.width * 0.5, self.clockView.bounds.size.height * 0.5);
    [self.clockView.layer addSublayer:secL];
    self.secL = secL;
    
}
@end
```