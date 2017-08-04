# 应用程序启动过程和原理

## 应用程序启动过程
1.执行main函数.
2.执行UIApplicationMain,创建UIApplication对象,并设置UIApplication它的代理.
> return UIApplicationMain(argc, argv, NSStringFromClass([UIApplication class]), NSStringFromClass([AppDelegate class]));

3.开启了一个事件循环.(主运行循环,死循环:保证应用程序不退出)
4.去加载info.plist.(判断info.plist当中有没有Mian,如果有,加载Mian.storyBoard)
5.应用程序启动完毕.(通知代理应用程序启动完毕)

## 启动原理