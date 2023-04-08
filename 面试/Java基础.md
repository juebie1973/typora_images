# **1.Java反射的优缺点**

## **（1）反射是Java中一个重要的特性，它在程序运行的过程中:**

### **key：（构造、获取、调用）**

**![image-20230320155124500](C:\Users\16644\AppData\Roaming\Typora\typora-user-images\image-20230320155124500.png)**

## **（2）反射在Java中的功能：（key：动态）**

**![image-20230320155249521](C:\Users\16644\AppData\Roaming\Typora\typora-user-images\image-20230320155249521.png)**

## **（3）java.lang.reflect包：**

**![image-20230320155435728](C:\Users\16644\AppData\Roaming\Typora\typora-user-images\image-20230320155435728.png)**

## **（4）反射的使用场景：（key:代理类、实例化）**

**![image-20230320155628281](C:\Users\16644\AppData\Roaming\Typora\typora-user-images\image-20230320155628281.png)**

## **（5）高手回答**

**![image-20230320154934097](C:\Users\16644\AppData\Roaming\Typora\typora-user-images\image-20230320154934097.png)**

**![image-20230320155006793](C:\Users\16644\AppData\Roaming\Typora\typora-user-images\image-20230320155006793.png)**



# **2. equals()与hashcode()**

## **（1）：问题对象相等的问题：**

### **引用到完全相同的对象？**

### **还是有相同内容（意义）的不同对象？**



## **（2）：引用相等性与对象相等性**

**![image-20230321212327335](C:\Users\16644\AppData\Roaming\Typora\typora-user-images\image-20230321212327335.png)**

 要想两个对象相等，

就必须重写hashCode方法，确保两个对象有相同的hashcode值

同时，确保以另外一个对象为参数的equals方法调用返回true

**![image-20230321212603305](C:\Users\16644\AppData\Roaming\Typora\typora-user-images\image-20230321212603305.png)**

## **（3）：String对象重写了equals方法，会有两个操作（也就是 引用相等 和 内容相等）**

![image-20230321212749700](C:\Users\16644\AppData\Roaming\Typora\typora-user-images\image-20230321212749700.png)**

**![image-20230321213006332](C:\Users\16644\AppData\Roaming\Typora\typora-user-images\image-20230321213006332.png)**

## **（3）：两者关系**

**![image-20230321213221527](C:\Users\16644\AppData\Roaming\Typora\typora-user-images\image-20230321213221527.png)**

**![image-20230321213319507](C:\Users\16644\AppData\Roaming\Typora\typora-user-images\image-20230321213319507.png)**

**![image-20230321213351269](C:\Users\16644\AppData\Roaming\Typora\typora-user-images\image-20230321213351269.png)**

**![image-20230321213402389](C:\Users\16644\AppData\Roaming\Typora\typora-user-images\image-20230321213402389.png)**

**![image-20230321213425961](C:\Users\16644\AppData\Roaming\Typora\typora-user-images\image-20230321213425961.png)**

**![image-20230321213445668](C:\Users\16644\AppData\Roaming\Typora\typora-user-images\image-20230321213445668.png)**

**![image-20230321213533827](C:\Users\16644\AppData\Roaming\Typora\typora-user-images\image-20230321213533827.png)**



**hashcode的值默认是jvm来随机生成的，对于不同的对象来说，有可能产生相同的哈希值，在这种情况下在哈希表里体现就是哈希冲突，通常会使用链表和线性探测等方式来解决冲突的问题。**

**同时：**

**![image-20230321214821364](C:\Users\16644\AppData\Roaming\Typora\typora-user-images\image-20230321214821364.png)**

## **（4）没有重写equals方法**

**![image-20230321215449766](C:\Users\16644\AppData\Roaming\Typora\typora-user-images\image-20230321215449766.png)**



## **（5）只重写了equals（）方法**

**![image-20230321215724386](C:\Users\16644\AppData\Roaming\Typora\typora-user-images\image-20230321215724386.png)**

**见解：如果两个判断两个对象相等的话，虽然重写了equals（）方法使得两个对象是相等使得true，但是两个对象的地址是不同的，而hashcode值就是根据对象的地址来获得的。**

## **（6）回答（为什么重写equals（））后，必须重写hashcode（）呢？）**

**![image-20230322093759020](C:\Users\16644\AppData\Roaming\Typora\typora-user-images\image-20230322093759020.png)**

**只重写equals（）方法的对象，在使用散列集合进行储存的时候就会出现问题，因为散列集合是使用哈希值来计算key的存储位置，没有重写hashcode（）方法如果存储两个完全相同的对象的话，但是会有不同的哈希值，就会导致两个相同的对象存储到哈希表的不同位置。当我们想要根据去根据对象去获取数据的时候，就会出现一个悖论，一个完全相等的对象出现在两个位置，就会破环大家约定俗称的规则，使得我们在程序中会出现不可预料的错误。**

