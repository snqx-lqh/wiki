## 一、java入门及环境安装。

1）Java是什么？是一门高级编程语言。

2）java由哪家公司研发，现在又属于哪家公司？sun、Oracle。

3）java之父是谁？詹姆斯.高斯林。

4）java能做什么？基本上什么都能干，主要是做互联网系统的开发。

5）java有哪些技术平台？JavaSE, JavaEE，JavaME。

6）搭建java平台需要安装什么？去哪里下载？JDK；Oracle官网。

7）JDK目前发展到哪里了，LTS版本有哪些，企业使用的JDK有什么特点? JDK 17,JDK 8、11、17，很多企业还在使用JDK 8。

8）如何验证JDK是否安装成功了? 在命令行窗口中，输入java -version、javac -version看版本号。

9）Java开发环境最重要的2个命令是什么啊? javac编译命令、java执行命令。

10）开发一个Java程序要经历哪些步骤? 编写、编译(javac)、运行(java)。

11）Java代码编写有什么基本要求? 文件名称的后缀必须是java结尾。文件名称必须与代码的类名称一致。必须使用英文模式下的符号。

12）JDK有哪些组成啊?		

* JVM虚拟机:真正运行Java程序的地方。
* 核心类库:Java自己写好的一些程序,给咱们的程序调用的
* 开发工具: javac、 java、….

13）Java的跨平台是什么含义，Java如何实现跨平台的? 一次编译、处处可用，我们的程序只需要开发一次，就可以在各种安装了JVM的系统平台上运行。

14）什么是Path环境变量? Path环境变量用于配置程序的路径。方便我们在命令行窗口的任意目录启动程序。

15）JDK安装时，环境变量需要注意什么? 较新版本的JDK会自动配置PATH环境变量，较老的JDK版本则不会。建议还是自己配置一下“Path" 、“JAVA_HOME”

16）https://www.cnblogs.com/xiondun/p/15565726.html  IDEA安装破解

17）IDEA的结构都是什么样的? project - module - package - class     project中可以创建多个module  module中可以创建多个package   package中可以创建多个class

## 二、基础语法部分

基础语法部分其实我觉得和C/C++语言差的地方不是很多，具体的内容可以翻阅以下在线文档，当作字典。

菜鸟教程：https://www.runoob.com/java/java-tutorial.html

## 三、方法

一种语法结构，将一段代码封装成一个功能，方便重复调用，其实就是函数吧。

基本类型和引用类型的参数传递，和C++一样。

方法重载，和C++一样。

## 四、面向对象

### 1、类的创建

建议一个文件只有一个类

```java
//类的创建和使用
class Teacher{
	String teacherId;
	private String name;
	String sex;
	public Teacher(String name)//构造初始化，可重载，类似C++
	{
		this.name = name;
	}
	public void print(){
		System.out.println("teacherId="+teacherId+",name="+name+",sex="+sex);
	}
}//建立一个类，和C++相像

public class HelloWorld{
	public static void main(String[] args){
		Teacher P1 = new Teacher("ZHANG");//创建对象

	}
}
```
### 2、两个变量同时指向一个对象内存图

```java
public class HelloWorld{
	public static void main(String[] args){
		Teacher P1 = new Teacher("ZHANG");//创建对象
		Teacher P2 = P1;
	}
}
```

### 3、构造函数

对象是通过构造器来的，构造器可以初始化对象，分为有参构造和无参构造。this指向当前对象。 

```java
class Teacher{
	String teacherId;
	private String name;
	String sex;
    public Teacher(){
        System.out.println("这是一个无参构造器");
    }
	public Teacher(String name)//构造初始化，可重载，类似C++
	{
        System.out.println("这是一个有参构造器");
		this.name = name;
	}
	public void print(){
		System.out.println("teacherId="+teacherId+",name="+name+",sex="+sex);
	}
}
```

### 5、封装

将变量用private封装，然后用public修饰的方法暴露其取值和赋值。

## 五、String和ArrayList

### 1、string的常用构造

直接的一个字符串对象的话，不同变量指向同一对象，指向常量池的对象。

如果是构造一个对象的话，不同的地址不同，指向堆内存的对象。

