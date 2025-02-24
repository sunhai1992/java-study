# java-study
java面试知识点总结(持续更新，欢迎star,fork补充)
###JDK，JRE和 jVM 的区别
JVM：Java 虚拟机，负责将编译产生的字节码转换为特定机器代码，实现一次编译多处执行；  
JRE：java运行时环境，包含了java虚拟机jvm，java基础类库。是使用java语言编写的程序运行所需要的软件环境；  
JDK：java开发工具包，是编写java程序所需的开发工具。JDK包含了JRE，同时还包含了编译器javac，调试和分析工具，JavaDoc。

###自动装箱(autoboxing)与拆箱(unboxing)
自动装箱是 Java 编译器在基本数据类型和对应的对象包装类型之间做的一个转化。
比如：把 int 转化成 Integer，double 转化成 Double等,反之就是自动拆箱。    
Integer  a=1;//这就是一个自动装箱，如果没有自动装箱的话，需要这样Integer  a=new Integer(1)  
int b=a;//这就是一个自动拆箱，如果没有自动拆箱的话，需要这样：int b=a.intValue()  
这样就能看出自动装箱和自动拆箱是简化了基本数据类型和相对应对象的转化步骤  
[Java中的自动装箱与拆箱](http://droidyue.com/blog/2015/04/07/autoboxing-and-autounboxing-in-java/index.html)

###Java中为什么要为基本类型提供封装类呢？ 
一是为了在各种类型间转化，通过各种方法的调用。否则你无法直接通过变量转化。  
比如，现在int要转为String    
int a=0;    
String result=Integer.toString(a);    
二是比如我现在要用泛型  
List<Integer> nums;   
这里<>需要类。如果你用int。它会报错的   

###Java 创建对象的几种方式
(1) 用 new 语句创建对象，这是最常见的创建对象的方法  
(2) 运用反射手段,调用 java.lang.Class 或者 java.lang.reflect.Constructor 类的 newInstance() 实例方法  
(3) 调用对象的 clone() 方法  
(4) 运用反序列化手段，调用 java.io.ObjectInputStream 对象的 readObject() 方法

(1)和(2)都会明确的显式的调用构造函数；(3)是在内存上对已有对象的影印，所以不会调用构造函数 (4)是从文件中还原类的对象，也不会调用构造函数。

###序列化(Serializable )与反序列化(Deserialize) 
对象序列化(Serializable)是指将对象转换为字节序列的过程，而反序列化则是根据字节序列恢复对象的过程。    
序列化一般用于以下场景：   
1.永久性保存对象，保存对象的字节序列到本地文件中；   
2.通过序列化对象在网络中传递对象；    
3.通过序列化在进程间传递对象。   

只有实现了Serializable和Externalizable接口的类的对象才能被序列化，   
java.io.ObjectOutputStream代表对象输出流，它的writeObject(Objectobj)方法可对参数指定的obj对象进行序列化，把得到的字节序列写到一个目标输出流中。
java.io.ObjectInputStream代表对象输入流，它的readObject()方法从一个源输入流中读取字节序列，再把它们反序列化为一个对象，并将其返回。

###覆盖 (Override) 和重载 (Overload)
Java 中的方法重载发生在同一个类里面两个或者是多个方法的方法名相同但是参数不同的情况；  
方法覆盖是说子类重新定义了父类的方法，方法覆盖必须有相同的方法名，参数列表和返回类型。

###内存中的栈（stack）、堆(heap)和静态存储区的用法
通常我们定义一个基本数据类型的变量，一个对象的引用，还有就是函数调用的现场保存都使用内存中的栈空间；而通过new关键字和构造器创建的对象放在堆空间；程序中的字面量（literal）如直接书写的100、“hello”和常量都是放在静态存储区中。栈空间操作最快但是也很小，通常大量的对象都是放在堆空间，整个内存包括硬盘上的虚拟内存都可以被当成堆空间来使用。   
```java
String str = new String(“hello”);
```   
上面的语句中 str 放在栈上，用 new 创建出来的字符串对象放在堆上，而“hello”这个字面量放在静态存储区。

###强引用、弱引用、软引用、虚引用  
**强引用**：如“Object obj = new Object（）”，这类引用是 Java 程序中最普遍的。只要强引用还存在，垃圾收集器就永远不会回收掉被引用的对象。   
**软引用**：它用来描述一些可能还有用，但并非必须的对象。在系统内存不够用时，这类引用关联的对象将被垃圾收集器回收。JDK1.2 之后提供了 SoftReference 类来实现软引用。   
**弱引用**：它也是用来描述非需对象的，但它的强度比软引用更弱些，被弱引用关联的对象只能生存岛下一次垃圾收集发生之前。当垃圾收集器工作时，无论当前内存是否足够，都会回收掉只被弱引用关联的对象。在 JDK1.2 之后，提供了 WeakReference 类来实现弱引用。    
**虚引用**：最弱的一种引用关系，完全不会对其生存时间构成影响，也无法通过虚引用来取得一个对象实例。为一个对象设置虚引用关联的唯一目的是希望能在这个对象被收集器回收时收到一个系统通知。JDK1.2 之后提供了 PhantomReference 类来实现虚引用。  
[Java 7之基础 - 强引用、弱引用、软引用、虚引用](http://blog.csdn.net/mazhimazh/article/details/19752475)   
[Java垃圾回收机制与引用类型](http://www.infoq.com/cn/articles/cf-java-garbage-references)     
[内存管理与垃圾回收](http://www.cnblogs.com/vamei/archive/2013/04/28/3048353.html)   

###Java垃圾回收机制
在C++中，对象所占的内存在程序结束运行之前一直被占用，在明确释放之前不能分配给其它对象；而在Java中，当没有对象引用指向原先分配给某个对象的内存时，该内存便成为垃圾。JVM的一个系统级线程会自动释放该内存块。垃圾收集意味着程序不再需要的对象是"无用信息"，这些信息将被丢弃。当一个对象不再被引用的时候，内存回收它占领的空间，以便空间被后来的新对象使用。事实上，除了释放没用的对象，垃圾收集也可以清除内存记录碎片。由于创建对象和垃圾收集器释放丢弃对象所占的内存空间，内存会出现碎片。碎片是分配给对象的内存块之间的空闲内存洞。碎片整理将所占用的堆内存移到堆的一端，JVM将整理出的内存分配给新的对象。   
垃圾收集能自动释放内存空间，减轻编程的负担。这使Java虚拟机具有一些优点。首先，它能使编程效率提高。在没有垃圾收集机制的时候，可能要花许多时间来解决一个难懂的存储器问题。在用Java语言编程的时候，靠垃圾收集机制可大大缩短时间。其次是它保护程序的完整性， 垃圾收集是Java语言安全性策略的一个重要部份。垃圾收集的一个潜在的缺点是它的开销影响程序性能。Java虚拟机必须追踪运行程序中有用的对象，而且最终释放没用的对象。这一个过程需要花费处理器的时间。其次垃圾收集算法的不完备性，早先采用的某些垃圾收集算法就不能保证100%收集到所有的废弃内存。当然随着垃圾收集算法的不断改进以及软硬件运行效率的不断提升，这些问题都可以迎刃而解。   
一般来说，Java开发人员可以不重视JVM中堆内存的分配和垃圾处理收集，但是，充分理解Java的这一特性可以让我们更有效地利用资源。同时要注意finalize()方法是Java的缺省机制，有时为确保对象资源的明确释放，可以编写自己的finalize方法。(引用自百度)   
[Java 垃圾收集机制](http://wiki.jikexueyuan.com/project/java-vm/garbage-collection-mechanism.html)

###List,Map,Set
![图示](https://github.com/ALLENnan/java-study/blob/master/pic.jpg)   
[集合大家族](http://wiki.jikexueyuan.com/project/java-enhancement/java-twenty.html)

###hashcode的作用  
Java中的hashCode方法就是根据一定的规则将与对象相关的信息（比如对象的存储地址，对象的字段等）映射成一个数值，这个数值称作为散列值。
如果集合中已经存在一万条数据或者更多的数据，如果采用equals方法去逐一比较，效率必然是一个问题。此时hashCode方法的作用就体现出来了，当集合要添加新的对象时，先调用这个对象的hashCode方法，得到对应的hashcode值，实际上在HashMap的具体实现中会用一个table保存已经存进去的对象的hashcode值，如果table中没有该hashcode值，它就可以直接存进去，不用再进行任何比较了；如果存在该hashcode值，就调用它的equals方法与新元素进行比较，相同的话就不存了，不相同就散列其它的地址，所以这里存在一个冲突解决的问题，这样一来实际调用equals方法的次数就大大降低了。  
[hashcode方法的作用](http://www.cnblogs.com/dolphin0520/p/3681042.html)

###HashCode和equal方法
1、hashCode的存在主要是用于查找的快捷性，如Hashtable，HashMap等，hashCode是用来在散列存储结构中确定对象的存储地址的；  
2、如果两个对象相同，就是适用于equals(java.lang.Object) 方法，那么这两个对象的hashCode一定要相同；  
3、如果对象的equals方法被重写，那么对象的hashCode也尽量重写，并且产生hashCode使用的对象，一定要和equals方法中使用的一致，否则就会违反上面提到的第2点；  
4、两个对象的hashCode相同，并不一定表示两个对象就相同，也就是不一定适用于equals(java.lang.Object)方法，只能够说明这两个对象在散列存储结构中，如Hashtable，他们“存放在同一个篮子里”。   
[HashCode和equal方法](http://www.cnblogs.com/nktblog/articles/2518111.html)

###用户线程(User Thread)与守护线程(Daemon Thread) 
守护线程，是指用户程序在运行的时候后台提供的一种通用服务的线程。只要当前JVM实例中尚存在任何一个用户线程没有结束，守护线程就全部工作；只有当最后一个用户线程结束时，守护线程随着 JVM 一同结束工作。 守护线程最典型的应用就是 GC (垃圾回收器)。        
[JAVA并发编程——守护线程(Daemon Thread)](http://www.cnblogs.com/luochengor/archive/2011/08/11/2134818.html)

###进程和线程的区别
一个进程对应一个程序的执行，而一个线程则是进程执行过程中的一个单独的执行序列，一个进程可以包含多个线程。线程有时候也被称为轻量级进程。
一个Java虚拟机的实例运行在一个单独的进程中，不同的线程共享Java虚拟机进程所属的堆内存。这也是为什么不同的线程可以访问同一个对象。线程彼此共享堆内存并保有他们自己独自的栈空间。这也是为什么当一个线程调用一个方法时，他的局部变量可以保证线程安全。但堆内存并不是线程安全的，必须通过显示的声明同步来确保线程安全。
