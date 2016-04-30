# iOS学习笔记之头文件引用

标签： iOS Objective-C

---

###1. #import<>和import“”的区别

 - #import<>：引用系统头文件
 - #import“”：引用自己创建的头文件

###2. #import和#include的区别 ##

 - #import大部分和#include一样，但是可以解决重复引用问题，#import其实类似于C++中#include和#ifndf的结合。

> 例如：有三个文件A.h、B.h、C.h
C.h要引用A.h和B.h，B.h引用A.h，那么就会出现重复引用，import就可以优化。

###3. #import与@class的区别 ##

 - #import会包含这个类的所有信息，包括实体变量和方法，而@class只是告诉编译器，其后面声明的名称是类的名称，至于这些类是如何定义的，暂时不用考虑，后面会再告诉你
 - 在头文件中，一般只需要知道被引用的类的名称就可以了。不需要知道其内部的实体变量和方法，所以在头文件中一般使用@class来声明这个名称是类的名称。而在实现类里面，因为会用到这个引用类的内部的实体变量和方法，所以需要使用#import来包含这个被引用类的头文件
 - 在编译效率方面考虑，如果你有100个头文件都#import了同一个头文件，或者这些文件是依次引用的，如A–>B,B–>C,C–>D这样的引用关系。当最开始的那个头文件有变化的话，后面所有引用它的类都需要重新编译，如果你的类有很多的话，这将耗费大量的时间。而是用@class则不会

###4. @class的作用 ##

 - @class是用来做类引用的
 - @class就是告诉编译器有一个这个类，至于类的具体定义是什么并不知道
 - @class一般用于头文件中需要声明该类的某个实例变量的时候用到，在.m文件中还是需要使用#import声明，这样可以加快编译速度


> 所以，一般来说，@class是放在interface中的，只是为了在interface中引用这个类，把这个类作为一个类型来用的。 在实现这个接口的实现类中，如果需要引用这个类的实体变量或者方法之类的，还是需要import在@class中声明的类进来

<br />
---
<i class="icon-envelope-alt"></i> [Email](https://mail.google.com/mail/u/0/#inbox)
<i class="icon-github"></i>  [Github](https://github.com/ZXIOU)
<i class="icon-weibo"></i>  [新浪微博](http://weibo.com/3895542020/profile?rightmod=1&wvr=6&mod=personinfo&is_all=1#_loginLayer_1461903468940)

<br />


 
  
