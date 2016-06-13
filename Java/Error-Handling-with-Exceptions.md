# Error-Handling-with-Exceptions

标签（空格分隔）： Java

---

- 发现错误的理想时机是在编译阶段，而编译期间不能找出所有错误，其余的问题必须在运行期间解决
- Java中的异常处理的目的在于通过使用少于目前数量的代码简化大型、可靠的程序的生成，让你的应用没有未处理的错误


## Concepts

- C的错误处理模式往往建立在约定俗成的基础之上；通常会返回某个特殊值或设置某个标识，接收者对其进行检查，以判断是否发生错误，程序员可以有选择的检查，导致健壮性降低
- 解决办法，也就是第一个好处是：用强制规定的形式消除错误处理中随心所欲的因素
- 另一个好处是降低错误代码的复杂度，不必在方法调用处进行检查，因为异常机制将保证能捕获这个错误，并且，只需在一个地方处理错误。这样不仅能够节省代码，而且分离开“在正常执行过程做什么”和“出了错误怎么办”的代码



## Basic exceptions

- 异常情形是指组织当前方法或作用于继续执行的问题，而普通问题是指，在当前环境下能够得到足够的信息，总能处理这个错误；而对于异常情形，就不能继续下去，因为在当前环境下无法获得必要的信息来解决问题，所能做的就是从当前环境跳出，把问题交给上一级环境，这就是抛出异常发生的事情
- 抛出异常后。首先，将使用new在堆上创建异常对象，然后，当前执行路径被终止，并且从当前环境中弹出对异常对象的引用。此时异常处理机制接管程序，并开始寻找一个适当的地方继续执行程序。这个适当的地方就是 异常处理程序，它的任务是将程序从错误状态中恢复，使程序要么换一种方式进行，要么继续运行下去
- 异常使我们可以将每一件事当作一个事务来考虑，而异常可以看护这些事务的底线，还可以将异常看作是内建的恢复系统，因为程序可以拥有不同的恢复到。
- 异常最重要的方面之一是如果发生问题，不允许程序沿着正常路径继续走下去，而C没有办法阻止。异常允许我们强制程序停止运行，并告诉我们出现了什么问题，或者强制程序处理问题，并返回到稳定状态


### Exception arguments

- 所有标准异常类都有两个构造器：默认构造器和接受字符串参数的构造器
- 使用 throw 后，在new创建异常对象后，此对象的引用将传给 throw，从效果上看，就像从方法返回，可以简单的看成是一种不同的返回机制；还能用抛出异常的方式从当前的作用于退出；这两种情况下，将会返回一个异常对象，然后退出方法或作用域
- 异常返回的地点和普通方法调用返回的地点完全不同；异常将在一个恰当的异常处理程序中得到解决，他的位置可能离异常被抛出的地方很远，也可能会跨越方法调用栈的许多层次
- 能够抛出任何类型的Throwable对象，它是异常类型的根类。通常对不同类型的错误，要抛出相应的异常。错误信息可以保存在异常对象内部或用异常类名称来暗示。上一层环境通过这些信息决定如何处理异常。通常异常对象仅有的信息就是异常类型，没有其他有意义的内容


## Catching an exception

要明白异常是如何被捕获，必须理解监控区域的概念，它是一段可能产生异常的代码，并且后面跟着处理这些异常的代码

### The try block

- 可以把所有检查错误的动作放在try块里，只需在一个地方就可以捕获所有异常，意味着代码更容易编写和阅读，没有把完成任务的代码与错误检查的代码混在一起
- 不支持异常处理的语言，要在每个方法调用前后加上设置和错误检查的代码


### Exception handlers

- 抛出的异常必须在某处得到处理，这个地点就是 异常处理程序；使用catch关键字
- 有时用不到标识符，因为异常的类型已经给了足够的信息，但标识符不可省略
- 异常处理程序必须紧跟在try之后。异常抛出后，异常处理机制将搜寻参数与异常类型相匹配的第一个处理程序，然后进入catch子句执行

### Termination vs. resumption

