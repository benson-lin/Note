# 浮点数运算结果不精确，以及用String来构造BigDecimal进行浮点数精确计算 

## 1.浮点数运算结果不精确
 先看如下代码

```java
1         System.out.println(1.0 - 0.8);
2         System.out.println(0.2 + 0.1);
3         System.out.println(1.0 - 0.8 == 0.2); //false
4         System.out.println(0.2 + 0.1 == 0.3); //false
```
输出结果为：

```java
0.19999999999999996
0.30000000000000004
false
false
```
发现不是我们想要的正确结果，出现了1.0-0.8不等于0.2，0.2+0.1也不等于0.3的现象。

这是因为将java将double浮点数转换为64位二进制性，出现了精度损失。这跟IEEE754标准有关。可参考

https://www.iovi.com/post/2014-07-07-talk-about-double-in-java.html

## 2.用String来构造BigDecimal做浮点数的精确计算

```java
1         //BigDecimal构造方法参数传入String，精准
2         BigDecimal b2 = new BigDecimal("1.0").subtract(new BigDecimal("0.8"));
3         System.out.println(b2);
4         System.out.println(new BigDecimal("0.1").add(new BigDecimal("0.1")).add(new BigDecimal("0.1")));
5         System.out.println(b2.equals(new BigDecimal(0.2))); //false
6         System.out.println(b2.equals(new BigDecimal("0.2"))); //true
7         
8         BigDecimal t2 = new BigDecimal("0.1");
9         System.out.println(t2);
```
输出结果为：

```java
0.2
0.3
false
true
0.1
```
**注意:只有用new BigDecimal(String)的构造方法，得到的才是浮点数的精确值。**

以上第5行代码，用0.2这个double类型构造的BigDecimal已经丢失了精度，所以equals结果为false。

用new BigDecimal(Double)的构造方法，构造的BigDecimal不精确。如下：

```java
1         //BigDecimal构造方法参数传入double类型，不精准
2         BigDecimal b = new BigDecimal(1.0).subtract(new BigDecimal(0.8));
3         System.out.println(b);
4         System.out.println(new BigDecimal(0.1).add(new BigDecimal(0.1)).add(new BigDecimal(0.1)));
5         
6         BigDecimal t = new BigDecimal(0.1);
7         System.out.println(t);
输出结果为：

0.1999999999999999555910790149937383830547332763671875
0.3000000000000000166533453693773481063544750213623046875
0.1000000000000000055511151231257827021181583404541015625
```