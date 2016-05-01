# iOS学习笔记之循环性能比较

标签： iOS Objective-C


---

####1. 遍历测试方式
 1. 分别使用有100个对象和1000000个对象的NSArray，只取对象，不执行操作，测试遍历速度
 2. 使用有100个对象的NSArray遍历执行doSomethingSlow方法，测试遍历中多任务运行速度

####2. 测试结果

 - 100对象遍历操作：
 ![][1]
 - 1000000对象遍历：
 ![][2]
 - 100对象遍历执行一个很费时的操作：
 ![][3]

####3. 值得注意的

 - 对于集合中对象数很多的情况下，for in (NSFastEnumeration)的遍历速度非常之快，但小规模的遍历并不明显（还没普通for循环快）
 - 使用kvc集合运算符运算很大规模的集合时，效率明显下降（100万的数组离谱的21秒多），同时占用了大量内存和cpu
 - enumerateObjectsWithOptions(NSEnumerationConcurrent)和dispatch_apply(Concurrent)的遍历执行可以利用到多核cpu的优势（实验中在双核cpu上效率基本上x2）

####4. 使用block同时遍历字典key, value
　　block版本的字典遍历可以同时取key和value（forin只能取key再手动取value）：
![][4]

####5. 代码可读性和效率的权衡
　　虽然说上面的测试结果表明，在集合内元素不多时，经典for循环的效率要比forin要高，但是从代码可读性上来看，就远不如forin看着更顺畅；同样的还有kvc的集合运算符，一些内置的操作以keypath的方式声明，相比自己用for循环实现，一行代码就能搞定，清楚明了，还省去了重复工作；在framework中增加了集合遍历的block支持后，对于需要index的遍历再也不需要经典for循环的写法了
<br />

---
<i class="icon-envelope-alt"></i> [Email](https://mail.google.com/mail/u/0/#inbox)  
<i class="icon-github"></i>  [Github](https://github.com/ZXIOU)  
<i class="icon-weibo"></i>  [新浪微博](http://weibo.com/3895542020/profile?rightmod=1&wvr=6&mod=personinfo&is_all=1#_loginLayer_1461903468940)

  [1]: https://github.com/ZXIOU/iOS-Study-Notes/blob/master/Photo%20Resource/%E5%A4%9A%E7%BB%A7%E6%89%BF%E6%96%B9%E5%BC%8F/100%E5%AF%B9%E8%B1%A1%E9%81%8D%E5%8E%86.png
  [2]: https://github.com/ZXIOU/iOS-Study-Notes/blob/master/Photo%20Resource/%E5%A4%9A%E7%BB%A7%E6%89%BF%E6%96%B9%E5%BC%8F/1000000%E5%AF%B9%E8%B1%A1%E9%81%8D%E5%8E%86.png
  [3]: https://github.com/ZXIOU/iOS-Study-Notes/blob/master/Photo%20Resource/%E5%A4%9A%E7%BB%A7%E6%89%BF%E6%96%B9%E5%BC%8F/100%E5%AF%B9%E8%B1%A1%E9%81%8D%E5%8E%86%E8%B4%B9%E6%97%B6%E6%93%8D%E4%BD%9C.png
  [4]: https://github.com/ZXIOU/iOS-Study-Notes/blob/master/Photo%20Resource/%E5%A4%9A%E7%BB%A7%E6%89%BF%E6%96%B9%E5%BC%8F/block%E9%81%8D%E5%8E%86.png
