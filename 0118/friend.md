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
## 一阶段 - 登录业务逻辑 正向传值
* 实现登录验证
* 登录按钮是否可点击的监听
* 实现记住密码和自动登录的开关UISwitch逻辑关系
* 实现storyboard的控制器手动segue跳转(控制器间的segue)及简单的**正向传值**
* 键盘弹出收回适应
* HUD指示  
* **UIAlertController的使用**

**code 160118-02**

### performSegueWithIdentifier的实现原理
1. 前往指定的storyboard查找有无指定标识的segue
2. 根据指定标识创建UIStoryboardSegue对象
3. 将当前控制器设置为该对象的源控制器SourceViewController
4. 创建目标控制器,并设置destinationViewController
5. 调用prepareForSegue,告诉用户当前segue已经设置好,用户可以通过源控制器(传递数据)设置目标控制器的属性(接收数据).**(正向传值)**
6. 自行调用[segue perform],push目标控制器
```objectivec
[segue.SourceViewController.navigationController push...]
```

### 正向传值
* 为目标控制器添加要传的属性,在prepare方法里拿到目标控制器,设置目标控制器的属性即可.

## 二阶段 - 添加通讯录界面 反向传值

* 笨拙的互相拥有属性 **code 160118 - 03**耦合性强
 > 耦合性: 控制器之间的关联
 > 内聚性: 相似的功能模块集合在一起,体现重复代码
 
* 解耦: 代理传值 
* **code 160118 - 04**
* 运用代理设计模式可以有效解决控制器间的相互拥有的情况.

## 三阶段 - 编辑界面
* **重要** 在选择cell进入下个界面进行正向传值的时候,设置子控件的数据不会显示,是因为viewController的view是懒加载的,只有在用到的时候才加载,才不会显示.
* 解决办法
![](/0118/images/WX20170806-204904.png)

* **理解** 我们正向传模型的时候,传的是dataArray数据数组里面的那个模型,我们接收模型的属性就指向了它
![](/0118/images/WX20170806-211819.png)
![](/0118/images/WX20170806-211521.png)
所以在我们目标控制器里更改模型的值就等于更改了dataArray的数据,就不用再反向传值了.

![](/0118/images/WX20170806-211541.png)
