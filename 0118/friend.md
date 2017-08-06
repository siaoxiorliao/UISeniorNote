# l
# 控制器跳转

![](/0118/images/WX20170806-112400.png)

* show
 > 和push没区别,只有在iPad中
* show Detail
* Present Modally
* Present As Popover
* Custom

# 控制器view生命周期
* 进入控制器
viewDidLoad -> viewWillAppear ->viewDidAppear
* 退出控制器
viewWillDisappear ->viewDidDisappear

* 如果是只加载一次的操作,写在viewDidLoad
* 如果希望每次进入和退出控制器都需要做的操作则写在appear和disappear里面
* 对于导航控制器,先push的不会被销毁,pop的会销毁

# 通讯录综合实例
## 一阶段 - 登录业务逻辑
* 实现登录验证
* 登录按钮是否可点击的监听
* 实现记住密码和自动登录的开关UISwitch逻辑关系
* 实现storyboard的控制器手动segue跳转(控制器间的segue)及简单的**数据顺传**
* 键盘弹出收回适应
* HUD指示  
* **UIAlertController的使用**

**code 160118-02**

### performSegueWithIdentifier的实现原理
1. 前往指定的storyboard查找有无指定标识的segue
2. 根据指定标识创建UIStoryboardSegue对象
3. 将当前控制器设置为该对象的源控制器SourceViewController
4. 创建目标控制器,并设置destinationViewController
5. 调用prepareForSegue,告诉用户当前segue已经设置好,用户可以通过源控制器(传递数据)设置目标控制器的属性(接收数据).**(数据顺传)**
6. 自行调用[segue perform],push目标控制器
```objectivec
[segue.SourceViewController.navigationController push...]
```

## 二阶段 - 添加通讯录界面