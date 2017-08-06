# hello
# 多控制器
* 苹果提供两个特殊的控制器方便于管理控制器
> UINavigationController导航控制器
> UITabBarController标签控制器

## UINavigationController
* 管理原则,通过栈管理
* push则是放入栈顶,如果调到指定的,则让指定的放在栈顶,在它上面的则被销毁
* pop回到上一个控制器,本身的控制器被移除销毁

### UINavigationItem
* 存储栈顶控制器的导航栏内容

    常见属性:
    backBackButtonItem
    titleView
    title
    leftBarButtonItem
    rightBarButtonItem