[TOC]



## Oracle笔记：

### 解决Oracle数据库启动异常

接下来说到核心了，进入sql*plus可编辑处窗口后即输入以下编码(文字可忽略)

1. 先链接 输入SQL> conn 用户名/密码 as sysdba 参考SQL> conn sys/orcl as sysdba
2. 查看你的数据库信息SQL> select * from v$log;
3. 关闭 例程，并卸载了数据库 SQL> shutdown immediate;
4. 重启数据库，并装载数据库 SQL> startup
5. 修改数据库打开方式：SQL> alter database open;
6. SQL> alter database open resetlogs; 

### Oracle  单行数据拆分字段成多条数据

		select ic, ie, regexp_substr(it, '[^,]', 1, level) as str
		from (select a.ic, a.ie, a.it, rownum as rm from tab_rec a)
		connect by nocycle(level <= regexp_count(it, ',') + 1
				and (prior rm) = rm
				and (prior dbms_random.value()) is not null)
		order by it, substr(it, level, 1);
### Oracle 带有正则表达式的函数：

1. regexp_count（字符串，正则表达式）-----记数
2. regexp_like() --返回满足条件的字段
   regexp_like(search_string ,pattern[,match_option]);
   参数说明：

- search_string是搜索值
- pattern 正则表达式元字符构成的匹配模式,长度限制在512字节内
- match_option是一个文本串，允许用户设置该函数的匹配行为。可以使用的选项有：
- c 匹配时，大小写敏感，默认值
- i 匹配时，大小写不敏感
- n 允许使用原点（.）匹配任何新增字符
- m 允许将源字符作为多个字符串对待 

3. regexp_instr() --返回满足条件的字符或字符串的位置
   regexp_instr(x,pattern[,start[,occurrence[,return_option[,match_option]]]])
   参数说明：

- x 待匹配的字符串
- pattern 正则表达式元字符构成的匹配模式
- start 开始匹配位置，如果不指定默认为1
- occurrence 匹配的次数，如果不指定，默认为1
- return_option 指定返回的类型，若该参数为0，则返回值为匹配位置的第一个字符，	若为非0，则返回匹配值的最后一个位置+1
- match_option 意义同regexp_like的一样

4. regexp_replace() --返回替换后的字符串
   regexp_replace(x,pattern[,replace_string[,start[,occurrence[match_option]]]])
   参数说明：

- x 待匹配的字符串
- pattern 正则表达式元字符构成的匹配模式
- replace_string 替换字符串
- start 开始匹配位置，如果不指定默认为1
- occurrence 匹配的次数，如果不指定，默认为1
- match_option 意义同regexp_like的一样

5. regexp_substr() --返回满足条件的字符或字符串
   regexp_substr(x,pattern[,start[,occurrence[match_option]]])
   参数说明：

- x 待匹配的字符串
- pattern 正则表达式元字符构成的匹配模式
- start 开始匹配位置，如果不指定默认为1
- occurrence 匹配的次数，如果不指定，默认为1
- match_option 意义同regexp_like的一样

## Java基础学习笔记

#### 1. Java常量

|    类型    |                  含义                  |            实例             |
| :--------: | :------------------------------------: | :-------------------------: |
|  整数常量  |               所有的整数               |      0，1，  567，  -9      |
|  小数常量  |               所有的小数               |     0.0，  -0.1，  2.55     |
|  字符常量  | 单引号引起来,只能写一个字符,必须有内容 |     'a' ， ' '，  '好'      |
| 字符串常量 | 双引号引起来,可以写多个字符,也可以不写 | "A" ，"Hello" ，"你好" ，"" |
|  布尔常量  |               只有两个值               |       true ，  false        |
|   空常量   |               只有一个值               |            null             |

#### 2. Java中进制的表示以及进制中的转换

​	二进制 	   以0b开头
​	八进制 	   以0开头
​	十六进制     以0x开头

1. 任意进制转化为10进制
2. 10进制转换为其他进制
   除基倒取余
3. 8421法
4. 原码、反码、补码
   正数的原、反、补相同
   负数的反码为原码按位取反（不包括符号位），负数的补码为反码加1

#### 3. 数据类型分类

- Java的数据类型分为两大类：
  -  基本数据类型：包括 整数 、 浮点数 、 字符 、 布尔 。
  -  引用数据类型：包括 类 、 数组 、 接口 

|   数据类型   |     关键字     | 内存占用 |        取值范围        |
| :----------: | :------------: | :------: | :--------------------: |
|    字节型    |      byte      | 1个字节  |        -128~127        |
|    短整型    |     short      | 2个字节  |      -32768~32767      |
|     整型     |  int（默认）   | 4个字节  |  -231次方~2的31次方-1  |
|    长整型    |      long      | 8个字节  | -2的63次方~2的63次方-1 |
| 单精度浮点型 |     float      | 4个字节  | 1.4013E-45~3.4028E+38  |
| 双精度浮点型 | double（默认） | 8个字节  |  4.9E-324~1.7977E+308  |
|    字符型    |      char      | 2个字节  |        0-65535         |
|    布尔型    |    boolean     | 1个字节  |      true，false       |

​	**注意：Java中的默认类型：整数类型是 int 、浮点类型是 double** 

​	注意：long类型：建议数据后加L表示；

​				ﬂoat类型：建议数据后加F表示；

#### 数据类型转换

**自动转换：**将取值范围小的类型 自动提升为 取值范围大的类型 

**转换规则：**范围小的类型向范围大的类型提升， byte、short、char 运算时直接提升为 int 

#### Switch分支

- switch中的case有穿透现象，只有遇到break才会终止，或者是switch结束

- switch语句中，表达式的数据类型，可以是byte，short，int，char，enum（枚举），JDK7后可以接收String

#### 方法：

- **方法重载：**指在同一个类中，允许存在一个以上的同名方法，只要它们的**参数列表不同即可**，**与修饰符和返回值类型无关**。 
  - 参数列表：个数不同，数据类型不同，顺序不同。 
  - 重载方法调用：JVM通过方法的参数列表，调用不同的方法
- **方法重写 ：**子类中出现与父类一模一样的方法时（返回值类型，方法名和参数列表都相同），会出现覆盖效果，也称为重写或者覆写。声明不变，重新实现
  - 子类方法覆盖父类方法，必须要保证权限大于等于父类权限
  - 子类的异常要比父类的异常少

#### JVM虚拟机的内存

- JVM的内存划分：

  | 区域名称   | 作用                                                         |
  | :--------- | :----------------------------------------------------------- |
  | 寄存器     | 给CPU使用，和我们开发无关。                                  |
  | 本地方法区 | JVM在使用操作系统功能的时候使用，和我们开发无关              |
  | 方法区     | 加载存储class对象；里面有一块静态区，用于加载class文件时，初始化静态变量； |
  | 堆         | 存储对象或者数组，new来创建的，都存储在堆内存。              |
  | 栈         | 方法运行时使用的内存，比如main方法运行，进入方法栈中执行     |

#### 面向对象

- **对象**泛指**现实中一切事物，每种事物都具备自己的属性和行为。**
- **面向对象思想**：就是指**在计算机程序设计过程中，参照现实中事物，将事物的属性特征、行为特征抽象出来，描述成计算机事件的设计思想。** 它区别于面向过程思想，强调的是通过调用对象的行为来实现功能，而不是自己一步一步的去操作实现。 
- **区别:**
  面向过程：强调步骤。
  面向对象：强调对象。
- **三大基本特征：**封装、继承和多态
  - **封装：**将属性隐藏起来，若需要访问某个属性，提供公共方法对其访问
  - **继承：**就是子类继承父类的属性和行为，使得子类对象具有与父类相同的属性、相同的行为。子类可以直接访问父类中的非私有的属性和行为。
  - **多态：**是指同一行为，具有多个不同表现形式。运行时多态（方法重写）和编译器多态（方法重载）
    - 三个条件：
      1. 必须继承和实现
      2. 必须存在方法的重写（多态的体现）
      3. 必须有父类引用指向子类的对象（多态的格式）
- **类：**是一组相关属性和行为的集合。可以看成是一类事物的模板，使用事物的属性特征和行为特征来描述该类事物。
- **对象：**是一类事物的具体体现。对象是类的一个实例，必然具备该类事物的属性 和 行为。
- **JavaBean** ：是 Java语言编写类的一种标准规范。符合 JavaBean 的类，要求类必须是具体的和公共的，并且具有无参数的构造方法，提供用来操作成员变量的 set 和 get 方法。

#### 4. 局部变量在使用前必须初始化，成员变量有默认初始化值

#### 5. 数组默认初始化

- 整型       byte /short/ int /long 默认初始化值：0
- 浮点型   float double 默认初始化值：0.0
- 字符型   char  默认初始化值 '\u0000' 
- 引用数据类型   数组、对象、接口默认初始化值为null

#### 7. 值传递和引用传递

1. 基本数据类型的值传递，传递的是基本数据类型的值，不改变原值，因为方法使用后会弹栈

2. 引用数据类型的值传递，传递的是引用数据类型的内存地址，通过内存地址找到堆内存相应的值，因此即使方法执行完成后会弹栈，堆中的对象还在，所以会改变原值。

#### 8. static 静态关键字的特点

- 随着类(class文件)的加载而加载

- 优先于对象存在

- 被所有类对象共享

- 可以使用类名调用

- 静态只能访问静态的（静态优先于对象存在）

- 静态方法中没有this关键字（this随着对象存在而存在）

  <img src="G:\Typroa\picture\static静态原理图.png" alt="1570780317762"  />

#### 9. 生成文档说明书

@param 参数
@return 返回值
@author 作者
@version 版本
Javadoc  -d api -version -author 类全名

#### 10. this和super的区别

- super() 和 this() 都必须是在构造方法的第一行，所以不能同时出现。 
- this代表当前对象的引用（既可以调用父类的成员变量，也可以调用本类的成员变量）
- super代表父类对象的引用（调用父类的成员变量）

#### 11. 子类任何构造方法都会默认访问父类的空参构造函数

**原因**：子类初始化对象时，会先初始化父类对象，所以会先调用父类的构造方法

#### 12. 代码块：局部代码块、静态代码块、构造代码块、同步代码块（多线程）

1. **局部代码块** ： 类中方法里存在，初始化局部变量，限定变量的生命周期
2. **静态代码块**：类中方法外出现， 加static修饰，用于给类初始化，在类加载的时候就执行（加载到方法区的静态区），只执行一次。开发中一般使用于加载驱动。
3. **构造代码块**：类中方法外，将多个构造方法的复用代码抽取，每次调用构造方法就会执行，并且优先于构造方法执行。

#### 13. final 关键字使用

1. final 修饰的变量为常量，值只能赋值一次，赋值后不能改变（final修饰基本数据类型，是值不能改变；修饰引用数据类型，是地址值不能改变，对象中的属性可以改变）

   final修饰的常量，只能显示初始化和在构造函数里初始化（构造函数结束前）

2. final 修饰的类不能被继承

3. final 修饰的方法不能被重写

#### 14. Java编译带包名的类

1. 编译带包名的类
    Java -d . HelloWord.java (当前目录下建立包) 
2. 运行带包名的类
    Java 全包名.Hell

#### 15. 将JSON格式的字符串转换为js对象

​	使用eval()函数 (字符串应被（）包含)

#### 16. ==号和equals方法的区别 (掌握)

- ==是一个比较运算符号,既可以比较基本数据类型,也可以比较引用数据类型
  - 基本数据类型比较的是值
  - 引用数据类型比较的是地址值
- equals方法是一个方法,只能比较引用数据类型。所有的对象都会继承Object类中的方法,如果没有重写Object类中的equals方法,equals方法和==号比较引用数据类型无区别,重写后的equals方法比较的是对象中的属性

#### 17. java中有常量优化机制

​	在编译时会将常量进行处理，如：

​	byte  b = 3 + 7; //在编译时就被运算成10，所以b=10;

### 接口

接口，是Java语言中一种引用类型，但一定要明确它并不是类，是方法的集合，如果说类的内部封装了成员变量、构造方法和成员方法，那么接口的内部主要就是封装了方法，包含抽象方法（JDK 7及以前），默认方法和静态方法（JDK 8），私有方法 （JDK 9）。

**特点：**

- 接口中，无法定义成员变量，但是可以定义常量，其值不可以改变，默认使用public static ﬁnal修饰。 
- 接口中，没有构造方法，不能创建对象。 
- 接口中，没有静态代码块

接口就是一种定义规则，是一种公共规范，提供给需要的去实现；

接口（Interface）中的内容有：

1. ##### 成员变量（实际是常量），格式：

   [public] [static] [final]  数据类型    常量名称   =   常量值；

   **注意：**常量必须赋值，而且一旦赋值就不能在改变；

   ​		   常量名称必须大写 ；

2. ##### 接口中最重要的就是抽象方法，格式：

   [public]  [abstract]   返回值类型  方法名称（参数列表）；

   注意：实现接口的实现类，必须重写接口的所有抽象方法；

3. ##### 从jdk1.8开始，接口中可以定义默认方法，格式：

   [public]  default   返回值类型方法名称 （参数列表）{

   ​	方法体：

   }

   **备注：**默认方法是为了解决接口升级的问题，当接口中需要增加方法是，已经投入生产的实现类会产生问题，因此有了默认方法后，实现类不用去重写此方法（也可以重写），因为实现类相当于继承了接口的默认方法，直接使用即可；

   **注意：**默认方法也可以被重写