```java
public class App {
    public static void main(String[] args) throws Exception {  
        String name = "我是中国人！";
        System.out.println(name);

        //直接无参构造
        String s1 = new String();
        System.out.println(s1);

        //带字符串内容
        String s2 = new String("中国人");
        System.out.println(s2);
        
        //char类型数组
        char[] sChar = new char[]{'A','b','C','d'};
        String s3 = new String(sChar);
        System.out.println(s3);

        //byte类型数组
        byte[] sByte = new byte[]{97,98,99,65,66,67};
        String s4 = new String(sByte);
        System.out.println(s4);
    }  
}
```

### 2、String常用API

```java
public class App {
    public static void main(String[] args) throws Exception {  
        String name = "我是中国人！";
        System.out.println(name.length()); //计算字符串长度       
        System.out.println(name.charAt(2));//返回对应索引字符
        char[] chars = name.toCharArray();//将字符串转换成字符数组返回
        for(int i=0;i<chars.length;i++){
            char ch = chars[i];
            System.out.println(ch);
        }
        System.out.println(name.substring(0, 2));//包前不包后
        System.out.println(name.substring(2));//从传入开始到末尾
        System.out.println(name.replace("中国", "**"));//旧值替换成新字符
        System.out.println(name.contains("我是"));//是否有该字符
        System.out.println(name.startsWith("我是"));//是否从该字符开始
        String name2 = "王宝强，贾乃亮，陈羽凡";
        String[] names = name2.split("，");//用该字符划分字符串
        for (int i = 0; i < names.length; i++) {
            System.out.println("选择了："+names[i]);
        }
    }
}
```

### 3、ArrayList的概述

类似一个集合，适合做元素个数不确定的场景，提供了许多丰富好用的API。

基础使用和泛型操作,泛型可以是类

```java

public class App {
    public static void main(String[] args) throws Exception {  
        ArrayList list = new ArrayList();
        list.add("java");
        list.add("黑马");
        list.add("沥青");
        System.out.println(list);
        list.add(1,"hello");//在索引位置插入
        System.out.println(list);
        ArrayList<Integer> list2 = new ArrayList<Integer>();//泛型后面可省略 String Integer
        list2.add(12);
        System.out.println(list2);
    }
}

```

### 4、ArrayList常用API

```java
import java.util.ArrayList;
public class App {
    public static void main(String[] args) throws Exception {  
        ArrayList<String> list = new ArrayList<String>();
        list.add("java");
        list.add("黑马");
        list.add("沥青");
        System.out.println(list.get(1));//取元素
        System.out.println(list.size());//取元素个数
        System.out.println(list.remove("黑马"));//删掉元素并返回
        System.out.println(list.set(0,"HTML")); //修改对应索引值
        System.out.println(list);
    }
}
```

## 六、Static常用方法

### 1、Static概述

static定义变量只有一份

```java
public class App {
    public static void main(String[] args) throws Exception {  
       User P1 = new User();
       User P2 = new User();
       P2.Display();
    }
}

class User{
    public static int playerNum = 0;
    User(){
        User.playerNum++;
    }
    void Display()
    {
        System.out.println(playerNum);
    }
}
```

### 2、实例方法和静态成员方法

```java
public class App {
    String name;
    //实例方法，必须创建对象才能调用
    void study(){
        System.out.println(this.name+"在学习");
    }
    //静态方法，可以直接调用，外部调用要用类名来调用
    public static void getMax(int a,int b){
        System.out.println(a>b?a:b);
    }
    public static void main(String[] args) throws Exception {  
       App P1 = new App();
       P1.study();
       getMax(2,2);
    }
}
```

### 3、工具类

都是静态方法，一次编写处处使用，建议私有化，不需要创建对象

```java
import java.util.Random;

public class VerifyTool {
    /**
     * 这是一个工具类，建议私有其构造，不让其创建对象，直接用类名调用
     */
    private VerifyTool(){
        
    }

    public static String creatCode(int n){
        String chars = "qwertyuiopasdfghjklzxcvbnmQWERTYUIOPASDFGHJKLZXCVBNM1234567890";

        String code = "";
        Random r = new Random();
        for (int i = 0; i < n; i++) {
            int index = r.nextInt(chars.length());
            code += chars.charAt(index);
        }
        return code;
    }
}

```

### 4、static静态访问注意事项

静态方法只能访问静态的成员，不可以直接访问实例成员。

实例方法可以访问静态的成员，也可以访问实例成员。

静态方法中是不可以出现this关键字的。

### 5、代码块

静态代码块