- 异常处理理论上有两种基本模型。Java支持终止模型，这种模型中，将假设错误非常关键，以至于程序无法返回到异常发生的地方继续执行，如果想要用Java实现类似恢复的功能，遇到错误时就不能抛出异常。

- 恢复模型是指异常处理程序的工作是修正错误，然后重新尝试调用出问题的方法，并认为第二次能成功。恢复模型通常希望异常被处理后能继续执行程序。Java可以调用方法修改该错误或者把try放到while循环中，这样就能不断进入try块，直到得到满意的结果

- 恢复性的处理程序需要了解异常抛出的地点，势必包含依赖于抛出位置的非通用性代码，增加代码编写和维护的困难


## Creating your own exceptions

可以自己定义异常类来表示程序中可能会遇到的特定问题

要自己定义异常类，必须从已有的异常类继承，最好是选择意思相近的异常类继承。建立新的异常类型最简单的方法就是让编译器为你产生默认构造器；编译器创建了默认构造器，它将自动调用基类的默认构造器。对异常来说，最重要的部分就是类名。

为异常类定义一个接受字符串参数的构造器


```java
class MyException extends Exception{

    public MyException{}
    public MyException(String msg){super(msg);}
}

```

在异常处理程序中，调用了在Throwable类声明的printStackTrace()方法，它将打印“从方法调用处知道抛出异常处”的方法调用序列，默认输出到标准错误流

### Exceptions and logging

使用java.util.logging工具将输出记录到日志中

```java
class LoggingException extends Exception{
    private static Logger logger = Logger.getLogger("LoggingException");

    public LoggingExcetpion(){
        StringWriter trace = new StringWriter();
        printStackTrace(new PrintWriter(trace));
        logger.server(trace.toString());
    }
}
```


为了产生日志记录消息，我们欲获取异常抛出处的栈轨迹，但printStackTrace()不会默认产生字符串。为了获取字符串，我们需要重载printStackTrace()方法，让它接受一个java.io.PrintWriter对象作为参数。如果我们将一个java.io.StringWriter对象传递给这个PrintWriter的构造器，那么通过调用toString()方法，就可以将输出抽取为一个String


更常见的是需要捕获和记录其他人编写的异常，必须在异常处理程序中生成日志消息

```java
class LoggingException2 extends Exception{
    private static Logger logger = Logger.getLogger("LoggingException2");

    static void logExcetpion(Exception e){
        StringWriter trace = new StringWriter();
        e.printStackTrace(new PrintWriter(trace));
        logger.server(trace.toString());
    }
}
```


可以进一步自定义异常，加入额外的构造器和成员，然后覆盖Throwable.getMessage()方法，以获取更详细的信息

但是使用程序包的客户端可能仅仅只是查看一下抛出的异常类型，其他的就不管了，所以对异常添加的其他功能可能根本用不上

## The exception specification 

Java鼓励把方法可能会抛出的异常告知使用此方法的客户端程序员。它使调用者能确切知道写什么样的代码可以捕获所有潜在的异常。如果知道源代码，则可以在源代码中查找throw语句来获知相关信息，而程序通常不和源代码一起发布。为了预防这个问题，Java提供了相应的语法，并强制使用了这种语法，告知客户端程序员可能会抛出的异常，然后客户端程序员就可以进行相应的处理，这就是**异常说明**。

异常说明使用了附加的关键字throws，后面接一个所有潜在的异常类型类别，可以定义如下

