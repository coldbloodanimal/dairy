类的实例化过程：父类的初始化->子类的初始化->父类的实例变量赋值和实例代码块->父类的构造函数->子类的实例变量和实例代码块->子类的构造函数

简单代码:


        public class StaticPure extends StaticFather{
            public static void main(String[] args) {
                StaticPure staticPure=new StaticPure();
                System.out.println(staticPure.staticFatherVar);
                System.out.println(staticPure.staticVar);
                System.out.println(staticPure.fatherVar);
                System.out.println(staticPure.var);
            }
            static String staticVar="子类静态变量立即赋值";
            String  var="子类实例变量立即赋值";
            static {
                System.out.println("子类静态代码块："+staticVar);
                staticVar="子类实例变量静态代码块赋值";
            }
            {
                System.out.println("子类实例代码块："+var);
                var="子类实例变量实例代码块赋值";
            }
            public StaticPure(){
                System.out.println("子类构造方法："+var);
                var="子类实例变量子类构造方法赋值";
            }
        }
        class StaticFather{
            static String staticFatherVar="父类静态变量立即赋值";
            String  fatherVar="父类实例变量立即赋值";
            static {
                System.out.println("父类静态代码块："+staticFatherVar);
                staticFatherVar="父类静态变量静态代码块赋值";
            }
            {
                System.out.println("父类实例代码块："+fatherVar);
                fatherVar="父类实例变量实例代码块赋值";
            }
            StaticFather(){
                System.out.println("父类构造方法："+fatherVar);
                fatherVar="父类实例变量父类构造方法赋值";
            }
        }
//执行结果
父类静态代码块：父类静态变量立即赋值
子类静态代码块：子类静态变量立即赋值
父类实例代码块：父类实例变量立即赋值
父类构造方法：父类实例变量实例代码块赋值
子类实例代码块：子类实例变量立即赋值
子类构造方法：子类实例变量实例代码块赋值
父类静态变量静态代码块赋值
子类实例变量静态代码块赋值
父类实例变量父类构造方法赋值
子类实例变量子类构造方法赋值

其它问题：
一.实例变量初始化顺序：
0.默认值（分配内存时）
1.直接赋值
2.代码块赋值
3.构造函数赋值
二、一些不太恰当的例子:
这个话题似乎可以用建造房子举例子，建造房子的图纸（父类的初始化），建造院子的图纸（子类的初始化，继承了房子），建造房子（父类的实例），建造院子（子类的实例）房子是院子的核心，院子还有花园。
房屋刚建设的时候材质本身就有属性，外墙是砖头的红色（默认值），后来被染成红色 （代码块或者构造方法赋值）。
从以上例子可以看到一些：
类本身也是对象，它本身也是有属性的（静态变量），比如图纸中房屋的长宽高比例，房子的颜色。房子按照图纸建造可以获得一样的值。
还看到了多态的一点点感觉。

参考: https://blog.csdn.net/justloveyou_/article/details/72466416###