```java
public class App {
    /**
     * 静态代码块，随着类的加载而加载，只执行一次
     * 在类加载的时候做一些静态数据初始化的操作
     */
    static{
        System.out.println("==静态代码块==");
    }
    public static void main(String[] args) {
        System.out.println("==main方法==");
    }
}

```

构造代码块{}，比构造先执行。

### 6、单例模式

饿汉单例

```java
public class SingleInstance1 {
    /**
     * 2、定义一个公开的静态成员变量存储一个类的对象
     */
    public static SingleInstance1 instance = new SingleInstance1();
    
    /**
     * 1、把构造器私有起来
     */
    private SingleInstance1(){

    }
}

public class App {
    public static void main(String[] args) {
        SingleInstance1 s1 = SingleInstance1.instance;
        SingleInstance1 s2 = SingleInstance1.instance;
        System.out.println(s1);
        System.out.println(s2);
    }
}

```

懒汉单例

```java
public class SingleInstance1 {
    /**
     * 2、定义一个公开的静态成员变量存储本类的对象
     */
    public static SingleInstance1 instance;
    
    /**
     * 1、把构造器私有起来
     */
    private SingleInstance1(){

    }

    /**
     * 3、定义一个方法，让其他地方可以调用获取一个对象
     */
    public static SingleInstance1 getInstance(){
        if(instance==null){
            instance = new SingleInstance1();
        }
        return instance;
    }
}

public class App {
    public static void main(String[] args) {
        SingleInstance1 s1 = SingleInstance1.getInstance();
        SingleInstance1 s2 = SingleInstance1.getInstance();
        System.out.println(s1);
        System.out.println(s2);
    }
}
```

## 七、继承

### 1、继承的简单使用

```JAVA
package extendsPractice;

public class People {
    private String name;
    private String age;

    public String getName() {
        return this.name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getAge() {
        return this.age;
    }

    public void setAge(String age) {
        this.age = age;
    }

}

package extendsPractice;

public class Student extends People {
    
    public void study(){
        System.out.println("好好学习，天天向上！");
    }
}

package extendsPractice;

public class Teacher extends People {
    public void teach(){
        System.out.println("好好教书育人");
    }
}

import extendsPractice.Student;
import extendsPractice.Teacher;

public class App {
    public static void main(String[] args) {
        Teacher P1 = new Teacher();
        Student P2 = new Student();
        P1.setAge("12");
        System.out.println(P1.getAge());
        P2.study();
    }
}

```

### 2、继承的特点

子类可以继承父类的属性和行为，但是子类不能继承父类的构造器。不可直接继承私有，可共享静态成员和方法。
Java是单继承模式:一个类只能继承一个直接父类。
Java不支持多继承、但是支持多层继承。
Java中所有的类都是Object类的子类。

### 3、继承后的特点

成员访问就近原则，先局部，再子类，再父类

```java
public class Student extends People {
    public String name = "子类名字";

    public void showName(){
        String name = "局部名字";
        System.out.println(name);
        System.out.println(this.name);
        System.out.println(super.name);
    }
}
```

方法可以重写，@Override是重写报警

重写方法的名称和形参列表应该与被重写方法一致。

私有方法不能被重写。

子类重写父类方法时，访问权限必须大于或者等于父。

子类中所有的构造器默认都会先访问父类中无参的构造器，再执行自己。

子类通过this...） 去调用本类的其他构造器，本类其他构造器会通过super去手动调用父类的构造

器，最终还是会调用父类构造器的。

注意: this(..) super(.)都只能放在构造器的第一行，所以二者不能共存在同一个构造器中。

## 八、包、权限修饰符、fina关键字、常量、枚举

### 1、包

包是用来分门别类的管理各种不同类的，类似于文件夹、建包利于程序的管理和维护。

建包的语法格式: package公司域名倒写.技术名称。报名建议全部英文小写，且具备意义。

如果有一样的，要把包的路径写好。

### 2、权限修饰符

权限修饰符:有四种作用范围由小到大( private ->缺省-> protected - > public )

成员变量一般私有。

方法一般公开。

如果该成员只希望本类访问，使用private修饰。

如果该成员只希望本类，同一个包下的其他类和子类访问，使用protected修饰。

### 3、fina关键字

final 关键字是最终的意思，可以修饰（方法，变量，类)

修饰方法:表明该方法是最终方法，不能被重写。

修饰变量:表示该变量第一次赋值后，不能再次被赋值(有且仅能被赋值一次)。

