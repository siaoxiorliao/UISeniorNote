

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

# modal模态
