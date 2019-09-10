博名: 乔巴的博客   
日期:   2019-09-10  
标题: Java中常见的Exception和Error    
写在前面：本文有助于开发和测试在查看日志时给问题定位或定向，希望能帮到各位更快找到问题。  
**一、Exception**     
&emsp;&emsp;在Java开发中，我们难免会遇到各种各样的异常（Exception），继承于Throwable的Exception作为所有异常的父类，在jdk中（包java.lang下）有两个非常重要的子类，分别是流异常IOException和运行异常RuntimeException。   
    Exception类有5个构造方法，包括  
Exception() 、  
Exception(String message)、  
Exception(String message, Throwable cause)、  
Exception(Throwable cause)、  
Exception(String message, Throwable cause, boolean enableSuppression, boolean writableStackTrace)，  
其实所有的构造方法都是通过调用父类相应方法实现的。  
    RuntimeException类及其子类在我们开发中最常见也是可以通过代码规避的，而Throwable的另一个子类Error就无法通过代码处理了。如果我们不希望代码报错可以通过try-catch保护起来。  
下面是一些常见的运行异常。  
1.NullPointerException：空指针异常不用多说了，没有踩过这个坑的程序员基本不存在。往往发生的情况是一个为null的对象作为另一个方法的入参导致，编程人员需要时刻提醒自己考虑该情况做相应check。  
2.IllegalArgumentException：非法参数异常，说明传入的参数违反了一个方法要求的某些特性。比如SimpleDateFormat创建时y、m、d都有特殊意义，如果传入一个r就会报这个错误。  
3.ArrayIndexOutOfBoundsException：数组下标越界,IndexOutOfBoundsException的子类，这个也是很常见的异常，特别是在循环处理一个List的时候，粗心的同学可能会把下标值赋值成了List的长度，此时就会出现该异常。  
4.ArithmeticException：数学计算异常，这也是我最近第一次遇到的异常，在做除法的时候没有判断分母为0的情况。  
5.ClassNotFoundException：找不到指定的类，ReflectiveOperationException的子类（ReflectiveOperationException类直接继承于Exception类），一般情况java编译时会帮忙避免该问题，但利用到java的反射机制时就不会及时发现了，比如在使用MyBatis的时候，mapper映射文件路径写错等。  
6.FileNotFoundException：与上面的类似，找不到指定的文件，继承于IOException，出现时需要检查文件路径是否正确。  
7.NumberFormatException：字符串转换为数字异常，IllegalArgumentException的子类，这也是我最近遇到的异常，字符串出现了非数字，这时用Integer的parseInt方法会出现该异常，测试同学截图给我一看日志中打印了for input string: "授权商户不一致"，这就表示字符串"授权商户不一致"无法转为数字，该字符串是第三方的返回值，原因就是对异常情况欠考虑。  
8.NegativeArraySizeException：Negative是负数的意思，如果试图创建大小为负数的数组会出现该异常（比如int[] i = new int[-1]）。  
9.NoSuchMethodException：未找到指定的方法异常，同5，ReflectiveOperationException的子类，一般会出现在反射机制运用中。  
10.IllegalAccessException：没有访问权限，ReflectiveOperationException的子类，比如要调用一个没有访问权限的类时会出现该异常，本人最近通过反射调用一个类的变量时出现过该异常，然后通过调用setAccessible(true)方法纠正了问题。  
11.SQLException：数据库异常，直接继承于Exception类，常见的有无效列名、表或视图不存在、缺少表达式、不能插入空值、sql命令未正确结束等。请检查sql。  
12.ClassCastException：类型强制转换异常，比如Json字符串转为相应的类时粗心工具类中少了个字段就会出现该异常。  
13.UnsupportedOperationException：不支持的方法异常，笔者暂时没有遇到过。      
**二、Error**   
    Exception的兄弟类Error同样有5个构造函数，包括  