# **3.HashMap是如何解决hash冲突的？**

## **HashMap是采用了数组的结构来存储数据元素，数组的默认长度是16，我们put方法添加数组的时候，HashMap会根据key的hash值进行取模运算，最终将取模运算后的值保存到数组的一个指定位置，但是这样也会存在hash冲突的问题，也就是两个不同hash值的key最终取模以后会落到同一个数组下标，所以HashMap引入一个链式寻址法来解决hash冲突的问题，也就是在存在冲突的key的话，就会使用拉链法，并采用尾插法，把冲突的key依次保存在尾部，同时避免链表过长，导致查询效率下降，所以当链表长度大于等于8，并且数组长度大于等于64的时候，HaspMap会把当前hash冲突的链表转化为红黑树，从而去减少链表数据查询的一个时间复杂度，进而去提高效率。**

# **4. 解决hash冲突的方法**

## **（1）再hash法**

**![image-20230322110957624](C:\Users\16644\AppData\Roaming\Typora\typora-user-images\image-20230322110957624.png)**

## **（2）开放寻址法（ThreadLocal）**

**![image-20230322111031786](C:\Users\16644\AppData\Roaming\Typora\typora-user-images\image-20230322111031786.png)**

## **（3）建立公共溢出区**

**![image-20230322111146668](C:\Users\16644\AppData\Roaming\Typora\typora-user-images\image-20230322111146668.png)**

# **5.HashMap与HashTable区别**

## **（1）集合**

**使用合适的集合类不仅要从不同的数据结构来看，也要从安全性、性能、功能特性等方面。去理解集合的差异性，底层的工作原理。**

**![image-20230322111946889](C:\Users\16644\AppData\Roaming\Typora\typora-user-images\image-20230322111946889.png)**

## **（2）相同点：**

## **基于hash表实现的key-value结构的集合**

## **（3）Hashtable**

**![image-20230322112542649](C:\Users\16644\AppData\Roaming\Typora\typora-user-images\image-20230322112542649.png)**

**这是因为所有的数据访问的方法，都加了synchronized同步锁**

**![image-20230322112717866](C:\Users\16644\AppData\Roaming\Typora\typora-user-images\image-20230322112717866.png)**

**链表主要是解决Hash冲突的问题**

## **（4）HashMap**

**HashMap是jdk1.2引入的一个线程安全的集合类。**

![image-20230322112808487](C:\Users\16644\AppData\Roaming\Typora\typora-user-images\image-20230322112808487.png)****



## **（5）回答**

### **一、从功能特性的角度**

**![image-20230322113141620](C:\Users\16644\AppData\Roaming\Typora\typora-user-images\image-20230322113141620.png)**

### **二、从内部实现的角度**

**![image-20230322113228612](C:\Users\16644\AppData\Roaming\Typora\typora-user-images\image-20230322113228612.png)**

**因为HashMap是将null转化为一个0进行储存，而Hashtable是不允许的。**

### **三、散列算法不同**

**![image-20230322114229631](C:\Users\16644\AppData\Roaming\Typora\typora-user-images\image-20230322114229631.png)**

# **6.HashMap为什么扩容，什么时候扩容**

## （1）数据存储容器

**在任何编程语言中，我们经常需要在内存中去临时存放一段数据，我们可以使用官方封装好的一些集合框架。**

**比如说用List、HashMap、Set等等作为临时数据存储的容器。**

**当我们创建一个集合对象的时候，实际上就是在内存里面一次性申请了一块内存空间。而这个内存空间的大小是在创建集合对象的时候去指定的。**

![image-20230322155226362](C:\Users\16644\AppData\Roaming\Typora\typora-user-images\image-20230322155226362.png)

## （2）默认大小

**![image-20230322120011248](C:\Users\16644\AppData\Roaming\Typora\typora-user-images\image-20230322120011248.png)**

## （3） 动态扩容

在实际开发过程中，我们需要去存储的数据量往往是大于存储容器的默认大小的。

所以，出现容量默认大小不能满足需求时，就需要扩容。

而这个扩容的动作是由集合自动完成的，每种集合的扩容规则都有差异。但总的扩容原则是，当集合存储容量达到某个阈值的时候，集合就会进行动态扩容，而更好地满足更多数据存储的需求。
![image-20230322155436412](C:\Users\16644\AppData\Roaming\Typora\typora-user-images\image-20230322155436412.png)

## （4）扩容原理

**当HashMap里面的元素个数超过临界值的时候会自动触发扩容。这个临界值的计算公式如图所示：**