4. ##### 从jdk1.8开始，接口可以定义静态方法 ，格式：

   [public]  static  返回值类型  方法名称（参数列表）{

   ​	方法体；

   }

   **注意：**应通过接口调用，不用创建实现类对象；

5. ##### 从jdk1.9开始，接口中可以定义私有方法，格式 ：

   - **普通私有方法（**解决接口内普通方法代码复用问题）

     private 返回值类型  方法名称（参数列表）{

     ​	方法体；

     }

   - **静态私有方法**（解决接口内静态方法代码复用问题）

     private  static 返回值类型  方法名称（参数列表）{

     ​	方法体；

     }

   **注意：**私有方法只有接口自己能使用；

7. **类可以实现多接口，类只能单继承**

8. 如果实现类实现多接口，有重复的方法，只用重写一个即可；（因为是抽象方法，没有实现体）

9. 如果实现类没有实现接口中的所有方法，那么实现类就是一个抽象类

10. 如果实现类实现的接口中，存在默认方法 ，默认方法冲突时必须重写覆盖默认方法；

11. ##### 接口、类之间的关系

    - 接口与接口之间可以实现多继承；
    - 接口与类之间是多实现关系；一个类可以实现多接口；
    - 类与类之间单继承；

### String 

#### 1. 常用方法（比较判断）

- boolean equals(Object obj):比较字符串的内容是否相同,区分大小写
- boolean equalsIgnoreCase(String str):比较字符串的内容是否相同,忽略大小写
- boolean contains(String str):判断大字符串中是否包含小字符串
- boolean startsWith(String str):判断字符串是否以某个指定的字符串开头
- boolean endsWith(String str):判断字符串是否以某个指定的字符串结尾
- boolean isEmpty():判断字符串是否为空。

#### 2. 常用方法（获取）

- int length():获取字符串的长度。
- char charAt(int index):获取指定索引位置的字符
- int indexOf(int ch):返回指定字符在此字符串中第一次出现处的索引。
- int indexOf(String str):返回指定字符串在此字符串中第一次出现处的索引。
- int indexOf(int ch,int fromIndex):返回指定字符在此字符串中从指定位置后第一次出现处的索引。
- int indexOf(String str,int fromIndex):返回指定字符串在此字符串中从指定位置后第一次出现处的索引。
- lastIndexOf
- String substring(int start):从指定位置开始截取字符串,默认到末尾。
- String substring(int start,int end):从指定位置开始到指定位置结束截取字符串。

#### 3. 常用（转换）

- byte[] getBytes():把字符串转换为字节数组。
- char[] toCharArray():把字符串转换为字符数组。
- static String valueOf(char[] chs):把字符数组转成字符串。
- static String valueOf(int i):把int类型的数据转成字符串。

  注意：String类的valueOf方法可以把任意类型的数据转成字符串
- String toLowerCase():把字符串转成小写。
- String toUpperCase():把字符串转成大写。
- String concat(String str):把字符串拼接。

#### 4. String作为参数传递

​	因为String创建后不能改变，因此传递到方法中会产生新的字符串，方法执行完了后就弹栈

```java
    public static void main(String[] args) {
        String string  = new String("abc");
        System.out.println(string);
        change(string);
        System.out.println(string);
    }

    private static void change(String string) {
        string += "def";
    }

最后打印出 "abc"!!
```
### StringBuffer

#### 1. 线程安全的可变序列

- 底层由数组实现，每个字符串缓冲区都有一定的容量，当字符串缓冲区的长度大于了此容量，此容量就会自动增大。（创建新的更大的数组，将原来的数组内容copy出来，然后抛弃老的数组变成垃圾）
- 初始容量(capacity)为：16
- Stringbuffer 线程同步，安全，效率低（相对）          
- StringBulider 线程不同步，不安全，效率高，速度快

#### 2. 构造方法

  	  StringBuffer sb = new StringBuffer();   
		StringBuffer sb1 = new StringBuffer(10);   //创建指定长度的字符串缓冲区
		StringBuffer sb2 = new StringBuffer("abc");//创建指定的字符串缓冲区

​		sb.**length**();       //字符串长度

​		sb.**capacity**();   //字符串缓冲区长度	

#### 3. 添加方法

 - append() 方法

   添加任意类型的数据到字符串缓冲区中（从末尾添加）

 - insert() 方法

   insert（int offset,string s）

   在制定索引处添加任意数据

#### 4. 删除方法

 - deleteCharAt(int index) 

   ```java
   	StringBuffer sb = new StringBuffer("wjt123");
   	sb.deleteCharAt(3);      //删除指定索引处的字符
   	System.out.println(sb);  //wjt23
   ```

- delete(int start, int end)

  ```java
      sb.delete(0, 3);          //删除从start到end的字符串,不包含end
      System.out.println(sb);   //23
  ```

#### 5. 替换和反转

 - replace(int start, int end, String str)

   将start到end索引下的字符替换成str(不包含end)

 - reverse()

   将字符串缓冲区反转

#### 6. 截取

- substring(int start) 
- substring(int start, int end)

### 链表

#### 1. 什么是链表？

- **链**表是由一系列非连续的节点组成的存储结构，简单分下类的话，链表又分为单向链表和双向链表，而单向/双向链表又可以分为循环链表和非循环链表，下面简单就这四种链表进行图解说明。

  - **单向链表**就是通过每个结点的指针指向下一个结点从而链接起来的结构，最后一个节点的next指向null

    ![](F:\Typora\picture\单项链表.png)

  - **单向循环链表**和单向列表的不同是，最后一个节点的next不是指向null，而是指向head节点，形成一个“环”。

    ![](F:\Typora\picture\单项循环链表.png)

  - **双向链表**是包含两个指针的，pre指向前一个节点，next指向后一个节点，但是第一个节点head的pre指向null，最后一个节点的tail指向null。

    ![](F:\Typora\picture\双向链表.png)

  - **双向循环链表**和双向链表的不同在于，第一个节点的pre指向最后一个节点，最后一个节点的next指向第一个节点，也形成一个“环”。**而LinkedList就是基于双向循环链表设计的。**

    ![](F:\Typora\picture\双向循环链表.png)

### Collection

#### 集合和数组的区别：

区别一：

- 数组可以存任何类型的值，基本数据类型存的是值，引用数据类型存的是地址值
- 集合只能存引用数据类型，当传入的是基本数据类型时，会默认自动装箱，改变成引用数据类型

区别二:

- 数组的长度是固定，不能自动增长
- 集合的长度不是固定，可以动态改变，集合元素增多，长度增长；集合元素减少，长度减少；

#### 1. Collection集合继承框架

![1564551272001](G:\Typroa\picture\Collection.png)

#### 2. Collection 接口基本方法

- 添加 	**boolean add()**
- 移除     **boolean remove()**
- 包含     **boolean contains()**
- 添加所有       **addAll(Collection c);**
- 移除所有        **removeAll(Collection c);**（移除交集部分）
- 包含所有        **containsAll(collection c);**(取交集)
- 包含交集部分    **retainAll(Collection c) ;**(取交集覆盖调用的集合，如果调用集合改变了返回true反之false)

#### 3. 集合遍历 iterator 

​	Collection c = new ArrayList();

​	Iterator it = c.iterator();  //获取迭代器 

​	it.hasNext(); //判断是否用元素

​	it.next();       //取元素

**Iterator是接口**

集合都实现了此接口，并且根据不同的集合分别重写了hasNext()方法和next()方法

#### 4. List接口

- 特有方法

  - 在指定位置添加元素    **boolean** **add**(int index,Element e)-
  - 通过索引删除元素        **boolean**  **remove**(int index)       (将被删除的元素返回,删除时不会自动装箱)
  - 通过索引获取元素        **Object**  **get**(int Index) 
  - 通过索引修改元素        **Object**  **set**(int Index)            (返回新添加的元素)

- **List集合可以通过索引进行遍历（List特有）**

- Iterator 迭代器，在迭代时不能添加元素

   ```java
   	List list = new ArrayList();
   	list.add("a");
   	list.add("b");
   	list.add("c");
   
   	Iterator iterator = list.iterator();
   	while (iterator.hasNext()) {
   		String str = (String) iterator.next();
   		if (str.equals("a")) {
   			list.add("d");         //报错： java.util.ConcurrentModificationException  并发修改异常
   		}
   		System.out.println(str);
   	}
   ```

- **List集合特有的**迭代器 **listIterator()** (能够在迭代器遍历的时候给集合添加新的元素)

   ```java
   	ListIterator lit = list.listIterator();
   	while(lit.hasNext()){
   		String str = (String) lit.next();
   		if(str.equals("a")){
   			lit.add("d");
   		}
   	}
   	System.out.println(list);
   ```
   - listiterator()迭代器包含以下方法：
     - Boolean hasNext（）            //判断后面是否有元素
     - Boolean hasPrevious()           //判断前面是否有元素
     - Object      next();       返回下一个元素
     - Object   previous()；返回上一个元素

- Vector 1.0出现，1.2被纳入List接口

   - 特有功能 

      - 添加元素 void addElement()

      - 根据索引获取元素  E elementAt(int index)

      - 迭代 （通过枚举实现迭代）

         Vector v = new Vector();
         Enumeration elements = v.elements();

#### 5. ArrayList集合和数组的比较

- 数组长度是不可变的，一旦初始化长度固定
- 集合的长度是可变的，能够自动扩容，初始长度为10，扩容长度为50%

#### 6. List集合的数据结构原理（数组和链表）

 - 数组
   	- 查询、修改快(根据索引，可以快速查找到相应位置的值)
    - 增加、删除慢（每当增加或删除时，需要将其他的值移动后，在执行删除或添加）
 - 链表（双向链表）
   	- 查询、修改慢（指针需要一个元素一个元素的遍历，直到找到元素才会停止）
    - 增加、删除快  （指针找到相应元素后，直接在此位置上增加、删除元素，不需要移动其他的数据）

#### 7. List接口的三个子类的特点：

 - Vector
   	- Vector底层是数组实现，并且是线程安全的，效率相对与ArrayList来说会低一点
    - 执行查询、修改时时速度较快，删除、添加时速度较慢
 - ArrayList 
   	- ArrayList底层由数组实现
    - 线程不安全，效率高
       - 查询、修改快
       - 删除、增加慢
- LinkedList
  - LinkedList底层是由双向链表实现
  - 线程不安全，效率高
  - 查询、修改较慢
  - 删除、增加较快

#### ArrayList

​	**在包含和删除引用数据类型时，需要重写equals方法**

- contains() 方法底层是通过equals实现
- remove() 方法底层是通过equals实现

#### LinkedList

- **特有方法**
  - void addFirst() \ adddLast()
  - E  removeFirst() \ removeLast()
  - E get(int index)
  - E getFirst() / getLast()

#### 泛型

1. ##### 什么是泛型？

   泛型是指对不同引用数据类型进行一个广泛的概括

2. ##### 泛型的本质是什么？

   泛型的本质是参数化类型，换句话说就是将被操作的数据类型指定为一个参数

3. ##### 为什么会有泛型？

   因为在没有泛型前，都是通过Object来接受不同的引用数据类型，接收后再进行操作时需要执行数据类型强转操作，会出现ClassCastException数据类型转换异常，因此为了提高程序的安全性，便衍生出了泛型。在编译时作用，提高程序运行时的安全性；

   **实际上引入泛型的主要目标有以下几点：**

   类型安全

   - 泛型的主要目标是提高 Java 程序的类型安全
   - 编译时期就可以检查出因 Java 类型不正确导致的 ClassCastException 异常
   - 符合越早出错代价越小原则

   消除强制类型转换

   - 泛型的一个附带好处是，使用时直接得到目标类型，消除许多强制类型转换
   - 所得即所需，这使得代码更加可读，并且减少了出错机会

   潜在的性能收益

   - 由于泛型的实现方式，支持泛型（几乎）不需要 JVM 或类文件更改
   - 所有工作都在编译器中完成
   - 编译器生成的代码跟不使用泛型（和强制类型转换）时所写的代码几乎一致，只是更能确保类型安全而已

4. ##### 泛型的使用

   - **泛型类**

     ```java
     public class Tool<T> {
     	private T t;
     	public T getObj() {
     		return t;
     	}
     	public void setObj(T t) {
     		this.t = t;
     	}
     }
     ```

     ```java
     public static void main(String[] args) {
     	Tool<String> tool = new Tool<String>();
     	tool.setObj("abc");
     	System.out.println(tool.getObj());
     }
     ```

   - **泛型方法**

     ```java
     	public void show(T t) {               //声明和类的泛型一直
     		System.out.println(t);
     	}
     	
     	public<W> void print(W w) {          //可以和类的泛型不一致，声明自己的泛型
     		System.out.println(w);
     	}
     	
     	public class Demo01 {
             public static void main(String[] args) {
                 Tool<String> tool = new Tool<String>();
                 tool.show("abc");
             }
     	}
     
     ```

   - **泛型接口**

     ```Java
     public class Demo01 implements a<String> {          //如果不指定类型的话，默认是Object没什么意义，最好是指定类型
     
     	public static void main(String[] args) {
     		
     		Demo01 demo = new Demo01();
     		demo.show("wjt");         //通过调用方法传入字符串
     	}
     
     	@Override
     	public void show(String t) {
     		System.out.println(t);
     	}
     }
     
     interface a<T> {                  //声明一个带泛型的接口
     	void show(T t);
     }
     ```
   
