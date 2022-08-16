# 1 bug类型

## 1.1 “.equals()” should not be used to test the values of “Atomic” classes.

bug 主要
不要使用equals方法对AtomicXXX进行是否相等的判断
Atomic变量永远只会和自身相等，Atomic变量没有覆写equals()方法.

## 1.2 “=+” should not be used instead of “+=”

bug 主要
“=+” 与 “=+” 意义不同
a =+ b;虽然正确但写法不合规，应写成 a = +b;

## 1.3 “@NonNull” values should not be set to null

bug 次要
标注非空假定非空且在使用之前不进行非空检查，设置为空会导致空指针异常

## 1.4 “BigDecimal(double)” should not be used

bug 主要
构造函数不应用于实例化“String”和原始包装类，因为浮点的不精确,可能使用BigDecimal(double)得不到期望的值。用BigDecimal.valueOf替换

## 1.5 “compareTo” results should not be checked for specific values

bug 次要
compareTo可能返回不是具体的值（除0外），建议用 >0、<0、=0

## 1.6 “compareTo” should not return “Integer.MIN_VALUE”

bug 次要
compareTo只代表一个不等标识，不代表不等的程度，应返回-1,0,1标识即可

## 1.7 “Double.longBitsToDouble” should not be used for “int”

bug 主要
Double.longBitsToDouble返回给定的位所代表的double值,需要一个64位的long类型参数.

## 1.8 “equals” method overrides should accept “Object” parameters

bug 主要
equals作为方法名应该仅用于重写Object.equals(Object)来避免混乱.

## 1.9 “equals(Object obj)” should test argument type

bug 次要
要比较obj的class type是否一样

## 1.10 “equals” methods should be symmetric and work for subclasses

bug 次要
equals应是对等并且在有子类参与时能正常工作

## 1.11 “equals(Object obj)” and “hashCode()” should be overridden in pairs

bug 次要
Equals和hashCode必须成对重写，只重写任何一个都是错误的。

## 1.12 “Externalizable” classes should have no-arguments constructors

bug 主要
Externalizable(可序列化与返序列化)类应该有无参构造器

## 1.13 “getClass” should not be used for synchronization

bug 主要
{synchronized (this.getClass())} 错误 子类继承此方法时不能做到同步
{synchronized (MyClass.class)} 正确

## 1.14 “hashCode” and “toString” should not be called on array instances

bug 主要
使用Arrays.toString(args)和Arrays.hashCode(args)代替.

## 1.15 “instanceof” operators that always return “true” or “false” should be removed

bug 主要

## 1.16 “InterruptedException” should not be ignored

bug 主要
中断异常不应该被忽略，必须处理
try {
while (true) {
// do stuff
}
}catch (InterruptedException e) {
LOGGER.log(Level.WARN, “Interrupted!”, e);
// Restore interrupted state…
Thread.currentThread().interrupt();
}

## 1.17 “Iterator.hasNext()” should not call “Iterator.next()”

bug 主要

## 1.18 “Iterator.next()” methods should throw “NoSuchElementException”

bug 次要
public String next(){
if(!hasNext()){
throw new NoSuchElementException();
}
…
}

## 1.19 “notifyAll” should be used

bug 主要
notify可能不能唤醒正确的线程，notifyAll代之。

## 1.20 “null” should not be used with “Optional”

bug 主要
把判空包装起来使用而不直接使用!=null

## 1.21 “PreparedStatement” and “ResultSet” methods should be called with valid indices

bug 阻断
PreparedStatement与ResultSet参数设置与获取数据由序号1开始而非0

## 1.22 “read” and “readLine” return values should be used

bug 主要
BufferedReader.readLine(), Reader.read()及子类中的相关方法都应该先存储再比较
buffReader = new BufferedReader(new FileReader(fileName));
String line = null;
while ((line = buffReader.readLine()) != null) {
// …
}

## 1.23 “runFinalizersOnExit” should not be called

bug 严重
JVM退出时不可能运行finalizers,System.runFinalizersOnExit 和 Runtime.runFinalizersOnExit可以在jvm退出时运行但是因为他们不安全而弃用.
正确用法:
Runtime.addShutdownHook(new Runnable() {
public void run(){
doSomething();
}
});

## 1.24 “ScheduledThreadPoolExecutor” should not have 0 core threads

bug 严重
java.util.concurrent.ScheduledThreadPoolExecutor由属性corePoolSize指定线程池大小，如果设置为0表示线程执行器无线程可用且不做任何事.

## 1.25 “Serializable” inner classes of non-serializable classes should be “static”

bug 次要
序列化非静态内部类将导致尝试序列化外部类,如果外部类不是序列化类,会产生运行时异常，内部类静态化会避免这种情况

## 1.26 “SingleConnectionFactory” instances should be set to “reconnectOnException”

bug 主要
使用Spring SingleConnectionFactory而不启用reconnectOnException设置当连接恶化将阻止自动连接恢复。

## 1.27 “StringBuilder” and “StringBuffer” should not be instantiated with a character

bug 主要
StringBuffer foo = new StringBuffer(‘x’); 错 equivalent to StringBuffer foo = new StringBuffer(120);
StringBuffer foo = new StringBuffer(“x”); 对

## 1.28 “super.finalize()” should be called at the end of “Object.finalize()” implementations

bug 严重
protected void finalize() { releaseSomeResources(); super.finalize(); //调用，最后调用
}

## 1.29 “toArray” should be passed an array of the proper type

bug 次要
toArray()无参且强制类型转换会产生运行时异常,应传入一个合适的类弄作参数
public String [] getStringArray(List strings) {return strings.toArray(new String[0]);
}

## 1.30 “toString()” and “clone()” methods should not return null

bug 主要
可返回""

## 1.31 “wait” should not be called when multiple locks are held

bug 阻断

## 1.32 “wait”, “notify” and “notifyAll” should only be called when a lock is obviously held on an object