修饰类:表明该类是最终类，不能被继承。

### 4、常量

```java
public static final String SCHOOL_NAME = "1234";
```

常量的作用和好处:可以用于做系统的配置信息，方便程序的维护，同时也能提高可读性。

常量命名规范;英文单词全部大写，多个单词下划线连接起来。

在编译阶段会进行“宏替换”，把使用常量的地方全部替换成真实的字面量。

这样做的好处是让使用常量的程序的执行性能与直接使用字面量是一样的。

### 5、枚举

枚举类都是继承了枚举类型:java.lang.Enum

枚举都是最终类，不可以被继承。

构造器的构造器都是私有的，枚举对外不能创建对象。

枚举类的第一行默认都是罗列枚举对象的名称的。

枚举类相当于多例。

和C基本一样。

## 九、抽象类、接口

### 1、抽象类

某个父类知道其所有子类要完成某功能，但是每个子类完成情况都不一样，父类就只定义该功能的

基本要求，具体实现由子类完成，这个类就可以是一个抽象类，抽象类其实就是一种不完全的设计

图。

抽象类用来被继承的,抽象方法是交给子类重写实现的。

```java
package abstract_class;

public abstract class Animal {
    public abstract void run();
}

package abstract_class;

public class Tiger extends Animal {
    @Override
    public void run(){
        System.out.println("老虎跑的快");
    }
}

package abstract_class;

public class Dog extends Animal {
    @Override
    public void run(){
        System.out.println("狗也跑的很快！");
    }
}
```

有得有失:得到了抽象方法，失去了创建对象的能力。

抽象类为什么不能创建对象?

类有的成员〔成员变量、方法、构造器）抽象类都具备

抽象类中不一定有抽象方法，有抽象方法的类一定是抽象类

一个类继承了抽象类必须重写完抽象类的全部抽象方法，否则这个类也必须定义或抽象类。

不能用abstract修饰变量、代码块、构造器。

模板方法，把其中一段代码用一个抽象方法代替。模板方法建议final修饰，免得被子类重写。

### 2、接口

接口不能创建对象

接口的基本使用

```java
package interface_class;

public interface SportManinterface {
    //接口只有常量和抽象方法,会默认加上
    String SCHOOL_NAME = "黑马";
    void run();
    void eat();
}
```

```java
package interface_class;

public interface SportMan {
    void run();
    void eat();
}

package interface_class;

public interface Law {
    void rule();
}

package interface_class;

public class PingPongMan implements SportMan,Law{
    private String name;
    public PingPongMan(String name){
        this.name=name;
    }
    @Override
    public void rule() {
        // TODO Auto-generated method stub
        System.out.println(name+"遵章守法");
    }

    @Override
    public void run() {
        // TODO Auto-generated method stub
        System.out.println(name+"要跑步");
    }

    @Override
    public void eat() {
        // TODO Auto-generated method stub
        System.out.println(name+"要吃饭");
    }

    
}
```

接口的继承

```java
public interface SportMan extends Law,People{
    
}
```

**JDK8以后允许直接定义有方法体的方法**

默认方法用default修饰，就是实例方法，对象调用。

静态方法static修饰，默认public，接口名调用。

私有方法必须在接口内部才能被访问。9之后

**使用接口的注意事项**

1、接口不能创建对象

2、一个类实现多个接口，多个接口中有同样的静态方法不冲突，对应的名调用自己的静态方法。

3、一个类继承了父类，同时又实现了接口，父类中和接口中有同名方法，默认用父类的。

4、一个类实现了多个接口，多个接口中存在同名的默认方法，不冲突，这个类重写该方法即可。

5、一个接口继承多个接口，是没有问题的，如果多个接口中存在规范冲突则不能多继承。

## 十、多态、内部类、常用API

### 1、多态

```java
// 父类 变量名 = new 子类
public class App {
    public static void main(String[] args) {
        Animal t = new Tiger();
        t.run();

        Animal t1 = new Dog();
        t1.run();
    }
}
```

多态中成员访问特点

方法调用:编译看左边，运行看右边。

变量调用:编译看左边，运行也看左边。〔多态侧重行为多态)

定义方法的时候，使用父类型作为参数，该方法就可以接收这父类的一切子类对象，体现出多态的

扩展性与便利。

多态下不能使用子类的独有功能

类型转换,为了强调子类特有的功能

