# aboutIos

MarkDown 常用快捷键
=================

>1.任意的==在文字下面都会变为最高阶标题<br>
>2.任意的__在位子下面都会变为二阶标题<br>
>3.如果不空行 那上面的文字会收到下面格式的影响 <br>
>4.区块引用使用 > 同样能进行嵌套 >> <br>
>5.+ * - 用于列表  同时*号还能用于缩进 <br>
>6.三个以上的* - - 可以作为分隔线 <br>
>7.链接提示文字用[]  链接具体地址用() <br>
>8.在文字的两端使用 * 或者 _ 会强调文字   两个* 或者两个_ 会加粗文字<br>
>9.用反向引号用来包裹提示信息  反向引号在esc下面  反向引号可以嵌套 <br>
>10.行内图片 `![文字描述9](图片地址)` <br>

* * *
[NSHipster 关注OC忽略的特性](http://nshipster.cn/)
* * *
## 1.手机唯一标示符
获取手机设别信息:[简书](http://www.jianshu.com/p/b23016bb97af)

>1. UDID 为手机的唯一标示符 ios5后以泄露用户隐私去掉
>2. UUID 统一唯一识别码 如果用户删除后又安装拿到的`UUID`是不同的
>3. 广告位标示符 ASIdentifierManager ios6后的新方法. 用户可以重置广告标示符, 重置系统时也有可能
>   重置此标示符 `注意: 如果没有使用广告会被打回`  
>4. mac地址每次都会改变

**使用KeyChain获取UDID** <br>

[参考连接](http://www.cnblogs.com/smileEvday/p/UDID.html)<br>
增加 删除  查询  更新
[参考代码](https://github.com/smileEvday/SvUDID)

* * *
## CFStringRef 与 NSString装换
>NSString *strNS = @"";
>CFStringRef strRef = (__bridge CFStringRef)strNS;
>strNS = (__bridge NSString *)strRef;


* * *
## 2.去除警告
首先会基本的语法  然后可以根据提示的信息替换 ignored后面的内容<br>
[警告查询](http://fuckingclangwarnings.com/)

``
* #pragma clang diagnostic push
* #pragma clang diagnostic ignored "-Wdeprecated-declarations"
* //这里写出现警告的代码
* #pragma clang diagnostic pop``

* * *
## 3.const修饰
const 简单原则：在const右侧的不可变
```
int   c = 22; int   d = 33;
const int  a = 10;  //a不可变
int  const b = 20;  //b不可变
int  const *p1 = &c;//*p1不可变  p1可变
p1  = &d;
int  * const p2 = &c;//*p1可变   p2不可变
*p2 = d;
NSLog(@"a:%d,b:%d,c:%d,p1:%d,p2:%d",a,b,c,*p1,*p2);
```
* * *
## 4.夜间模式

1.写一个公共类用于存放两种状态的颜色  根据状态修改每个视图的颜色<br>
2.采用成熟的第三方框架[DKNightVersion](https://github.com/Draveness/DKNightVersion) <br>

DKNightVersion 有一个manager用于管理夜间 普通模式的状态；   一个文件配置所要管理状态的颜色；
对于基础的控件添加了对应的属性 

* * *
## 5.NSPredicate
>cocoa 框架中用于查询的关键字
[参考链接](http://blog.csdn.net/lianbaixue/article/details/10579117) <br>

>基本的语法格式为 <br>
`` 
NSPredicate *predicateStr = [NSPredicate predicateWithFormat:@"self > %ld",100];
NSArray     *temp = [objectArray filteredArrayUsingPredicate:predicateStr];
``
<br>
详细使用参考[NSPredicate](http://blog.csdn.net/lianbaixue/article/details/10579117) <br>

1.比较运算符  `> ,< , == , >=, <=, !=` <br>
2.范围运算符  `IN, BETWEEN`   <br>
3.字符串本身:self  <br>
4.字符串相关: `BEGINSWITH、ENDSWITH、CONTAINS`  [c]不区分大小写  [d]不区分发音符号 音标 [cd]既不区分也不区分 <br>
* @"name CONTAIN[cd] 'ang'"   //包含某个字符串  并且不区分大小写与发音符号
5.通配符：`LIKE` <br>
6.正则表达式: `MATCHES`

### 通过正则表达式截取字符串

```
//组装一个字符串，需要把里面的网址解析出来
NSString *urlString=@"http://xxxxmall.com/membershare/mine?memberId=24530";
NSRegularExpression *regex = [NSRegularExpression regularExpressionWithPattern:@".*/membershare/mine\\?memberId=(\\d+)" options:NSRegularExpressionCaseInsensitive error:nil];
if (regex != nil) {
NSTextCheckingResult *firstMatch = [regex firstMatchInString:urlString options:0 range:NSMakeRange(0, [urlString length])];
//有n个括号就存在n+1个分段
//第0个位匹配到的全部字符串  1个位匹配到的第一个
NSLog(@"number:%ld",[firstMatch numberOfRanges]);
if (firstMatch) {
NSRange resultRange = [firstMatch rangeAtIndex:1];
//从urlString当中截取数据 输出结果
NSLog(@"%@",[urlString substringWithRange:resultRange]);
}
}
```


## 补充正则表达式相关
* 1.字符分为普通字符
* 2.非打印字符：\n， \f， \t， \v
* 3.特殊字符： $字符串结尾，  （），  *，[ ],
* 4.限定符

**元字符**
* .  匹配除换行符外的任意字符
* ^  字符串开始
* $  字符串结束
* \w 字母，数字，下划线，汉字     反义为\W
* \s 任意的空白符               反义为\S
* \d 匹配任意的数字 只是一个数字  反义为\D
* \b 任意的单词开始或结束        反义为\B

**限定符**
* `*` 重复0次或者更多次 
* `+` 重复1次或者更多次
* ？重复0次或者一次
* {n}  重复n次或者更多次
* {n,} 重复n次或者更多次
* {n,m} 重复n-m次

**分歧条件**
* 用|进行分区  执行为从左到右
* 或者的意思 如ip地址 (25[0-5]|2[0-4]\d|(1\d{2}|[1-9]?\d))标示其中的一个小段 
* (?:exp) 不补货匹配的文本 也不给此分组分配组号  ((?:(?:25[0-5]|2[0-4]\d|((1\d{2})|([1-9]?\d)))\.){3}(?:25[0-5]|2[0-4]\d|((1\d{2})|([1-9]?\d))))
**此处需要举个栗子**


**懒惰与贪婪**
  待匹配`aaaabaabb`,  需要匹配`axb` x用下面的几种方式代替
* *？ 重复任意次， 但尽可能少重复
* +？ 重复1次或者多次，但尽可能少重复
* ？？    重复0次或者1次
* {n，}？ 重复n次以上
* {n,m}? 重复n到m此


## 6. setNeedsLayout

## 7. 手势

## 8. 事件传递响应链
[参考链接](http://blog.csdn.net/xiaoxiaobukuang/article/details/55046601) <br>
>ios中能够响应事件的对象都是UIResponder的子类对象 <br>
>UIEvent 事件对象 ：多点触控  摇一摇  3DTouch <br>
>UITouch 点击对象 ：用户点击的状态  开始  移动  停止不动  结束  取消5中状态。取消点击只有当程序强制退出或者来电时才会响应<br>

#### 通过点击事件包含在子视图  依次获取处理这个事件的所有视图对象
``- (nullable UIView *)hitTest:(CGPoint)point withEvent:(nullable UIEvent *)event; // recursively calls -pointInside:withEvent:. point is in the receiver's coordinate system`` <br>
``- (BOOL)pointInside:(CGPoint)point withEvent:(nullable UIEvent *)event; // default returns YES if point is in bounds;``
#### 通过以上方法事件查找的顺序为  
> 视图的顺序为 BView  AView  UIView <br>
> 事件查找顺序 UIStatusBar相关的视图 -> UIWindow -> UIView -> AView  -> BView（系统在事件链传递的过程中一定会遍历所有的子视图判断是否能够响应点击事件）<br>
> 事件响应顺序 BView -> AView -> UIView -> UIWindow -> UIStatusBar相关的视图 <br>

## 9.断点调试
[参考地址](http://www.jianshu.com/p/8e9fc9a8ab78) <br>
1. breakpoint 断点相关的  提示文档 breakpoint help command 可以查看命令 <br>
    **加断点  代码的某一行**  <br>
    *``breakpoint set -f xxVC.m -l``  在某个文件的第x行加上一个断点 <br>
    **加断点  代码的某一类的某个方法**  <br>
    *``breakpoint set -n "[xxVC method]"`` 在某个文件的第x行加上一个断点 <br>
    **查看添加的断点**  <br>
    *``breakpoint list`` <br>
    **删除断点**   <br>
    *``breakpoint delete breakpointID`` 删除具体ID断点   断点可以用list命令查看 <br>
    *``breakpoint delete`` 选择是否删除全部断点 <br>
 2. watchpoint 可以观察值的具体变化 <br>
 3. process 暂不知道啥用 <br>
 4. thred      <br>
 5. call  修改属性    <br>
    `` po [self.view setBackgroundColor:[UIColor redColor]]``  <br>
    ``call [self.view setBackgroundColor:[UIColor redColor]]`` <br>
 6. image  展示崩溃   可以用image help产看帮助  <br>
   ``image lookup --address 0x0000000104147544``


## 10. 多线程  网络请求  GCD

## 11 命令行工具

* which 命令的位置 <br>
>  [卸载pod](http://blog.csdn.net/qq_18670721/article/details/50432892) <br>
>  [pod安装遇到问题](http://www.jianshu.com/p/f79cd5256531)  <br>