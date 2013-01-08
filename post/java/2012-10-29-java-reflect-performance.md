{
layout: "pages",
title: "Java反射效率",
category: "java",
description: "java反射效率测试",
tags: ["java","reflect"]
}

Java的反射是个老问题了，因为有了反射的特性，Java是一个接近动态类型的语言，但是Java的反射的效率一直都是被诟病的地方。
自从有了这个特性几乎每一次JDK大版本更新都会说反射的效率大大提高。
因为我的ChaosBuff要更新，原来用数组的方式管理对象，实在是不适合调试。所以想一下还是降低一下运行效率提高生产效率吧。
由于网上很多的测试都是抄来抄去的，这里我就重新测一下看看反射有多慢。

当然反射不止一种方法的，而且也有一些比较常见的优化方式。我们将会测试一下：

1. 直接访问的耗时
2. 直接反射的耗时
3. 缓存需要查找的函数反射的耗时
4. 使用reflectasm的反射耗时

```{java}
long now;
long sum = 0;
TestClass t = new TestClass();
 
now = System.currentTimeMillis();
 
for(int i = 0; i<500000; ++i){
	t.setNum(i);
	sum += t.getNum();
}
 
System.out.println("get-set耗时"+(System.currentTimeMillis() - now) + "ms秒，和是" +sum);
 
sum = 0;
now = System.currentTimeMillis();
 
for(int i = 0; i<500000; ++i){
	Class<?> c = Class.forName("test.TestClass");
	Class<?>[] argsType = new Class[1];
	argsType[0] = int.class;
	Method m = c.getMethod("setNum", argsType);
	m.invoke(t, i);
	sum += t.getNum();
}
System.out.println("标准反射耗时"+(System.currentTimeMillis() - now) + "ms，和是" +sum);
 
sum = 0;
 
Class<?> c = Class.forName("test.TestClass");
Class<?>[] argsType = new Class[1];
argsType[0] = int.class;
Method m = c.getMethod("setNum", argsType);
 
now = System.currentTimeMillis();
 
for(int i = 0; i<500000; ++i){
	m.invoke(t, i);
	sum += t.getNum();
}
System.out.println("缓存反射耗时"+(System.currentTimeMillis() - now) + "ms，和是" +sum);
 
sum = 0;
MethodAccess ma = MethodAccess.get(TestClass.class);
int index = ma.getIndex("setNum");
now = System.currentTimeMillis();
 
for(int i = 0; i<500000; ++i){
	ma.invoke(t, index, i);
	sum += t.getNum();
}
System.out.println("reflectasm反射耗时"+(System.currentTimeMillis() - now) + "ms，和是" +sum);
```

测试结果如下：
<pre>
get-set耗时6ms秒，和是124999750000
标准反射耗时1838ms，和是124999750000
缓存反射耗时70ms，和是124999750000
reflectasm反射耗时20ms，和是124999750000
</pre>

可以看出，查找函数依然是耗时最长的部分，JDK7的优化确实很不错，由JDK6的40倍降到10倍左右，reflectasm invoke的效率比java原生invoke好，大致是直接访问的4倍时间。效率确实可以一用。
