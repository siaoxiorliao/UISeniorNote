# 应用程序启动过程和原理

# 应用程序启动过程和原理
1.执行main函数.

2.执行UIApplicationMain,创建UIApplication对象,并设置UIApplication它的代理.
> return UIApplicationMain(argc, argv, NSStringFromClass([UIApplication class]), NSStringFromClass([AppDelegate class]));

3.开启了一个事件循环.(主运行循环,死循环:保证应用程序不退出,程序正常退出时UIApplicationMain函数才返回)

4.去加载info.plist.(判断info.plist当中有没有Mian,如果有,加载Mian.storyBoard)

5.应用程序启动完毕.(通知代理应用程序启动完毕)

# UIWindow
* UIWindow是一种特殊的UIView,继承自UIView,一个app至少有一个UIWindow
* app加载创建的第一个控件即是UIWindow,接着创建控制器的view添加到UIWindow
* UI之所以能显示,是因为添加到了UIWindow
* 一个窗口必须得有根控制器
* 当指定了UIWindow的根控制器，该控制器的view会自动添加在UIWindow上
* 程序加载显示的UIWindow即是AppDelegate的那个UIWindow(window属性)
> 若增多一个UIWindow类型的属性并建多一个UIWindow对象给它则还是不能显示,因为创建的window和默认的都是Normal级别(0),系统优先显示AppDelegate的window属性,要想显示则设置为Alert(2000)最高级别再设置系统的为Normal即可(只要高于就行,等于不行).

## 加载视图过程
* 程序开始加载,如果有Main,则加载main.storyboard(没有则什么都不做self.window = null;),先创建UIWindow窗口,然后将main.storyboard指向的控制器ViewController设为该窗口的根控制器,然后显示UIWindow窗口,然后将根控制器的View添加到UIWindow窗口上并将该UIWindow设成应用程序的主窗口KeyWindow.

## 窗口层级
> self.window.windowLevel = UIWindowLevelNoraml;
UIWindowLevelAlert > UIWindowLevelStatusBar > UIWindowLevelNormal

## 通过StoryBoard加载控制器 - UIStoryboard
* 可以将storyboard文件变为一个对象来使用
* 设置storyboard的控制器为UIWindow的根控制器

```objectivec
    self.window = [[UIWindow alloc] initWithFrame:[UIScreen mainScreen].bounds];
    //创建storyBoard对象
    UIStoryboard *storyBoard = [UIStoryboard storyboardWithName:@"One" bundle:nil];
    //加载storyBoard箭头指向的控制器
    UIViewController *vc =  [storyBoard instantiateInitialViewController];
    //加载指定ID的控制器
    //UIViewController *vc = [storyBoard instantiateViewControllerWithIdentifier:@"VC"];
    self.window.rootViewController = vc;
    [self.window makeKeyAndVisible];
    
```
## 通过Xib加载控制器的View
* 控制器的init方法会调用initWithNibName:方法.

> 之前学的是通过xib自定义View
```objectivec
UINib *nib = [UINib nibWithNibName:@"CarView" bundle:nil];
UIView *carView = [[nib instantiateWithOwner:nil options:nil] firstObject];
[self.view addSubview:carView];
```

* initWithNibName

>     //initWithNibName:如果指定了特定的名称的xib,会去加载指定的xib
>    //如果指定是nil
>    //1.判断有没有当前控制器相同名称的xib,如果有,自动加载跟它相同名称的xib(XMGViewController.xib)
>    //2.如果没有跟它相同名称的xib.自动加载跟它相同名称并且是去掉controller(XMGView.xib)
>    **init底层自动调用initWithNibName.**
>    //XMGViewController *vc = [[XMGViewController alloc] init];
>    //vc.view.backgroundColor = [UIColor redColor];
    
### 控制器loadView方法 - 自定义控制器view
* 在控制器View第一次使用的时候调用
* 底层: 
   1. 判断view是否从storyboard中加载,如果是就设置sb中的view设为当前控制器的view
   2. 判断是否从xib中加载,如果是就设置xib指定的view设为当前控制器的view
   3. 如果都不是,则会创建空白的view(cleaColor,不是alpha=0)
   
* 重写loadView方法能让控制器一创建就创建自己想要的控件并做为View
* 控制器的view是懒加载的.

```objectivec
-(void)loadView{
//    XMGView *xmgV = [[XMGView alloc] initWithFrame:[UIScreen mainScreen].bounds];
//    self.view = xmgV;
    //如果一个控件不能够接收事件,那么它里面子控件也是能不能够接收事件
    //如果一个控件是alpha=0的(不是clearColor),则它不能接收事件
    UIImageView *imageV = [[UIImageView alloc] initWithImage:[UIImage imageNamed:@"火影"]];
    imageV.userInteractionEnabled = YES;
    self.view = imageV;
}
```

### 控制器view懒加载过程
* 什么时候用到控制器View的时候,才会调用loadView方法创建控制器的View
```objectivec
-(UIView *)view{
    if (_view == nil) {
        [self loadView];
        [self viewDidLoad];

    }
    return _view
}
```

   