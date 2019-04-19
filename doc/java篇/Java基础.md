#### 一、 面向对象和面向过程
* 区别：面向过程一般是针对比较简单的事物，可以用线性的思维去解决，自顶向下，逐步细化，面向对象一般是针对比较复杂的事物
* 共同点：两者都是解决问题的一种方式，相辅相成，在解决问题中两者都有，宏观物体可以用面向对象，具体的事物使用面向过程方法解决

#### 二、 面向对象三大特征（继承、分装、多态）
 1. 继承
 对某一类具有统一特征的抽象，实现更好的建模，提高代码的复用性，继承是从已有的类中派生出新的类，新的类能吸收已有类的数据属性和行为，并能扩展新的能力。
 2. 分装
 “高内聚、低耦合”
隐藏了内部的复杂性，对外开放简单的接口调用，以此来提高系统的可维护，可扩展
3. 多态
程序的最后的结果在执行的时候决定，并不是编译的时候决定,方法的多态，属性没有多态
三个必要的条件：
    * 继承
    * 重写
    * 父类引用指向子类
    
   两种类型：
   * 编译时：针对父类，由声明的对象决定
   * 运行时：具体的由子类决定
   
#### 三、 String、StringBuffer和StringBuilder区别
* String是常量，源码中这样解释“Strings are constant; their values cannot be changed after they are created”，也就是说创建后不能改变值，每次修改String对象的时候其实是创建了一个新的对象，然后指向它，试想一下这个多个String对象存在内存中，GC会干嘛，自然回收，效率会很低，看如下代码
~~~
 String str = "abc";
        for (int i = 0; i < 100; i++) {
            str += i;
        }
~~~
这样会创建多个对象，最后指向最后一个对象，别的依然占内存
*  StringBuffer：方法用synchronized修饰，线程安全的，每次结果都会对 StringBuffer 对象本身进行操作，而不是生成新的对象，再改变对象引用。所以在一般情况下我们推荐使用 StringBuffer ，特别是字符串对象经常改变的情况下
* StringBuilder：线程不安全，一个可变的字符序列是5.0新增的。此类提供一个与 StringBuffer 兼容的 API，但不保证同步。该类被设计用作 StringBuffer 的一个简易替换，用在字符串缓冲区被单个线程使用的时候（这种情况很普遍）。如果可能，建议优先采用该类，因为在大多数实现中，它比 StringBuffer 要快。两者的方法基本相同。


[详细博客->戳我](https://www.cnblogs.com/goody9807/p/6516374.html)
#### 四、equales和hashcode
* 两个内容相同的对象，hashcode相等，但是hashcode 相等并一定，hashcode确定了存放的地址， 里面的对象并不一定相同
Object类中默认的实现方式是  :   return this == obj  。也就是说只有this 和 obj引用同一个对象，才会返回true。
JDK默认实现：
~~~
  public boolean equals(Object obj) {
        return (this == obj);
    }
~~~
重写equals方法
~~~
public class Student {
    private int id;
    private String name;

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;

        Student student = (Student) o;

        if (id != student.id) return false;
        return name.equals(student.name);
    } 
}
~~~
* 最终的hash是个int值，而不能溢出。不同的对象的hash码应该尽量不同，避免hash冲突
重写hashcode方法：
~~~
 @Override
    public int hashCode() {
        int result = id;
        result = 31 * result + name.hashCode();
        return result;
    }
~~~
[详细博客->戳我](https://www.cnblogs.com/lulipro/p/5628750.html)
#### 五、equals与==的区别
* ==常用于比较原生类型，而equals()方法用于检查对象的相等性。
* 另一个不同的点是：如果==和equals()用于比较对象，当两个引用地址相同，==返回true。而equals()可以返回true或者false主要取决于重写实现。最常见的一个例子，字符串的比较，不同情况==和equals()返回不同的结果

字符串字符串的==和equals比较：
equals,如果两个字符串对象包含有相同的内容返回true，但是==只有他们的引用地址相同时才返回true
~~~
String str1 = new String("qwe");
        String str2 = new String("qwe");
        String str3 = "qwe";
        String str4 = "qwe";

        System.out.println(str1.equals(str2));
        System.out.println(str1 == str2);
        System.out.println(str3.equals(str4));
        System.out.println(str3 == str4);
~~~
输出：
~~~
true
false
true
true
~~~

[详细博客->戳我](http://www.importnew.com/6804.html)

#### 六、重载和重写区别
其实个人认为这两者没有什么关系，非要说那就是有个“重”字，手动滑稽
* 重载（Overload）有相同的函数名称，但是参数名、类型不能相同
* 重写(Override)在子类继承父类的时候子类中可以定义某方法与其父类有相同的名称和参数，当子类在调用这一函数时自动调用子类的方法，而父类相当于被覆盖（重写）了
#### 七、接口和抽象类的区别
1. 抽象类可以有构造方法，接口中不能有构造方法。

2. 抽象类中可以有普通成员变量，接口中没有普通成员变量

3. 抽象类中可以包含非抽象的普通方法，接口中的所有方法必须都是抽象的，不能有非抽象的普通方法。

4. 抽象类中的抽象方法的访问类型可以是public，protected，但接口中的抽象方法只能是public类型的，并且默认即为public abstract类型。

5. 抽象类中可以包含静态方法，接口中不能包含静态方法

6. 抽象类和接口中都可以包含静态成员变量，抽象类中的静态成员变量的访问类型可以任意，但接口中定义的变量只能是public static final类型，并且默认即为public static final类型。

7. 一个类可以实现多个接口，但只能继承一个抽象类。
#### 八、ArrayList、LinkedList、Vector的底层实现和区别
* ArrayList底层实现是数组，线程不安全的，查询快，修改，删除慢
* LinkedList底层实现是链表，线程不安全的，查询慢，删除快
* Vector线程安全的，多线程中使用
  
