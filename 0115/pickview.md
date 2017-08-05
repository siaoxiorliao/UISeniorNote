# 0115UIPickView/UITextField键盘弹出pickview/KVC转模型原理,KVC原理

# UIPickView

* 和tablView差不多的套路

* 点击textFiled弹出pickView
> 

        self.inputView = pick//输入后的view
        let button = UIButton(type: .custom)
        button .addTarget(self, action: #selector(self.hideKeyBoard), for: .touchUpInside)
        button.setTitle("完成", for: .normal)
        bar.addSubview(button)
        self.inputAccessoryView = bar//顶部指示

* 如果多行的情况,设置选中后设置下一行数据再刷新即可

* pickView的基本使用在项目中已经充分体现了 **LoginScreen-custom-ProvenceTexF** 这里就不再做此笔记了 **code 160115-01**

# UITextField
* 可以拦截用户输入或限制用户输入

```objectivec
- (void)viewDidLoad {
    [super viewDidLoad];
    //设置代理监听文本框内容
    self.countryTextF.delegate = self;
    self.birdthDayTextF.delegate = self;
    self.cityTextF.delegate = self;
}
//是否允许开始编辑
- (BOOL)textFieldShouldBeginEditing:(UITextField *)textField 
//开始编辑时调用(文本框成为第一响应者的时候调用)
- (void)textFieldDidBeginEditing:(UITextField *)textField
//是否允许结束编辑
- (BOOL)textFieldShouldEndEditing:(UITextField *)textField
//结束编辑时调用
- (void)textFieldDidEndEditing:(UITextField *)textField
//是否允许文字改变(拦截用户输入或限制用户输入)
- (BOOL)textField:(UITextField *)textField shouldChangeCharactersInRange:(NSRange)range replacementString:(NSString *)string{
    return NO;
}
```

# KVC转模型原理,KVC原理
```objectivec
+ (instancetype)itemWithDict:(NSDictionary *)dict{
    
    FlagItem *item = [[FlagItem alloc] init];
    //[item setValuesForKeysWithDictionary:dict];
    //item.icon = [UIImage imageNamed:item.icon];

    //KVC转模型实现原理 - 遍历字典逐个赋值.
    [dict enumerateKeysAndObjectsUsingBlock:^(id  _Nonnull key, id  _Nonnull obj, BOOL * _Nonnull stop) {
        [item setValue:obj forKeyPath:key];
        NSLog(@"%@--%@",key,obj);
    }];
    /**
    [item setValue:@"中国" forKeyPath:@"name"];
     setValue:forKeyPath实现原理
     1.先查看有没有对应key值的set方法,如果有set方法,就会调用set方法,给对应的属性赋值
     2.如果没有set方法,去查看有没有跟key值相同并且带有下划线的成员属性.如果有的话,就给带有下划线的成员属性赋值
     3.如果没有跟key值相同并且带有下划线的成员属性,还会去找有没有跟key值相同名称的成员属性.如果有,就给它赋值.
     4.如果没有直接报错.
     */

    return item;
}

//了解到原理后
//重写即可将String *icon转化为UIImage icon;
-(void)setIcon:(UIImage *)icon{
    NSString *imageName = (NSString *)icon;
    _icon =  [UIImage imageNamed:imageName];
}

```