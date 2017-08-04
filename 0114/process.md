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
## 加载视图过程
* 程序开始加载,如果有Main,则加载main.storyboard(没有则什么都不做self.window = null;),先创建UIWindow窗口,然后将main.storyboard指向的控制器ViewController设为该窗口的根控制器,然后显示UIWindow窗口,然后将根控制器的View添加到UIWindow窗口上并将该UIWindow设成应用程序的主窗口KeyWindow.