```java
public class App {
    public static void main(String[] args) {
        //自动类型转换
        Animal t = new Tiger();
        t.run();

        Animal t1 = new Dog();
        t1.run();
        //强制类型转换前，先判断变量的实际类型
        if(t1 instanceof Tiger){
            Tiger a = (Tiger)t1;
            a.lay();
        }else if(t1 instanceof Dog){
            Dog a = (Dog)t1;
            a.look();
        }

    }
}
```

### 2、内部类

当一个事物的内部，还有一个部分需要一个完整的结构进行描述，而这个内部的完整的结构又只为

外部事物提供服务，那么整个内部的完整结构可以选择使用内部类来设计。

内部类通常可以方便访问外部类的成员。包括私有的成员。

内部类提供了更好的封装性，内部类本身就可以用private protectecd等修饰，封装性可以做更多控

制。

#### 1、静态成员内部类

类有的他都有,不能直接访问外部成员变量

```java
public class Outter(){
	public static class Inner(){
        
    }
}

Outter.Inner in = new Outter.Inner();
```

#### 2、成员内部类

不加static修饰。JDK16开始支持静态成员，可以直接访问外部成员变量。

```java
Outter.Inner in = new Outter().new Inner();

Outter.this.
this.

```

#### 3、局部内部类

方法里面的一个类

#### 4、匿名内部类

本质上是一个没有名字的局部内部类，定义在方法中、代码块中、等。

作用:方便创建子类对象，最终目的为了简化代码编写。

```java
public class App {
    public static void main(String[] args) {
        Animal a = new Animal(){
            @Override
            public void run() {
                // TODO Auto-generated method stub
                System.out.println("跑的快");    
            }
        };
        a.run();
    }
}
```

### 3、常用API

1、Object类

```java
toString();//重写返回类的信息
equals();//比较两个对象地址是否相同,子类重写，比较信息
```

2、Objects类

```java
equals();//更加安全，会做空指针判断
isNull();//判断是否是空指针
```

3、StringBuilder类

```java
append();//string的拼接

```

4、math类

```java
Math.abs(int a);//获取绝对值
Math.ceil(double a);//向上取整
Math.floor(double a);//向下取整
Math.round(float a);//四舍五入
Math.max(int a,int b);//获取最大值
Math.pow(double a,double b);//a的b次方
Math.random()//返回值为double的随机值 0.0-1.0
```

5、system类

```java
void exit(int status);//终止当前运行的java虚拟机
long currentTimeMillis();//返回当前系统的时间毫秒值形式
void arraycopy(数据源数组，起始索引，目的地数组，起始索引，拷贝个数);//数组拷贝

```

6、BigDecimal类，解决精度问题

```java
BigDecimal a1 = BigDecimal.valueof(a);
BigDecimal b1 = BigDecimal.valueof(b);
BigDecimal c1 = a1.add(b1);
BigDecimal c1 = a1.subtract(b1);
BigDecimal c1 = a1.multiply(b1);
BigDecimal c1 = a1.divide(b1);
BigDecimal c1 = a1.divide(b1，2，RoundingMode.HALF);//除数，保留小数位数，舍入模式

```

## 十一、时间API、Lambda、常见算法

### 1、时间API

1、Date类

```java
Date d = new Date();
getTime();//获取时间对象的毫秒值
setTime(long time);
```

2、SimpleDateFormat类

```java
Date d = new Date();
SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss EEE a"); 
```

3、Calendar类

```java
Calendar rightNow = Calendar.getInstance(); 
int year = cal.get(Calendar.YEAR);
```

4、LocalDate、LocalTime、LocalDateTime类

```java
//创建对象,现在日期
LocalDate ld = LocalDate.now();
System.out.println(ld);
int year = ld.getYear();
//创建对象,现在时间
LocalTime lt = LocalTime.now();
System.out.println(lt);
//创建对象,现在时期
LocalDateTime ldt = LocalDateTime.now();
System.out.println(ldt);
```

5、Instant时间戳

```java
Instant i = Instant.now();
Date d = Date.from(i);
i=date.toInstant();
```

6、DateTimeFormatter类

```java
//日期的转换
LocalDateTime ldt = LocalDateTime.now();
System.out.println(ldt);
//解析器
DateTimeFormatter dtf = DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss");
String ldtStr = ldt.format(dtf);
System.out.println(ldtStr);

String ldtStr1 = dtf.format(ldt);
System.out.println(ldtStr1);

LocalDateTime ldt1 = LocalDateTime.parse("2019-11-11 11:11:11",dtf);
System.out.println(ldt1);
```