bug 主要
先要获得对象锁才能进行上述操作
private void removeElement() { synchronized(obj) { while (!suitableCondition()){ obj.wait(); } … // Perform removal }
}
or
private synchronized void removeElement() { while (!suitableCondition()){ wait(); } … // Perform removal
}

## 1.33 “wait(…)” should be used instead of “Thread.sleep(…)” when a lock is held

bug 阻断
当持有锁的当前线程调用Thread.sleep(…)可能导致性能和扩展性问题，甚至死锁因为持有锁的当前线程已冻结.合适的做法是锁对象wait()释放锁让其它线程进来运行.

## 1.34 A “for” loop update clause should move the counter in the right direction

bug 主要
检查for循环下标递增或递减正确

## 1.35 All branches in a conditional structure should not have exactly the same implementation

bug 主要
分支中不应该有相同的实现

## 1.36 Blocks should be synchronized on “private final” fields or parameters

bug 主要
synchronized同步块应该锁在private final fields或parameters对象上,因为同步块内非final锁对象可能改变导致其它线程进来运行.

## 1.37 Boxing and unboxing should not be immediately reversed

bug 次要
自动拆箱和装箱不需手动转换

## 1.38 Child class methods named for parent class methods should be overrides

bug 主要
以下情况不是重写:
a、父类方法是static的而子类方法不是static的
b、子类方法的参数或返回值与父类方法不是同一个包
c、父类方法是private
为了不产生混乱，不要与父类方法同名

## 1.39 Classes extending java.lang.Thread should override the “run” method

bug 主要
线程类应该重写run方法

## 1.40 Classes should not be compared by name

bug 主要
不要用类名称比较类是否相同，而用instanceof或者Class.isAssignableFrom()进行底动类型比较

## 1.41 Classes that don’t define “hashCode()” should not be used in hashes

bug 主要
没有定义hashCode()方法的类不能作为hash集合中的键值，因为equal相同的实例对像可能返回不同的hash值.

## 1.42 Collections should not be passed as arguments to their own methods

bug 主要
集合不应该作为参数传递给它们自己的方法

## 1.43 Conditionally executed blocks should be reachable

bug 主要
条件执行块应该可达

## 1.44 Constructor injection should be used instead of field injection

bug 主要
构造器注入应该替代属性注入(非Spring framework)
因为任何非Spring framework实例化而是通过构造器实例化的实例不能注入属性,这样公有的构造器实化化后可能产生NullPointerException，除非所有的构造器都是私有的

## 1.45 Consumed Stream pipelines should not be reused

bug 主要
流不应该重用

## 1.46 Custom resources should be closed

bug 阻断
打开的资源应该关闭并且放到finally块中进行关闭,jdk1.7后对含有资源对象必须使用try with resource，禁止使用
Try catch finally

## 1.47 Custom serialization method signatures should meet requirements

bug 主要
自定义类序列化方法签名应该合法

## 1.48 Dependencies should not have “system” scope

bug 严重
maven依赖不要在system scope

## 1.49 Dissimilar primitive wrappers should not be used with the ternary operator without explicit casting

bug 主要
不同的原始包装类如果没有明确的类转换不能用于三元操作中

## 1.50 Double Brace Initialization should not be used

bug 次要
双构造初始不要用
Map source = new HashMap(){{ // Noncompliant put(“firstName”, “John”); put(“lastName”, “Smith”);
}};
此操作如一个anonymous inner class，如果anonymous inner class返回且被其它对象引用，可能产生memory leaks，既使不产生memory leaks也会让大多维护者感到迷惑

## 1.51 Double-checked locking should not be used

bug 阻断
重复检查的锁块不要使用
public static Resource getInstance() {
if (resource == null) {
synchronized (DoubleCheckedLocking.class) {
if (resource == null)
resource = new Resource();
}
}
return resource;
}
应
public synchronized static Resource getInstance() {
if (resource == null)
resource = new Resource();
return resource;
}

## 1.52 Equals Hash Code

bug 严重
成对重写equals()与hashCode()

## 1.53 Exception should not be created without being thrown

bug 主要
不被抛出的异常不要创建

## 1.54 Expressions used in “assert” should not produce side effects

bug 主要
assert表达式不要产生负影响，不要改变数据状态

## 1.55 Failed unit tests should be fixed

bug 主要
失败的单元测试应该尽快解决掉

## 1.56 Floating point numbers should not be tested for equality

bug 主要
浮点数不要进行比较

## 1.57 Getters and setters should be synchronized in pairs

bug 主要
get与set应该成对进行同步操作

## 1.58 Identical expressions should not be used on both sides of a binary operator

bug 主要
相同的表达式不要作为二进制操作的操作数使用,应该简化

## 1.59 Inappropriate “Collection” calls should not be made

bug 主要
正确使用集合元素类型

## 1.60 Inappropriate regular expressions should not be used

bug 主要
正确使用正则表达式

## 1.61 Intermediate Stream methods should not be left unused

bug 主要
中间流应该被使用

## 1.62 Ints and longs should not be shifted by zero or more than their number of bits-1

bug 次要
整型与长整型位移操作数应该价于1与类型占位数-1

## 1.63 Invalid “Date” values should not be used

bug 主要
正确使用日期

## 1.64 Jump statements should not occur in “finally” blocks

bug 主要
finally块中使用return, break, throw等跳转语句，会阻止在try catch中抛出的未处理异常的传播

## 1.65 Locks should be released

bug 严重
保证锁的能够释放

## 1.66 Loop conditions should be true at least once

bug 主要
循环应该至少走一次

## 1.67 Loops should not be infinit

bug 阻断
循环不应该死循环

## 1.68 Math operands should be cast before assignment

bug 次要
数字操作在操作或赋值前要转化

## 1.69 Math should not be performed on floats

bug 次要
BigDecimal代替floats进行大数精确运算

## 1.70 Methods “wait(…)”, “notify()” and “notifyAll()” should not be called on Thread instances

bug 阻断
不要在线程中使用"wait(…)", “notify()” and “notifyAll()”

## 1.71 Methods should not be named “hashcode” or “equal”

bug 主要
除非Override重写这些方法

## 1.72 Multiline blocks should be enclosed in curly braces

bug 主要
多列块应用大括号括起来

## 1.73 Neither “Math.abs” nor negation should be used on numbers that could be “MIN_VALUE”

bug 次要
不要对数值类型的MIN_VALUE值或返回值为此值进行Math.abs与取反操作，因为不会起作用。

## 1.74 Non-public methods should not be “@Transactional”

bug 主要
非public方法不要注解Transactional,调用时spring 会抛出异常

## 1.75 Non-serializable classes should not be written

bug 主要
执行写操作的类要序列化，否则会抛出异常

## 1.76 Non-serializable objects should not be stored in “HttpSession” objects

bug 主要
HttpSession要保存序列化的对象

## 1.77 Non-thread-safe fields should not be static

bug 主要
非线程安全的域不应该静态化。注：可改为静态方法返回对象

## 1.78 Null pointers should not be dereferenced

bug 主要
空指针引用不应被访问

## 1.79 Optional value should only be accessed after calling isPresent()

bug 主要
Optional实例值的获取要isPresent()之后再做操作

## 1.80 Printf-style format strings should not lead to unexpected behavior at runtime

bug 阻断
因为Printf风格格式化是在运行期解读，而不是在编译期检验,会存在风险格式类型必须一至

## 1.81 Raw byte values should not be used in bitwise operations in combination with shifts

bug 主要
原始字节值不应参与位运算
result = (result << 8) | readByte(); // Noncompliant
正:
result = (result << 8) | (readByte() & 0xff);

## 1.82 Reflection should not be used to check non-runtime annotations

bug 主要
反射操作不应该运于检查非运行时注解

## 1.83 Related “if/else if” statements should not have the same condition

bug 主要
if/else if中不应该有相同的条件

## 1.84 Resources should be closed

bug 阻断
打开的资源应该关闭并且放到finally块中进行关闭

## 1.85 Return values from functions without side effects should not be ignored

bug 主要
操作对函数返回值没有影响的应该忽略
public void handle(String command){ command.toLowerCase(); // Noncompliant; result of method thrown away …
}

## 1.86 Servlets should not have mutable instance fields

bug 主要
servlet容器对每一个servlet创建一个实例导致实例变量共享产生问题
struts1.x 也是单例

## 1.87 Short-circuit logic should be used to prevent null pointer dereferences in conditionals

bug 主要
应正确使用短路逻辑来防止条件中的空指针引用访问

## 1.88 Silly equality checks should not be made

bug 主要
愚蠢的相等检查不应该做
非同类型的对象equal

## 1.89 Spring “@Controller” classes should not use “@Scope”

bug 主要
保持spring controller的单例

## 1.90 Synchronization should not be based on Strings or boxed primitives

bug 主要
字符串和封箱类不应该被用作锁定对象，因为它们被合并和重用。

## 1.91 The non-serializable super class of a “Serializable” class should have a no-argument constructor

bug 次要
序列化的类的非序列化父类应有一个无参构造器

## 1.92 The Object.finalize() method should not be called

bug 主要
Object.finalize()不要人为去调用

## 1.93 The Object.finalize() method should not be overriden

bug 主要
Object.finalize()不要重写

## 1.94 The signature of “finalize()” should match that of “Object.finalize()”

bug 主要
Object.finalize()不要重写

## 1.95 The value returned from a stream read should be checked

bug 次要
从流中读取的值应先检查再操作

## 1.96 Thread.run() should not be called directly

bug 主要
调用start()

## 1.97 Useless “if(true) {…}” and “if(false){…}” blocks should be removed

bug 主要
无用的if(true)和if(false)块应移除

## 1.98 Value-based classes should not be used for locking

bug 主要
基于值的类不要用于锁对象

## 1.99 Value-based objects should not be serialized

bug 次要
基于值的对象不应被用于序列化

## 1.100 Values should not be uselessly incremented

bug 主要
值增减后不存储是代码浪费甚至是bug

## 1.101 Variables should not be self-assigned

bug 主要
变量不应该自分配如下:
public void setName(String name) { name = name;
}

## 1.102 Week Year (“YYYY”) should not be used for date formatting

bug 主要
日期格式化错误

## 1.103 Zero should not be a possible denominator

bug 严重
零不应该是一个可能的分母

## 1.104 Loops should not be infinite

Bug 阻断
循环不应该是无限的

# 2 漏洞类型

## 2.1 “@RequestMapping” methods should be “public”

漏洞 阻断
标注了RequestMapping是controller是处理web请求。既使方法修饰为private,同样也能被外部调用，因为spring通过反射调用方法,没有检查方法可视度，

## 2.2 “enum” fields should not be publicly mutable

漏洞 次要
枚举类域不应该是public，也不应该进行set

## 2.3 File.createTempFile" should not be used to create a directory

漏洞 严重
File.createTempFile不应该被用来创建目录

## 2.4 “HttpServletRequest.getRequestedSessionId()” should not be used

漏洞 严重
HttpServletRequest.getRequestedSessionId()返回客户端浏览器会话id不要用，用HttpServletRequest.getSession().getId()

## 2.5 “javax.crypto.NullCipher” should not be used for anything other than testing

漏洞 阻断
NullCipher类提供了一种“身份密码”，不会以任何方式转换或加密明文。 因此，密文与明文相同。 所以这个类应该用于测试，从不在生产代码中。

## 2.6 “public static” fields should be constant

漏洞 次要
public static 域应该 final

## 2.7 Class variable fields should not have public accessibility

漏洞 次要
类变量域应该是private,通过set，get进行操作

## 2.8 Classes should not be loaded dynamically

漏洞 严重
不应该动态加载类,动态加载的类可能包含由静态类初始化程序执行的恶意代码.
Class clazz = Class.forName(className); // Noncompliant

## 2.9 Cookies should be “secure”

漏洞 次要
Cookie c = new Cookie(SECRET, secret); // Noncompliant; cookie is not secure
response.addCookie©;
正:
Cookie c = new Cookie(SECRET, secret);
c.setSecure(true);
response.addCookie©;

## 2.10 Credentials should not be hard-coded

漏洞 阻断
凭证不应该硬编码

## 2.11 Cryptographic RSA algorithms should always incorporate OAEP (Optimal Asymmetric Encryption Padding)

漏洞 严重
加密RSA算法应始终包含OAEP（最优非对称加密填充）

## 2.12 Default EJB interceptors should be declared in “ejb-jar.xml”

漏洞 阻断
默认EJB拦截器应在“ejb-jar.xml”中声明

## 2.13 Defined filters should be used

漏洞 严重
web.xml文件中定义的每个过滤器都应该在元素中使用。 否则不会调用此类过滤器。

## 2.14 Exceptions should not be thrown from servlet methods

漏洞 次要
不应该从servlet方法抛出异常

## 2.15 HTTP referers should not be relied on

漏洞 严重
不应依赖于http，将这些参数值中止后可能是安全的，但绝不应根据其内容作出决定。
如:
String referer = request.getHeader(“referer”); // Noncompliant
if(isTrustedReferer(referer)){
//…
}

## 2.16 IP addresses should not be hardcoded

漏洞 次要
ip 地址不应该硬编码

## 2.17 Member variable visibility should be specified

漏洞 次要
应指定成员变量的可见性

## 2.18 Members of Spring components should be injected

漏洞 严重
spring组件的成员应注入，单例注入非静态成员共享会产生风险

## 2.19 Mutable fields should not be “public static”

漏洞 次要
可变字段不应为 public static

## 2.20 Neither DES (Data Encryption Standard) nor DESede (3DES) should be used

漏洞 阻断
不应使用DES（数据加密标准）和DESEDE（3DES）

## 2.21 Only standard cryptographic algorithms should be used

漏洞 严重
标准的加密算法如 SHA-256, SHA-384, SHA-512等,非标准算法是危险的，可能被功能者攻破算法

## 2.22 Pseudorandom number generators (PRNGs) should not be used in secure contexts

漏洞 严重
伪随机数生成器（PRNG）不应在安全上下文中使用

## 2.23 Return values should not be ignored when they contain the operation status code

漏洞 次要
当函数调用的返回值包含操作状态代码时，应该测试此值以确保操作成功完成。

## 2.24 Security constraints should be defined in

漏洞 阻断
应定义安全约束，当web.xml文件没有元素时，此规则引发了一个问题

## 2.25 SHA-1 and Message-Digest hash algorithms should not be used

漏洞 严重
不应该使用SHA-1和消息摘要散列算法,已证实不再安全

## 2.26 SQL binding mechanisms should be used

漏洞 阻断
应该使用SQL绑定机制

## 2.27 Struts validation forms should have unique names

漏洞 阻断
struts验证表单应有唯一名称

## 2.28 Throwable.printStackTrace(…) should not be called

漏洞 次要
Throwable.printStackTrace(…)会打印异常信息，但会暴露敏感信息
用Loggers来处理（就是打Log）。获取日志对象用LogManager.getLogger(Xxx.class)

## 2.29 Untrusted data should not be stored in sessions

漏洞 主要
不受信任的数据不应存储在会话中。
Web会话中的数据被认为在“信任边界”内。 也就是说，它被认为是值得信赖的。 但存储未经身份验证的用户未经验证的数据违反信任边界，并可能导致该数据被不当使用。

## 2.30 Values passed to LDAP queries should be sanitized

漏洞 严重
传递到LDAP查询的值应该被清理

## 2.31 Values passed to OS commands should be sanitized

漏洞 严重
传递给OS命令的值应该被清理

## 2.32 Web applications should not have a “main” method

漏洞 严重
web 应用中不应有一个main方法

# 3 坏味道

## 3.1 “" and “!=” should not be used when “equals” is overridden

坏味道 次要
当类重写equals方法后,不应该再用"“与”!=“进行对象比较

## 3.2 “@Deprecated” code should not be used

坏味道 次要
弃用方代码不应再用,弃用代码意味首将会移除,应该使用替代代码

## 3.3 “@Override” should be used on overriding and implementing methods

坏味道 主要
重写的和实现的方法要加Override标注

## 3.4 “action” mappings should not have too many “forward” entries

坏味道 次要
默认 4
action不要有太多的forward

## 3.5 “Arrays.stream” should be used for primitive arrays

坏味道 主要
Arrays.stream用于原始流类型(IntStream, LongStream, DoubleStream)会有更好的性能

## 3.6 “catch” clauses should do more than rethrow

坏味道 次要
只是重新抛出捕获的异常和完全放弃异常捕获效果一样，但会给维护者带来疑惑

## 3.7 “clone” should not be overridden

坏味道 阻断
不应重写clone方法

## 3.8 “Cloneables” should implement “clone”

坏味道 严重
Cloneables类应该实现clone方法

## 3.9 “collect” should be used with “Streams” instead of “list::add”

坏味道 次要
虽然您可以使用forEach（list :: add）或使用Stream收集，但是收集是更好的选择，因为它自动线程安全并且可并行

## 3.10 “Collections.EMPTY_LIST”, “EMPTY_MAP”, and “EMPTY_SET” should not be used

坏味道 次要

## 3.11 “DateUtils.truncate” from Apache Commons Lang library should not be used

坏味道 主要
使用Java 8中引入的Instant类来截断日期可能会比Commons Lang的DateUtils类快得多。

## 3.12 “deleteOnExit” should not be used

坏味道 主要
不推荐使用File.deleteOnExit()

## 3.13 “entrySet()” should be iterated when both the key and value are needed

坏味道 主要
当循环中只需要一个map的键时，迭代keySet就是有意义的。 但是，当需要键和值两者时，迭代entrySet更有效，这将允许访问键和值

## 3.14 “equals(Object obj)” should be overridden along with the “compareTo(T obj)” method

坏味道 次要
“equals（Object obj）”应该与“compareTo（T obj）”方法一起被重写

## 3.15 “Exception” should not be caught when not required by called methods

坏味道 次要
如果被调方法没有抛出“Exception”时不要捕获"Exception”,应捕后被调方法抛出的异常

## 3.16 “final” classes should not have “protected” members

坏味道 次要
最终类意味首不可继承所以不要有受保存成员，这样没有意义

## 3.17 “finalize” should not set fields to “null”

坏味道 次要
finalize不应将字段值设置为空，对垃圾收集是没必要的，还可能为垃圾收集带来额外开消

## 3.18 “for” loop increment clauses should modify the loops’ counters

坏味道 严重
loop循环应该增加递增序号变量

## 3.19 “for” loop stop conditions should be invariant

坏味道 主要
循环停止条件应为不变

## 3.20 、“indexOf” checks should not be for positive numbers

坏味道 严重
检查indexOf返回不应使用正数

## 3.21 indexOf” checks should use a start position

坏味道 次要
如果您需要查看一个子字符串是否位于字符串中某个特定点之外，则可以测试该子字符串与该目标点的indexOf，也可以使用该起始点参数的indexOf版本。 后者可以更清楚，因为结果是针对-1测试的，这是一个容易识别的“未找到”指标

## 3.22 “java.lang.Error” should not be extended

坏味道 主要
java.lang.Error及其子类表示异常情况，例如OutOfMemoryError，它只能由Java虚拟机进行处理。

## 3.23 “java.nio.Files#delete” should be preferred

坏味道 主要
删除文件必须用Files.delete(path)，不能用文件对象的delete方法

## 3.24 “java.time” classes should be used for dates and times

坏味道 主要
Date 和 Calendar类非线程同步，推荐使用LocalDate

## 3.25 “Lock” objects should not be “synchronized”

坏味道 主要
“锁定”对象不应“同步”
java.util.concurrent.locks.Lock提供比同步块更强大和灵活的锁定操作，应该使用tryLock（）和unlock（）锁定和解锁这些对象

## 3.26 “main” should not “throw” anything

坏味道 阻断
main方法不应该抛出异常

## 3.27 “NullPointerException” should not be caught

坏味道 主要
空指针不应捕获处理，应该避免NullPointerException，而不是被捕获

## 3.28 “NullPointerException” should not be explicitly thrown

坏味道 主要
“NullPointerException”不应该被显式抛出

## 3.29 “Object.finalize()” should remain protected (versus public) when overriding

坏味道 严重
重写finalizey方法应为protected

## 3.30 “Object.wait(…)” and “Condition.await(…)” should be called inside a “while” loop

坏味道 严重
“Object.wait（…）”和“Condition.await（…）”应该在“while”循环内调用

## 3.31 “Object.wait(…)” should never be called on objects that implement “java.util.concurrent.locks.Condition”

坏味道 主要
不应该在实现“java.util.concurrent.locks.Condition”的对象上调用“Object.wait（…）”

## 3.32 “Optional” should not be used for parameters

坏味道 主要
Optional不要被用作参数

## 3.33 “Preconditions” and logging arguments should not require evaluation

坏味道 主要
将连接的字符串传递到日志记录方法也可能导致不必要的性能消耗，因为每次调用该方法时将执行级联，无论日志级别是否足够低以显示消息

## 3.34 “private” methods called only by inner classes should be moved to those classes

坏味道 次要
只被内部类调用的方法，应该移到内部类内部

## 3.35 “private” methods that don’t access instance data should be “static”

坏味道 次要
不访问实例数据的“私有”方法应该是“静态”

## 3.36 “readObject” should not be “synchronized”

坏味道 主要
“readObject”不应该被“同步”

## 3.37 “readResolve” methods should be inheritable

坏味道 严重
“readResolve”方法应该是可继承的

## 3.38 “ResultSet.isLast()” should not be used

坏味道 主要
ResultSet.isLast()不应用

## 3.39 “Serializable” classes should have a version id

坏味道 严重
“Serializable”类应该有一个版本号

## 3.40 “Serializable” inner classes of “Serializable” classes should be static

坏味道 次要
实现序列化的内部类应该是静态的

## 3.41 “static” members should be accessed statically

坏味道 主要
“静态”成员应类访问

## 3.42 “Stream.anyMatch()” should be preferred

坏味道 次要
“Stream.anyMatch（）”应该是首选的

## 3.43 “switch case” clauses should not have too many lines of code

坏味道 主要
默认 5
“switch case”子句不应该有太多的代码行

## 3.44 “switch” statements should end with “default” clauses

坏味道 严重
“switch”语句应以“default”子句结尾

## 3.45 “switch” statements should have at least 3 “case” clauses

坏味道 次要
“switch”语句应具有至少3个“case”子句，可以用if替代

## 3.46 “switch” statements should not contain non-case labels

坏味道 阻断
switch语句不用包含非case标签

## 3.47 “switch” statements should not have too many “case” clauses

坏味道 主要
默认 30
switch语句不应包含太多case语句

## 3.48 “Thread.sleep” should not be used in tests

坏味道 主要
“Thread.sleep”不应该在测试中使用

## 3.49 “ThreadLocal.withInitial” should be preferred

坏味道 次要
“ThreadLocal.withInitial”应该是首选
ThreadLocal<List> myThreadLocal = ThreadLocal.withInitial(ArrayList::new);

## 3.50 “Threads” should not be used where “Runnables” are expected

坏味道 主要
Noncompliant Code Examplepublic static void main(String[] args) {Thread r =new Thread() {int p;@Overridepublic void run() {while(true)System.out.println(“a”);}};new Thread®.start(); // NoncompliantCompliant Solutionpublic static void main(String[] args) {Runnable r =new Runnable() {int p;@Overridepublic void run() {while(true)System.out.println(“a”);}};new Thread®.start();

## 3.51 “throws” declarations should not be superfluous

坏味道 次要
“抛出”声明不应该是多余的

## 3.52 “toString()” should never be called on a String object

坏味道 次要
不应该在String对象上调用“toString（）”

## 3.53 “URL.hashCode” and “URL.equals” should be avoided

坏味道 主要
应避免使用“URL.hashCode”和“URL.equals”

## 3.54 “writeObject” should not be the only “synchronized” code in a class

坏味道 主要
“writeObject”不应该是类中唯一的“同步”代码

## 3.55 @FunctionalInterface annotation should be used to flag Single Abstract Method interfaces

坏味道 严重
一个只有一个抽象方法的接口应加FunctionalInterface注释

## 3.56 A “while” loop should be used instead of a “for” loop

坏味道 次要
当在for循环中仅定义条件表达式，并且缺少初始化和增量表达式时，应使用while循环来增加可读性

## 3.57 A close curly brace should be located at the beginning of a line

坏味道 次要
共享编码约定使得团队有可能有效地进行协作。 这个规则使得强制要在行的开头放置一个大括号。

## 3.58 A field should not duplicate the name of its containing class

坏味道 主要
字段不应该重复其包含的类的名称

## 3.59 Abbreviation As Word In Name

坏味道 主要
检查验证标识符名称中的缩写（连续大写字母）长度，还允许执行骆驼案例命名

## 3.60 Abstract Class Name

坏味道 主要
检查抽象类名是否符合指定的格式
ignoreName
Controls whether to ignore checking the name. Realistically only useful if using the check to identify that match name and do not have the abstract modifier name. Default is false.默认值falseignoreModifier
Controls whether to ignore checking for the abstract modifier on classes that match the name. Default is false.默认值falseformat
Regular expression默认值

## 3.61 Abstract class names should comply with a naming convention

坏味道 次要
抽象类名称应符合命名约定

Regular expression used to check the abstract class names against.
默认值

## 3.62 Abstract classes without fields should be converted to interfaces

坏味道 次要
没有字段的抽象类应该转换为接口

## 3.63 Abstract methods should not be redundant

坏味道 次要
抽象方法不应该是多余的

## 3.64 An abstract class should have both abstract and concrete methods

坏味道 次要
抽象类应该有抽象和具体的方法

## 3.65 An open curly brace should be located at the beginning of a line

坏味道 次要
开放的大括号应位于一行的开头

## 3.66 An open curly brace should be located at the end of a line

坏味道 次要
开放的大括号应位于一行的末尾

## 3.67 Annotation arguments should appear in the order in which they were declared

坏味道 次要
注释参数应按其声明顺序显示

## 3.68 Annotation Location

坏味道 主要
注释位置
allowSamelineSingleParameterlessAnnotation
To allow single parameterless annotation to be located on the same line as target element.默认值trueallowSamelineParameterizedAnnotation
To allow parameterized annotation to be located on the same line as target element.默认值falseallowSamelineMultipleAnnotations
To allow annotation to be located on the same line as target element.默认值falsetokens
tokens to check默认值CLASS_DEF,INTERFACE_DEF,ENUM_DEF,METHOD_DEF,CTOR_DEF,VARIABLE_DEF

## 3.69 Annotation repetitions should not be wrapped

坏味道 次要
注释重复不应包装

## 3.70 Annotation Use Style

坏味道 主要
trailingArrayComma
Defines the policy for trailing comma in arrays. Default is never.closingParens
Defines the policy for ending parenthesis. Default is never.elementStyle
Defines the annotation element styles. Default value is compact_no_array.

## 3.71 Anon Inner Length

坏味道 主要
检查长匿名内部类。max maximum allowable number of lines. Default is 20.

## 3.72 Anonymous inner classes containing only one method should become lambdas

坏味道 主要
只有一个方法的匿名内部类应该变成lambdas
jdk8以下自动禁用

## 3.73 Array designators “[]” should be located after the type in method signatures

坏味道 次要
数组代号“[]”应位于方法签名类型之后

## 3.74 Array designators “[]” should be on the type, not the variable

坏味道 次要
数组代号“[]”应位于类型之后而不是变量之后

## 3.75 Array Trailing Comma

坏味道 主要
检查数组初始化是否包含逗号

## 3.76 Array Type Style

坏味道 次要
数组类型样式javaStyle
Controls whether to enforce Java style (true) or C style (false). Default is true.

## 3.77 Arrays should not be created for varargs parameters

坏味道 次要
不应为varargs参数创建数组

## 3.78 Artifact ids should follow a naming convention

坏味道 次要
共享命名约定允许团队有效协作。 当pom的artifactId与提供的正则表达式不匹配时，此规则引发了一个问题
regex The regular expression the “artifactId” should match默认值[a-z][a-z-0-9]+

## 3.79 Assertions should be complete

坏味道 阻断
断言应该是完整的

## 3.80 Assignments should not be made from within sub-expressions

坏味道 主要
不应在子表达式中作出赋值操作当赋值变量没有用到

## 3.81 At-clause Order

坏味道 主要
检查从句顺序
tagOrder allows to specify the order by tags.默认值@author,@version,@param,@return,@throws,@exception,@see,@since,@serial,@serialField,@serialData,@deprecatedtarget
allows to specify targets to check at-clauses.

## 3.82 Avoid Escaped Unicode Characters

坏味道 主要
避免转义的Unicode字符
allowIfAllCharactersEscaped
Allow if all characters in literal are escaped.默认值falseallowNonPrintableEscapes
Allow non-printable escapes.默认值falseallowByTailComment
Allow use escapes if trail comment is present.默认值falseallowEscapesForControlCharacters
Allow use escapes for non-printable(control) characters.默认值false

## 3.83 Avoid Inline Conditionals

坏味道 次要
避免内联条件

## 3.84 Avoid Nested Blocks

坏味道 主要
避免嵌套块
allowInSwitchCase
Allow nested blocks in case statements. Default is false.

## 3.85 Avoid Star Import

坏味道 次要
检查发现使用符号的导入语句excludes
packages where star imports are allowed. Note that this property is not recursive, subpackages of excluded packages are not automatically excluded.allowStaticMemberImports
whether to allow starred static member imports like import static org.junit.Assert.;. Default is false.默认值falseallowClassImports
whether to allow starred class imports like import java.util.;. Default is false.默认值false

## 3.86 Avoid Static Import

坏味道 次要
避免静态导入

## 3.87 Boolean checks should not be inverted

坏味道 次要
布尔检查不应该被反转
Noncompliant Code Exampleif ( !(a == 2)) { …} // Noncompliantboolean b = !(i < 10); // NoncompliantCompliant Solutionif (a != 2) { …}boolean b = (i >= 10);

## 3.88 Boolean Expression Complexity

坏味道 主要
将嵌套布尔运算符（&&，||和^）限制为指定的深度（默认= 3）。
max the maximum allowed number of boolean operations in one expression. Default is 3.默认值3tokens
tokens to check. Default is LAND,BAND,LOR,BOR,BXOR.默认值LAND,BAND,LOR,BOR,BXOR

## 3.89 Boolean expressions should not be gratuitous

坏味道 主要
如果boolean表达式的值是已定的，那么boolean表达式是没有必要的可以移除

## 3.90 Boolean literals should not be redundant

坏味道 次要
boolean不需再与true,false比较作为boolean表达式

## 3.91 Branches should have sufficient coverage by tests

坏味道 主要
分支应有足够的测试覆盖
minimumBranchCoverageRatio
默认值65

## 3.92 Case insensitive string comparisons should be made without intermediate upper or lower casing

坏味道 次要
使用toLowerCase（）或toUpperCase（）来使不区分大小写的比较无效，因为它需要创建临时的中间String对象。应使用equalsIgnoreCase来比较

## 3.93 Catch Parameter Name

坏味道 主要
检查catch参数名是否符合format属性指定的格式
format
Specifies valid identifiers. Default is ^(e|t|ex|[a-z][a-z][a-zA-Z]+)默 认 值 ( e ∣ t ∣ e x ∣ [ a − z ] [ a − z ] [ a − z A − Z ] + ) 默认值^(e|t|ex|[a-z][a-z][a-zA-Z]+)默认值 
(
 e∣t∣ex∣[a−z][a−z][a−zA−Z]+)

## 3.94 Catches should be combined

坏味道 次要
由于Java 7以后可以一次捕获多个异常。 因此，当多个catch块具有相同的代码时，它们应该被组合以便更好的可读性，sonar.java.source低于7时，此规则将自动禁用

## 3.95 Checked exceptions should not be thrown

坏味道 主要
检查的异常不应该被抛出，要处理

## 3.96 Child class fields should not shadow parent class fields

坏味道 阻断
子类字段不应该private父类的非private字段

## 3.97 Class Data Abstraction Coupling

坏味道 主要
度量衡量给定类中其他类的实例化数。
max the maximum threshold allowed. Default is 7.excludedClasses
User-configured class names to ignore.excludeClassesRegexps
User-configured regular expressions to ignore classesexcludedPackages
User-configured packages to ignore

## 3.98 Class Fan Out Complexity

坏味道 主要
类的依赖类数量
max the maximum threshold allowed. Default is 20.excludedClasses
User-configured class names to ignoreexcludeClassesRegexps
User-configured regular expressions to ignore classesexcludedPackages
User-configured packages to ignore

## 3.99 Class names should comply with a naming convention

坏味道 次要
类名应符合命名约定
format
Regular expression used to check the class names against.默认值1[a-zA-Z0-9]

## 3.100 Class names should not shadow interfaces or superclasses

坏味道 严重
类名称不应该影响接口或超类（相同）

## 3.101 Class Type(Generic) Parameter Name

坏味道 主要
泛型参数名称符合指定的格式
format
Regular expression默认值2

## 3.102 Classes and enums with private members should have a constructor

坏味道 主要
有私有成员的类和枚举应该有一个构造函数

## 3.103 Classes and methods that rely on the default system encoding should not be used

坏味道 次要
不应使用依赖于默认系统编码的类和方法

## 3.104 Classes from "sun." packages should not be used

坏味道 主要
不得使用“sun.”软件包的类,sun类或com.sun 包被视为实现细节，不属于Java API
Exclude
Comma separated list of Sun packages to be ignored by this rule. Example: com.sun.jna,sun.misc

## 3.105 Classes named like “Exception” should extend “Exception” or a subclass

坏味道 主要
名为“异常”的类应该扩展“异常”或者一个子类

## 3.106 Classes should not access their own subclasses during initialization

坏味道 严重
类在初始化期间不应访问自己的子类

## 3.107 Classes should not be coupled to too many other classes (Single Responsibility Principle)

坏味道 主要
类不应与太多其他类（单一责任原则）相耦合（依赖）
max Maximum number of classes a single class is allowed to depend upon默认值20

## 3.108 Classes should not be empty

坏味道 次要
空类没意义，作为公共扩展点可以作为接口

## 3.109 Classes should not be too complex

坏味道 严重 废弃
类不应太复杂
max Maximum complexity allowed.默认值200

## 3.110 Classes should not have too many “static” imports

坏味道 主要
静态导入类允许您使用其公共静态成员，而不必使用类名。 这可以很方便，但如果静态导入太多的类，你的代码可能会变得混乱，很难维护
threshold
The maximum number of static imports allowed默认值4

## 3.111 Classes should not have too many fields

坏味道 主要
类不应有太多字段
countNonpublicFields Whether or not to include non-public fields in the count默认值truemaximumFieldThreshold
The maximum number of fields默认值20

## 3.112 Classes should not have too many methods

坏味道 主要
类不应该有太多方法
countNonpublicMethods
Whether or not to include non-public methods in the count.默认值truemaximumMethodThreshold
The maximum number of methods authorized in a class.默认值35

## 3.113 Classes that override “clone” should be “Cloneable” and call “super.clone()”

坏味道 次要
覆盖“克隆”的类应该是“可克隆”，并调用“super.clone（）”

## 3.114 Classes with only “static” methods should not be instantiated

坏味道 主要
只有“静态”方法的类不应该被实例化

## 3.115 Classes without “public” constructors should be “final”

坏味道 次要
只有私有构造函数的类应该被标记为final，以防止任何错误的扩展尝试。

## 3.116 Close curly brace and the next “else”, “catch” and “finally” keywords should be located on the same line

坏味道 次要
关闭大括号，下一个“else”，“catch”和“finally”关键字应位于同一行

## 3.117 Close curly brace and the next “else”, “catch” and “finally” keywords should be on two different lines

坏味道 次要
关闭大括号和下一个“else”，“catch”和“finally”关键字应该在两个不同的行

## 3.118 Cognitive Complexity of methods should not be too high

坏味道 严重
认知复杂度是衡量一种方法的控制流程难以理解的度量。 认知复杂性较高的方法难以维持。
Threshold The maximum authorized complexity.默认值15

## 3.119 Collapsible “if” statements should be merged

坏味道 主要
可合并的“if”语句应该合并

## 3.120 Collection methods with O(n) performance should be used carefully

坏味道 次要
应仔细使用具有O（n）性能的集合方法

## 3.121 Collection.isEmpty() should be used to test for emptiness

坏味道 次要
应该使用Collection.isEmpty（）来判断空集合

## 3.122 Comment pattern matcher

坏味道 次要
该规则允许在TODO，NOPMD，…之外的任何类型的内容中找到任何类型的模式，NOSONAR除外

## 3.123 Comments Indentation

坏味道 次要
注释缩进
tokens tokens to check默认值SINGLE_LINE_COMMENT,BLOCK_COMMENT_BEGIN

## 3.124 Comments should not be located at the end of lines of code

坏味道 次要
注释不应位于代码行的末尾
legalTrailingCommentPattern
Description Pattern for text of trailing comments that are allowed. By default, comments containing only one word.默认值\s*+[\s]++

## 3.125 Comparators should be “Serializable”

坏味道 严重
Comparators should be “Serializable”
3.126 Conditionals should start on new lines
坏味道 严重
条件表达式应该起始新行

## 3.127 Constant Name

坏味道 次要
检查常数名称是否符合指定的格式
applyToPackage Controls whether to apply the check to package-private member默认值trueformat
Regular expression默认值3[A-Z0-9](_[A-Z0-9]+)KaTeX parse error: Can't use function '\.' in math mode at position 322: …定 默认: ^[a-z_]+(\̲.̲[a-z_][a-z0-9_]…:

## 3.129 Constant names should comply with a naming convention坏味道 严重

常数名称应符合命名约定
format Regular expression used to check the constant names against.默认值4[A-Z0-9](_[A-Z0-9]+)$

## 3.130 Constants should not be defined in interfaces

坏味道 严重
常量不应在接口中定义

## 3.131 Constructors should not be used to instantiate “String” and primitive-wrapper classes

坏味道 主要
构造函数不应用于实例化“String”和原始包装类

## 3.132 Constructors should only call non-overridable methods

坏味道 严重
构造函数只应该调用不可覆盖的方法

## 3.133 Control flow statements “if”, “for”, “while”, “switch” and “try” should not be nested too deeply

坏味道 严重
控制流程语句“if”，“for”，“while”，“switch”和“try”不能嵌套太深
max Maximum allowed control flow statement nesting depth.默认值3

## 3.134 Control structures should use curly braces

坏味道 严重
控制结构应使用花括号

## 3.135 Covariant Equals

坏味道 严重
检查一个类是否定义了一个协变方法equals，那么它定义了方法equals（java.lang.Object）

## 3.136 Custom Import Order

坏味道 主要
检查导入声明组按照用户指定的顺序显示。 如果有导入，但是在组态中未指定其组，则导入应放在导入列表的末尾。
thirdPartyPackageRegExp RegExp for THIRDPARTY_PACKAGE group imports.默认值^KaTeX parse error: Expected group after '^' at position 219: …oup imports.默认值^̲customImportOrderRules
List of order declaration customizing by user.standardPackageRegExp
RegExp for STANDARD_JAVA_PACKAGE group imports.默认值java|javax

## 3.137 Cyclomatic Complexity

坏味道 主要
检查针对特定限制的方法的循环复杂性
switchBlockAsSingleDecisionPoint
whether to treat the whole switch block as a single decision point默认值falsemax the maximum threshold allowed.默认值10tokens
tokens to check默认值LITERAL_WHILE,LITERAL_DO,LITERAL_FOR,LITERAL_IF,LITERAL_SWITCH,LITERAL_CASE,LITERAL_CATCH,QUESTION,LAND,LOR

## 3.138 Dead stores should be removed

坏味道 主要
没用的变量应该除移

## 3.139 Declaration Order

坏味道 提示
声名的顺序
ignoreModifiers Whether to ignore modifiers默认值falseignoreConstructors
Whether to ignore constructors默认值false

## 3.140 Declarations should use Java collection interfaces such as “List” rather than specific implementation classes such as “LinkedList”

坏味道 次要
声明应该使用Java集合接口，例如“List”，而不是特定的实现类，如“LinkedList”

## 3.141 Default annotation parameter values should not be passed as arguments

坏味道 次要
默认注解参数值不应作为参数传递

## 3.142 Default Comes Last

坏味道 主要
检查在switch语句中的所有情况之后的默认值。
skipIfLastAndSharedWithCase
whether to allow default along with case if they are not last默认值false

## 3.143 Deprecated "p o m &quot; p r o p e r t i e s s h o u l d n o t b e u s e d 坏 味 道 次 要 不 应 使 用 不 推 荐 使 用 的 “ {pom}&quot; properties should not be used 

坏味道 次要 不应使用不推荐使用的“pom"propertiesshouldnotbeused坏味道次要不应使用不推荐使用的“ {pom}”属性

## 3.144 Deprecated code should be removed

坏味道 提示
应删除弃用的代码

## 3.145 Deprecated elements should have both the annotation and the Javadoc tag

坏味道 主要
弃用元素应有注解和doc标签

## 3.146 Descendant Token

坏味道 次要
检查其他令牌下的限制令牌maximumMessage
error message when the maximum count is exceededmaximumDepth
the maximum depth for descendant countslimitedTokens
set of tokens with limited occurrences as descendantsmaximumNumber
a maximum count for descendantsminimumMessage
error message when the maximum count is exceededminimumNumber
a minimum count for descendantsminimumDepth
the minimum depth for descendant countssumTokenCounts
whether the number of tokens found should be calculated from the sum of the individual token counts
默认值false

## 3.147 Design For Extension

坏味道 次要
扩展设计
ignoredAnnotations
Annotations which allow the check to skip the method from validation.默认值Test,Before,After,BeforeClass,AfterClass

## 3.148 EJB interceptor exclusions should be declared as annotations

坏味道 阻断
EJB interceptor exclusions应该以注解的形式使用

## 3.149 Empty arrays and collections should be returned instead of null

坏味道 主要
应该返回空数组和集合，而不是null

## 3.150 Empty Block

坏味道 主要
Checks for empty blocks
tokens blocks to check默认值LITERAL_WHILE,LITERAL_TRY,LITERAL_FINALLY,LITERAL_DO,LITERAL_IF,LITERAL_ELSE,LITERAL_FOR,INSTANCE_INIT,STATIC_INIT,LITERAL_SWITCH,LITERAL_SYNCHRONIZEDoption
policy on block contents默认值stmt

## 3.151 Empty catch block

坏味道 主要
检查空的catch块。 有两个选项可以使验证更加精确（默认情况下，检查允许空的catch块和任何注释）
exceptionVariableName
Format of skipping exception’‘s variable name.默认值^c o m m e n t F o r m a t F o r m a t o f c o m m e n t . 默 认 值 . 

##  3.152 E m p t y F o r I n i t i a l i z e r P a d 

坏 味 道 次 要 检 查 初 始 化 程 序 为 空 的 填 充 ; 那 是 空 的 是 否 需 要 一 个 空 的 初 始 化 程 序 ， 或 者 禁 止 这 样 的 空 格 。 示 例 ： f o r （ ; i &lt; j ; i + + ， j − − ） o p t i o n p o l i c y o n h o w t o p a d a n e m p t y f o r i t e r a t o r

##  3.153 Empty For Iterator Pad 

坏味道 次要 检查一个空的填充迭代器; 那就是空格是否需要一个空的迭代器，否则这样的空格是被禁止的。 示例：for（Iterator foo = very.long.line.iterator（）; foo.hasNext（）;） option policy on how to pad an empty for iterator 

## 3.154 Empty Line Separator 

坏味道 主要 在标题，包，所有导入声明，字段，构造函数，方法，嵌套类，静态初始化器和实例初始化器之后检查空行分隔符 allowNoEmptyLineBetweenFields Allow no empty line between fields默认值falseallowMultipleEmptyLines Allows multiple empty lines between class members.默认值truetokens assignments to check默认值PACKAGE_DEF,IMPORT,CLASS_DEF,INTERFACE_DEF,ENUM_DEF,STATIC_INIT,INSTANCE_INIT,METHOD_DEF,CTOR_DEF,VARIABLE_DEFallowMultipleEmptyLinesInsideClassMembers Allow multiple empty lines inside class members默认值true 3.155 Empty Statement 坏味道 次要 检测空的语句（独立的&#x27;;&#x27;）。

## 3.156 Empty statements should be removed

 坏味道 次要 移除空语句

##  3.157 Enumeration should not be implemented 

坏味道 主要 不应实同Enumeration 

## 3.158 Equality operators should not be used in &quot;for&quot; loop termination conditions 坏味道 严重 循环终止条件下不应使用平等运算符 3.159 Equals Avoid Null 

坏味道 主要 

## 3.160 Escaped Unicode characters should not be used

 坏味道 主要

 不应使用转义的Unicode字符 3.161 Exception classes should be immutable 坏味道 次要 异常类应该是不可变的 

## 3.162 Exception handlers should preserve the original exceptions

 坏味道 主要 

异常处理程序应保留原始异常

## 3.163 Exception types should not be tested using &quot;instanceof&quot; in catch blocks 

坏味道 主要 

异常类型不应该在catch块中使用“instanceof”进行测试 

## 3.164 Exceptions should not be thrown in finally blocks 

坏味道 严重 

异常不应该在finally块中抛出 

## 3.165 Executable Statement Count 

坏味道 主要 

将可执行语句的数量限制为指定的限制（默认= 30）。 max the maximum threshold allowed. Default is 30.默认值30tokens members to check默认值CTOR_DEF,METHOD_DEF,INSTANCE_INIT,STATIC_INIT 

## 3.166 Execution of the Garbage Collector should be triggered only by the JVM

 坏味道 严重 

垃圾收集器的执行只能由JVM触发

## 3.167 Exit methods should not be called

 坏味道 阻断 

调用System.exit（int status）或Rutime.getRuntime（）。exit（int status）调用关闭挂钩并关闭整个Java虚拟机。 调用Runtime.getRuntime（）。halt（int）立即关闭，而不调用关闭挂钩，并跳过完成 

## 3.168 Explicit Initialization 

坏味道 主要 

检查任何类或对象成员是否明确地初始化为其类型值的默认值（对于对象引用为空，数字类型为零，对于布尔为char为false）。 

## 3.169 Expressions should not be too complex 

坏味道 严重

 表达式不应太复杂 max Maximum number of allowed conditional operators in an expression默认值3 

## 3.170 Extensions and implementations should not be redundant 

坏味道 次要 

扩展和实现不应该是多余的 

## 3.171 Fall Through 

坏味道 主要 

## 3.172 Field names should comply with a naming convention 

坏味道 次要 

字段名称应符合命名约定,默认值^[a-z][a-zA-Z0-9]*commentFormatFormatofcomment.默认值.

## 3.173 Fields in a “Serializable” class should either be transient or serializable

坏味道 严重
“Serializable”类中的字段应该是transient或可序列化的
List序列化如下：List<? extends Serializable> fileCds;

## 3.174 Fields in non-serializable classes should not be “transient”

坏味道 次要
不可序列化类的字段不应该是“transient”

## 3.175 ields should not be initialized to default values

坏味道 次要
不应将字段初始化为默认值

## 3.176 File Contents Holder

坏味道 次要
配置为TreeWalker子模块时，保留当前的全局访问文件内容。 例如，过滤器可以通过此模块访问当前文件内容

## 3.177 File Length

坏味道 主要
如果源文件变得很长，那么很难理解。 因此，长类通常应该重构到专注于特定任务的几个单独的类中
fileExtensions
file type extension of files to processmax maximum allowable number of lines. Default is 2000.

## 3.178 File Tab Character

坏味道 次要
检查源代码中没有制表符（’\ t’）
fileExtensions
file type extension of files to processeachLine
whether to report on each line containing a tab, or just the first instance. Default is false.

## 3.179 Files should contain an empty new line at the end

坏味道 次要
文件最后应该包含一个空的新行

## 3.180 Files should contain only one top-level class or interface each

坏味道 主要
文件应该只包含一个顶级类或接口

## 3.181 Files should not be empty

坏味道 次要
删除空文件

## 3.182 Files should not have too many lines of code

坏味道 主要
源码文件代码行数检查

## 3.183 Modifiers should be declared in the correct order

坏味道 次要
Java语言规范建议按以下顺序列出修饰符：

1. Annotations
2. public
3. protected
4. private
5. abstract
6. static
7. final
8. transient
9. volatile
10. synchronized
11. native
12. strictfp

## 3.184 Sections of code should not be “commented out”

坏味道 主要
不要有注掉的代码，影响可读性，可以删除

## 3.185 Strings should not be concatenated using ‘+’ in a loop

坏味道 次要
用StringBuilder代替String拼接

## 3.186 String function use should be optimized for single characters

坏味道 主要
字符串方法操作中单字符建议优先用单引号

## 3.187 Unused local variables should be removed

坏味道 次要
如果一个局部变量被声明但未被使用，那么它是死代码，应该被删除。 这样做会提高可维护性，因为开发人员不会想知道使用什么变量

## 3.188 The diamond operator ("<>") should be used

坏味道 次要
Java 7引入了操作符（<>）来减少泛型代码的冗长度。
List strings = new ArrayList<>()

## 3.189 Useless imports should be removed

坏味道 次要
不要导入没有用到的导入

## 3.190 Source files should not have any duplicated blocks

坏味道 主要
源文件不应有任何重复块

## 3.191 Only static class initializers should be used

坏味道 主要
静态代码块应加，static标识

## 3.192 Generic exceptions should never be thrown

坏味道 主要
通用异常如Error, RuntimeException, Throwable, and Exception不应抛出,应定义和抛出一个专门的异常，而不是使用通用异常，注不允许除抛出DseException之外的异常

## 3.193 Method names should comply with a naming convention

坏味道 次要
方法名称应符合命名约定
默认规则：5[a-zA-Z0-9]

## 3.194 Synchronized classes Vector, Hashtable, Stack and StringBuffer should not be used

坏味道 主要
不要使用同步的Vector/HashTable/Stack/StringBuffer等。在早期，出于线程安全问题考虑，java API 提供了这些类。但是同步会极大影响性能，即使是在同一个线程中使用他们。
通常可以这样取代：
ArrayList or LinkedList instead of Vector
Deque instead of Stack
HashMap instead of Hashtable
StringBuilder instead of StringBuffer

## 3.195 Standard outputs should not be used directly to log anything

坏味道 主要
用日志记录代替标准输出

## 3.196 Local variable and method parameter names should comply with a naming convention

坏味道 次要
局部变量和方法参数名称应符合命名约定
默认:6[a-zA-Z0-9]$

## 3.197 Local Variables should not be declared and then immediately returned or thrown

坏味道 次要
声明一个变量只是立即返回或抛出它是一个糟糕的做法

## 3.198 Instance methods should not write to “static” fields

坏味道 严重
不要用实例方法改变静态成员，理想情况下，静态变量只通过同步的静态方法来改变

## 3.199 Methods should not be empty

坏味道 严重
不要存在空方法，VO,POJO对象默认实例化就是带空参数的构造方法，不需要再写

## 3.200 Utility classes should not have public constructors

坏味道 主要
帮助类不应该有公共构造函数，帮助类不宜实例化，且应该有一个如下的私有构造方法
private StringUtils() { throw new IllegalStateException(“Utility class”); }

## 3.201 Static non-final field names should comply with a naming convention

坏味道 次要
静态非最终字段名称应符合命名约定
默认:7[a-zA-Z0-9]*$

## 3.202 Methods returns should not be invariant

坏味道 阻断
方法返回值不应该是相同的值

## 3.203 Return of boolean expressions should not be wrapped into an “if-then-else” statement

坏味道 次要
可以根据boolean表达式就能返回的直接返回boolean表达式，不需要if-then-else语句

## 3.204 Try-with-resources should be used

坏味道 严重
对含有资源对象必须使用try with resource，禁止使用ry-catch-finally

## 3.205 String literals should not be duplicated

坏味道 严重
重复的字符串文字会使重构过程容易出错，因为您必须确保更新所有
threshold
Number of times a literal must be duplicated to trigger an issue默认值3

## 3.206 Methods and field names should not be the same or differ only by capitalization

坏味道 主要
方法和字段名不应该是相同的，也不应该只根据大小写来区别

## 3.207 Methods should not have identical implementations

坏味道 主要
方法不应该有相同的实现

## 3.208 Unused “private” fields should be removed

坏味道 主要
移除无用的变量

## 3.209 Parsing should be used to convert “Strings” to primitives

坏味道 次要
字符串转基本数据类型要用parse

## 3.210 Loops should not contain more than a single “break” or “continue” statement

坏味道 次要
循环不应该包含多个“break”或“continue”语句。

## 3.211 Redundant casts should not be used

坏味道 次要
不应该使用冗余的操作

## 3.212 Local variables should not shadow class fields

坏味道 主要

## 3.213 局部变量不应该和类属性重名

The default unnamed package should not be used
坏味道 次要
不应该使用默认的未命名包

## 3.214 Generic wildcard types should not be used in return parameters

坏味道 严重
通配符不应该出现在返回声明中

## 3.215 Unused method parameters should be removed

坏味道 主要
没有用到的方法参数应该移除掉

## 3.216 Jump statements should not be redundant

坏味道 次要

跳转语句不应该是冗余的

## 3.217 Ternary operators should not be nested

坏味道 主要
不应该嵌套三元运算符

## 3.218 Assignments should not be redundant

坏味道 主要
参数不应该是多余的

## 3.219 Try-catch blocks should not be nested

坏味道 主要
try catch 不应该被嵌套

## 3.220 Unused “private” methods should be removed

坏味道 主要
应该删除未使用的“私有”方法

## 3.221 Multiple variables should not be declared on the same line

坏味道 次要
不要在同一行定义多个变量

## 3.222 Methods should not return constants

坏味道 次要
方法不应返回常量



## 4.1 导出代码质量检查规则清单

The sonarqube version I have shows 781 java rules my objective is to extract them to an excel or a csv file

[1]curl -X GET -v -u admin:admin http://localhost:9000/api/rules?language=java

[2]curl -X GET -v -u admin:admin http://localhost:9000/api/rules/search?languages=java >> java.json



https://rules.sonarsource.com/java (代码质量检查规则清单 - 外网英文版且无法导出)

# 4 阿里p3c插件（考虑设置为默认的规则库）

https://www.cnblogs.com/shenh/p/13674450.html

https://github.com/caowenliang/sonar-pmd-p3c

https://github.com/rhinoceros/sonar-p3c-pmd



https://developer.aliyun.com/article/826717（引入P3C规则 ~ 仅激活其中的51项规则）

# 5 （错误拦截）每次提交前进行代码检查，若不符合规范则禁止提交

![img](https://upload-images.jianshu.io/upload_images/13080552-d1c0e753cc764da5?imageMogr2/auto-orient/strip|imageView2/2/format/webp)