Error()、  
Error(String message)、  
Error(String message, Throwable cause)、  
Error(Throwable cause)、  
Error(String message, Throwable cause, boolean enableSuppression, boolean writableStackTrace)，  
同样所有的构造方法都是通过调用父类相应方法实现的。  
下面是常见的错误：   
1.AssertionError 断言错误，用来指示一个断言失败的情况,比如调用了一个未经初始化的对象或者不存在的对象。   
2.ClassCircularityError 类循环依赖错误，LinkageError的子类。在初始化一个类时，若检测到类之间循环依赖则抛出该异常，一般是类与类之间的继承关系不对导致的错误。   
3.ClassFormatError 类格式错误，LinkageError的子类。当Java虚拟机试图从一个文件中读取Java类，而检测到该文件的内容不符合类的有效格式时抛出，往往由于jdk版本不匹配导致。   
4.ExceptionInInitializerError 初始化程序错误，LinkageError的子类。当执行一个类的静态初始化程序的过程中，发生了异常时抛出。静态初始化程序是指直接包含于类中的static语句段，请检查自己使用的静态变量是否已初始化，往往由于初始化顺序不对导致。    
5.IllegalAccessError 违法访问错误，LinkageError的子类IncompatibleClassChangeError的子类。当一个应用试图访问、修改某个类的域（Field）或者调用其方法，但是又违反域或方法的可见性声明，则抛出错误，可以通过调用setAccessible(true)方法纠正错误。  
6.IncompatibleClassChangeError 不兼容的类变化错误，LinkageError的子类。当正在执行的方法所依赖的类定义发生了不兼容的改变时，抛出该异常。一般在修改了应用中的某些类的声明定义而没有对整个应用重新编译而直接运行的情况下，容易引发该错误，也可能是两个依赖包的版本号不兼容导致的错误。   
7.InstantiationError 实例化错误， LinkageError的子类IncompatibleClassChangeError的子类。当一个应用试图通过Java的new操作符构造一个抽象类或者接口时抛出该异常。   
8.InternalError 内部错误，VirtualMachineError的子类。用于指示Java虚拟机发生了内部错误。   
9.LinkageError 链接错误。该错误及其所有子类指示某个类依赖于另外一些类，在该类编译之后，被依赖的类改变了其类定义而没有重新编译所有的类，进而引发错误的情况。  
10.NoClassDefFoundError 未找到类定义错误，LinkageError的子类。当Java虚拟机或者类装载器试图实例化某个类，而找不到该类的定义时抛出该错误。  
11.NoSuchFieldError 域不存在错误， LinkageError的子类IncompatibleClassChangeError的子类。当应用试图访问或者修改某类的某个域，而该类的定义中没有该域的定义时抛出该错误。  
 12.NoSuchMethodError 方法不存在错误， LinkageError的子类IncompatibleClassChangeError的子类。当应用试图调用某类的某个方法，而该类的定义中没有该方法的定义时抛出该错误。  
13.OutOfMemoryError 内存不足错误，VirtualMachineError的子类。当可用内存不足以让Java虚拟机分配给一个对象时抛出该错误，这也是我刚入职时最常见的错误，可以通过配置虚拟机内存避免。  
14.StackOverflowError 堆栈溢出错误，VirtualMachineError的子类。当一个应用递归调用的层次太深而导致堆栈溢出时抛出该错误。   
15.ThreadDeath 线程结束。当调用Thread类的stop方法时抛出该错误，用于指示线程结束。   
16.UnknownError 未知错误，VirtualMachineError的子类。用于指示Java虚拟机发生了未知严重错误的情况。   
17.UnsatisfiedLinkError 未满足的链接错误，LinkageError的子类。当Java虚拟机未找到某个类的声明为native方法的本机语言定义时抛出。  
18.UnsupportedClassVersionError 不支持的类版本错误，LinkageError的子类ClassFormatError的子类。当Java虚拟机试图从读取某个类文件，但是发现该文件的主、次版本号不被当前Java虚拟机支持的时候，抛出该错误。  
19.VerifyError 验证错误，LinkageError的子类。当验证器检测到某个类文件中存在内部不兼容或者安全问题时抛出该错误。  
20.VirtualMachineError 虚拟机错误。用于指示虚拟机被破坏或者继续执行操作所需的资源不足的情况。  