5. ##### 泛型通配符

   - <?>

     **无界限通配符**，？可以匹配任意数据类型

     ```java
     ArrayList<?> list = new ArrayList<String>();
     ArrayList<?> list2 = new ArrayList<Integer>();
     ArrayList<?> list3 = new ArrayList<Boolean>();
     ```

   - <? extends E>

     **下界通配符**，？只能匹配E、或者E的子类

   - <? super E>

     **上界通配符**，？只能匹配E、或者E的父类

6. ##### 泛型擦除

   - 实际上泛型程序也是首先被转化成一般的、不带泛型的 Java 程序后再进行处理的，编译器自动完成了从 Generic Java 到普通 Java 的翻译，Java 虚拟机运行时对泛型基本一无所知。**泛型擦除是指泛型在编译期会对类型做出检测，但在运行期就失效了；可以通过反射越过泛型检测；**
- **当编译器对带有泛型的java代码进行编译时，它会去执行类型检查和类型推断，然后生成普通的不带泛型的字节码，这种普通的字节码可以被一般的 Java 虚拟机接收并执行，这在就叫做 类型擦除（type erasure）。**

#### List集合遍历三种方式

1. ##### 普通索引循环遍历

2. ##### Iterator迭代遍历

3. ##### foreach 增强for循环（jdk1.5）

#### 数据结构

- **栈**（了解数据结构**栈**）

  栈  先进后出的数据结构（类似于一个桶，先进去的都在最下面，只能等上面的元素都出去了才能出去）

- **队列**   （了解数据结构**队列**）

  队列  先进先出的数据结构（类似于一个管道，先进去的会先从管道的另一个口出去）
  
- **二叉树**

  二叉树：是每个节点都不超过两个分支的有序树；分为：平衡二叉树（排序树/查找树）、非平衡二叉树；

  红黑树：趋近于平衡二叉树，查询特别快；
  
  遍历方式：
  
  1. 前序遍历：根节点--->左节点--->右节点
  2. 中序遍历：左节点--->根节点--->右节点
  3. 后序遍历：左节点--->右节点--->根节点

#### JDK1.5新特性

1. ##### 静态导入

2. ##### 可变参数

   - 并不知道传多少个参数时使用
   - 本质是将参数转化为一个数组
   - 格式：权限修饰符  返回值类型  方法名（数据类型 ... 变量名）{}
   - **注意事项**：可变参数只能放在参数列表的最后

   ```java
   public class Demo2 {
   	public static void main(String[] args) {
   		int[] arr = {1,2,3,4,5};
   		print(arr);
   		System.out.println("----------------------");
   		print(11,22,33);                             //参数11，22，33都被arr接收，arr本质是一个数组
   		
   	}
   	public static void print(int...arr){
   		for (int i : arr) {
   			System.out.println(i);
   		}
   	}
   }
   ```
   

#### 数组转集合

- 数组转集合，数组必须是引用数据类型；如果是基本数据类型的数组则会被当作一个对象，转成集合

- 集合转数组，使用toArray(T a[])方法，传入泛型。长度小于等于集合的size时，数组长度等于集合；大于时，数组长度为泛型定义长度

  		```java
    ArrayList<String> list = new ArrayList<String>();
    list.add("a");
    list.add("b");
    list.add("c");
    String[] array = list.toArray(new String[3]);             //泛型定义长度为3，大于3时数组的长度就为相应的值
    for(Object object : array){
    	System.out.println(object);
    }
    ```

####   Set集合

##### 	1. Set集合特点

 - Set集合无索引、不能重复、无序（存、取的顺序不一样）
 - Set子类有：HashSet  LinkedHashSet  TreeSet

#####	2. HashSet

- 无序，无索引、元素唯一（不重复）
- 底层基于HashMap实现

#####	3. HashSet集合如何实现不能重复？

- HashSet不能重复是基于hashCode() 和equals()方法实现
- HashSet底层是通过hashMap实现，当set集合调用添加add()方法时，会调用存入对象的hashCode()算出hash值然后去判断集合中是否有相同的hash值；如何不相等，直接存入；如果相等再去调用对象的equals方法进行比较，如果相等则添加失败，反之添加成功；
- 自定义对象存入Set集合时**需要重写自定义对象的hashCode()和equals()方法**（因为Object的hashCode()和equals方法比较的是内存地址）

##### 4. LinkedHashSet

- HashSet的子类，基于链表实现的HashSet,使HashSet不在无序。存入和取出的顺序一样；
- 作为HashSet的子类，无索引、元素唯一（不重复）

##### 5. TreeSet

- TreeSet 无索引、元素唯一、元素实现排序
- 底层基于**二叉树**实现（**了解数据结构二叉树原理**）
- TreeSet如何实现不能重复，和排序的？
  - TreeSet的元素唯一和排序是基于Comparable接口的compareTo()方法实现，因此存放在TreeSet中的元素**需要重写compareTo方法**
  - 当使用TreeSet添加元素时，添加的元素调用compareTo()方法进行比较：
    - 返回值为**正数**，存放在根节点的右侧，再依次去比较，确定存放的位置
    - 返回值为**0**，不存
    - 返回值为**负数**，存放在根节点的左侧，再依次去比较，确定存放的位置

##### 6. 自然排序和比较器排序

 - **自然排序**
   	- 以TreeSet为例，添加到集合的元素需要实现Comparable接口，并且重写compareTo方法。如果没有就报ClassCastException类型转换异常
	- 当TreeSet存元素时，会自动将元素对象向上转型为Comparable类型，然后调用compareTo()，存入的元素调用方法和集合内的元素比较，根据compareTo()返回值执行存储
- **比较器排序**
    - TreeSet创建对象时，传入Comparator接口的子类（需要重写compare()方法）
    - TreeSet优先使用比较器对象，集合存值时调用compare()方法

##### 7. LIst集合扩容机制

 - **ArrayList**底层数组实现(当容量不足以添加元素时，开始自动扩容)，初始容量为**10**，扩容为：**oldCapacity + (oldCapacity >> 1)** 为初始长度 的1.5倍，扩容的长度为**1.5倍**

   ```java
   	private void grow(int minCapacity) {          //ArrayList扩容源码
           // overflow-conscious code
           int oldCapacity = elementData.length;
           int newCapacity = oldCapacity + (oldCapacity >> 1);
           if (newCapacity - minCapacity < 0)
               newCapacity = minCapacity;
           if (newCapacity - MAX_ARRAY_SIZE > 0)
               newCapacity = hugeCapacity(minCapacity);
           // minCapacity is usually close to size, so this is a win:
           elementData = Arrays.copyOf(elementData, newCapacity);
       }
   ```

	- **HashSet**  底层是由HashMap实现（当元素个数超过初始容量的0.75，开始自动扩容），初始容量为：**16**，扩容长度 为： oldCapacity <<<1 (即扩容1倍)

### Map集合

#### 1. 什么是Map集合？

 - **Map** 是一个接口，代表一个双列集合，底层由数组、链表和红黑树（链表）实现。元素都已键值对的形式存在，一个key映射一个value，key不能重复，只能存在一个null值。

#### 2. Map集合分类

- **HashMap**底层由hash算法实现；

  jdk1.7底层是**数组+链表**（单项链表）；

  jdk1.8底层是**数组+链表+红黑树**，链表长度超过8，就转为红黑树储存；

- **TreeMap**底层由二叉树实现

- **常用方法**
  - Object   put(key,value);             添加元素
  - Object   remove(key);                移除元素
  - Boolean  containsKey(key);       是否包含键
  - Boolean  containsValue(value);          是否包含值
  - Boolean   isEmpty()                   map集合是否为空

#### 3. Map遍历

- **通过获取所有的key键值遍历value**

  ```java
  	Map<String, Integer> map = new HashMap();
  	map.put("吴江涛", 23);
  	map.put("窦瑞琳", 19);
  	// 获取集合所有的key值
  	Set<String> keySet = map.keySet();             //通过迭代器进行遍历
  	Iterator<String> it = keySet.iterator();
  	while (it.hasNext()) {
  		String key = it.next();
  		Integer value = map.get(key);
  		System.out.println(key + "=" + value);
  	}
  	for (String key : map.keySet()) {            //通过增强for循环进行遍历
  		Integer value = map.get(key);
  		System.out.println(key + "=" + value);
  	}
  ```

- **将每一对键值对封装为Entry对象进行遍历**

  ```java
  	Map<String, Integer> map = new HashMap();
  	map.put("吴江涛", 23);
  	map.put("窦瑞琳", 19);
  	
  	Set<Entry<String, Integer>> entrySet = map.entrySet();
  	Iterator<Entry<String, Integer>> it = entrySet.iterator();    //将每一对键值对封装到entry对象中，获取所有Entry对象
  	while(it.hasNext()) {
  		Entry<String, Integer> entry = it.next();
  		String key = entry.getKey();
  		Integer value = entry.getValue();
  		System.out.println(key + "=" + value);
  		
  	}
  	System.out.println("++++++++++++++++++++++++++++++");
  	for(Map.Entry<String, Integer> entry : map.entrySet()) {
  		String key = entry.getKey();
  		Integer value = entry.getValue();
  		System.out.println(key + "=" + value);
  	}
  ```

#### 4. HashMap

- **HashMap**不能重复，无序（指存取顺序不一致）

- **HashMap**是通过**hashCode**和**equals()**方法是实现key键不能重复

- **HashMap**的底层实现原理，是由**数组+单向链表+红黑树**实现（jdk1.8新增），当链表长度大于8时，转为红黑树存储，提高了查询的效率

  ![](F:\Typora\picture\HashMap.jpg)

#### 5. LinkedHashMap

​	**LinkedHashMap**不能重复 ，通过链表实现了存取顺序一致

#### 6. TreeMap

- **TreeMap**元素不能重复，具有排序特性
- **TreeMap**是通过实现**Comparable** 或者**Comparator**接口实现元素不能重复和排序的，因此添加到集合的元素需要实现这两个接口中的一个，并重写相应的**compareTo**()方法或者**compare**()方法

#### 7. HashMap和HashTable的区别

- **HashMap**可以存null值

  **Hashtable**不可以存null值

- **HashMap**实现Map接口，**Hashtable**（1.0）实现Map接口和Dictionary抽象类

- 二者底层的hash算法不同

- **HashMap** 线程不安全，效率略高；**Hashtable**线程安全，效率低；如果要在多线程情况下使用Map集合，最好使用**ConcurrentHashMap**

- **HashMap**的初始容量为16，**hashtable**的初始容量为11；两者的填充因子默认都是0.75



### 异常

#### 1. 什么是异常？

​	异常是指导致Java程序不能正常运行的原因。

#### 2. Throwable 

 - **Throwable**是所有异常的父类，异常都继承于它

 - **Throwable** 分为两类：

    - **Error** (错误)

      Error是指java运行环境出现错误，jvm虚拟机异常等不能由程序本身解决的问题；

    - **Exception**（异常）

      Exception是指程序本身的语法、逻辑等有问题，能够通过修改程序来解决的异常；Exception主要分为两大类：

      - 运行时异常（RuntimeException）

        运行时异常是指程序在运行是才会抛出的错误，这些异常一般是由程序逻辑错误引起的，程序应该从逻辑角度尽可能避免这类异常的发生。

      - 编译期异常

        编译期异常是指程序在编译时所抛出的异常，必须在编译前解决否则编译不通过。如IOException、SQLException等
    
- **Throwable**常用的方法

    - e.getMessege()                         获取异常信息
    - e.toString()                                获取异常类名和异常信息
    - e.printStackTrace                     获取异常类名和异常信息、异常出现的位置（默认方式）

#### 3. 异常处理方式

 - 通过try……catch……finally来捕获和处理异常

    - try ……catch

    - try……catch……finally

    - try……finally

    - try  用来检测异常

      catch  用来捕获异常

      finally    释放资源（除了System.exit(0)以外都会执行）

 - 通过throws来抛出异常

	 - **throws** 定义在方法上，当需要把方法中的异常暴露时定义，可以抛出多个异常
	 - **throw** 定义在方法内部，用于抛出异常对象，只能抛出单个异常对象

#### 4. finally关键字

​	被finally控制的语句一定会执行，除非在finally前jvm虚拟机退出了（System.exit(0)）

​	finally语句块会在catch的return执行后，在return返回前执行。如果finally中有return会先返回；

 - 面试题：

    - final 、finally 、finalize()这三个有什么区别

      - final 可以修饰类、方法、变量

        修饰类，类不能被继承

        修饰方法，方法不能被重写

        修饰变量，只能赋值一次

      - finally 是try catch语法体的一部分，不能单独使用

      - finalize（）是一个方法，手动去呼叫jvm调用垃圾处理

    - 如果catch中有return语句，finally会执行吗？如果会，是return之前，还是之后？

      会执行，除了jvm退出否则都会执行。在return之后执行！

#### 5. 自定义异常

- 意义：当需要的异常，java中没有提供时，我们就需要自定义异常了。
- 自定义异常只需要新写一个类然后继承Exception，并重写构造方法。