7、Duration/Period类，看日子间的时间差

```java
//时期
LocalDateTime today = LocalDateTime.now();
System.out.println(today);
LocalDateTime birthDate = LocalDateTime.of(1995, 1, 11,10,50,30);
System.out.println(birthDate);

Duration duration = Duration.between(birthDate, today);
System.out.println(duration.toDays());

//日期
LocalDate today = LocalDate.now();
System.out.println(today);
LocalDate birthDate = LocalDate.of(1995, 1, 11);
System.out.println(birthDate);
Period period = Period.between(birthDate, today);
System.out.println(period.getYears());
```

8、ChronoUnit类

```java
LocalDateTime today = LocalDateTime.now();
System.out.println(today);
LocalDateTime birthDate = LocalDateTime.of(1995, 1, 11,10,50,30);
System.out.println(birthDate);

System.out.println(ChronoUnit.YEARS.between(birthDate, today));
```

### 2、包装类

八种基本数据类型对应的引用类型

```
int --- Integer
char--- Character
//其他都是首字母大写
```

```java
int a = 22;
Integer a1 = 11;
Integer a2 = a;// 自动装箱
int a3 = a1;// 自动拆箱
String str;
str = a1.toString();
System.out.println(str);
```

### 3、正则表达式

正则表达式可以用一些规定的字符来制定规则，并用来校验数据格式的合法性。打开API文档搜索Pattern。

```java
public static void main(String[] args) {
    System.out.println(cheak("145224114"));
    System.out.println(cheak("d4"));
}
public static boolean cheak(String QQ){
	return QQ != null && QQ.matches("\\d{6,20}");
}
```

```java
replaceAll(String regex,String newStr);//按照正则表达式匹配的内容进行替换
split(String regex);//按照正则表达式匹配的内容进行分割字符串，返回一个字符串数组
```

### 4、Arrays类

```java
Arrays.toString(类型[] a);//对数组进行排序
sort(类型[] a);//对数组默认升序排序
sort(类型[] a,Comparator<?super T>);//使用比较器对象自定义排序
binarySearch(int[] a,int key);//二分搜索数组中的数据
```

比较器

```java
Integer[] ages1 = {34,12,42,23};
Arrays.sort(ages1, new Comparator<Integer>() {
    @Override
    public int compare(Integer o1,Integer o2){
    	//return o1-o2;升序
    	return o2-o1;//降序
	}
});
System.out.println(Arrays.toString(ages1));
```

### 5、常用算法

选择排序

二分查找

### 6、Lambda

作用:简化匿名内部类的代码写法。

```java
public class App {
    public static void main(String[] args) {
        // Swimming s1 = new Swimming() {
        //     @Override
        //     public void swim(){
        //         System.out.println("老师游泳很快");
        //     }
        // }; 
        Swimming s1 = ()->{
            System.out.println("老师游泳很快");
        };
        go(s1);  
    }
    public static void go(Swimming s){
        s.swim();
    }
    
}

@FunctionalInterface
interface Swimming{
    public void swim();
}
```

省略规则：

参数类型可以省略不写。

如果只有一个参数，参数类型可以省略，同时()也可以省略。

如果Lambda表达式的方法体代码只有一行代码。可以省略大括号不写,同时要省略分号!

如果Lambda表达式的方法体代码只有一行代码。可以省略大括号不写。此时，如果这行代码是return语句，必须省略return不写，同时

也必须省略";""不写

## 十二、集合

### 1、集合的特点

集合的大小不固定，启动后可以动态变化，类型也可以选择不固定。集合更像气球。集合非常适合做元素的增删操作。
注意:集合中只能存储引用类型数据，如果要存储基本类型数据可以选用包装类。

List系列集合:添加的元素是有序、可重复、有索引。

ArrayList、LinekdList∶有序、可重复，有索引。

Set系列集合:添加的元素是无序、不重复、无索引。

HashSet:无序、不重复、无索引;LinkedHashSet:有序、不重复、无索引。

