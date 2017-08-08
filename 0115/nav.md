# 导航控制器及管理原则/导航栏
# 多控制器
* 苹果提供两个特殊的控制器方便于管理控制器
> UINavigationController导航控制器
> UITabBarController标签控制器

## UINavigationController
* 管理原则,通过栈管理
* push则是放入栈顶,如果调到指定的,则让指定的放在栈顶,在它上面的则被销毁
* pop回到上一个控制器,本身的控制器被移除销毁
* 只要push到UINavigationController,则就归它管理,就有了self.navigationController,self.navigationItem属性
* push的rootViewController永远指向栈底控制器
### UINavigationItem
* 存储栈顶控制器的导航栏内容

    常见属性:
    backBackButtonItem
    title 
    titleView
    leftBarButtonItem
    rightBarButtonItem
> 默认会渲染导航栏内容,设置图片不可渲染即可

> ```objectivec
//设置右侧图片
    UIImage *image = [UIImage imageNamed:@"navigationbar_friendsearch"];
    UIImage *oriImage = [image imageWithRenderingMode:UIImageRenderingModeAlwaysOriginal];
    self.navigationItem.rightBarButtonItem = [[UIBarButtonItem alloc] initWithImage:oriImage style:0 target:self action:@selector(rightClick)];
```