# UINavigationItem导航条定制个人详情页

* 在UITableviewController里,默认设置了内边距偏移(viewDisappear)

```objectivec
//取消自动偏移
    self.automaticallyAdjustsScrollViewInsets = NO;
```
* 苹果不允许为导航条和控件为透明的
* 可以设置空的图片,并且模式是default就有效果了

```objectivec
    [self.navigationController.navigationBar setBackgroundImage:[[UIImage alloc] init] forBarMetrics:UIBarMetricsDefault];
    [self.navigationController.navigationBar setShadowImage:[[UIImage alloc] init]];
```

## 设置tableview偏移对应的透明度变化,图片拉伸和粘性条

* 得到 scrollView.contentOffset.y,通过它来确定tableview顶部和view顶部距离,再除以总的高度244即可得到透明度变化
* 设置图片clip to bounds裁剪即可控制背景图片不拉伸
* 控制背景view的高度约束最大为64,即可在tableview刚好拉到最顶部而粘性条保留

```objectivec
- (void)scrollViewDidScroll:(UIScrollView *)scrollView{
    CGFloat height = 244;
    CGFloat offset = height + scrollView.contentOffset.y;
    CGFloat h = 200 - offset;

    if (h < 64) {
        h = 64;
    }
    self.heightConstr.constant = h;
    
    CGFloat alpha = offset / height;
    if (alpha >= 1) {
        alpha = 1;
    }
    self.navigationController.navigationBar.alpha = alpha;
}
```
* **code 160119-03**