```java
public class App {
    public static void main(String[] args) {
        Collection list1 = new ArrayList();
        list1.add("java");
        list1.add("java");
        list1.add("23");
        list1.add("23");
        list1.add("flase");
        list1.add("flase");
        System.out.println(list1);

        Collection list2 = new HashSet();
        list2.add("java");
        list2.add("java");
        list2.add("23");
        list2.add("23");
        list2.add("flase");
        list2.add("flase");
        System.out.println(list2);
    }
}
```

集合对于泛型的支持:

集合都是支持泛型的，可以在编译阶段约束集合只能操作某种数据类型

```java
 Collection<String> list3 = new ArrayList<>();
 list3.add("java");
 list3.add("java");
 list3.add(23);//报错
 list3.add("23");
 list3.add("flase");
 list3.add("flase");
 System.out.println(list3);
```

### 2、Collection的常用API

单列集合的祖宗接口

```java
public class App {
    public static void main(String[] args) {
        Collection<String> list = new ArrayList<>();
        list.add("23");//添加
        list.add("java");
        list.add("java");
        list.add("lqh");
        System.out.println(list.isEmpty()); //判断是否为空
        System.out.println(list.size());//判断集合大小
        System.out.println(list.contains("lqh"));//是否包含
        System.out.println(list.remove("java"));//删除元素
        
        Object[] arrs = list.toArray();//把集合中的元素，存储到数组中去
        System.out.println(Arrays.toString(arrs));
    }
}
```

### 3、Collection的遍历

方法一：迭代器

```java
public class App {
    public static void main(String[] args) {
        Collection<String> list = new ArrayList<>();
        list.add("23");//添加
        list.add("java");
        list.add("java");
        list.add("lqh");
        //1、得到当前集合的迭代器对象
        Iterator<String> it = list.iterator();
        // String ele = it.next();
        // System.out.println(ele);
        // System.out.println(it.next());
        // System.out.println(it.next());
        // System.out.println(it.next());
        while(it.hasNext()){
            System.out.println(it.next());
        }
    }
}
```

方法二：foreach/增强for循环

```java
public class App {
    public static void main(String[] args) {
        Collection<String> list = new ArrayList<>();
        list.add("23");//添加
        list.add("java");
        list.add("java");
        list.add("lqh");
        //增强for可遍历集合也可以遍历数组
        for (String ele : list) {
            System.out.println(ele);
        }
    }
}
```

方法三：lambda表达式

```java
public class App {
    public static void main(String[] args) {
        Collection<String> list = new ArrayList<>();
        list.add("23");//添加
        list.add("java");
        list.add("java");
        list.add("lqh");
        
        list.forEach(new Consumer<String>() {
            @Override
            public void accept(String s){
                System.out.println(s);
            }
        });

        list.forEach(s->System.out.println(s));
    }
}
```

### 4、常用数据结构

* 各种数据结构的特点和作用是什么样的
* 队列:先进先出，后进后出。
* 栈:后进先出，先进后出。
* 数组:内存连续区域，查询快，增删慢。
* 链表:元素是游离的，查询慢，首尾操作极快。
* 二叉树:永远只有一个根节点,每个结点不超过2个子节点的树。
* 查找二叉树:小的左边，大的右边，但是可能树很高，查询性能变差。
* 平衡查找二叉树:让树的高度差不大于1，增删改查都提高了。
* 红黑树（就是基于红黑规则实现了自平衡的排序二叉树)

### 5、List系列集合

#### 1、List集合基本操作

```
ArrayList al = new ArrayList();
add
remove
get
set
clear
```

#### 2、遍历方式

for循环

迭代器

增强for循环

lambda表达式

#### 3、ArrayList集合底层原理

ArrayList底层是基于数组实现的:根据索引定位元素快，增删需要做元素的移位操作。

第一次创建集合并添加第一个元素的时候，在底层创建一个默认长度为10的数组。

#### 4、Linklist独有功能

操作首尾

```
addFirst
addLast
removeFirst
removeLast
```

#### 5、特殊用法

并发修改异常

迭代器遍历删除的话要使用迭代器删除

for循环遍历删除

### 6、泛型

只支持引用数据类型

避免强类型转换

自定义泛型类,类似ArrayList

```
public class MyArrayList<T>{}
```

自定义泛型方法,可以接受一切数据类型

```
public <T> void show(T t){}
```

自定义泛型接口

```
public interface Data<E>{}
```

泛型通配符和上下限

？在使用泛型的时候可以代表一切类型

? extends Car: ?必须是Car或者其子类泛型上限? 

? super Car : ?必须是Car或者其父类泛型下限