#### 6. 异常的注意事项

- 子类重写父类方法时，子类只能抛出和父类相同的异常或父类抛出异常的子类
- 如果父类抛出多个异常，子类继承父类时只能抛出相同异常和其异常的子集，子类不能抛出父类没有的异常
- 如果父类的方法没有异常，子类重写时有异常产生，子类不能throws抛出异常，只能try解决异常

### File

#### 1. File概述

​	File代表的是一个文件，支持对文件的各种操作；

 - ##### 绝对路径

   绝对路径是指一个文件或文件夹的固定路径，换句话说就是文件或文件夹的存在电脑硬盘中的实际路径地址；（从盘符开始）

 - ##### 相对路径

   相对路径是指相对于某个位置，表示的是相对的位置路径

#### 2. File构造方法

 - File(String pathName)            				//根据路径名获取文件对象（相对路径和绝对路径都行）
 - File(String parent , String child)            //根据根据一个目录和一个子文件都到文件对象
 - File(File parent, String child)                 //根据一个目录对象和一个子文件得到文件对象

#### 3. File类创建功能

- boolean   createNewFile();                       //创建文件 ,存在不创建，不存在就创建
- boolean    mkdir()                                         //创建文件夹
- boolean    mkdirs()                                      //创建多层文件夹 

#### 4. File类重命名和删除

- boolean   renameTo(File  dest)                //如果路径名相同，就是改名 ；路径名不同，就是改名并剪切
- boolean    delete()                                     //删除的文件夹或文件，文件夹为空时才能删除

#### 5. File类判断

- boolean   exists();                                       //是否存在
- boolean   isDirectory();                             //是否为文件夹
- boolean   isFile()                                         //是否为文件
- Boolean   canRead();						         //是否可读

- Boolean   canWrite();						        //是否可写
- Boolean    isHidden()                                  //是否隐藏

#### 6. File类获取

- String   getAbsolutePath（）      //获取绝对路径
- String   getPath()                            //获取构造方法传入的路径值
- String   getName()                         //获取文件名
- Long     length()                              //获取文件中的字节数
- Long     lastModified()                   //获取文件最后修改的毫秒数
- String[]    list()                                 //获取制定目录下所有的文件名或文件夹名
- String[]    list(FilenameFilter filenameFilter)            //通过文件过滤器筛选，获得指定目录下的文件名
- File[]         listFiles()                         //获取指定目录下所有的文件或文件夹FIle

### IO流

#### 1. 什么是IO流？

​	**IO流**是指不同设备之间进行的**数据传输**，Java中操作数据都是通过流的方式

 - 根据操作的**数据类型**不同分为：**字节流** 和 **字符流**
   	 - 字节流：可以操作任何数据，因为计算机所有数据都是以字节的形式存储
    - 字符流：操作纯字符数据，方便
 - 根据数据的传输方向分为：**输入流 **和 **输出流**

#### 2. IO流——字节流基类

 - 字节流
   	- InputStream
    - OutputStream
 - 字符流
   	- Reader
    - Writer

#### 3. FileInputStream 和FileOutPutStream

​	文件输入输出流对象，分别操作文件输入输出

- FileInputStream 

  - int 	read()     					 //读取一个字节

  - int     read(byte[] a )           //将文件中的字节读取到内存中的字节数组中    返回值为读取到数组的长度

- FileOutputStream

  	- void    write(int b)						//写入一个字节写到文件中				
   - void    write(byte[]  b)   		       //将一个字节数组写入到文件中
  
- 文件拷贝方式

   - 一个一个字节读写

   - 将所有字节一次性读取到数组中，然后一次性写入文件

   - 遍历多次；一次将部分数据读取到数组中，然后写入文件；

      ```java
      	FileInputStream fis = new FileInputStream("bbb.txt");
      	FileOutputStream fos = new FileOutputStream("aaa.txt");
      	
      	byte[] a = new byte[1024];       //小数组的长度一般定义为1024的整数倍
      	int b;
      	while((b = fis.read(a)) != -1){
      		fos.write(a);
      	}
      	fis.close();
      	fos.close();
      ```

#### 4. BufferInputStream 和BufferOutputStream

​	带缓冲区的输入输出流

 - **BufferInputStream** 
    - 缓冲区输入流，具有一个缓冲区（数组），大小为1024*8；\
    - 操作缓冲区输入流进行读取时，先将读取的数据存到缓冲区中，当缓冲区数据满了后，再将缓冲区的数据一个一个输入出去。当缓冲区变为空后，在进行下一步操作；

 - **BufferOutputStream**
    - 缓冲区输处流，也具有一个缓冲区（数组），大小为1024*8；
    - 先将数据写入到缓冲去中，当缓冲区数据填满后，再一次性写入到文件中；然后在进行下一次写入缓冲区操作，直到写入完成；

    ```java
    	BufferedInputStream bis = new BufferedInputStream(new FileInputStream("aaa.txt"));
    	BufferedOutputStream bos = new BufferedOutputStream(new FileOutputStream("aa.txt"));
    	
    	int a ;
    	while((a = bis.read()) != -1){
    		System.out.println(a);
    		bos.write(a);
    	}
    	
    	bis.close();
    	bos.close();
    ```

- **flush()和close()**
  - 因为缓冲去输出流，需要等缓冲区填满后才会进行数据写入到文件操作。那么存在有时数据不足以填满数据缓冲区，因此需要刷新缓冲区，手动促进缓冲区数据写入文件
  - **flush()**  刷新缓冲区数据(手动将缓冲区数据写入文件)，调用后可以继续写入数据；
  - **close()**  关闭输入流的时候，刷新缓冲区，将缓冲区数据写入文件，调用后不能在写入数据；
  
- 字节输入流操作中文数据时，可能会出现乱码（最好用字符输入流）

- 字节输出流操作中文数据时，需要将中文数据转换为字节数组，然后进行数据写入到文件

#### 5. IO——字符流·

 - Reader

   输入字符流，直接读取字符

   - FileReader            //文件
   - BufferedReader   //缓冲区
   - LineNumberReader   

 - Writer

   输出字符流，直接写出字符

   - FileWriter    
   - BufferedWriter

#### 6. FileReader 和 FileWriter

​	文件字符输入输出流，里面具有一个长度为2*1024的缓冲区，必须flush或close才能读写； 

 - **FileReader** 

   	- int   read();                 //读取单个字符
   - int    read(char[] a);   //将字符读取到数组a中

 - **FileWriter**

   	- void    write(int a );              //写入单个字符
   - void    write（char[] a ）  //将字符数组写入文件
   - void     write(String s)          //写入字符串

- **什么情况下使用字符流？**

  - 当只是读取文件时使用字符输入流
  - 当只是写入文件时使用字符输出流
  - **原因**：因为字符输入输出流，拷贝时，需要先将字节转换为字符读取，再将字符转为字节写入文件；

- ##### 字符流不可以拷贝非文本文件，因为在字节转换为字符时，会出现转换成非法字符，再进行字符转字节时就会有问题，从而导致文件失效

#### 7.BufferedReader 和BufferedWriter

同4

- 特殊方法
  - BufferedReader   readLine()     //  读取一行字符，到文件结尾判断是否为**null**
  - BufferedWriter     newLine()     // 写入换行

#### 8. 制定编码表读写文件

- UTF-8码表中  一个中文字符占3个字节
- GBK码表中 一个中文字符占2个字节 
- 字符输入输出流默认使用GBK码表进行读写

#### 9. 使用指定码表进行读写的IO类

- InputStreamReader(inputStream , String charset)
- OutputStreamWriter(outputStream , String charset)

#### 10. 序列流

​	序列流将多个字节输入流整合成一个输入流；

- SequenceInputStream(InputStream in1 , InputStream  in2)        //将两个字节输入流合并成一个输入流
- SequenceInputStream( Enumeration<? extends InputStream> e)     //将多个输入流装入枚举中，在合成一个输入流

#### 11. 内存输出流

​	内存输出流是指，将读出的内容存入内存中（实际上是在内存中生成一个字节数组存），把内存当成一个缓冲区，写出时一次性获取 内存中所有内容

- **ByteArrayOutputStream**()

```Java
    ByteArrayOutputStream b = new ByteArrayOutputStream();
    int a; 
    while((a = fis.read()) != -1) {
    	b.write(a);             //将读出的内容写入到内存中
    }
    byte[] byteArray = b.toByteArray();                 //输出的第一种方式    将内存中的缓冲区转换为字节数组，然后转换为字符串打印出来
    System.out.println(new String(byteArray));
    System.out.println(b.toString());                   //输出的第二种方式，直接调用toString,输出字符串
```
#### 12. 对象输入输出流

 - **ObjectInputStream**(InputStream)   反序列化

   将字节数据转换为对象，读取

 - **ObjectOutputStream**(OutputStream)  序列化

   将对象转化为字节数据，输出

   ```java
   	//1.将对象写入到文件中
   	Person p1 = new Person("吴江涛",23);
   	Person p2 = new Person("窦瑞林", 19);
   	
   	ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("a.txt"));
   	oos.writeObject(p1);
   	oos.writeObject(p2);   //将对象写出
   	
   	oos.close();
   	
   	//2.将对象读取出来
   	ObjectInputStream ois = new ObjectInputStream(new FileInputStream("a.txt"));
   	Person person = (Person) ois.readObject();
   	Person person2 = (Person) ois.readObject();
   	System.out.println(person);
   	System.out.println(person2);
   ```

- **注意事项：** 

  - 对象必须事项**Serializable**接口才能被序列化；
  - 可以自定义序列化Id，id代表的是当前对象序列化的版本号，如果不自定义，Java会默认自动定义；

#### 13.数据输入输出流

​	可以按照基本数据类型的大小读写数据

 - **DataInputStream(InputStream)**
   	- readLong()    //读取长整型
    - readInt()       //读取整数
 - **DataOutputStream(OutputStream)**
   	- writeInt()
    - writeLong()

#### 14. 打印流

 - **PrintStream**(OutputStream)
   	- 打印流可以将对象的toString（）方法的结果输出
   - System.out就是打印流，默认指向控制台
	- **PrintWrite**(OutputStream)

#### 15. 标准输入输出流

 - **什么是标准输入输出流？**
   	- System.in是InputStream, 标准输入流, 默认可以从键盘输入读取字节数据
   - System.out是PrintStream, 标准输出流, 默认可以向Console中输出字符和字节数据
 - **修改标准输入输出流**
   - 修改输入流: System.setIn(InputStream)
   - 修改输出流: System.setOut(PrintStream)

#### 16. Properties的概述和作为Map集合的使用 

- ##### **Properties的概述**
  
  - Properties是**Hashtable**的子类，是一个双列结合，具备双列集合的功能
  - Properties 类表示了一个持久的属性集。
  - Properties 可保存在流中或从流中加载。
  - 属性列表中每个键及其对应值都是一个字符串。 
  
- ##### **Properties的特殊功能使用**

  - **Properties的特殊功能**
    
    - public Object setProperty(String key,String value)
    - public String  getProperty(String key)
    - public Enumeration<String> stringPropertyNames()
    
    	```
    	Properties p = new Properties();                 //双列集合对象
    	p.load(new FileInputStream("userInfo.properties"));
    	//获取所有的键
    	Enumeration<String> elements = (Enumeration<String>) p.propertyNames();
    	while(elements.hasMoreElements()){
    		String e = (String) elements.nextElement();
    		System.out.println(e);
    		System.out.println(p.getProperty(e));
    	}
    	```

- ##### Properties的load()和store()功能

  - **load**(InputStream)      //读取文件，转为Properties对象
  - **store**(OutputStream，String  remarks)   //写入文件，将Properties对象写入文件,并留下备注信息

  ```java
  	Properties p = new Properties();                      //双列集合对象
  	System.out.println(p);
  	p.load(new FileInputStream("userInfo.properties"));   //读取userInfo.properties文件
  	p.setProperty("name", "drl"); 
  	p.store(new FileOutputStream("userInfo.properties"), "吴江涛"); //写入文件将修改的内容
  ```

### 多线程

#### 1. 多线程概念

 - ##### 什么是线程？什么是进程？

    - **进程**就类似于一个程序，如qq、微信等应用运行时就属于一个进程，进程包含多个线程；每个进程都有自己的内存空间，进程不能访问其他进程的私有空间；

    - **线程**是操作系统能够进行运算调度的最小单位，它被包含在进程之中，是进程中的实际运作单位；线程就像是是一个程序执行的一条路径，每个路径的执行就构成了一个进程；

       - 线程是可以使用所属进程内的共享资源

      - 线程是进程的一个实体，是进程的一条执行路径。

      线程是进程的一个特定执行路径。当一个线程修改了进程的资源，它的兄弟线程可以立即看到这种变化。
   
- ##### 多线程

    多线程就是指多个线程的同时运行

    - **多线程实例** ： 如迅雷下载东西时，可以开启多个下载任务，而每一个任务就是一个线程

#### 2. 多线程（执行方式分类）

 - **并行** 

   并行是指，多个线程同时执行；就是线程A运行的同时，线程B也在运行；并行需要多核cpu才能实现；

 - **并发**

   并发是指，多个线程同时想cpu申请资源来执行，但是处理器一次只能执行一个线程，然后就把多个线程安排依次执行，时间间隔很短，因此看起来像是同时进行；
   
   - **分时调度**：每个线程轮流获取 CPU 使用权，各个线程平均 CPU 
   - **时间片抢占式调度**：Java 虚拟机使用该调度模型，它会根据线程优先级，先调度优先级高的线程，若线程优先级相同则随机选取线程执行

