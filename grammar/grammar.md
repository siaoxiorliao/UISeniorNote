# 语法外传
# 备忘录
* 类继承关系
![](/grammar/images/20140320225429296.jpeg)

## 我的超级语法训练机器😎😎😎😎😎

1 .   请转换?
>if (_dataArr == nil)**

<!--sec data-title="😳对没对?😳" data-id="section5" data-show=false ces-->
if (!_dataArr)
<!--endsec-->
<button class="section" target="section5" show="👌" hide="🙃"></button>

2 .   创建同名字典?

<!--sec data-title="😳对没对?😳" data-id="section1" data-show=false ces-->
```objectivec
    NSDictionary *views = NSDictionaryOfVariableBindings(redView,blueView);
```
<!--endsec-->

<button class="section" target="section1" show="👌" hide="🙃"></button>  

3 .   为数组中每个对象发送消息?

<!--sec data-title="😳对没对?😳" data-id="section2" data-show=false ces-->
```objectivec
    [self.scrollView.subviews makeObjectsPerformSelector:@selector(removeFromSuperview)];
```
<!--endsec-->

<button class="section" target="section2" show="👌" hide="🙃"></button>

4 .   取反操作

<!--sec data-title="😳对没对?😳" data-id="section3" data-show=false ces-->
```objectivec
self.tableView.editing = !self.tableView.isEditing;
self.deletedButton.hidden = !self.tableView.isEditing;
self.reduceBtn.enabled = (wine.count > 0);
```
<!--endsec-->
<button class="section" target="section3" show="👌" hide="🙃"></button>

5 .   日期转格式成字符串
<!--sec data-title="😳对没对?😳" data-id="section4" data-show=false ces-->
```objectivec
NSDateFormatter * formatter = [[NSDateFormatter alloc]init];
formatter.dateFormat = @"yyyy-MM-dd";
NSDate *date = datePick.date;
self.text = [formatter stringFromDate:date];
```
<!--endsec-->
<button class="section" target="section4" show="👌" hide="🙃"></button>

6 .   遍历字典操作 KVC转模型原理
<!--sec data-title="😳对没对?😳" data-id="section4" data-show=false ces-->
```objectivec
+ (instancetype) itemWithDict: (NSDictionary *) dict{
    FlagItem *item = [[FlagItem alloc]init];
    //KVC转模型原理
    [dict enumerateKeysAndObjectsUsingBlock:^(id  _Nonnull key, id  _Nonnull obj, BOOL * _Nonnull stop) {
        [item setValue:obj forKeyPath:key];
    }];
    return item;
}
    //KVC原理
- (void)setIcon:(UIImage *)icon{
    NSString *imageName = (NSString *)icon;
    _icon = [UIImage imageNamed:imageName];
}
```
<!--endsec-->
<button class="section" target="section4" show="👌" hide="🙃"></button>




