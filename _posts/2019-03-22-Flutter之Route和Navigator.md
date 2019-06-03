---
layout:     post
title:      Flutter之Route和Navigator
subtitle:   Route & Navigator
date:       2019-03-22
author:     John Guo
header-img: img/post_bg_flutter.jpg
catalog: true
tags:
    - Flutter
---

# 名词解释
* Route：路由，flutter中表示页面路由，类似于Activity；
* Navigator：导航器，flutter表示页面控制器，控制页面跳转，类似于ActivityManager；
以上两者需结合使用。
    
# Route&Navigator
在flutter中Navigator实现push、pop页面，页面间的参数传递等等。
flutter路由可以分成两种：
* 一种是直接注册，不能传递参数，可以称为 静态路由
* 一种要自己构造实例，可以传递参数，可以称为 动态路由。

# 静态路由
这里我们也叫app顶级路由，一般我们在materialApp中设置Routes定义静态路由页面；  
使用场景：明确已知App通用跳转页面和值(比如 启动页/主页/登录页)；  
我们需要定义应用中页面跳转规则，该对象是一个 Map<String, WidgetBuilder>；  
当使用 Navigator.pushNamed 来路由的时候，会在 routes 查找路由名字，然后使用 对应的 WidgetBuilder 来构造一个带有页面切换动画的 MaterialPageRoute；   
如果应用只有一个界面，则不用设置这个属性，使用 home 设置这个界面即可；  
注意：MaterialApp的home的属性 等价于 routes中包含 Navigator.defaultRouteName('/')，两者冲突不能同时使用 ，如果所查找的路由在 routes 中不存在，则会通过 onGenerateRoute 来查找。  
缺点：它不可以向下一个页面传递参数

# 动态路由
当需要向下一个页面传递参数时，要用到所谓的动态路由，自己生成页面对象，所以可以传递自己想要的参数。
```dart
onPressed: () {
  Navigator.push(context, MaterialPageRoute(
      builder: (context) {
        return new EchoRoute("传入跳转参数");
      }));
  //或者
  Navigator.of（(context).push(MaterialPageRoute(
  builder: (context) {
  return new EchoRoute("传入跳转参数");
  }));
},
...............................
```

# 小结
Navigator的职责是负责管理Route的，管理方式就是利用一个栈不停压入弹出，当然也可以直接替换其中某一个Route。而Route作为一个管理单元，主要负责创建对应的界面，响应Navigator压入路由和弹出路由；  
####入栈：
* 使用Navigator.of(context).pushName(“path“)或者Navigator.pushName(context,“path“)可以进行静态路由的跳转前提是需要在route属性里面注册
使用push(Route)可以进行态路由的跳转，动态路由可以传入未知数据；  
####出栈
* 使用pop()可以进行路由的出栈并且可以传递参数
可以使用Future接收上个页面返回的值；