#### 3. Java程序运行原理和jvm的启动是多线程吗？

- java程序通过jvm虚拟机运行时，相当于开启了一个进程，进程中会自动开启一个主线程，然后通过主线程去执行程序；
- jvm启动时会开启主线程和垃圾回收线程，因此是多线程；

#### 4. 多线程的实现方式

 - 第一种：**继承Thread类**

    - 通过继承Thread类，并重写run()方法，然后通过调用start()方法，开启线程；

    - **注意：**run()方法中是线程的执行体，如果要开启线程必须调用Start()方法；

      ​			如果直接调用run()方法，代表普通的方法调用，并不是线程调用

      ```Java
      public class Demo4_Thread {
      	public static void main(String[] args) {
      		MyThread mt = new MyThread();    //3.创建线程对象
      		mt.start();            //4.开启线程
      	}
      }
      class MyThread extends Thread{  //1.继承Thread类
      	@Override
      	public void run() {    //2.重写run方法
      		super.run();
      	}
      	
      }
      ```

 - 第二种：**实现Runnable接口**

    - 通过实现Runnable接口，然后重写run()方法，然后将实现类传入Thread ，生成线程对象，然后调用start()方法，开启线程；

      ```Java
      public class Demo5_Thread {
      	public static void main(String[] args) {
      		Thread t = new Thread(new MyThread2());    //3.创建线程对象，将实现了Runnable接口的子类对象作为参数创建线程对象
      		t.start();           //4.开启线程
      	}
      }
      class MyThread2 implements Runnable{    //1.实现Runnable接口
      	@Override
      	public void run() {            //2.重写run方法
      		System.out.println("重写了run方法");
      	}
      	 
       }
      ```

- 第三种：**实现Callable接口**

  - 通过实现Callable接口，实现call()方法，（call()方法为线程的执行体，有返回值），然后使用FutureTask进行包装，再将其传入Thread类，生成线程对象，调用start()方法开启线程；

    		```java
      public class Demo2_Callable {
          public static void main(String[] args) throws Exception {
              // 创建Callable子类对象实例
              MyCallable mc = new MyCallable(100);
      
              // 用FutureTask进行包装
              FutureTask<Integer> ft = new FutureTask<Integer>(mc);
      
              // 传入Thread，创建线程对象
              Thread t = new Thread(ft);
              // 开启线程
              t.start();
      
              // 获取Call()方法的返回值
              Integer callNum = ft.get();
              System.out.println(callNum);
          }
      }
      ```
      
      

- **三种方式的区别**
  - 第一种是继承了Thread类，实例化子类对象，当直接调用父类的start()方法后，就直接调用重写的run()方法；
  - 第二种是实现Runnable接口，将子类对象传入Thread中，当调用start()方法时，去调用Runnable子类对象的run()方法；
  - 第三种是实现了Callable接口，更加灵活，call()方法有返回值

#### 5. 多线程的特有方法

 - **获取线程名字和设置**

   	- getName()
    - setName()

 - **获取当前线程对象**

   	- Thread.currentThread();

- **休眠线程**

  - Thread.**sleep**(int 毫秒值)；     //让当前线程暂停多少毫秒后，再继续执行

- **守护线程**：将一个线程设置为另一个线程的守护线程，当被守护的线程结束后，守护线程自动结束；

  - t.**setDaemon**(True);                //开启守护线程设置

- **插队线程**：当一个线程B执行时，遇到另一个线程A的join()方法，线程B停止，等待线程A执行完成后，在执行B线程

  - **join**()  、join(int 毫秒值)

    		```java
      Thread t1 = new Thread(){
        		@Override 
        		public void run(){
        			super.run();
        		}
        	};
        	
        	Thread t2 = new Thread() {
        		@Override
        		public void run() {
        			try {
        				t1.join(); //再次插入中断线程t2执行，执行线程t1，等待t1执行完了后再执行t2
        			} catch (InterruptedException e) {
        				e.printStackTrace();
        			}   
        			super.run();
        		}
        	};
      ```
      
      
  
- **礼让线程** 
  
  - **yield**()                //让出cpu资源让其他线程优先执行（**只能让同优先级或者优先级更高的线程执行**）
- **设置线程优先级**
  
  - t2.**setPriority**(int newPriority);    //设置线程优先级，从1-10层优先级，数字越大优先级越高；

#### 6. 同步代码块

 - ##### 什么时候需要同步？
   
   - 当多段代码需要同时执行时，程序只能每次执行一段，因此就需要同步，让一段执行完了后再执行另一段**(当多个线程需要执行同一段代码时，就需要线程同步)**
- ##### 同步代码块用法：
  
   - 作用：多个线程同时访问一个共享资源时，线程A在执行共享资源中的一部分代码时，另一个线程B也进入执行共享资源的一部分代码，此时就导致线程A未执行完，就开始了B的执行，从而导致程序执行错误；为了避免这一问题，就应该使用同步代码块；
   
   - 格式：
   
     synchronized(锁对象){                  //锁对象为任意对象即可

​					//需要同步的代码

​				}  

```java
public class Demo4_synchronized {

	public static void main(String[] args) {
		Print p = new Print();
		
		new Thread() {               //通过匿名内部类创建线程对象
			@Override
			public void run() {      //重写线程的run()方法
				p.print1();    
			}
		}.start();                   //开启线程
		
		new Thread() {
			@Override
			public void run() {
				p.print2();    
			}
		}.start();
	}

}

class Print{
	public void print1() {
		synchronized (this) {   //this代表同步锁对象，这里是指当前对象
			System.out.println("我爱小猪窦瑞林");
		}
	}
	public void print2() {
		synchronized (this) {
			System.out.println("小猪窦瑞林是我的小宝贝");
		}
	}
}
```
   - **注意：** 多个线程必须具有相同的锁对象
        - 非静态的方法中的代码块的锁是**this**
        - 静态方法中的代码块的锁是 **当前对象的字节码文件**

#### 7. 同步方法

- 由**synchronized**关键字修饰的方法为同步方法

- 多个线程访问共享资源时，必须一个同步方法执行完后才能执行另个方法
	
    ```java
    	class Print {
        	         private static Integer flag = 1;
        	   		public synchronized static void print1() throws InterruptedException {
        	   			if (flag != 1) {
        	   				Print.class.wait(); // 让线程等待
        	   			}
        	   			System.out.println("哈哈哈");
        	   			flag = 2;
        	   			Print.class.notify(); // 唤醒线程
        	   		}
        	   	}	
        	  		private static Integer flag = 1;
        	  	
        	  		public synchronized static void print1() throws InterruptedException {
        	  			if (flag != 1s) {
        	  				Print.class.wait(); // 让线程等待
        	  			
        	  			System.out.println("哈哈哈");
        	  			flag = 2;
        	  			Print.class.notify(); // 唤醒线程
        	  		}
        	  	}
       ```
     
     

#### 8. 死锁

​	  所谓**死锁**是指两个或两个以上的线程在执行过程中，因争夺资源而造成的一种互相等待的现象，若无外力作用，它们都将无法推进下去。

- **产生死锁必须同时满足以下四个条件，只要其中任一条件不成立，死锁就不会发生**。

  - **互斥条件**：线程要求对所分配的资源（如打印机）进行排他性控制，即在一段时间内某资源仅为一个线程所占有。此时若有其他线程请求该资源，则请求线程只能等待。

  - **不剥夺条件**：线程所获得的资源在未使用完毕之前，不能被其他线程强行夺走，即只能由获得该资源的线程自己来释放（只能是主动释放)。

  - **请求和保持条件**：线程已经保持了至少一个资源，但又提出了新的资源请求，而该资源已被其他线程占有，此时请求进程被阻塞，但对自己已获得的资源保持不放。

  - **循环等待条件**：存在一种线程资源的循环等待链，链中每一个线程已获得的资源同时被链中下一个线程所请求。即存在一个处于等待状态的线程集合{Pl,  P2, ..., pn}，其中Pi等待的资源被P(i+1)占有（i=0, 1, ..., n-1)，Pn等待的资源被P0占有，如图1所示。

#### 9. Runtime 运行时类

​	Runtime是一个单例设计模式类，运行时类可以获取当前Java程序相关的运行时对象。

```java
	Runtime r = Runtime.getRuntime();           //获取和当前java程序有关的运行时对象
	//执行字符串命令（是指cmd窗口下执行的命令语句）
	//r.exec("shutdown -s -t 300"); 			//设置五分钟后关闭电脑           
	r.exec("shutdown -a");                      //取消五分钟后关闭电脑
```
#### 10. Timer定时器类

​	**Timer**可以设置定时器任务，类似于闹钟

```java
	public class Demo3_Timer {
	public static void main(String[] args) throws InterruptedException {
		//创建定时器对象
		Timer t = new Timer();           
		//t.schedule(task, time);        //timer是指TimerTask定时器任务对象，设置了定时器，就需要设置定时器任务对象去执行相应任务，time值定时器任务执行的时间
		//t.schedule(new MyTimerTask(), new Date(119,7,8,22,46,30));     //设置在此时间时执行定时器任务
		t.schedule(new MyTimerTask(),  new Date(119,7,8,22,48,05), 3);   //第三个参数为每隔多少毫秒打印一次
		while(true) {
			Thread.sleep(1000);
			System.out.println(new Date());
		}
	} 
}
class MyTimerTask extends TimerTask{          //继承TimerTask定时器任务类
	@Override
	public void run() {
		System.out.println("睡觉了！");
	}
	
}
```
#### 11. 多线程之间的通信

 - **什么时候需要线程之间通信？**

   答：多个线程并发执行时，是按照cpu随机调度执行的；如果我们想按照一定顺序执行的话，就需要使用线程通信；

 - 通信方式

    - **线程等待**：**wait();**

    - **唤醒等待线程**：**notify()**    //随机唤醒一个等待线程

      ​						   **notifyAll(**)   //唤醒所有等待线程

   - **这两个方法必须在同步代码中执行，并且使用同步锁对象来调用**

     ```Java
     public class Demo4_synchronized {
     
     	public static void main(String[] args) {
     		Print p = new Print();
     		
     		new Thread() {
     			@Override
     			public void run() {
     				try {
     					p.print2();
     				} catch (InterruptedException e) {
     					e.printStackTrace();
     				}    
     			}
     		}.start();
     		
     		new Thread() {               //通过匿名内部类创建线程对象
     			@Override
     			public void run() {      //重写线程的run()方法
     				try {
     					p.print1();
     				} catch (InterruptedException e) {
     					e.printStackTrace();
     				}    
     			}
     		}.start();                   //开启线程
     		
     	}
     
     }
     
     class Print{
     	private Integer flag = 1;
     	public void print1() throws InterruptedException {
     		synchronized (this) {   //this代表同步锁对象，这里是指当前对象
     			if(flag != 1 ) {
     				this.wait();      //让线程等待
     			}
     			System.out.println("猪一样");
     			flag = 2;
     			this.notify();    //唤醒线程
     		}
     	}
     	public void print2() throws InterruptedException {
     		synchronized (this) {
     			if(flag != 2) {
     				this.wait();
     			}
     			System.out.println("哦？你才是");
     			flag = 1;
     			this.notify();
     		}
     	}
     }
     ```

   例如：当一个线程不满足某个条件时，使用wait()方法释放cpu资源让其等待；执行其他其他进程，执行完后，使用notify()方法唤醒等待的进程去执行；

- ##### 线程之间的通信注意事项

  - 在同步代码中，用什么同步对象锁，就得用什么对象调用wait()和notify()方法
  - 为什么wait()和notify()定义到Object类中？
    - 因为同步锁是任意对象，Object是所有类的父类
  - **sleep()和wait()的区别**
    - **sleep()**方法被调用后，线程进入休眠状态，休眠时间一到，线程就可以回到就绪状态等待cpu调度，从而继续执行线程；sleep()执行时，不释放锁；
    - **wait()**方法，被调用后线程进入等待状态（阻塞状态），必须要被唤醒（notify()|notifyAll())后，才能回到就绪状态，等待cpu调度；wait()执行时，释放锁；
    - **两个回复就绪状态后，再次获取资源时，都是从被阻塞的地方，再次开始执行**

#### 12. JDK1.5新特性——互斥锁

​	**互斥锁使用的时ReentrantLock类的lock()和unlock()实现同步**，同时实现了唤醒指定对象

- 通过互斥锁实现同步，用互斥锁标记特定的等待与唤醒，实现指定线程唤醒和等待

```Java
	class Print2{
	private int flag = 1;
	private ReentrantLock rt = new  ReentrantLock();       //获取互斥锁对象
	private Condition c1 = rt.newCondition();              //获取一个互斥锁的执行的实例
	private Condition c2 = rt.newCondition();
	
	public void print1() throws InterruptedException {
			rt.lock();                  //用互斥锁实现同步——开启互斥锁
			if(flag != 1) {
				c1.await();             //让线程等待
			}
			System.out.println("我爱小猪窦瑞林");
			flag = 2;
			c2.signal();                //唤醒c2等待线程
			rt.unlock();                //用互斥锁实现同步——关闭互斥锁
	}
	
	public void print2() throws InterruptedException {
		rt.lock();
		if(flag != 2) {
			c2.await();             //让线程c2等待
		}
		System.out.println("哈哈哈");
		flag = 2;
		c1.signal();                //唤醒c1等待线程
		rt.unlock();
	}	
}
```
#### 13. 线程组

