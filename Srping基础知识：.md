### 一、JavaSE知识

##### IO流

| 抽象基类 | 字节流(1字节-8bit) | 字符流(1字符(char)-16bit) |
| :------: | :----------------: | :-----------------------: |
|  输入流  |    InputStream     |          Reader           |
|  输出流  |    OutputStream    |          Writer           |

1.按照操作单位不同可以分为：字节流和字符流。
字节流处理图片、视频等数据。
字符流处理文本数据。

2.按照数据的流向可以分为：输入流和输出流。

3.按照流的角色不同可以分为：节点流和处理流。
节点流——直接作用于文件上。
处理流——作用于已有的流之上，作用是提升流的速度等等。

| 抽象基类     | 节点流（或文件流） | 缓冲流（处理流的一种） |
| ------------ | ------------------ | ---------------------- |
| InputStream  | FileInputStream    | BufferedInputStream    |
| OutputStream | FileOutputStream   | BufferedOutputStream   |
| Reader       | FileReader         | BufferedReader         |
| Writer       | FileWriter         | BufferedWriter         |



<u>FileReader读入数据的基本操作</u>

1.实例化File类的对象，指明要操作的文件
File file = new File("文件路径");

2.提供具体的流
FileReader fr = new FileReader(file);

3.数据的读入
int data;
while( (data = fr.read( ) ) != -1){
System.out.print( (char) data);
}

4.关闭流
**fr.close( );**

**JVM自动垃圾回收机制对于物理连接，比如数据库连接、输入输出流、Socket连接无能为力，开启后需要手动关闭。同时，为了保证流资源一定可以执行关闭操作，需要采用try-catch-finally处理异常。**







 

##### 反射

<u>关于java.lang.Class类的理解：</u>

1.类的加载过程：

程序经过javac.exe命令以后，会生成一个或多个字节码文件（.class结尾），接着我们使用java.exe的命令对某个字节码文件进行解释运行。相当于将某个字节码文件加载到内存中，此过程称为类的加载。**加载到内存中的类称为运行时类**，**此运行时类本身就作为Class的一个实例**。（按照java万物皆对象的理论，类也是一个对象）
Class clazz = Person.class;（后面这部分就是这个Person类本身，Person是个类型，为了区分写法，加了个.class）

2.换句话说，Class的实例就对应着一个运行时类。

3.加载到内存中的运行时类，会缓存一定的时间，在此时间之内，我们可以通过不同的方式获取此运行时类（只有一个）。



<u>获取Class的实例的方式：</u>

**方式一：**调用运行时类的属性：.class
Class<Person>  clazz = Person.class;（加上泛型之后避免后续需要强转）

**方式二：**通过运行时类的对象，调用getClass()
Person p1 = new Person();
Class  clazz = p1.getClass();（拿到生成p1对象的类）

★**方式三：**调用Class的静态方法：**forName(String classPath)**
Class clazz = **Class.forName(具体包.Person)**;（包含包名在内的类的完整路径称为全类名）（能够更好的体现动态性，运行时的动态性能）

方式四：使用类的加载器：ClassLoader（了解即可）
ClassLoader classLoader = 测试类.class.getClassLoader();
Class clazz = classLoader.loadClass(具体包.Person);

对于自定义类，使用系统类加载器进行加载。
调用系统类加载器的getParent()：获取扩展类加载器。
调用扩展类加载器的getParent()：无法获取引导类加载器。
引导类加载器主要负责java的核心类库，无法加载自定义类的。



<u>通过反射创建对应的运行时类的对象：</u>

Class <Person>clazz = Person.class;

**★Person obj = clazz.newInstance();**因为前面加了泛型，所以创建的时候可以直接转为Person类的对象，造对象的时候都是用的构造器，只不过调用的形式不一样。



<u>在javabean中要求提供一个public的空参构造器，原因是：</u>

1.便于通过反射，创建运行时类的对象。

2.便于子类继承此运行时类时，默认调用super()时，保证父类有此构造器。



<u>获取运行时类的对象的属性结构</u>

Class clazz = Person.class;
Field[ ] fields = **clazz.getFields()**;
**getFields( )**：获取当前运行时类及其父类中声明为public访问权限的属性。
**getDeclaredFields( )：**获取当前运行时类中声明的所有属性。

<u>获取运行时类的对象的方法</u>

Class clazz = Person.class;
Method[ ] methods = **clazz.getMethods( );**
**getMethods( )：**获取当前运行时类及其父类中声明为public访问权限的属性。
**getDeclaredMethods( )：**获取当前运行时类中声明的所有方法。

★<u>调用运行时类的指定属性</u>

Class clazz= Person.class();
Person p = (Person) clazz.newInstance();
Field name = clazz.getDeclaredField(“Person类中的具体属性名:name”);
name.setAccessible(true);打开访问权限
name.set(p,"Tom");为对象p的name属性设置值
System.out.println(name.get(p));-------------------------结果为“Tom”



