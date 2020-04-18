类的实例化过程：父类初始化 -> 子类初始化-> 父类的成员变量和实例代码块 -> 父类的构造函数 -> 子类的成员变量和实例代码块 -> 子类的构造函数

简单代码:


    public class StaticPure extends StaticFather{
        static String staticVar="子类静态变量立即赋值";
        String  var="子类实例变量立即赋值";
        static {
            System.out.println("子类静态代码块："+staticVar);
        }
        {
            System.out.println("实例代码块："+staticVar);
            var="子类实例变量实例代码块赋值";
        }
        public StaticPure(){
            var="构造方法子类实例变量赋值";
            System.out.println("子类初始化方法:"+var);
        }
        public static void main(String[] args) {
            StaticPure staticPure=new StaticPure();
            System.out.println(staticPure.var);
        }
    }
    class StaticFather{
        static String staticFatherVar="父类静态变量立即赋值";
        String  fatherVar="父类实例变量立即赋值";
        static {
            System.out.println("父类静态代码块："+staticFatherVar);
        }
        {
            System.out.println("父类实例代码块："+fatherVar);
        }
    }



--看到这里就好了



后面的未完待续…………
为了方便理解，尝试举一个建造大楼的例子。
普遍情况下如何建造一个大楼。
1.你需要设计图稿
2.你首先需要一块地皮
3.每个


我们把创建对象比作造房子
首先我们要给你分配一块儿土地（分配内存），
1. 为对象分配内存空间
2. 调用构造器

初始化和创建概念上是两件事情


创建一个对象的，初始化它的成员变量，自动初始化会给每个

初始化：
对象初始化，有一个自动初始化的过程，比如你给成员变量赋值，它会先被赋予默认值，然后再赋予你给定的值。

成员变量初始化：如果是及基本类型，它们会有一个初始值，如果是对象，他们会有一个特殊值null.
构造器初始化：


分配内存的同时，这些实例变量也会被赋予默认值
实例变量初始化、实例代码块初始化 以及 构造函数初始化
实例变量初始化顺序：
0.默认值（分配内存时）
1.直接赋值
2.代码块赋值
3.构造函数赋值
构造函数初始化：
**Java要求在实例化类之前，必须先实例化其超类，以保证所创建实例的完整性**超类的构造函数如果你没有调用，编译器会给你生成

参考: https://blog.csdn.net/justloveyou_/article/details/72466416###