**它等于负载因子 乘以 容量大小，负载因子的默认值是0.75，而容量大小默认是16,。也就是说，第1次扩容的动作会在元素个数达到12的时候触发，扩容的大小是原来的2倍。HashMap的最大容量是Integer.MAX_VALUE也就是2的31次方减1。**

![image-20230322155755446](C:\Users\16644\AppData\Roaming\Typora\typora-user-images\image-20230322155755446.png)****

假设，我们向HashMap中插入1024个元素，如果按照默认容量大小是16的情况下，随着元素的不断增加，会造成至少7次扩容。而这7次扩容过程中，需要重新去创建新的Hash表，并且进行数据的迁移，对性能的影响是非常大的。

## （5）负载因子

![image-20230322154911110](C:\Users\16644\AppData\Roaming\Typora\typora-user-images\image-20230322154911110.png)

我们知道，HashMap采用的是链式寻址的方式来解决Hash冲突的问题。而为了避免链表过长，导致时间复杂度增加的情况，所以，HashMap判断链表长度大于等于8的时候，就会转换为红黑树，从而提升检索的效率。

因此，扩容因子的值的设置，本质上就是一个冲突的概率以及空间利用率之间的一个平衡。关于0.75这个值的来源，和统计学里面的泊松分布有关系。

我们知道，HashMap采用的是链式寻址的方式来解决Hash冲突的问题。而为了避免链表过长，导致时间复杂度增加的情况，所以，HashMap判断链表长度大于等于8的时候，就会转换为红黑树，从而提升检索的效率。

**![image-20230322112808487](C:\Users\16644\AppData\Roaming\Typora\typora-user-images\image-20230322112808487.png)

当负载因子为0.75的时候，链表长度达到8的可能性几乎为0，也就是说，比较好的做到了空间成本和时间成本的平衡。

好了，以上就是我对HashMap扩容的理解。

# **7.Java自动装箱和拆箱**

## （1）背景

**是否直接将基本数据类型当作对象来处理？例如：在jdk1.5之前，我们无法将基本数据类型放入到ArrayList或者HashMap中，这是因为泛型的规则是只能指定 类 或者 接口类型，而没有基本数据类型。**

## （2）自动装箱

### JDK1.5之前

```java
list.add(new Integer(3));
// 或者
list.add(Integer.valueOf(3));  // Java的每个包装类都在Java的lang包中
```

### JDK1.5之后

```java
list.add(3);  // 直接加int值就行，会自动装箱的
```

## （3）自动拆箱

### JDK1.5之前

```java
list.get(0),intValue();
```

### JDK1.5之后

```java
list.get(0);
```

## （4）题目

![image-20230322153045258](C:\Users\16644\AppData\Roaming\Typora\typora-user-images\image-20230322153045258.png)**

![image-20230322153132421](C:\Users\16644\AppData\Roaming\Typora\typora-user-images\image-20230322153132421.png)**

# 8.   序列化

![image-20230322185834559](C:\Users\16644\AppData\Roaming\Typora\typora-user-images\image-20230322185834559.png)

## （1）用途

- **把对象的字节序列永久保存到硬盘上，通常存放在一个文件中（序列化对象）**
- **在网络上传送对象的字节序列（网络传输对象）**

## （2）实际上就是将数据持久化，防止一直存储在内存当中，消耗内存资源。而且序列化后也能更好的便于网络运输何传播

- 序列化：将java对象转换为字节序列
- 反序列化：把字节序列回复为原先的java对象

## （3）序列化实现

![image-20230322194359688](C:\Users\16644\AppData\Roaming\Typora\typora-user-images\image-20230322194359688.png)

### 将序列化对象写入文件

![image-20230322195040350](C:\Users\16644\AppData\Roaming\Typora\typora-user-images\image-20230322195040350.png)

### 将类序列化写入文件

![image-20230322195610503](C:\Users\16644\AppData\Roaming\Typora\typora-user-images\image-20230322195610503.png)

![image-20230322195742609](C:\Users\16644\AppData\Roaming\Typora\typora-user-images\image-20230322195742609.png)

## （4）反序列化实现

![image-20230322200010383](C:\Users\16644\AppData\Roaming\Typora\typora-user-images\image-20230322200010383.png)

![image-20230322200202023](C:\Users\16644\AppData\Roaming\Typora\typora-user-images\image-20230322200202023.png)

![image-20230322200211275](C:\Users\16644\AppData\Roaming\Typora\typora-user-images\image-20230322200211275.png)

## （5）问题

![image-20230322200346974](C:\Users\16644\AppData\Roaming\Typora\typora-user-images\image-20230322200346974.png)

## （6）要点

![image-20230322200441687](C:\Users\16644\AppData\Roaming\Typora\typora-user-images\image-20230322200441687.png)

![image-20230322200451792](C:\Users\16644\AppData\Roaming\Typora\typora-user-images\image-20230322200451792.png)