​	程序默认的线程组都是main主线程组

- ThreadGroup(String  新线程组的名字 )  来定义线程组

  		```java
    ThreadGroup tg = new ThreadGroup("新线程组");    //创建线程组
    
    System.out.println(tg.getName());    
    
    Thread t = new Thread(tg, new Thread() {      //将线程对象放入到线程组中
        public void run() {
            super.run();
        }
     });
    ```

#### 14. 线程的生命周期

- **创建线程**

  通过创建Thread类实例对象，来创建线程

- **就绪状态**

  通过调用start(),让线程进入就绪状态等待cpu分配调度

- **执行状态**

  当cpu分配给线程资源时，线程进入执行状态（cpu调度是随机的）

  - **阻塞状态**

    当线程被强制休眠sleep()、等待wait()或等待I/O设备传输时，会进入阻塞状态；只有当引起阻塞的条件结束了才会重新进入就绪状态，等待cpu调度；

- **线程终止**

  当线程执行完毕或强制终止时，线程结束

  **生命周期原理图：**

  ![](..\pic\线程生命周期.jpg)

  

#### 15.线程池

当多个线程的生命周期短暂时，为了避免每次使用去新建线程对象，因此就出现了线程池；将线程放入线程池中，使用时去线程池中拿，用完后放回线程池；线程池中的每一个线程代码执行结束后，线程不会死亡，而是回到线程池中处于空闲状态，等待下一次调用；大大节省了时间，提高了性能；

- ##### 通过Executors工厂类来创建线程池

  - **public static  ExecutorService  newFixedThreadPool(int nThreads);**            创建存放指定个数的线程池，返回ExecutorService线程池对象

    例子： ExecutorService pool = **Executors**.newFixedThreadPool(2); 

  - **public static  ExecutorService   newSingleThreadExecutor();**                  创建存放单个线程的线程次

  - **submit(Runnable\Callable)**    将线程放入线程池并执行线程     参数为：Runnable和Callable的子类实例对象

  - **shutdown()** 关闭线程池

    			```java
       public class Demo1_Executors {
          	
          		public static void main(String[] args) throws InterruptedException, ExecutionException {
          			ExecutorService pool = Executors.newFixedThreadPool(2);
          			//创建单个线程的线程池
          			// Executors.newSingleThreadExecutor();
          			
          			// 创建多线程
          			Thread thread = new Thread() { 
          				public void run() {
          					super.run();
          				}
          			};
          			MyCallable mc = new MyCallable(100);
          			
          			// 将线程放入线程池,并运行线程
          			pool.submit(thread); 
          			// 参数为：Runnable和Callable的子类实例对象 call()方法会返回值，用Future接收
          			Future<Integer> future = pool.submit(mc);
          			
          			System.out.println(future.get());
          			pool.shutdown(); // 关掉线程池
          		}
          	}
          	
          class MyCallable implements Callable<Integer> {
          	
          		private int num;
          	
          		public MyCallable(int num) {
          			this.num = num;
          		}
          	
          		@Override
          		public Integer call() throws Exception {
          			int sum = 0;
          			for (int i = 0; i < num; i++) {
          				sum = sum + i;
          			}
          			return sum;
          		}
          	
          	}
       ```

#### 16. **volatile** **关键字的作用（变量可见性、禁止重排序）**

Java 语言提供了一种稍弱的同步机制，即 **volatile** 变量，用来确保将变量的更新操作通知到其他线程。volatile 变量具备两种特性，volatile 变量不会被缓存在寄存器或者对其他处理器不可见的地方，因此在读取 volatile 类型的变量时总会返回最新写入的值。

1. ##### **变量可见性**

   其一是保证该变量对所有线程可见，这里的可见性指的是当一个线程修改了变量的值，那么新的值对于其他线程是可以立即获取的。

2. **禁止重排序**

   volatile 禁止了指令重排。 

3. **比** **sychronized** **更轻量级的同步锁**

   在访问 volatile 变量时不会执行加锁操作，因此也就不会使执行线程阻塞，因此 volatile 变量是一种比 sychronized 关键字更轻量级的同步机制。volatile 适合这种场景：一个变量被多个线程共享，线程直接给这个变量赋值。

#### 17. **ThreadLocal** **作用（****线程本地存储）**

ThreadLocal 的作用是提供线程内的局部变量，这种变量在线程的生命周期内起作用，减少同一个线程内多个函数或者组件之间一些公共变量的传递的复杂度。局部变量的本地线程副本，实现多线程之间不共享数据，避免数据异常。

#### 18. **synchronized** **和** **ReentrantLock** **的区别**

##### **两者的共同点：**

	1. 都是用来协调多线程对共享对象、变量的访问
 	2. 都是可重入锁，同一线程可以多次获得同一个锁

3. 都保证了可见性和互斥性

##### 两者的不同点：**

1. ReentrantLock 显示的获得、释放锁，synchronized 隐式获得释放锁、
2. ReentrantLock 是 API 级别的，synchronized 是 JVM 级别的
3. ReentrantLock 通过 Condition 可以绑定多个条件，实现多路通知
4. 底层实现不一样， **synchronized 是同步阻塞**，使用的是**悲观并发策略**；**lock 是同步非阻塞**，采用的是**乐观并发策略**
5. synchronized 在发生异常时，会自动释放线程占有的锁，因此不会导致死锁现象发生；而 ReentrantLock 在发生异常时，如果没有主动通过 unLock()去释放锁，则很可能造成死锁现象，因此使用 Lock 时需要在 finally 块中释放锁。

### 网络编程

#### 1. 计算机网络

计算机网络是指将地理位置不同的具有独立功能的多台计算机及其外部设备，通过通信线路连接起来，在网络操作系统、网络管理软件及网络通信协议的管理和协调下，实现资源共享和信息传递的计算机系统。

#### 2. 网络编程

​	网络编程就是指不同是计算机运行的程序之间通过网络互联实现数据交互

#### 3. 网络编程三要素——IP

- IP是指每台计算机在网络中的唯一标识（相当于在网络中的位置）

- 每台计算机在网络中都有一个唯一的地址，我们就是通过这个地址来实现数据交互

- Ip地址分为：

  - IPV4 : IPv4使用32位（4字节）地址，因此[地址空间](https://baike.baidu.com/item/地址空间)中只有4,294,967,296（2）个地址。

    例如：192.168.0.1

  - IPV6：IPv6的地址长度为128位（8字节），是IPv4地址长度的4倍。于是IPv4点分十进制格式不再适用，采用十六进制表示；

    例如：fe80::b494:baa3:9a58:433e%8

- 本地回路地址 ： 127.0.0.1

- 广播地址：255.255.255.255

#### 4. 网络编程三要素——port(端口)

- 端口是指每台计算机上运行程序的唯一标识
- 每个网络程序都要绑定一个唯一标识的端口，传输的数据才能知道从一台计算机传到那台计算机的那个应用程序上
- 端口号范围：0-62535（日常网络编程的端口号，应在1024以上）
- 常用 端口：
  - mysql:3306
  - oracle:1521
  - tomcat:80

#### 5. 网络编程三要素——协议

- 为计算机网络进行数据交互而建立的规则、标准和约定的集合。

- 常用的协议：

  - UDP

    UPD是一种面向无连接的网络协议，不保证数据一定能传到，因此不能保证数据完整；不需要先建立链接，传输速度较快，

    基于数据报传输。

  - TCP

    TCP是一种面向连接的、可靠的、基于字节流的传输层通信协议；TCP是一种面向连接的网络协议，传输数据前，需要确保客户端与服务端完成连接；数据安全，速度相对较慢。通常使用**三次握手**，来建立连接；

    - **三次握手**

      **第一次握手**：当客户端想要和服务端进行数据交换时，会先发出连接请求**SYN包（SYN=j）**至服务端，并进入**SYN_SENT**状态；

      **第二次握手**：服务端接收的到客户端的请求，并发送自己的**SYN包(SYN=k)**、确认客户的请求回应**ACK包(ACK=j+1)** 给客户端，服务端进入**SYN_RECV**状态；

      **第三次握手**：然后客户端收到服务端的**SYN+ACK包**，并告知服务端可以开始连接，发送确认包**ACK（ACK=k+1）**给服务器；连接建立完成，完成三次握手；

      ![](F:\Typora\picture\三次握手原理.gif)

    - ##### 为什么不是两次握手？

      因为为了防止客户端发送的已失效的连接请求报文，突然又发送到服务器端，从而导致浪费资源；
  
      **解析：**客户端A发送的请求连接报文，可能因为网络堵塞或其他的一些原因导致报文丢失；此时A没有收到服务端B的回应，A会再次发送 请求，这次请求正常到达B,并完成连接，完成了数据交互；此时之前丢失的报文，可能因网络问题解决，从而发送到了B，服务器B以为A再次发送请求并给与回应，进入等待接收模式。如果没有第三次握手，B会一直等待A传输数据；但此时A根本没有发送请求所有并不会发送数据，B一直等待从而就会浪费资源；三次握手正好能解决这个问题；

#### 6. Socket套接字

**套接字（socket）**是一个抽象层，应用程序可以通过它发送或接收数据，可对其进行像对文件一样的打开、读写和关闭等操作。套接字允许应用程序将I/O插入到网络中，并与网络中的其他应用程序进行通信。网络套接字是IP地址与端口的组合。**网络上具有唯一标识的IP地址和端口号才能组成具有唯一标识的Socket套接字**；

- 套接字Socket是指通信两端的端点，两端的通信是从Socket连接的；就相当于数据传输的**入口**；

- 网络通信的两端都有Socket

- 网络通信其实就是Socket间进行通信

  ![](F:\Typora\picture\Socket通信原理.PNG)

#### 7. UPD传输

- **发送端：Send**
  - 创建DatagramSocket，绑定端口号（随机，与接收端对应）
  - 创建DatagramPacket，绑定数据、数据长度、Ip地址、端口
  - 使用DatagramSocket发送DatagramPacket；**socket.sent(packet)**
  - 关闭DatagramSocket


```java
//创建一个Socket对象（网络传输对象）
DatagramSocket socket = new DatagramSocket();

//发送的数据
String str = "女孩你知道嘛？爱上你我不愿回家！";

//创建一个packet对象（封装数据包）
DatagramPacket packet = new DatagramPacket(str.getBytes(), str.getBytes().length, InetAddress.getByName("127.0.0.1"), 5555);

//发送数据包
socket.send(packet);         
//关闭
socket.close();va
```
- **接收端：Receive**

  - 创建DatagramSocket，绑定端口号（随机，与发送端对应）
  - 创建DatagramPacket，绑定接收数据的数组和数组长度；
  - 使用DatagramSocket接收DatagramPacket；**socket.receive(packet);**
  - 关闭Socket

  		```java
  	DatagramSocket socket = new DatagramSocket(5555);
  	DatagramPacket packet = new DatagramPacket(new byte[1024], 1024);
  	//接收数据报
  	socket.receive(packet);      
  	
  	//获取数据包数据
  	byte[] data = packet.getData();
  	
  	//获取数据长度
  	int len = packet.getLength();   
  	
  	System.out.println(new String(data,0,len));
  	socket.close();
  	```

#### 8. TCP传输

- **客户端（Client）**

  - 创建Socket连接服务器端（绑定Ip地址和端口），通过IP地址找到对应的服务器
  - 调用Socket的getInputStream()和getOutputStream()方法，来获取与服务器连接的IO流
  - 输入流可以获取服务器传给客户端的内容
  - 输出流可以写出传给服务器的内容	

  	```java
  	// 创建Socket对象，并绑定服务器的IP地址和端口
  	Socket socket = new Socket("127.0.0.1", 6666);
  	
  	// 获取输入输出流
  	InputStream is = socket.getInputStream();
  	OutputStream os = socket.getOutputStream();
  	
  	// 传向服务器端的内容
  	os.write("我靠为什么你接受不到！".getBytes());
  	
  	// 接收服务器端内容
  	byte[] arr = new byte[8192];
  	int len;
  	while ((len = is.read(arr)) != -1) {
  		System.out.println(new String(arr, 0, len));
  	}
  	socket.close();
  	```

- 服务端（Server）

  - 创建ServerSocket(需要指定端口)
  
  - 调用ServerSocket的accept()方法接收客户端请求，得到一个Socket;
  
  - 调用Socket的getInputStream()和getOutputStream()方法，来获取与客户端连接的IO流

  - 输入流读取客户端传过来的内容
  
  - 输出流写出服务端发送给客户端的内容；
  
  	```java
  	// 创建ServerSocket对象，绑定端口号
  	ServerSocket server = new ServerSocket(6666);
  			
  	// 接收客户端的Soket
  	Socket socket = server.accept();
  	
  	// 获取与客户端连接的输入输出流
  	InputStream is = socket.getInputStream();
  	OutputStream os = socket.getOutputStream();
  	
  	// 向客户端写出数据
  	os.write("我怎么知道，Baby".getBytes());
  	
  	// 获取客户端请求内容
  	byte[] arr = new byte[8192];
  	int len = is.read(arr);
  	System.out.println(new String(arr, 0, len));
  	
  	socket.close();
  	```

### 反射

#### 1. 类的加载

​	当程序需要使用某个类时，如果该类还没有被加载进内存，则系统会通过加载、连接和初始化三步对这个类进行初始化。

- 加载：加载是指将该类的字节码对象读入到内存中，并创建一个Class对象。任何类使用时都会创建一个Class对象。
- 连接：
  - 验证：是否有正确的内部结构，并和其他类协调一致
  - 准备：负责为类中的静态成员分配内存，并设置默认值
  - 解析：将类的二进制数据中的符号引用替换为直接引用
- 初始化：为类非静态成员进行初始化操作

#### 2. 类的加载时机

- 创建类的实例
- 访问静态变量，或为静态变量赋值
- 调用静态方法时
- 通过反射来强制创建某个类或者接口的Class对象时
- 初始化某个子类时（初始化子类时，优先调用父类的构造，初始化父类对象）
- 通过java.exe来运行某个主类

#### 3. 反射

- ##### 类加载器

  负责将.class文件加载到内存，并为之生成对应的Class对象。

- ##### 类加载器分类

  - **根类加载器** ：BootStrap ClasssLoader

    也被称为**引导类加载器**，负责加载Java的核心类（就是Java官方提供的类）；如System、String等(rt.jar)

  - **扩展类加载器：** Extension ClassLoader

    负责Jre的扩展目录中的jar加载(ext)

  - **系统类加载器：** System ClassLoader

    负责Jvm启动时加载来自java命令的class文件，如自定义java类等；

- ##### 反射概述

  - Java反射机制是指在运行状态时，对于任意一个类，都能够获取到它的属性和方法；
  - 动态获取类的信息以及动态调用对象的方法的功能就称为Java反射机制；
  - 反射是基于Class类来实现的，因此要想实现反射应该先获取每一个类的字节码对象；

- **获取字节码对象的三种方式**

  - 通过Object类的getClass()方法；一般用来判断是否为同一个字节码对象，如重写equals()方法中使用

       ```java
       Student stu = new Student();
       
       //获取Student类的字节码对象
       Class clazz = stu.getClass();  
       
       //通过反射获取Student类的对象(无参构造)
       Student stu = (Student) clazz.newInstance(); 
       
       stu.setName("吴江涛");
       System.out.println(stu); 
       ```

  - 直接通过类名获取；Person.class，一般用作静态同步锁

       ```java
       //获取Student类的字节码对象
       Class clazz = Student.class;
       
       //通过反射获取Student类的对象
       Student stu = (Student) clazz.newInstance();
       
       stu.setName("吴江涛");
       System.out.println(stu);  
       ```

  - 通过Class.forName(全包名)来获取  ；一般通过读取配置文件获取全类名，从而获取字节码对象

    	```java
     public class Demo1 {
        
        public static void main(String[] args) throws Exception {
        	//获取Student类的字节码对象
            Class  clazz = Class.forName("com.wjt.reflect.Student");   
            
            //通过反射获取Student对象
            Student stu = (Student) clazz.newInstance();   
            
            stu.setName("吴江涛");
            System.out.println(stu);
        }
     }
     
     class Student{
     	private String name;
        public String getName() {
            return name;
        }
        
        public void setName(String name) {
            this.name = name;
        }
        
        @Override
        public String toString() {
            return "Student [name=" + name + "]";
        }
     ```
     
        } 

