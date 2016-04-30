# iOS学习笔记之“伪”多继承

标签： iOS Objective-C


---

###1. Category

　　因为OC不支持多继承（可能是由于多继承效率较低，开发者给抛弃了，个人观点），所以开发出了分类机制。简单来讲，Category就是为已经存在的类中添加新的方法。

####其中有一些注意的点：

 - 可以访问原类中的成员变量，但是不可以定义添加新的变量，仅仅是添加新的方法
 - Category可以重载原始类的方法，但不推荐这么做，这么做的后果是你再也不能访问原来的方法。如果确实要重载，正确的选择是创建子类
 - 和普通接口有所区别的是，在分类的实现文件中可以不必实现所有声明的方法，只要你不去调用它

####那么Category可以应用的场景是：

 - 当你在定义类的时候，在某些情况下（例如需求变更），你可能想要为其中的某个或几个类中添加方法
 - 一个类中包含了许多不同的方法需要实现，而这些方法需要不同团队的成员实现
 - 当你在使用基础类库中的类时，你可能希望这些类实现一些你需要的方法

> 实现方法：只需要简单的定义两个文件SomeClass+XXX.h和SomeClass+XXX.m，在声明文件和实现文件中用“()”把Category的名称XXX括起来即可，SomeClass就是要添加新的方法的类名称。

###2. Protocol
　　简单来说就是一系列不属于任何类的方法列表，其中声明的方法可以被任何类实现，但是协议一般都和委托（Delegate）一起使用来发挥作用，其实我认为这两个是没有必然联系的，只不过很多时候一起协同工作而已。

####其中有一些注意的点：

 - Protocol本身是`可继承`的，例如：
 
```
protocol A
    - (void)methodA;
@end

protocol B <A>
    - (void)methodB;
@end

```
 - Protocol是`类无关`的，任何类都可以实现定义好的Protocol。如果我们想知道某个类是否实现了某个Protocol，还可以使用conformsToProtocol进行判断:
 
```
[obj conformsToProtocol:@procotol(ProcessDataDelegate)];
```

####那么Protocol可以应用的场景是：

 - Objective-C里的Protocol和Java语言中的接口很类似，也和C++中的纯虚函数类似，但是和C++不同的是这些类没有继承关系，如果一些类之间没有继承关系，但是又具备某些相同的行为，则可以使用Protocol来描述它们的关系。不同的类，可以遵守同一个Protocol，在不同的场景下注入不同的实例，实现不同的功能
 - 最常用的就是委托代理模式，Cocoa框架中大量采用了这种模式实现数据和UI的分离。例如UIView产生的所有事件，都是通过委托的方式交给Controller完成。根据约定，框架中后缀为Delegate的都是Protocol，例如UIApplicationDelegate，UIWebViewDelegate等

###3. 组合
　　其实这种方式就是完全模拟继承，简单讲就是因为OC无法多继承，那么就将想要继承的类的一个实例对象当做自己的一个成员变量，间接地利用其他类的方法。例如有A和B两个类，B无法再继承A，那么可以把A *a当做B中的一个成员变量来实现。

<br />

---
<i class="icon-envelope-alt"></i> [Email](https://mail.google.com/mail/u/0/#inbox)  
<i class="icon-github"></i>  [Github](https://github.com/ZXIOU)  
<i class="icon-weibo"></i>  [新浪微博](http://weibo.com/3895542020/profile?rightmod=1&wvr=6&mod=personinfo&is_all=1#_loginLayer_1461903468940)

<br />
