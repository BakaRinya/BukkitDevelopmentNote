# 定义变量的新方法与基本的运算  
```  
// Demo3.java 文件  
public class Demo3{  
   public static void main(String[] args){  
      int x=666, y=777;  
      System.out.println(x+y);  
   }  
}  
```  
上面的程序运行后在控制台输出信息如下：  
```
1443
```
上述程序第三行使用了一种常用的定义变量方式, 这种方式看上去很骚, 并且能同时定义两个int类型的变量, 一个是初始值为666的x, 一个是初始值为777的y.   
但是如果要使用这种方式定义变量, 请警惕下面这种情况：  
```
int x, y = 666;
```
上面的程序中, 我们定义了两个int类型的变量, 一个是x一个是y. 但是它们的初始值呢？x与y都是666吗？错, x没有初始值, 而y的初始值是666.   
上面我们叙述了加法, 同理, 还有减法、乘法和除法.   
可能你发现了, 键盘上并没有一个符号是 “×”, 也没有一个符号是“÷”. 想必你已经猜出来了, “*”是乘号, “/”是除号.   

# 数字型的乘除运算  
```
// Demo4.java 文件
public class Demo4{
   public static void main(String[] args){
      int x=8, y=5;
      System.out.println(x/y);
   }
}
```
我们编写了这样的一个程序. 现在有两个数字, 一个是8, 一个是5, 相比程序运行结果应该是1.6, 而实际情况却与我们想象的结果不一样, 控制台中输出了这样的信息： 
``` 
1
```
而我们都知道, 8除以5应当为1.6, 这是怎么回事呢？让我们试试看下面的这个程序：  
```
// Demo5.java 文件
public class Demo5{
   public static void main(String[] args){
      System.out.println(8.0/5.0);
   }
}
```

程序运行后, 控制台输出信息如下:  
```
1.6
```  
那我们将“8.0”与“5.0”改为“8”与“5”呢？结果输出了“1”. 这证明, 小数与小数之间的加减乘除与整数与整数之间的加减乘除并不一样.  现在, 我们将不再称呼小数为“小数”, 而要称呼得更加严谨, 即“浮点数”. 整数与整数相除被称作整数除法, 浮点数与浮点数相除被称作浮点除法.   
在小学的数学学习中, 我们知道, 被除数与除数相除, 会得到商和余数. 在上面的整数除法中, 8是被除数, 5是除数, 得到的数值1为其商, 这说明, 整数除法计算的结果是商, 而正如我们所见的一样, 浮点数除法得到的是一个确实的浮点数.   
```
// Demo6.java 文件 
public class Demo6{
   public static void main(String[] args){
      System.out.println(7.0/3.0);
   }
}
``` 

看上面的这个程序. 在上面的程序中, 我们进行了浮点数相除. 而我们得到的结果并不是无限个3, 而是有限个3. 这说明浮点数除法通常会丢失数据, 我们应当尽可能避免浮点数除法, 或使用更好地方案来计算浮点数相除.   
```
// Demo7.java 文件
public class Demo7{
   public static void main(String[] args){
      System.out.println(8.1/3);
   }
}
```  

在上面的程序中, 我们可以发现, 有一个数是8.1, 有一个数是3, 一个是整数, 一个是浮点数, 那这是什么神奇的操作呢？其实, 在计算中, 3会被转换为一个浮点数, 所以这个计算仍然是一个浮点数除法计算, 同理, 2.5+3得到的是5.5, 得到的数字是浮点数, 减法与乘法与此相同. 程序最终在控制台输出内容如下:   
```
2.7
```  
byte、short、int和long均为整数类型, 区别在于他们能存储的数字大小不一样, byte<short<int<long, 但不能将所有的整数都用long, 我们常用int, 滥用long会无意义的浪费内存空间.   
double是一种浮点数类型.   
事实上, 在Demo7.java文件中的代码里, 8.1就是一个double类型的数据, 3是一个int类型的数据, 在计算过程中会被转换为double类型的3.0. Demo5中的8.0和5.0都是double类型的数据, Demo6中的7.0和3.0都是double类型的数据. 这也就是说：  
```
int x=6; double y=2.0;
System.out.println(x/y);
```
在上面的代码里, 我们定义了int类型的x与double类型的y, 相除过程中将会把x变量中的6转换为double类型的6.0, 6.0与2.0相除得到3.0, 所以程序在控制台中输出内容如下:  
```  
3.0
```  
浮点数与整数相除, 浮点数永远不会被转换为整数, 永远是整数被转换为浮点数.   

# 取余运算  
刚才我们了解整数除法的时候, 知道了“/”可以取商, 但是怎么取余数呢？用“%”符号, 没错, 就是这个百分号. 就像这样  
```
System.out.println( 8%5 );
```
在控制台中输出结果如下:  
```
3
```
这是因为 8÷5=1 …… 3. 一个常用用法是, 利用取余来获得一个数字的个位数字, 例如, 26的个位数字是 26%10, 得到6. 