- ##### 通过反射获取对象

  - **clazz.newInstance()** （根据无参构造返回对象）  通过字节码对象利用反射获取对象

  - **通过Class类方法通过反射获取构造函数**

    - **Constructor<T>    getConstructor(Class<?>... parameterTypes)**  //获取单个构造函数（有参无参都能获取）
  
    - **Constructor<?>[] getConstructors()**     获取所有的构造函数
  
        	```java
         //获取Student类的字节码对象
         Class  clazz = Class.forName("com.wjt.reflect.Student");
         
         //获取Student类所有的构造函数
         Constructor[] constructors = clazz.getConstructors(); 
         
         //获取单个构造方法，参数为可变参数（jdk1.5）,可以传多个或一个参数,参数为有参构造参数类型的字节码文件
         Constructor c = clazz.getConstructor(String.class); 
         ```
         
         
  
  - **获取了构造函数，在调用newInstance()方法即可获取对象实例**
  
- ##### 通过反射获取成员变量

  - **Field getField(String name)**      name为成员变量的名称； 通过反射获取单个指定名称的成员变量

  - **Field[] getFields()**                         通过反射获取所有成员变量

  - **Field getDeclaredField(String name)**       name为成员变量的名称； 通过反射**暴力**获取单个指定名称的成员变量（能获取私有的成员变量）

  - **Field[] getDeclaredFields()**        通过反射暴力获取所有成员变量（能获取所有的私有变量）

  - **df.setAccessible(true);**               去除私有权限

    			```java
       //获取Student类的字节码对象
        Class  clazz = Class.forName("com.wjt.reflect.Student");  
        //通过反射获取Student类的对象
        Student stu = (Student) clazz.newInstance();                
       
        //获取指定名称的成员变量
        Field f = clazz.getField("name");
       
        //获取所有成员变量
        Field[] fields = clazz.getFields();         
       
        //给stu对象设置name="吴江涛"
        f.set(stu, "吴江涛"); 
       
        //获取私有的字段，暴力获取
        Field df = clazz.getDeclaredField("name"); 
        Field[] declaredFields = clazz.getDeclaredFields();
            
        //去除私有权限
        df.setAccessible(true);  
            
        df.set(stu, "吴江涛");
        System.out.println(stu);
       ```

- ##### 通过反射获取成员方法

  - **Method getMethod(String name, Class<?>... parameterTypes)** 

    通过反射获取成员方法，参数为：方法名，方法的参数列表

  - **Method[] getMethods()** 

    通过反射获取所有的成员方法

  - **Method getDeclaredMethod(String name, Class<?>... parameterTypes)**

    通过反射强制获取成员方法，参数为：方法名，方法的参数列表

  - **Method[]   getDeclaredMethods()**

    通过反射强制获取获取所有的成员方法

  - 获取的方法通过**invoke(Object obj, args)**来执行,obj指那个对象，args表示方法所需传入的参数

    ​	

    ```java
    		Class  clazz = Class.forName("com.wjt.reflect.Student");    //获取Student类的字节码对象
    		Student stu = (Student) clazz.newInstance();         		//通过反射获取Student类的对象
    		
    		Method m = clazz.getMethod("print", int.class);
    		Method[] methods = clazz.getMethods();
    		m.invoke(stu,100);
    ————————————————————————————————————————————————————————————————————————————————		
    		Method dm = clazz.getDeclaredMethod("print", int.class);
    		Method[] declaredMethods = clazz.getDeclaredMethods();
    		
    		dm.setAccessible(true);   //去除权限
    		dm.invoke(stu, 100);      //执行方法
    ```

- #### 通过反射越过泛型检测

  		```java
    ArrayList<Integer> list = new ArrayList<Integer>();
      list.add(100);
      list.add(200);
    
      Class clazz = Class.forName("java.util.ArrayList");    //获取ArrayList类的字节码文件
      Method m = clazz.getMethod("add", Object.class);       //获取ArrayList的add(Object)方法 
    
      m.invoke(list, "吴江涛");             //向list集合中添加字符串，越过了泛型检测，在运行时泛型失效，编译完成就执行了泛型擦除
      System.out.println(list);
    
      //输出为：[100, 200, 吴江涛]
    ```
    

#### 4. 动态代理

​	动态代理的意思是通过反射动态生成一个代理对象

- ##### 动态代理

  - JDK动态代理，针对接口动态代理
  - Cglib动态代理（第三方提供），针对子类的动态代理

- ##### JDK动态代理

  通过代理类Proxy来生成动态代理对象；java.lang.reflect包下提供了Proxy类和一个InvocationHandler接口，通过这个类和接口就可以生成代理对象；

  - **使用JDK动态代理的前提是:** 被代理类实现了某个接口；因为JDK动态代理是基于接口实现的；

  -  **Object newProxyInstance(ClassLoader loader, Class<?>[] interfaces, InvocationHandler h)**

    获取动态代理对象

    - 参数一：被代理对象的类加载器
    - 参数二：被代理对象所实现的所有接口
    - 参数三：InvocationHandler接口的实现类

  - **InvocationHandler接口**的实现类实现了invoke （）方法，定义了想要给被代理对象所附加的操作

  	```java
  	//InvocationHandler的实现类，定义JDK动态代理的核心代码  
  	public class MyInvocationHandler implements InvocationHandler{
  	    private Object obj;
  	    public MyInvocationHandler(Object obj) {
  	        this.obj = obj;
  	    }
  		/**
  		 * @param proxy  被代理的对象
  		 * @param method 被代理的方法
  		 * @param args    被代理的方法的参数
  		 */
  		 public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
  		 	System.out.println("权限校验");
  			method.invoke(obj, args);
  			System.out.println("日志记录");
  			return null;
  		}
  	}
  	//定义User接口
  	public interface User {
  		public void login();
  		public void buy();
  	}
  	
  	//实现User接口
  	public class UserImpl implements User {
  		@Override
  		public void login() {
  			System.out.println("登录");
  		}
  	
  		@Override
  		public void buy() {
  			System.out.println("购买");
  		}
  	}
  	```
  	
  	
  	
  	
  	​	
  	```java
  	    //测试类
  		public class Demo3_daili {
  			public static void main(String[] args) {
  	            // 创建User的实现类对象
  	            UserImpl ui = new UserImpl();
  	            // 创建InvocationHandle接口的实现类
  	            MyInvocationHandler my = new MyInvocationHandler(ui);
  	
  	            // 通过Proxy的newProxyInstance()方法生成代理对象
  	            // 创建动态代理对象
  	            User u2 = (User) Proxy.newProxyInstance(ui.getClass().getClassLoader(), ui.getClass().getInterfaces(), my);
  	
  	            // 调用代理后的方法
  	            u2.buy();
  	            u2.login();
  			}
  		}
  	```

### JDK1.7新特性

1. 二进制字面量0b001(JDK1.7后可以表示二进制字面量)

2. 数字字面量可以出现下划线  100_100
3. switch语句可以使用字符串
4. 泛型简化，菱形泛型
5. 异常的多个catch合并，用|隔开
6. 表中异常处理，关流问题

### JDK1.8新特性

1. 接口中可以写方法体，非静态方法，必须要default修饰；
2.  **重量级新特性：**Lambda表达式

#### 1. 函数式接口

**概念：**有且只有一个抽象方法的接口，称之为函数式接口；接口中可以有其他方法（默认方法、静态方法、私有方法）；

**备注：**使用@FunctionalInterface注解来检测接口是否为函数式接口

函数式接口的使用：一般作为方法的参数和返回值类型

#### 2. 函数式编程

1. ##### 面向对象与函数式编程对比

   - **面向对象编程思想：**做一件事情，首先要确定解决这个事情的对象，再通过使用对象的属性和行为去解决这个事情；
   - **函数式编程思想：**做一件事，只关心结果，怎么做，谁做不关心；只重视结果，不重视过程；

2. ##### 函数式编程：**Lambda表达式**

   - **由三部分组成：**

   ​	a. 一些参数

   ​	b. 一个箭头

   ​	c.  一段代码

   - **格式：**（参数列表）-->  { 方法体 } 

   - **解释说明格式：**

     （参数列表）：接口中抽象方法的参数列表，没有参数就空着，有多个参数列表就使用逗号分隔

        -->：传递的意思，把参数传个方法体

     ​	{方法体} ：重写接口的抽象方法的方法体

     ```java
     public class Demo02_Lambda {    
     	public static void main(String[] args) {        
             Person[] arr = new Person[]{               
                 new Person("吴江涛", 22),                
                 new Person("窦瑞林", 19),                
                 new Person("年后", 15)        
             };        
             //使用匿名内部类的方式实现Comparator接口
             /* Arrays.sort(arr, new Comparator<Person>() {                
                 @Override                
                 public int compare(Person o1, Person o2) {                    
                     return o1.getAge() - o2.getAge();                
                 }            
             });*/        
             //使用Lambda表达式实现        
             Arrays.sort(arr, (Person p1, Person p2) -> {return p1.getAge() - p2.getAge();});        
             for (Person person : arr) {
                 System.out.println(person);        
             }    
         }
     }
     ```

3. ##### Lambda表达式省略格式

   **Lambda表达式省略条件：是可推导，可以省略**

   凡是可以根据上下文推导出来的内容，可以省略；

   可以省略的内容：

   - (参数列表)：括号中的参数列表只有一个参数，括号可以省略

   - (参数列表)：括号内的参数列表 的数据类型，可以省略不写

   - (一些代码)：如果代码只有一行，无论是否有返回值，都可以省略{}、return、分号

     注意：{}、return、分号必须一起省略

4. ##### Lambda表达式使用前提

   - 使用Lambda表达式必须具有接口，且接口有且只有一个抽象方法
   - 使用Lambda表达式必须有上下文判断

   **注意：**有且只有一个抽象方法的接口，称为函数式接口

