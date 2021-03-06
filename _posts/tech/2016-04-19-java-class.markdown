---
layout: post
title:  Java Class.forName()的作用与使用总结
date:   2016-04-13 16:13:26
categories: javaBasic
---


## 1、Class类简介：
 Java程序在运行时，Java运行时系统一直对所有的对象进行所谓的运行时类型标识。这项信息纪录了每个对象所属的类。虚拟机通常使用运行时类型信息选准正确方法去执行，用来保存这些类型信息的类是Class类。Class类封装一个对象和接口运行时的状态，当装载类时，Class类型的对象自动创建。

Class 没有公共构造方法。Class 对象是在加载类时由Java 虚拟机以及通过调用类加载器中的 defineClass 方法自动构造的，因此不能显式地声明一个Class对象。

虚拟机为每种类型管理一个独一无二的Class对象。也就是说，每个类（型）都有一个Class对象。运行程序时，Java虚拟机(JVM)首先检查是否所要加载的类对应的Class对象是否已经加载。如果没有加载，JVM就会根据类名查找.class文件，并将其Class对象载入。

基本的 Java 类型（boolean、byte、char、short、int、long、float 和 double）和关键字 void 也都对应一个 Class 对象。

每个数组属于被映射为 Class 对象的一个类，所有具有相同元素类型和维数的数组都共享该 Class 对象。

一般某个类的Class对象被载入内存，它就用来创建这个类的所有对象。

### 一、如何得到Class的对象呢？有三种方法可以的获取：

* 调用Object类的getClass()方法来得到Class对象，这也是最常见的产生Class对象的方法。例如：

      MyObject x;
      Class c1 = x.getClass();

* 使用Class类的中静态forName()方法获得与字符串对应的Class对象。例如：

      Class c2 = Class.forName("MyObject")

  Employee必须是接口或者类的名字。

* 获取Class类型对象的第三个方法非常简单。如果T是一个Java类型，那么T.class就代表了匹配的类对象。例如

      Class cl1 = Manager.class;
      Class cl2 = int.class;
      Class cl3 = Double[].class;

  注意：Class对象实际上描述的只是类型，而这类型未必是类或者接口。例如上面的int.class是一个Class类型的对象。由于历史原因，数组类型的getName方法会返回奇怪的名字。

### 二、Class类的常用方法

1、getName()

一个Class对象描述了一个特定类的属性，Class类中最常用的方法getName以 String 的形式返回此 Class 对象所表示的实体（类、接口、数组类、基本类型或 void）名称。

2、newInstance()

Class还有一个有用的方法可以为类创建一个实例，这个方法叫做newInstance()。例如：
    x.getClass.newInstance()，创建了一个同x一样类型的新实例。newInstance()方法调用默认构造器（无参数构造器）初始化新建对象。

3、getClassLoader() 返回该类的类加载器。

4、getComponentType()
    返回表示数组组件类型的 Class。

5、getSuperclass()
    返回表示此 Class 所表示的实体（类、接口、基本类型或 void）的超类的 Class。

6、isArray()
    判定此 Class 对象是否表示一个数组类。

### 三、Class的一些使用技巧

1、forName和newInstance结合起来使用，可以根据存储在字符串中的类名创建对象。例如
    Object obj = Class.forName(s).newInstance();

2、虚拟机为每种类型管理一个独一无二的Class对象。因此可以使用==操作符来比较类对象。例如：
    if(e.getClass() == Employee.class)...

## 2、 Class.forName()方法:
Class.forName：返回与给定的字符串名称相关联类或接口的Class对象。

Class.forName是一个静态方法，同样可以用来加载类。该方法有两种形式：Class.forName(String name, boolean initialize, ClassLoader loader)和 Class.forName(String className)。第一种形式的参数 name表示的是类的全名；initialize表示是否初始化类；loader表示加载时使用的类加载器。第二种形式则相当于设置了参数 initialize的值为 true，loader的值为当前类的类加载器。

static Class<?>

forName(String className)

Returns the Class object associated with the class or interface with the given string name.

static Class<?>

forName(String name, boolean initialize, ClassLoader loader)

Returns the Class object associated with the class or interface with the given string name, using the given class loader.

说明：

publicstatic Class<?> forName(String className)

Returns the Class object associated withthe class or interface with the given string name. Invokingthis method is equivalent to:

Class.forName(className,true, currentLoader)

where currentLoader denotes the definingclass loader of the current class.

For example, thefollowing code fragment returns the runtime Class descriptor for theclass named java.lang.Thread:

Class t =Class.forName("java.lang.Thread")

A call to forName("X") causes theclass named X to beinitialized.

Parameters:

className - the fully qualifiedname of the desired class.

Returns:

the Class object for the classwith the specified name.

从官方给出的API文档中可以看出:

Class.forName(className)实际上是调用Class.forName(className,true, this.getClass().getClassLoader())。第二个参数，是指Class被loading后是不是必须被初始化。可以看出，使用Class.forName（className）加载类时则已初始化。

所以Class.forName(className)可以简单的理解为：获得字符串参数中指定的类，并初始化该类。



