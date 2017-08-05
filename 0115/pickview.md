# 0115UIPickView/

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

* pickView的基本使用在项目中已经充分体现了 **LoginScreen-custom-ProvenceTexF** 这里就不再做此笔记了