void f() throws TooBig,TooSmall,DivZero{//...

如果如下定义
void f() { //...

说明此方法不会抛出任何异常(**除了从RuntimeExcpetion继承的异常**，这种情况下可以在没有异常说明的情况下被抛出)

如果方法里的代码产生了异常而不进行处理，编译器会提醒：**要么在此方法中处理这个异常；要么在异常说明中表明此方法将产生异常，让调用者处理**

当然，可以声明方法将抛出异常，实际上却不抛出，编译器相信了这个声明，并强制此方法的用户像真正抛出异常那样调用这个方法。这样做的好处是：为异常占个位置，以后就可以抛出这种异常而不用修改已有的代码。在定义抽象基类和接口时很重要，这样派生类或接口实现就能抛出这些预先声明的异常.

这种在编译时被强制检查的异常称为**被检查异常**.

```java
public static void exce() throws MyException{

    //不用添加throw
}
```




## Catching any exception

可以只写一个异常处理程序捕获所有类型的异常。通过捕获异常类型的基类Exception就可以做到

```java
catch(Exception e){
   System.out.println("Catch any exceptions");
}
```
因为可以捕获所有异常，所以做好把它放到异常处理程序列表的末尾，以防它抢在其它处理程序之前先将异常捕获了。

Exception不会包含太多具体的信息，但可以调用它从基类Throwable继承的方法
```java
String getMessage()//获取异常详细信息
String getLocalizedMessage()
String toString()
void printStackTrace() //打印Throwable和Throwable的调用栈轨迹
void printStackTrace(PrintStream)
void printStackTrace(java.io.PrintWriter)
Throwable fillInStackTrace()//用于在Throwable对象的内部记录栈帧的当前状态
```

调用栈显示了“把你带到异常抛出点”的方法调用序列，第一个版本输出到标准错误，后两个版本允许选择输出的流


也可以使用Throwable从基类Object继承的方法，对于异常来说，getClass()也是个号方法，它将返回一个表示此对象类型的对象，然后可以使用getName()方法查询这个Class对象包含包信息的名称，或者使用只产生类名称的getSimpleName()方法

```java

public class ExceptionTest {

	public static void exce() throws MyException{

		throw new MyException("exception");
	}
	public static void main(String[] args) {
	
		try {
			exce();
		} catch (MyException e) {
		
			System.out.println(e.getMessage());
			System.out.println(e.getLocalizedMessage());
			System.out.println(e);
			e.printStackTrace();
		}
	
	}
	

}

class MyException extends Exception{

	private static final long serialVersionUID = 1L;
	
	public MyException(){
		
	}
	
	public MyException(String msg){
		super(msg);
	}
	
}

```

输出：（实际上，后面一个都是前面一个的超集）
```java
exception
exception
MyException: exception
MyException: exception
	at Test.exce(Test.java:7)
	at Test.main(Test.java:12)

```

### The stack trace

printStackTrace()方法所提供的信息可以通过getStackTrace()方法来直接访问，这个方法将返回一个由栈轨迹中的元素所构成的StackTraceElement数组，其中每一个元素都表示栈的一帧，元素0是栈顶元素，并且是调用寻列中的最后被调用的方法(这个Throwable被创建和抛出之处)。


同样使用上面的例子
```java
try {
	exce();
} catch (MyException e) {

	System.out.println((e.getStackTrace())[0]);
	e.printStackTrace();

	System.out.println((e.getStackTrace())[0].getMethodName());//只打印方法名
}
```
输出：

```java
ExceptionTest.exce(ExceptionTest.java:7)
MyException: exception
	at ExceptionTest.exce(ExceptionTest.java:7)
	at ExceptionTest.main(ExceptionTest.java:12)
exce

```

可以看到，第一个元素则是程序中`throw new MyException("exception");`代码所在位置，也就是印证了是Throwable被创建和抛出之处

(在调用函数时，会将经过的每一个方法入栈，所以会出现了main函数在栈底，而exec函数在栈顶的情况。)


### Rethrowing an exception

有时希望把刚捕获的异常重新抛出，尤其在使用Exception捕获所有异常的时候；既然已经得到了当前异常对象的引用，可以直接把它重新抛出

```java
catch(Exception e){

    throw e;
}
```

重新抛出异常会把异常抛给上一级环境的异常处理程序，同一个try块后面的catch子句将被忽略。此外，异常对象的所有信息都得以保持，所以高一级环境中捕获此异常的处理程序可以从这个异常对象中得到所有信息。


如果只是把当前异常对象抛出，那么**printStackTrace()方法显示的是异常抛出点的调用栈信息，而非重新抛出点的信息**。要更新这个信息，可以调用fillInStackTrace()方法，这个方法将返回一个Throwable对象，它是通过把当前调用栈信息填入原来那个异常对象而建立的

下面代码中,one方法有两种重新抛出的方式，第一种是常规的`throw e`，第二种是使用fillInStackTrace()方法。观察输出结果，可以看到throw e 的方式与不重新抛出时的结果是一样的(即和不用try捕获，直接在方法上加上`throws MyException`)。而**调用了fillInStackTrace()方法后，将当前one方法作为异常栈的栈顶**，而不是two方法。

```java
public class ExceptionFillInTest {

	public static void one() throws MyException {

		try {
			two();
		} catch (MyException e) {
//			throw e; //第一种
			throw (MyException)e.fillInStackTrace();//第二种
		}
	}
	
	public static void two() throws MyException{
		
		throw new MyException("exception");
	}
	public static void main(String[] args) {
	
		try {
			one();
		} catch (MyException e) {
			e.printStackTrace(System.out);
		}
	
	}
}

class MyException extends Exception{
	
	public MyException(){}
	public MyException(String msg){super(msg);}
}
```

输出：
```java
//第一种
MyException: exception
	at ExceptionFillInTest.two(ExceptionFillInTest.java:17)
	at ExceptionFillInTest.one(ExceptionFillInTest.java:8)
	at ExceptionFillInTest.main(ExceptionFillInTest.java:22)

//第二种
MyException: exception
	at ExceptionTest.one(ExceptionTest.java:11)
	at ExceptionTest.main(ExceptionTest.java:22)
```

**也可以在捕获异常后抛出另一种异常**。这么做的话，效果类似于使用fillInStackTrace()，因为**原来异常发生点的信息都消失**了，剩下的是与新的抛出点有关的信息。

### Exception chaining

如果我们希望在捕获异常之后抛出另一个异常，同时希望把原始的信息保存下来，被称为**异常链**；而上面的方法都做不到。

JDK1.4之前，必须自己编写代码保存原始异常信息。现在Throwable子类有一个构造器可以接受一个cause(因由)对象作为参数。这个cause用来表示原始异常，这样就完成把原始异常传递给新的异常；即使在当前位置创建并抛出了新的异常，也能通过这个异常链追踪到异常最初发生的地方

Throwable子类中，三个基本的异常类提供了带cause参数的构造器：分别是Error，Exception和RuntimeException。如果要把其他异常链接起来，应使用initCause()方法而不是构造器。(错误输出中可以看到有Caused by)

```java
public class ExceptionCauseTest {

	public static void one() throws MyException {

		try {
			two();
		} catch (MyException e) {
			e.initCause(new MyException2("exception2"));
			throw e;
		}
	}
	public static void two() throws MyException{
		
		throw new MyException("exception");
	}
	public static void main(String[] args) {
	
		try {
			one();
		} catch (MyException e) {
			e.printStackTrace(System.out);
		}
	}
}

class MyException extends Exception{

	public MyException(){}
	public MyException(String msg){super(msg);}
}

class MyException2 extends Exception{

	public MyException2(){}
	public MyException2(String msg){super(msg);}
}

```

输出：
```java
MyException: exception
	at ExceptionCauseTest.two(ExceptionCauseTest.java:17)
	at ExceptionCauseTest.one(ExceptionCauseTest.java:8)
	at ExceptionCauseTest.main(ExceptionCauseTest.java:22)
Caused by: MyException2: exception2
	at ExceptionCauseTest.one(ExceptionCauseTest.java:10)
	... 1 more

```


## Standard Java exceptions

Throwable被用来表示任何可以作为异常被抛出的类。Throwable对象的子类可以分为两种类型：Error用来表示编译时和系统错误；Exception是可以被抛出的基本类型

异常的基本概念是用名称代表发生的问题，并且异常的名称应该可以望文生义。异常并非全是在java.lang包中定义的;有些异常用来支持想util,net和io等程序包或者第三方包。

### Special case: RuntimeException

属于运行时异常的类型很多，它们会自动被虚拟机抛出，所以不必再异常说明中把它们列出来。

比如下面的例子是不需要的，错误会自动抛出，不用自行判断:
```java
if(t==null)
    throw new NullPointerException();
```

这些异常都是从RuntimeException类继承而来。

这构成一组具有相同特征和行为的异常类型。并且也**不需要**在异常说明中声明方法将抛出RuntimeException类型或其子类的异常。它们被称为"不受检查异常"。

尽管可以捕获RuntimeException异常，但通常不捕获；可以在代码中抛出RuntimeException类型的异常。如果不捕获，RuntimeException类型的异常也会穿过所有执行路径到达main方法，同时在程序退出前调用printStackTrace()方法。

只能在代码中忽略RuntimeException及其子类类型的异常，其它类型异常的处理都是编译器强制实施的。最根本的原因是**RuntimeException是编程错误**！


## Performing cleanup with finally

可能希望无论try块的异常是否抛出，某段代码都能得到执行。这通常适用于内存回收之外的情况(回收由垃圾收集器完成)。

当Java中的异常不允许我们回到异常抛出的地点，可以把try块放到循环中，建立一个“程序继续之前必须要到达”的条件。还可以加入一个static类型的计数器，使循环在放弃以前能尝试一定的次数。

### What's finally for?

对于没有垃圾回收和西沟函数自动调用机制的语言来说，finally非常重要。因为它保证了try里无论发生了什么，内存总是能得到释放。但Java有垃圾回收机制，内存释放不是问题，而且Java也没有析构函数可供调用。那什么时候使用finally？当要把除内存之外的资源恢复到它们的初始状态时，就要用到finally，这种资源包括：已经打开的文件或网络连接，在屏幕上画的图形等

在异常没有被当前异常处理程序捕获的情况下，异常处理机制也会在跳到更高一层的异常处理程序之前，执行finally语句。

```java
public class ExceptionFinallyTest {

	public static void one()  {
		try { 
			System.out.println("first try");
			try {
				System.out.println("second try");
				throw new MyException();
			} finally{
				System.out.println("second finally");//在被catch前执行了
			}
		} catch (MyException e) {
			System.out.println("first catch");
		}finally{
			System.out.println("first finally");
		}
	}
	public static void main(String[] args) {
		one();
	}
}
```


当涉及continue和break语句时，finally也会执行。

### Using finally during return

因为finally子句总会被执行，所以在一个方法中，可以从多个点返回，并且保证重要的清理工作仍然会执行


### Pitfall: the lost exception

异常作为程序出错的标识，绝不应该被忽略，但它还是有可能轻易被忽略。在某些特殊方式使用finally字节，就会发送这种情况


下面的例子，从输出可以看到，ImportantException不见了，它被finally子句中的OrdinaryException取代了。

```java
public class ExceptionLost {
	
	void important() throws ImportantException{
		
		throw new ImportantException();
	}
	void dispose() throws OrdinaryException{
		
		throw new OrdinaryException();
	}
	public static void main(String[] args) {
		try {
			ExceptionLost lost = new ExceptionLost();
			try {
				lost.important();
			} finally{
				lost.dispose();
			}
		} catch (Exception e) {
			System.out.println(e);
		}
	}
}
class ImportantException extends Exception{
	
	public String toString(){
		return "import exception";
	}
}
class OrdinaryException extends Exception{
	
	public String toString(){
		return "ordinary exception";
	}
}
//输出:
//ordinary exception
```


如果在finally中使用return也会导致异常丢失，即使抛出了异常，也不会产生任何输出。会发现编译器会警告，

```java
public class ExceptionLost2 {
	
	public static void main(String[] args) {
		try {
			throw new NullPointerException();
		} finally{
			return;
		}
	}
}
```


## Exception restrictions
当覆盖方法的时候，只能抛出在基类方法的异常说明里列出的那些异常。这个限制很有用，意味着当基类使用的代码应用到派生类对象时，一样能够工作。
