### 一.首先你要明白在java里面任何class都要装载在虚拟机上才能运行。

1.      forName这句话就是 **装载类** 用的(new是根据加载到内存中的类 **创建一个实例**，要分清楚)。

2.      至于什么时候用，可以考虑一下这个问题，给你一个字符串变量，它代表一个类的包名和类名，你怎么实例化它？

           A a = (A)Class.forName("pacage.A").newInstance();这和 A a =new A();是一样的效果。

3.      jvm在装载类时会执行类的静态代码段，要记住静态代码是和class绑定的，class装载成功就表示执行了你的静态代码了，而且以后不会再执行这段静态代码了。

4.      Class.forName(xxx.xx.xx)的作用是要求JVM查找并加载指定的类，也就是说JVM会执行该类的静态代码段。

5.      动态加载和创建Class 对象，比如想根据用户输入的字符串来创建对象

       String str = 用户输入的字符串  

       Class t = Class.forName(str);  

       t.newInstance();

### 二.在初始化一个类，生成一个实例的时候，newInstance()方法和new关键字除了一个是方法，一个是关键字外，最主要有什么区别？

1. 它们的区别在于创建对象的方式不一样，前者是使用类加载机制，后者是创建一个新类。

2. 那么为什么会有两种创建对象方式？

这主要考虑到软件的可伸缩、可扩展和可重用等软件设计思想。  
Java中工厂模式经常使用newInstance()方法来创建对象，因此从为什么要使用工厂模式上可以找到具体答案。例如：

           class c = Class.forName(“Example”);  

           factory = (ExampleInterface)c.newInstance();  

其中ExampleInterface是Example的接口，可以写成如下形式：

          String className = "Example";  

          class c = Class.forName(className);  

          factory = (ExampleInterface)c.newInstance();  

进一步可以写成如下形式：

          String className = readfromXMlConfig;//从xml 配置文件中获得字符串

         class c = Class.forName(className);  

         factory = (ExampleInterface)c.newInstance();  

上面代码已经不存在Example的类名称，它的优点是，无论Example类怎么变化，上述代码不变，甚至可以更换Example的兄弟类Example2 , Example3 , Example4……，只要他们继承ExampleInterface就可以。

3. 从JVM的角度看，我们使用关键字new创建一个类的时候，这个类可以没有被加载。  但是使用newInstance()方法的时候，就必须保证：

    1、这个类已经加载；

    2、这个类已经连接了。

而完成上面两个步骤的正是Class的静态方法forName()所完成的，这个静态方法调用了启动类加载器，即加载 java API的那个加载器。  

现在可以看出，newInstance()实际上是把new这个方式分解为两步，即首先调用Class加载方法加载某个类，然后实例化。这样分步的好处是显而易见的。我们可以在调用class的静态加载方法forName时获得更好的灵活性，提供给了一种降耦的手段。  

### 三.最后用最简单的描述来区分new关键字和newInstance()方法的区别：

1. newInstance: 弱类型。低效率。只能调用无参构造。  

2. new: 强类型。相对高效。能调用任何public构造。

## 3、应用情景：

* 情景一：加载数据库驱动的时候

Class.forName的一个很常见的用法是在加载数据库驱动的时候。

如：


    Class.forName("com.microsoft.sqlserver.jdbc.SQLServerDriver");  
    Connection con= DriverManager.getConnection( "jdbc:sqlserver://localhost:1433;DatabaseName==JSP","jph","jph");      

  为什么在我们加载数据库驱动包的时候有的却没有调用newInstance( )方法呢？
即有的jdbc连接数据库的写法里是Class.forName(xxx.xx.xx);而有一些：Class.forName(xxx.xx.xx).newInstance()，为什么会有这两种写法呢？

刚才提到，Class.forName("");的作用是要求JVM查找并加载指定的类，如果在类中有静态初始化器的话，JVM必然会执行该类的静态代码段。

而在JDBC规范中明确要求这个Driver类必须向DriverManager注册自己，即任何一个JDBCDriver的Driver类的代码都必须类似如下：

      public classMyJDBCDriver implements Driver {
        static{
           DriverManager.registerDriver(new MyJDBCDriver());
         }
      }

  既然在静态初始化器的中已经进行了注册，所以我们在使用JDBC时只需要Class.forName(XXX.XXX);就可以了。

* 情景二：使用AIDL与电话管理Servic进行通信

      Method method = Class.forName("Android.os.ServiceManager").getMethod("getService",String.class);
      // 获取远程TELEPHONY_SERVICE的IBinder对象的代理
      IBinder binder =(IBinder) method.invoke(null, new Object[] { TELEPHONY_SERVICE});
      // 将IBinder对象的代理转换为ITelephony对象
      ITelephonytelephony = ITelephony.Stub.asInterface(binder);
      // 挂断电话
      telephony.endCall();



参考资料：

JDK1.8_API.../docs/api/java/lang/Class.html

http://www.ibm.com/developerworks/cn/java/j-lo-classloader/


[(来源)](http://blog.csdn.net/fengyuzhengfan/article/details/38086743?utm_source=tuicool&utm_medium=referral)
