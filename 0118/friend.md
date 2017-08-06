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