5. ##### Lambda表达式延迟加载（性能优化）

6. ##### 函数式接口作为方法的参数

   替换匿名内部类的操作，如线程；

   ```java
   public class Demo03_LambdaInter {    
   	public static void getThread(Runnable r){         
   		new Thread(r).start();    
   	}    
   	public static void main(String[] args) {        
   		getThread(()->{
   			System.out.println(Thread.currentThread().getName() + "线程开始了！");
   		});    
   	}
   }
   ```

7. ##### 函数式接口作为返回值；

   ```java
   public class Demo03_LambdaInter {
   
       public static Comparator<String> getComparator(){
       	//用Lambda表达式返回Comprarator对象
           return (String s1,String s2)->{
             return s1.length() - s2.length();
           };
       }
   
       public static void main(String[] args) {
           String[] arr = {"a","cccc","bv","dddddd"};
           Arrays.sort(arr, getComparator());
   
           for (String s : arr) {
               System.out.println(s);
           }
       }
   }
   ```

8. ##### 常用的函数式接口

   java.util.function包下；

   - ##### Supplier<T>   被称之为生产接口； 包含一个无参方法  T  get();  用来获取一个泛型参数指定类型的对象；

   - ##### Consumer<T>  消耗数据，处理数据显示或操作数据；包含方法：

     - void accept(T t) ;  抽象方法
     - andThen(Consumer<T> t);  默认方法    连接两个Consumer接口，在进行消费

   - ##### Predicate<T>接口  对某种类型的数据进行判断，返回Boolean值；包含方法 :

     - **boolean test(T t );     抽象方法**
     - **and(T t);  默认方法，连接多个判断；and()方法表示  与运算'&&'**
     - **or(T t) ;    默认方法；or()方法标识或运算'||'**
     - **negate()  默认方法： 取反操作；  标识非运算 '^'**

   - ##### Function<T,R>  接口用来根据接收到一个类型，转换为另一个类型，包含方法：

     - **R  apply(T t) ;  抽象方法**
     - R  andThen(T t);  默认方法；用来连接操作

#### 3. Stream流

Stream流用来将对集合和数组简化操作。解决集合和数组存在的弊端；使代码更加优雅；

- ##### 常用方法：

  - **延迟方法：**返回值类型还是流，支持链式编程
  
    - Stream<T>     **filter**(Predicate<t> p) ；将一个流转变成一个新的流；
  
      满足条件就将其放入新的流中，反之舍去；

    - Stream<T>    **map**( Function<T,R> f) ；将一个流的元素映射到另一个流中（转换）；

    - Stream<T>    **limit**（long maxSize)；截取元素到新的流
  
    - Stream<T>    **skip**(long n) ; 跳过前几个元素，返回一个新的流
  
      如果跳过个数大于流的长度，则返回一个空的流
  
    - Stream<T>  **concat**(Stream<T> s,Stream<T> s2)  ; 将两个流合并为一个流，返回一个新的流
  
  - **终结方法：**返回值类型不再是Stream接口自身类型的方法，不支持链式接口
  
    - **foreach**(Consumer<T>  c)；遍历流中数据，调用后不能 在使用Stream的 方法
    - long  **count**()；返回流中的数据个数；
  
  **注意：** Stream是一个管道流，只能被使用一次，第一个流对象调用完方法后，就会进入下一个新的流，这个流就会关闭，不能在被使用；
  
  例子：
  
  ```java
  public static void main(String[] args) {
      ArrayList<String> list01 = new ArrayList<>();
      list01.add("张无忌");
      list01.add("张三丰");
      list01.add("成昆");
      list01.add("郭靖");
      list01.add("吴江涛");
      list01.add("张");
  
      list01.stream().filter(name->name.length()>2)
              .filter(name->name.startsWith("张")).forEach(name-> System.out.println(name));
  }
  ```

**注意：**Stream流是一个集合元素的函数模型；集合元素是没有真正被操作的，只有当结束方法执行的时候，整个模型才会按照指定的策略进行操作。得益于Lambda的延时加载特性。

- **Stream流式一个来自数据源的元素队列**
- 元素式特定类型的对象，形成一个队列。
  - 数据源是流的来源，可以是集合、数组等。
  
- **Stream流的两个基础特征：**

  - **Pipelining：**中间操作都会返回新的流对象，多个操作串联成一个管道，如流式风格(fluent style);这样可以优化操作；

  - **内部迭代：** 以前遍历集合都是通过迭代器iterator或for循环，显示在集合外部迭代，这叫做外部迭代；Stream提供了内部迭代的方式，流可以直接调用遍历方法；

**备注：**当使用一个流的时候，有三个基本操作 ：获取数据源——>数据转换——>执行操作想要的结果；

- **获取流的方式（两种）：**

  java.util.stream.Stream<T> java8的新接口（不是函数式接口）

  - 所有的Collection集合 都可以提通过stream获取流；

    **default  Stream<E> stream();**

  **注意：**map集合需要转变为单列集合后才能转为stream流

  - 通过Stream接口的静态方法  of()   来获取数组对应的流

    **static <T> Stream<T>  of(T...values)**

    参数是可变参数，我们可以传数组

#### 4. 方法引用

对Lambda表达式进行优化；

- 方法引用符

  双冒号：：为引用运算符，他所在的表达式为方法引用运算表达式；

- 如果Lambda表达的函数块中对象已经存在，方法也已经存在，就可以直接使用方法引用

  例子：

  - lamdba表达式：s -> System.out.println(s);
  - 方法引用：  System.out::println ;

- 方法引用分类：

  - 对象名引用：对象名：：方法名
  - 静态方法引用：     类名：：方法名
  - super引用：存在子父类  ;   super：：方法名
  - this本类对象引用：this：：方法名
  - 类构造器引用：类名：：new
  - 数组构造器引用：数组名：：new

### 测试Junit

#### 1. 测试分类

- 黑盒测试：不需要关心代码的内部实现，只需要关注执行结果哦是否符合预期；
- 白盒测试：需要测试代码的内部实现，去测试代码的性能、逻辑等是否有问题；Junit单元测试是白盒测试

#### 2. Junit步骤

步骤：

- 定义一个测试类

  - 测试类名 ： 被测试的类Test
  - 测试类的包：xxx.xxx.test

- 定义一个测试方法

  - 方法名：test方法名
  - 返回值类型：void
  - 参数列表 ：空参

- 给方法加上@Junit

- 导入Junit的依赖

- 判断结果

  - 红色：失败 

  - 绿色：成功

  - **一般我们会使用断言操作来处理结果**

    Assert.accertEquals(Object a,Object b);    //a代表期望值，b代表真是值

- @Before（一般资源申请）

  在所有测试方法执行之前，执行

- @After（一般资源释放）

  在所有测试方法执行之后，执行

### 注解

概念：JDK1.5之后的新特性，说明程序；

作用：

- 编写文档
- 代码分析
- 编译检查

##### JDK预定义注解

- @Override  检测该注解是否是继承父类的
- @Deprecated   标识内容已过时
- @SuppressWarning:压制警告

##### 自定义注解

格式：元注解

​			public @interface  注解名称{

​			}

本质：本质上就是一个接口，该接口默认继承Annotation接口

属性：接口中的抽象方法；

- 要求：

  - 属性的返回值类型：基本数据类型、String、枚举、数组、注解

  - 定义了属性，在使用时需要给属性赋值

    - 如果定义了属性，可以使用default关键字给属性默认初始化值,在使用注解时可以不做赋值操作；

      @MyAnno（属性名=值，...）

    - 如果只有一个属性需要赋值，属性的名称时value，使用时value可以省略

      @MyAnno（值）//省略了value @MyAnno(value = 值)

    - 数组赋值时，使用{}包裹。如果数组只有一个，{}可以省略

  - 元注解 ：用于描述注解的注解；

    - @Target  ：描述了注解注解作用的位置
      - ElementType.Type   作用在类上
      - ElementType.METHOD   方法上
      - ElementType.FIELD    成员变量上
    - @Retention：描述注解被保留的阶段
      - RetentionPolicy.RUNTIME   当前描述的注解，保留到class字节码文件，被JVM读取
    - @Documented：描述注解是否被抽取到API文档中
    - @Inherited :描述注解是否被子类继承

  - **注解的解析**（获取注解属性的值，从而做一些处理）

    - 获取注解定义位置的对象（Class,Method,Field）
    - 获取指定的注解   getAnnocation(Class)
    - 调用注解中的抽象方法获取配置的属性值；

**javap 字节码文件  反编译**

### 设计模式

#### 1. 装饰者设计模式

 - 装饰者设计模式就是通过获取被装饰者的对象，将其装饰以后，产生新的对象具备跟优秀的功能呢；例如：BufferedReader(Reader reader),就是将reader进行修饰包装，就形成了BufferedReader,同时具备了更好的功能

 - 好处：耦合性低，被修饰的类变化和修饰类变化无关

   		```java
     interface Coder {
     	void code();
     }
     
   class Student implements Coder {
   
   	@Override
   	public void code() {
   		System.out.println("我爱祖国");
   	}
   
   }
   
   class AGuoStudent implements Coder { // 通过接收student对象的引用从而实现student的功能，并对其进行提升，装饰；
   	private Student s;
   
   	public AGuoStudent(Student s) {
   		this.s = s;
   	}
   
   	@Override
   	public void code() {
   		s.code();
   		System.out.println("我爱成都");
   	}
   }
   ```

#### 2. 单例设计模式

 - 单例设计模式，是指被设计的对象只能有一个实例。

 - **三种单例设计模式**

    - **第一种：饿汉式（）**

         ```java
         public class Demo1 {
            	
             public static void main(String[] args) {
                 // 因为方法为静态的，所以直接通过类名调用
                 Singleton s = Singleton.getInstance();
             }
         }
         
         // 饿汉式
         class Singleton {
             // 1.私有构造方法，外部类不能访问该对象
             private Singleton() {
             }
         
             // 2.私用该对象的引用，外部不能直接访问
             private static Singleton s;
         
             // 3.通过公共的静态的getInstance()方法返回该对象的实例
             public static Singleton getInstance() {
                 s = new Singleton();
                 return s;
             }
         }
         ```
         
- **第二种：单例设计的延迟加载模式（懒汉式）**
    
     ```java
         //单例模式延迟加载模式
         //好处：在使用时才去创建对象，节省空间
         class Singleton {
             // 1.私有构造方法，外部类不能访问该对象
             private Singleton() {
             }
         
             // 2.私用该对象的引用，外部不能直接访问
             private static Singleton s;
         
             // 3.通过公共的静态的getInstance()方法返回该对象的实例
             public static Singleton getInstance() {
                 // 先判断对象是否为null
                 if (s == null) {
                     s = new Singleton();
                 }
                 return s;
             }
         }
         ```
    
- **第三种：私有常量**
    
  			```java
         //私有常量模式
         class Singleton {
             // 1.私有构造方法，外部类不能访问该对象
             private Singleton() {
             }
         
             // 2.将对象实例的引用设置为静态常量，不能改变常量值
             public static final Singleton s = new Singleton();
         }
         ```
    
- ##### 饿汉式和懒汉式的区别

  - 饿汉式是以空间换时间，懒汉式是以时间换空间；推荐使用饿汉式，时间珍贵；
  - 在多线程访问下，懒汉式存在线程安全隐患，可能会生成多个对象；

#### 3. 简单工厂方法

- 什么是简单工厂？

  简单工厂模式，**也成为静态工厂模式**；将生成不同类实例的过程封装为一个工厂，进行统一加工和创建对象；不再需要手动去创建各个类的实例，而是通过工厂类来创建相应的实例；

  ```Java
  public class Demo3_factory {
  
  	public static void main(String[] args) {
  		Cat cat = AnimalFactory.createCat();    //通过静态工厂获取相应对象实例
  		Dog dog = AnimalFactory.createDog();
  	}
  
  }
  
  abstract class Animal {
  	public abstract void eat();
  }
  
  class Dog extends Animal{           //狗类
  	@Override
  	public void eat() {
  		System.out.println("狗吃肉");
  	}
  	
  }
  
  class Cat extends Animal{         //猫类
  	@Override
  	public void eat() {
  		System.out.println("猫吃鱼");
  	}
  }
  
  class AnimalFactory {                      //动物工厂类
  	public static Dog createDog() {        //创建狗对象
  		return new Dog();
  	}
  	
  	public static Cat createCat() {        //创建猫对象
  		return new Cat();
  	}
  }
  ```



#### 4. 工厂方法设计模式


工厂方法模式通过定义一个抽象类来实现创建对象的方法，具体的实现由继承了工厂的具体类来实现；当我们创建对象时，通过这个具体类来创建。

#### 5. 适配器设计模式

通过实现一个中间类，去实现我们需要使用的接口，并实现所有的方法（空方法）；然后中间类就可以适配我们具体所需要的某个方法，然后只需继承中间类，重写相应的方法就好；不必去实现接口重写所有方法；这个中间类我们就称为适配器类，这种设计模式就称为适配器设计模式。

#### 6. 模板设计模式

通过定义一个模板类，确定一些固定的算法骨架，将算法骨架的核心部分抽象出来，交给模板类的子类去实现，然后我们通过模板类的子类对象，根据实际需求去实现算法骨架的核心部分；这种设计模式成为模板设计模式；

