

# UITabBarController

* 可以在tabBar中添加导航控制器
* 也可以添加普通的控制器
* 第一个添加的即是第一个默认选择和显示的,可以以设置默认选择的

    
## UITabBar管理原则
* tabBar一创建,它的子控件也会跟随创建,并且来回切换都不会销毁

## UITabBar使用及属性
* addChildViewController
* UITabBar - > UITabBarButton,由对应子控制器vc的tabBarItem决定
  1. titile 后面设置的会覆盖前面设置的;
  2. image
  3. selectedImage
  4. badgeValue
  
# 主流框架搭建
* 主流框架 : 最好设置tabBar为窗口根控制器,在设置nav为tabBar子控制器,vc为nav的根控制器,tabBar里面可以有很多的nav,这就是最简单实用的框架.

# 另一种控制器切换方式 - modal模态
* 底部弹出
* 任何控制器都能通过model形式展示出来

## modal和push的区别
* modal显示出来只是将原本的view清除,再将目标控制器的view显示到窗口上,并不是整个将控制器和push一样放上去,modal后UIWindow.keyWindow.rootViewController指向的还是源控制器(和push不同,push的rootViewController永远指向栈底控制器).
* 这样目标控制器不是modal后被销毁吗?
 > 并不会,modal过后,源控制器也有会属性self.presentedViewController强引用着目标控制器,只有dismiss过后该属性就会被清空,目标控制器被销毁,目标控制器view也会相应被移除,而源控制器及其view始终没被销毁(显示回来也不需要重新加载viewDidLoad).
 
## 使用
* presentViewController弹出
* dismissViewController消失