**代理模式**

1.静态代理

在编译时就已经把**接口**、**代理类**和**被代理类**等确定了下来。

2.动态代理

代理类在程序运行时根据加载到内存中的被代理类的信息创建的代理方式。



















Class为反射的源头，



























## 二、计算机网络知识

##### 第一章 Web浏览器

##### 1.输入URL，浏览器解析网址，生成请求消息（告诉要服务器做什么）

不同形式的URL，以HTTP为例，HTTP协议定义了URL的书写格式，定义了客户端（浏览器）和服务器之间交互的消息内容和步骤。

##### 2.向DNS服务器查询服务器的IP地址

通过DNS解析器向DNS服务器发出请求，得到相应响应，获取IP地址。

网络号；主机号；子网掩码；

IP地址由一串32比特的数字组成，8比特为一组，转为十进制后用圆点隔开。网络号和主机号的具体结构是不固定的，因此需要子网掩码来确定网络号和主机号，子网掩码表示网络号和主机号之间的边界。

访问时用域名（服务器名称）来代替IP地址更容易记忆，确定请求指令接收位置时用IP地址来代替域名时运行效率更高。

<u>（人来使用名称，路由器使用IP地址）</u>

##### 3.DNS服务器查询IP地址

##### 4.浏览器委托给操作系统，将消息发送给Web服务器

消息先经过子网中的集线器，转发到最近的路由器，再传递到下一个路由器（通过集线器进行中转），最后由子网中的集线器传递给服务器。

浏览器将服务器IP地址和消息委托给操作系统，操作系统根据IP地址将消息发送给服务器。



##### 第二章 协议栈、网卡

1.

















### 三、Spring基础知识

Spring全家桶：Spring、SpringMVC、Springboot、Springcloud

Spring核心技术：IOC（控制反转：创建对象的权力移交给IOC容器）、AOP（面向切面编程）



##### IOC（控制反转）

控制：对象的创建、属性的赋值、管理对象之间的关系。

反转：不采用new的方式创建对象，将权限移交给外部资源（IOC容器），让IOC容器来代替开发者来创建对象、给属性赋值和关系对象关系。

优点：**解耦**，便于后期改动代码。

##### DI（依赖注入）

在容器中建立bean和bean之间的依赖关系的过程，叫做依赖注入。

IOC和DI的目标：充分解耦



### 四、MyBatis知识

sqlSession：会话对象

mapper：接口实现类对象，用来调用接口中的方法



<u>获取参数值的两种方式：</u>

★#{}：预编译处理，有效方式sql注入

${}：字符串拼接，使用时需要加单引号

**1.若mapper接口方法的参数为单个的字面量类型**
此时可以通过#{}和${}以任意的内容获取参数值，一定要注意${}的单引号问题。

**2.若mapper接口方法的参数为多个的字面量类型**
此时MyBatis会将参数放在map集合中，以两种方式存储数据，arg0, arg1...为键，以参数为值或者是以param1 ,param2...为键，以参数为值。
因此，只需要通过#{}和${}访问map集合中的键，就能获取相对应的值，一定要注意${}的单引号问题。

**3.若mapper接口方法的参数为map集合类型的参数**
只需要通过#{}和${}访问map集合中的键，就能获取相对应的值，一定要注意${}的单引号问题。

**★4.若mapper接口方法的参数为实体类型的参数**
只需要通过#{}和${}访问实体类中的属性名，就可以获取相对应的属性值，一定要注意${}的单引号问题。

★**5.可以在mapper接口方法的参数上设置@Param注解**
此时MyBatis会将这些参数放在map中，以两种方式进行存储
（1）以@Param注解的value属性值为键
（2）以我们param1，param2为键
通过#{}和${}访问map集合中的键，就能获取相对应的值，一定要注意${}的单引号问题。



**成员变量和属性**

成员变量：
类中初始化定义的变量称为成员变量

属性：
类中set/get方法中set/get后面紧跟着的名字称为属性



**字段名和属性名不一致的情况，如何处理映射关系**

1.为查询的字段设置别名，和属性名保持一致

2.当字段符合MysQL的要求使用_，而属性符合java的要求使用驼峰
此时可以在MyBatis的核心配置文件中设置一个全局配置，可以自动将下划线映射为驼峰emp_id:empId, emp_name : empName

3.使用resultMap自定义映射

```java
resultMap:设置自定义的映射关系
	id：唯一标识
	type：处理映射关系的实体类的类型
	
常用的标签：
id：处理主键和实体类中属性的映射关系
result：处理普通字段和实体类中属性的映射关系
    column：设置映射关系中的字段名，必须是sql查询出的某个字段
    property：设置映射关系中的属性的属性名，必须是处理的实体类类型中的属性名
```

