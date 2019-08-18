# Dart学习笔记



任何一门编程语言的第一节课基本上都会是Hello,world!
估计很少有人会打破这个传统

```dart
void main()
{
  print("Hello, world!");
}
```

顺带一提，Dart有 两种运行模式：

- 检查模式（checked）：进行类型检查，如果发现实际类型与声明或期望的类型不匹配就报错
- 生产模式（production）：不进行类型检查，忽略声明的类型信息，忽略 assert 语句

检查模式运行较慢，生产模式运行快
但检查模式可以及早地发现程序在的问题，所以建议在开发过程中使用检查模式
而在正式环境中使用生产模式运行

Dart VM 默认在**生产模式**下运行，而我们用 WebStorm 开发时默认在**检查模式**下运行



Dart中所有东西都是对象，包括数字、函数等

Dart中支持以下数据类型：

- Numbers
- Strings
- Booleans
- List（也就是数组）
- Maps

```dart
void main()
{
  //Dart 语言本质上是动态类型语言，类型是可选的
  //可以使用 var 声明变量，也可以使用类型来声明变量
  //一个变量也可以被赋予不同类型的对象
  //但大多数情况，我们不会去改变一个变量的类型
  
  //字符串赋值的时候，可以使用单引号，也可以使用双引号
  var str1 = "Ok?";
  
  //如果使用的是双引号，可以内嵌单引号
  //当然，如果使用的是单引号，可以内嵌双引号，否则需要“\”转义
  //String str2 = ‘It\’s ok!’;
  String str2 = "It's ok!";
  
  //使用三个单引号或者双引号可以多行字符串赋值
  var str3 = """Dart Lang
  Hello,World!""";
  
  //在Dart中，相邻的字符串在编译的时候会自动连接
  //这里发现一个问题，如果多个字符串相邻，中间的字符串不能为空，否则报错
  //但是如果单引号和双引号相邻，即使是空值也不会报错，但相信没有人这么做
  //var name = 'Wang''''Jianfei'; 报错
  var name = 'Wang'' ''Jianfei';
  
  //assert 是语言内置的断言函数，仅在检查模式下有效
  //如果断言失败则程序立刻终止
  assert(name == "Wang Jianfei");
  
  //Dart中字符串不支持“+”操作符，如str1 + str2
  //如果要链接字符串，除了上面诉说，相邻字符串自动连接外
  //还可以使用“$”插入变量的值
  print("Name：$name");
  
  //声明原始字符串，直接在字符串前加字符“r”
  //可以避免“\”的转义作用，在正则表达式里特别有用
  print(r"换行符：\n");
  
  //Dart中数值是num，它有两个子类型：int 和 double
  //int是任意长度的整数，double是双精度浮点数
  var hex = 0xDEADBEEF;
      
  //翻了半天的文档，才找打一个重要的函数：转换进制，英文太不过关了
  //上面提到的字符串插值，还可以插入表达式：${}
  print("整型转换为16进制：$hex —> 0x${hex.toRadixString(16).toUpperCase()}");

    // 在使用const和final的时候，可以省略var或者其他类型

var i = 10；
const i = 10；
final i = 10；

int i = 10；
const int i = 10；
final int i = 10；
    
const list = const[1,2,3];//Ok
const list = [1,2,3];//Error

final list = [1,2,3];//Ok
final list = const[1,2,3];//Ok
final list = const[new DateTime.now(),2,3];//Error,const右边必须是常量
    
        // const定义的是编译时常量，只能用编译时常量来初始化
// final定义的常量可以用变量来初始化
final time = new DateTime.now(); //Ok
const time = new DateTime.now(); //Error，new DateTime.now()不是const常量
    

}
```

函数

```dart
String sayHello(String name)
{
  return 'Hello $name!';
}

// 断言函数assert()，Debug模式下，当表达式的值为false时抛出异常。
//is  is!操作符判断对象是否为指定类型，如num、String等
assert(sayHello is Function, "当表达式为false时, 打印此信息");

// 函数简写
var  sayHello = (name)=>'Hello $name!';

// 函数别名
// 略

// 函数闭包

Function makeSubstract(num n)
{
  return (num i) => n - i; // 注意了, 返回一个函数
}

void main()
{
  var x = makeSubstract(5);
  print(x(2));
}

// 
var callbacks = [];
for (var i = 0; i < 3; i++) {
  // 在列表 callbacks 中添加一个函数对象，这个函数会记住 for 循环中当前 i 的值。
  callbacks.add(() => print('Save $i')); 
}
callbacks.forEach((c) => c()); // 分别输出 0 1 2

```



## 可选参数

Dart中支持两种可选参数：命名可选参数和位置可选参数
但两种可选不能同时使用

- 命名可选参数使用大括号{}，默认值用冒号:
- 位置可选参数使用方括号[]，默认值用等号=

```dart
FunX(a, {b, c:3, d:4, e})
{
  print('$a $b $c $d $e');
}

FunY(a, [b, c=3, d=4, e])
{
  print('$a $b $c $d $e');
}


void main()
{
  FunX(1, b:3, d:5);
  FunY(1, 3, 5);
}
```



# 操作符和流程控制语句

```dart
// 取整~/操作符
int a = 3；
int b = 2；
print(a~/b);//输出1

// 级联

class Person {
    String name;
    String country;
    void setCountry(String country){
      this.country = country;
    }
    String toString() => 'Name:$name\nCountry:$country';
}
void main() {
  Person p = new Person();
  p ..name = 'Wang'
    ..setCountry('China');
  print(p);
}

// 循环
for(int i = 0; i<3; i++) {
  print(i);
}


var collection = [0, 1, 2];
collection.forEach((x) => print(x));//forEach的参数为Function
for(var x in collection) {
  print(x);
}

while(boolean){
  //do something
}
 
do{
  //do something
} while(boolean)

// Switch And Case
var command = 'OPEN';
switch (command) {
  case 'CLOSED':
    break;
  case 'OPEN':
    break;
  default:
    print('Default');
}

// 控制fall-through落空
var command = 'CLOSED';
  switch (command) {
    case 'CLOSED':
      print('CLOSED');
      continue nowClosed; // Continues executing at the nowClosed label.
    case 'OPEN':
      print('OPEN');
      break;
    nowClosed: // Runs for both CLOSED and NOW_CLOSED.
    case 'NOW_CLOSED':
      print('NOW_CLOSED');
      break;
  }

```



## 异常处理

```dart
throw new ExpectException('值必须大于0！');
throw '值必须大于0!';

try {
      throw 'This a Exception!';
  } on Exception catch(e) {
    print('Unknown exception: $e');
  } catch(e) {
    print('Unknown type: $e');
  }


try {
      throw 'This a Exception!';
  } catch(e) {//可以试试删除catch语句，只用try...finally是什么效果
    print('Catch Exception: $e');
  } finally {
    print('Close');
  }


```



# 类和对象



Dart是一门使用类和单继承的面向对象语言
所有的对象都是类的实例，并且所有的类都是Object的子类

在Dart中类和接口是统一的，类就是接口

使用abstract关键字来定义抽象类，并且抽象类不能被实例化
抽象方法不需要关键字，直接以分号 ; 结束即可

如果你想重写部分功能，那么你可以继承一个类
如果你想实现某些功能，那么你也可以实现一个类

- 实现的关键字为implements
- 继承的关键字为extends

```dart
// 实现接口

abstract class Person {  // 此时abstract关键字可加可不加，如果加上的话Person不能被实例化 
    String greet(who); // 函数可以没有实现语句，名曰隐式接口，前面不用加 abstract 关键字 
}

class Student implements Person { 
    String name; 
    Student(this.name); 
     
    String greet(who) => 'Student: I am $name!'; 
} 

class Teacher implements Person { 
    String name; 
    Teacher(this.name); 
   
    String greet(who) => 'Teacher: I am $name!'; 
} 

void main() { 
    Person student = new Student('Wang'); 
    Person teacher = new Teacher('Lee'); 
    
    print( student.greet('Chen')); 
    print(teacher.greet('Chen')); 
}

```

```dart
// 继承
class Person { 
  String name; 
   
  Person(this.name); 
  String greet(who) => 'I am $name!'; 
} 

class Student extends Person {   
  Student(String name):super(name); 
  String greet(who) => 'Student: I am $name!'; 
} 

class Teacher extends Person { 
  Teacher(String name):super(name); 
     String greet(who) => 'Teacher: I am $name!'; 
} 

void main() { 
   Person p1 = new Student('Wang'); 
   Person p2 = new Teacher('Lee'); 
    
   print(p1.greet('Chen')); 
   print(p2.greet('Chen')); 
}
```



# Mixin混合模式

多重继承有很多不足，例如，结构复杂化，优先顺序模糊，功能冲突等问题，而Mixin则是为了解决多继承的问题而出现。

对于Mixin并没有一个准确的概念，有人理解为Mix in混入。它类似于多继承，但通常混入Mixin的类和Mixin类本身并不是is-a的关系。

实质上，Mixin是通过语言特性，来更简洁地实现**组合模式**。因此，Mixin可以灵活地添加某些功能。传统的接口概念中，并不包含实现部分，而Mixin包含实现。

本节将介绍在Dart中使用混合模式的一些细节。近来，Dart开放了一些早期关于实现部分的限制，当前状态如下：
  **·** Dart 1.13以及更高版本支持Mixin，可extend继承类（不仅是Object），并且可以调用super()。
  **·** Dart 1.12或低版本支持Mixin，但只能继承Object，并且不能调用super()。

## 1、基本概念

如果你熟悉关于Mixin的学术文献资料，你可以跳过这一部分。否则，建议你了解一下，因为它定义了重要概念和注解。如果你希望深入探究，可以从该文章开始：**Mixins in Strongtalk**

注：Smalltalk被公认为历史上第二个面向对象的程序设计语言和第一个真正的集成开发环境 (IDE)。并且，它对其它众多的程序设计语言的产生起到了极大的推动作用，主要有：Objective-C，Actor， Java 和Ruby等。90年代的许多软件开发思想得利于Smalltalk，例如Design Patterns， Extreme Programming(XP)和Refactoring等。Strongtalk的最独特之处是支持渐进式的类型注解，这种思想在Dart、PHP、Python 3和TypeScript等语言中都有体现。Gilad Bracha是Dart开发团队的一员，在20世纪90年代，Gilad同Urs Hölzle和Lars Bak等人一起创建了语言Smalltalk的一个高性能版本即Strongtalk。但是随着Java的流行，Sun停止了Strongtalk的投入，并将团队成员重新分配来优化Java的性能，而Strongtalk演变成了官方JVM即Hotspot。

作为一个支持类和继承的语言，类隐式地定义了Mixin。Mixin隐式地通过类主体（Class Body）进行定义，并建立子类和父类之间的变量增量（Delta）。而Class类实际上则是一个Mixin应用，即是通过隐式定义的Mixin应用于父类的结果。

Mixin Application混合应用类似于Function Application函数应用。在数学中，混合类M可以视作从父类到子类新增的一个功能，将M注入超类S，并且返回一个S的子类。在研究文献中，这通常写作：*M |> S*。

基于函数应用的概念，可以定义**复合函数**（即函数组合）。该概念贯穿于混合组合中。我们定义混合M1和M2的组合，写作*M1\*M2*，如：*(M1 \* M2) |> S = M1 |> (M2 |> S)*。

函数非常有用，因为他们可以应用于不同的参数。同样，Class隐式定义的Mixin，通常仅在类声明给出的父类中应用一次。为允许Mixin应用于不同的父类，我们要么声明Mixin不依赖于特定的父类，要么脱离于Class隐式的Mixin，然后重用外部的原始定义。

## 2、语法和语义

Mixin通过正常的类声明被隐式定义。原则上，每个类都定义了一个Mixin，并可以从类中提取出来。然而，Mixin只能从未定义构造函数的类中提取。由于沿着继承链传递构造函数参数的需要，该约束能避免出现新的连锁问题。例如：

```dart
abstract class Collection<E> {
  Collection<E> newInstance();

  void forEach(void f(E element)) {
    // ...
  }

  void add(E element) {
    // ...
  }

  // ...
}

abstract class DOMElementList<E> = DOMList with Collection<E>;
abstract class DOMElementSet<E> = DOMSet with Collection<E>;
```

这里，Collection<E>是一个标准的类，并用来声明一个Mixin。另外，DOMElementList和DOMElementSet也是混合应用。这两个Mixin在类声明的时候，通过特殊的形式来定义。在类声明中包含一个名字，并用with语句来声明他们与父类中的混合应用相同。Collection<E> 是抽象类，因为它并没有实现类中定义的抽象方法newInstance();。

上述中，事实上DOMElementList混合为*Collection mixin |> DOMList*，而DOMElementSet则是 *Collection mixin |> DOMSet*。

这样做的好处是，Collection中的代码可以在类的多个继承层次中被共享。因此，无论DOMList还是DOMSet，都不需要重复、复制Collection中的代码，并且任何变化都会使Collection传递到这两个继承结构中，大大简化了维护代码。上面的代码介绍了Mixin应用的一种方式：混合应用指定应用的Mixin和父类，以及混合应用的名称。

另外一种情况，混合应用出现在类声明的with语句中，以逗号来分隔标识符列表的时候。此时，所有的标识符代表Class。在这种情况下，多混合在extends语句中可以构成及应用于父类名称，生成一个匿名的父类。再以同样的例子：

```dart
class DOMElementList<E> extends DOMList with Collection<E> {
  DOMElementList<E> newInstance() => new DOMElementList<E>();
}

class DOMElementSet<E> extends DOMSet with Collection<E> {
  DOMElementSet<E> newInstance() => new DOMElementSet<E>();
}
```

这里，DOMElementList并不是应用*Collection mixin |> DOMList*。相反，它是一个父类为应用的新定义的类，DOMElementSet同样如此。注意，在每一种情况下，抽象函数newInstance()必须单独实现，以便类能够直接被实例化。

想象一下，如果DOMList有一个带参数的构造函数：

```dart
class DOMElementList<E> extends DOMList with Collection<E> {
  DOMElementList<E> newInstance() => new DOMElementList<E>(0);
  DOMElementList(size): super(size);
}
```

构造函数可以为各个字段以及泛型参数设置值。每个Mixin都有一个自定义的构造函数被单独调用，父类也是如此。因为Mixin的构造函数不能够被声明，所以调用函数可以省略。在底层的实现中，调用总是放在初始列表之前。

第二种是以方便实用的语法糖形式，将多个Mixin混入类中，而不需要引入多个中间声明。例如：

```dart
class Person {
  String name;
  Person(this.name);
}

class Maestroends P
 with Musical, Aggressive, Demented {
  Maestro(name):super(name);
}
```

这里，父类是一个混合应用：*Demented mixin |> Aggressive mixin |> Musical mixin |> Person*

假设仅Person有带参数的构造函数，则 *Musical mixin |> Person* 将继承Person的构造函数。以此类推，一直到Maestro实际的父类Person，它由一系列的Mixin应用组成。

## 3、细节描述

### Privacy私有

一个混合应用很可能是在外部最初声明Class的库中被声明，这对访问混合应用的成员没有任何影响。根据混合应用的语义可知，访问成员取决于库最初声明的位置。这与普通的继承一样，是由底层语言（C++）的继承语义决定的。

### Statics静态

是否可以通过混合应用使用最初Class的静态值？同样，由继承的语义进行分析，在Dart中静态成员不会被继承。

### Types类型

混合应用的实例是什么类型？通常，它是父类的子类型，以及通过Mixin名称表示的类型的子类型。换句话说，它与最初Class的类型相同。

最初的Class有它自身的父类。为确保特定的混合应用与最初进行混入的Class兼容，需要使用with语句。例如，如果通过with语句定义了Class A，并应用了一个混合M，M源自Class K，那么A必须支持K定义的接口。

```dart
class S {
  twice(int x) => 2 * x;
}

abstract class I {
  twice(x);
}

abstract class J {
  thrice(x);
}
class K extends S implements I, J {
  int thrice(x) => 3* x;
}

class B {
  twice(x) => x + x;
}
class A = B with K;
```

需注意的是，A必须支持K的父类S的隐式接口。这确保A实际上是M的子类型，尽管它的继承结构并不相同。在上面的例子中，K必须实现twice()来满足I的需求，以及实现J中声明的thrice()。K满足这两个条件，因为它直接定义了thrice()，以及继承了S的twice()。

现在我们定义A，得到来自K的Mixin的thrice()实现。然而，虽然K继承了S，但Mixin并没有提供twice()的实现。幸运的是，B实现了该接口。所以总的来说，A满足I、J、S的要求。

相比之下，下面定义Class D：

```dart
class D {
  double(x) => x+x;
}

class E = D with K;
```

该代码运行的时候会得到一个警告，因为Class E没有实现I声明的twice()函数。







# 容器

## StringBuffer

按照官方的说法，StringBuffer可以特别高效的构建多个字符串

```dart
StringBuffer sb = new StringBuffer();

sb.write("Use a StringBuffer ");
sb.writeAll(['for ', 'efficient ', 'string ', 'creation ']);
sb..write('if you are ')..write('building lots of string.');

print(sb.toString());

sb.clear();
```



```dart
// 使用List的构造函数，也可以添加int参数，表示List固定长度
var vegetables = new List();

// 或者简单的用List来赋值
var fruits = ['apples', 'oranges'];

// 添加元素
fruits.add('kiwis');

// 添加多个元素
fruits.addAll(['grapes', 'bananas']);

// 获取List的长度
assert(fruits.length == 5);

// 利用索引获取元素
assert(fruits[0] == 'apples');

// 查找某个元素的索引号
assert(fruits.indexOf('apples') == 0);

// 利用索引号删除某个元素
var appleIndex = fruits.indexOf('apples');
fruits.removeAt(appleIndex);
assert(fruits.length == 4);

// 删除所有的元素
fruits.clear();
assert(fruits.length == 0);
```



```dart
ar ingredients = new Set();

ingredients.addAll(['gold', 'titanium', 'xenon']);
assert(ingredients.length == 3);

// 添加已存在的元素无效
ingredients.add('gold');
assert(ingredients.length == 3);

// 删除元素
ingredients.remove('gold');
assert(ingredients.length == 2);

// 检查在Set中是否包含某个元素
assert(ingredients.contains('titanium'));

// 检查在Set中是否包含多个元素
assert(ingredients.containsAll(['titanium', 'xenon']));
ingredients.addAll(['gold', 'titanium', 'xenon']);

// 获取两个集合的交集
var nobleGases = new Set.from(['xenon', 'argon']);
var intersection = ingredients.intersection(nobleGases);
assert(intersection.length == 1);
assert(intersection.contains('xenon'));

// 检查一个Set是否是另一个Set的子集
var allElements = ['hydrogen', 'helium', 'lithium', 'beryllium',
'gold', 'titanium', 'xenon'];
assert(ingredients.isSubsetOf(allElements));
```



```dart
// Map的声明
var hawaiianBeaches = {
    'oahu' : ['waikiki', 'kailua', 'waimanalo'],
    'big island' : ['wailea bay', 'pololu beach'],
    'kauai' : ['hanalei', 'poipu']
};
var searchTerms = new Map();

// 指定键值对的参数类型
var nobleGases = new Map<int, String>();

// Map的赋值，中括号中是Key，这里可不是数组
nobleGase[54] = 'dart';

//Map中的键值对是唯一的
//同Set不同，第二次输入的Key如果存在，Value会覆盖之前的数据
nobleGases[54] = 'xenon';
assert(nobleGases[54] == 'xenon');

// 检索Map是否含有某Key
assert(nobleGases.containsKey(54));

//删除某个键值对
nobleGases.remove(54);
assert(!nobleGases.containsKey(54));
```



# 库的引用

Dart中任何文件都是一个库，即使你没有用关键字library声明





import

import语句用来导入一个库

```dart
//dart:前缀表示Dart的标准库，如dart:io、dart:html
import 'dart:math';

//当然，你也可以用相对路径或绝对路径的dart文件来引用
import 'lib/student/student.dart';

//Pub包管理系统中有很多功能强大、实用的库，可以使用前缀 package:
import 'package:args/args.dart';

// 别名
import 'lib/student/student.dart' as Stu;
Stu.Student s = new Stu.Student();

// show关键字可以显示某个成员（屏蔽其他）
// hide关键字可以隐藏某个成员（显示其他）
import 'lib/student/student.dart' show Student, Person;
import 'lib/student/student.dart' hide Person;
```

## export

你可以使用export关键字导出一个更大的库

```dart
library math;

export 'random.dart';
export 'point.dart';
```

也可以导出部分组合成一个新库

```dart
library math;

export 'random.dart' show Random;
export 'point.dart' hide Sin;
```



## 利用Pub管理自己的库



略



# 异步与并发实例



Dart中引入了Future和Completer的概念

在Dart中并没有线程的概念，词典翻译isolate，大概意思是隔离区
应该说很形象，实际上isolate在Dart VM中可能是一个线程
在Web中可能是一个Web Worker

Dart中的代码都是在不同的isolate中运行的，包括main函数
每个isolate都有自己的堆（Heap）和栈（Stack）
所以即使是全局数据，也彼此隔离

isolate仅在**端口（Port）上通过消息进行通信**，并且消息在接收前会进行复制
因此无法通过消息操作同一个对象
于是C++中通过线程间引用传递参数操作对象的方法，在Dart中行不通

main.dart

```dart
import "dart:html";
import "dart:isolate";

//清空页面中的中奖号码
void resetLottery() {
  querySelector("#ball1").innerHtml = "";
  querySelector("#ball2").innerHtml = "";
  querySelector("#ball3").innerHtml = "";
}

updateResult(List<int> list) {
  if(list.length < 2) return;
  
  var ballDiv = querySelector("#ball${list[0]}");
  ballDiv.innerHtml = "${list[1]}";
}

void main() {
  ReceivePort receivePort = new ReceivePort();
  
  //接收获奖号码
  receivePort.listen((msg) {
    if(msg is List) {
      updateResult(msg);
    }
  });

  String Costlyprocess = './lib/lottery.dart';

  //通过ID获取页面元素
  var startBtn = querySelector("#startBtn");
  var replayBtn = querySelector("#replayBtn");
  
  //添加onClick的事件
  startBtn.onClick.listen((e){
    //点击开始按钮后，页面元素中开始按钮不可用，重置按钮可用
    startBtn.disabled = true;
    replayBtn.disabled = false;
    
    //开始计算中奖号码
    Isolate.spawnUri(Uri.parse(Costlyprocess), ["START"], receivePort.sendPort);
  });
  
  replayBtn.onClick.listen((e){
    //点击重置按钮后，页面元素中开始按钮可用，重置按钮不可用
    replayBtn.disabled = true;
    startBtn.disabled = false;
    
    //清空页面中的中奖号码
    resetLottery();
  });
}
```

lottery.dart

```dart
library lottery;

import "dart:math";
import "dart:async";
import "dart:isolate";

//计算中奖号码
void sendWinningNumber(SendPort sendPort) {
  
  Random r = new Random();
    
  var currentMs = 0;
  var waitMs = 1000;
  var duraMs = 50;
  
  new Timer.periodic(new Duration(milliseconds: duraMs), (t){
    currentMs += duraMs;
    if(currentMs ~/ waitMs > 2) {
      t.cancle();
    } else {
      sendPort.send([(currentMs ~/ waitMs) + 1, r.nextInt(32) + 1]);
    }
  });
}

main(List<String> args, SendPort sendPort) {
  if(args[0] == "START") {
    sendWinningNumber(sendPort);
  }
}
```



# Dart和Web技术

main.css

```css
body {
  background-color: #F8F8F8;
  font-family: '黑体';
  font-size: 14px;
  font-weight: normal;
  line-height: 1.2em;
  margin: 15px;
}

h1, p {
  color: #333;
}

#sample_container_id {
  width: 80%;
  height: 50px;
  position:relative;
}

#sample_text_id {
  font-size: 24pt;
  text-align: center;
  margin-top: 14px;
}

.success {
  background: #E6EFC2 5px 5px;
  padding: 1em;
  padding-left: 3.5em;
  border: 2px solid #C6D880;
  color: #529214;
}

.error {
  background: #FBE3E4 5px 5px;
  padding: 1em;
  padding-left: 3.5em;
  border: 2px solid #FBC2C4;
  color: #D12F19;
}

.warning {
  background: #FFF6BF 5px 5px;
  padding: 1em;
  padding-left: 3.5em;
  border: 2px solid #FFD324;
  color: #817134;
}

.toggle {
  background: #cbe0f4 5px 5px;
  padding: 1em;
  padding-left: 3.5em;
  border: 2px solid #96b6d3;
  color: #286eae;
}
```

main.html

```html
<!DOCTYPE html>

<html>
  <head>
          <meta charset="utf-8">
          <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Note 12</title>
    
    <script type="application/dart" src="main.dart"></script>
    <script src="packages/browser/dart.js"></script>
    
    <link rel="stylesheet" href="main.css">
  </head>

  <body>   
    <h1>Note 12</h1>
    
    <p>Hello world from Dart!</p>
    
    <div id="sample_container_id">
      <p id="sample_text_id">Click Button!</p>
    </div>
    
  </body>
</html>
```

main.dart

```dart
import 'dart:html';

void main() {
  // 获取用于显示框的DIV元素和显示文字的P元素
  var container = document.querySelector('#sample_container_id');
  var p = document.querySelector('#sample_text_id');
  
  // 新建按钮，并添加onClick的事件处理程序
  var btn1 = new ButtonElement()
  ..id = 'btn1'
  ..text = ' ERROR '
  ..onClick.listen((event){
    container.classes
    //清空样式
    ..clear()
    //添加样式
    ..add('error');
    p.text = 'This is an error message !';
  });
  
  var btn2 = new ButtonElement()
  ..id = 'btn2'
  ..text = 'ALERT'
  ..onClick.listen((event) {
    container.classes
    ..clear()
    ..add('warning');
    p.text = 'This is an alert message !';
  });
  
  var btn3 = new ButtonElement()
  ..id = 'btn3'
  ..text = 'OK'
  ..onClick.listen((event) {
    container.classes
    ..clear()
    ..add('success');
    p.text = 'This is an OK message';
  });
  
  var btn4 = new ButtonElement()
  ..id = 'btn4'
  ..text = 'Toggle'
  ..onClick.listen((event) {
    //切换样式
    container.classes.toggle('toggle');
    p.text = ' Toggle !';
  });
  
  // 将所有的按钮添加到document中
  document.body.nodes.addAll([btn1, btn2, btn3, btn4]);
}
```



拖放 Drag and Drop，有时又被称为 DnD
是现代软件开发中必不可少的一项技术

它提供了一种能够在应用程序内部甚至是应用程序之间进行信息交换的机制
并且，操作系统与应用程序之间进行剪贴板的内容交换
也可以被认为是 DnD的一部分

## 1、可拖动的名片

basics.css

```css
body {
  background-color: #F8F8F8;
  font-family: '黑体';
  font-size: 14px;
  line-height: 1.2em;
}

/* 可拖动元素的文本设置为不能选择 */
[draggable] {
  user-select: none;
}

.column {
  background-color: #ccc;
  border: 2px solid #666;
  border-radius: 5px;
  cursor: move;
  
  /*设置多行文本水平和垂直居中显示*/
  width: 150px;
  text-align: center;
  padding-top:50px;
  padding-bottom:50px;
  
  float: left;
  margin-right: 5px;
  
  /*设置变换的特效*/
  transition: transform 0.2s ease-out;
}

/*拖动到元素上方时，显示为虚线边框*/
.column.over {
  border: 2px dashed #000;
}

/*拖动的时候，原DIV透明度降低、缩小尺寸*/
.column.moving {
  opacity: 0.25;
  transform: scale(0.8);
}
```

index.html

```html
<!DOCTYPE html>

<html>
  <head>
    <meta charset="utf-8">
    <title>Note14: Drag and Drop</title>
    <link rel="stylesheet" href="basics.css">
  </head>
  <body>
    <div id="columns">
      <div class="column" draggable="true">Wong Jeff<br/>ID:060806110006</div>
      <div class="column" draggable="true">Lee Lei<br/>ID:060806110018</div>
      <div class="column" draggable="true">Zhang Han<br/>ID:060806110037</div>
    </div>
    <script type="application/dart" src="basics.dart"></script>
    <script src="packages/browser/dart.js"></script>
  </body>
</html>
```

basics.dart

```dart
import 'dart:html';

class Basics {
  Element _dragSourceEl;

  void start() {
    var cols = document.querySelectorAll('#columns .column');
    for (var col in cols) {
      //遍历所有#columns .column元素，并添加拖动事件的监听程序
      col.onDragStart.listen(_onDragStart);
      col.onDragEnd.listen(_onDragEnd);
      col.onDragEnter.listen(_onDragEnter);
      col.onDragOver.listen(_onDragOver);
      col.onDragLeave.listen(_onDragLeave);
      col.onDrop.listen(_onDrop);
    }
  }

  void _onDragStart(MouseEvent event) {
    //开始拖动，目标元素即拖动的Element，保存到私有变量中，并添加moving样式
    Element dragTarget = event.target;
    dragTarget.classes.add('moving');
    _dragSourceEl = dragTarget;
    event.dataTransfer.effectAllowed = 'move';
    
    //设置需要移动的数据
    event.dataTransfer.setData('text/html', dragTarget.innerHtml);
  }

  void _onDragEnd(MouseEvent event) {
    //结束拖动，Element删除moving样式
    Element dragTarget = event.target;
    dragTarget.classes.remove('moving');
    
    var cols = document.querySelectorAll('#columns .column');
    //其实在Leave的时候，已经删除了over样式
    //所以cols中只有一个Element有over样式（拖放结束，最后那个Element未触发Leave事件）
    for (var col in cols) {
      col.classes.remove('over');
    }
  }

  void _onDragEnter(MouseEvent event) {
    //目标Element添加over样式
    Element dropTarget = event.target;
    dropTarget.classes.add('over');
  }

  void _onDragOver(MouseEvent event) {
    //通知 Web 浏览器不要执行与事件关联的默认动作（如果存在这样的动作）
    //如点击<a>标签会链接到某网址，这就是<a>标签的默认动作
    //如果执行preventDefault后，点击<a>标签就不会有什么动作
    event.preventDefault();
    event.dataTransfer.dropEffect = 'move';
  }

  void _onDragLeave(MouseEvent event) {
    //拖动离开Element的时候，删除over样式
    Element dropTarget = event.target;
    dropTarget.classes.remove('over');
  }

  void _onDrop(MouseEvent event) {
    //结束事件传递
    event.stopPropagation();

    // 如果拖动和放下的Element不是同一个，那么交换数据
    Element dropTarget = event.target;
    if (_dragSourceEl != dropTarget) {
      _dragSourceEl.innerHtml = dropTarget.innerHtml;
      dropTarget.innerHtml = event.dataTransfer.getData('text/html');
    }
  }
}

void main() {
  var basics = new Basics();
  basics.start();
}
```



拖放在HTML5中很容易做到，需注意标签的draggable设为true
然后就是监听拖放过程中的各个事件，代码很简洁

## 2、DataTransfer对象

如果网页只能简单的拖放而没有数据的变化，那没多大实用性
因此，为了在拖放的时候实现数据交换，引入了DataTransfer对象
并且，DataTransfer是事件event的一个属性

同时，DataTransfer并不只是用来传递数据
还能通过它来确定被拖动的元素以及作为放置目标的元素能够接受什么操作
为此需要访问DataTransfer的两个属性，effectAllowed和dropEffect

必须在onDragStart事件处理程序中设置effectAllowed属性
并且，dropEffect属性只有搭配effectAllowed属性才有用

effectAllowed属性表示允许拖放元素的哪种dropEffect，effectAllowed可能的值如下：

- uninitialized：没有该被拖动元素放置行为。
- none：被拖动的元素不能有任何行为。
- copy：只允许值为“copy”的dropEffect。
- link：只允许值为“link”的dropEffect。
- move：只允许值为“move”的dropEffect。
- copyLink：允许值为“copy”和“link”的dropEffect。
- copyMove：允许值为“copy”和”link”的dropEffect。
- linkMove：允许值为“link”和”move”的dropEffect。
- all：允许任意dropEffect。

通过dropEffect属性可以知道被拖动的元素能够执行哪种放置行为这个属性有下列4个可能的值：

- none：不能把拖动的元素放在这里。这是除文本框之外所有元素的默认值。
- move：可以把拖动的元素移动到放置目标。
- copy：可以把拖动的元素复制到放置目标。
- link：表示放置目标会打开拖动的元素（但拖动的元素必须是一个链接，有URL）。







## 附表：Dart中正则表达式支持的特性

| **特性**          | **描述**                                                     |
| ----------------- | ------------------------------------------------------------ |
| \                 | 将下一个字符标记为一个特殊字符、或一个原义字符、或一个后向引用、或一个八进制转义符。例如，”n”匹配字符”n”。”\n”匹配一个换行符。序列”\\”匹配”\”,”\(“则匹配”(“。 |
| .                 | 匹配任何单个字符。                                           |
| ^                 | 匹配输入字符串的开始位置。如果设置了**RegExp** 对象的**Multiline**属性，^也匹配”\n”或”\r”之后的位置。例如，”^.”可以匹配”abc\n123″中的”a”和”1″。 |
| $                 | 匹配输入字符串的结束位置。如果设置了RegExp对象的Multiline属性，$也匹配 “\n” 或”\r”之前的位置。例如，”.$”可以匹配”abc\n123″中的”c”和”3″。 |
| *                 | 匹配前面的子表达式零次或多次。例如，”zo*”能匹配 “z”或”zoo”整个字符串。”*”等价于{0,}。 |
| +                 | 匹配前面的子表达式一次或多次。例如，”zo+” 能匹配 “zo” 以及 “zoo”，但不能匹配 “z”。”+”等价于{1,}。 |
| ? （限制符）      | 匹配前面的子表达式零次或一次。例如，”do(es)?” 可以匹配 “do” 或 “does”整个字符串。”?”等价于 {0,1}。 |
| ？（非贪婪模式）  | 当该字符紧跟在任何一个其他限制符 (*, +, ?, {n}, {n,}, {n,m}) 后面时，匹配模式是非贪婪的。非贪婪模式**尽可能少的匹配**所搜索的字符串，而默认的贪婪模式则尽可能多的匹配所搜索的字符串。例如，对于字符串”oooo”，”o+?”将匹配单个”o”，而”o+”将匹配”oooo”。 |
| x\|y              | 匹配x或y。例如，”z\|food”能匹配”z”或”food”。”(z\|f)ood” 则匹配”zood”或”food”。 |
| [xyz]             | 字符集合。匹配所包含的任意一个字符。例如，”[abc]” 可以匹配”plain”中的”a”。Dart中不支持字符组嵌套，如：[[a-z]&&[^b]]。 |
| [^xyz]            | 排除字符集合。匹配未包含的任意字符。例如， “[^abc]” 可以匹配 “plain” 中的”p”。 |
| [a-z]             | 字符范围。匹配指定范围内的任意字符。例如，”[a-z]”可以匹配”a”到”z”范围内的任意小写字母字符。 |
| [^a-z]            | 排除字符范围。匹配任何不在指定范围内的任意字符。例如，”[^a-z]”可以匹配任何不在”a”到”z”范围内的任意字符。 |
| {n}               | n 是一个非负整数。匹配确定的n次。例如，”o{2}”不能匹配”Bob”中的”o”，但是能匹配”food”中的”oo”。 |
| {n,}              | n 是一个非负整数。至少匹配n 次。例如，”o{2,}”不能匹配”Bob”中的”o”，但能匹配”fooood”中的”oooo”。”o{1,}”等价于”o+”。”o{0,}”则等价于”o*”。 |
| {n,m}             | m 和 n 均为非负整数，其中n <= m。最少匹配 n 次且最多匹配 m 次。刘， “o{1,3}” 将匹配 “fooood” 中的”ooo”和”o”。”o{0,1}”等价于”o?”。请注意在逗号和两个数之间不能有空格。 |
| (pattern)         | 匹配pattern 并获取这一匹配。此时，整个表达式无论多复杂，都被视为一个单元。所获取的匹配可以从产生的 Match对象的group(id) 得到。要匹配圆括号字符，请使用 ‘\(‘ 或 ‘\)’。 |
| (?:pattern)       | 非匹配型括号。相比普通括号，非匹配型括号**只分组不捕获**。这样一是可以避免不必要的捕获，提高效率，二是可以让代码可以更加清晰[例如，在捕获字符串中某个名字的时候，可能是group(1)或2、3…使用非匹配型括号，可以仅返回group(1)]。 |
| (?=pattern)       | 肯定顺序环视。**环视匹配的是特定位置，不匹配任何字符，也就是并不会“占用”字符。**顺序环视从左到右匹配表达式，如果能匹配，则返回匹配成功信息。 |
| (?!pattern)       | 否定顺序环视。从左到右匹配表达式，如果不能匹配，则返回匹配成功信息。 |
| (?<=pattern)      | 肯定逆序环视，不支持。                                       |
| (?<!pattern)      | 否定逆序环视，不支持。                                       |
| (?>pattern)       | 固化分组，不支持。                                           |
| (?if then \|else) | 条件判断，不支持。                                           |
| (?<name>pattern)  | 捕获文本到命名组，不支持。                                   |
| (?#text)          | 注释，不支持。                                               |
| \w                | 匹配包括下划线的任何单词字符。等价于”[A-Za-z0-9_]”。         |
| \W                | 匹配任何非单词字符。等价于”[^A-Za-z0-9_]”。                  |
| \b                | 匹配单词（\w+）的边界（开始或末尾）。例如，对于字符串”never??!!”，”er\b”可以匹配”er”，”\bne”可以匹配”ne”。 |
| \B                | 匹配单词（\w+）的非末尾，对于非单词（\W、\W+）字符没有意义。例如，对于字符串”never??!!”，表达式”ev\B”能匹配”ev”；表达式”\?\B”能匹配两个”?”，”\?!\B”能匹配”?!”，此时\B没有意义。 |
| \cx               | 匹配由x指明的控制字符。例如，\cM匹配Control+M，等价于\r，\cI匹配\t，\cJ匹配\n,。x的值必须为A-Z或a-z之一。否则，将c视为一个原义的”c”字符。 |
| \d                | 匹配一个数字字符。等价于 [0-9]。                             |
| \D                | 匹配一个非数字字符。等价于 [^0-9]。                          |
| \f                | 匹配一个换页符。等价于 \x0c 和 \cL。                         |
| \n                | 匹配一个换行符。等价于 \x0a 和 \cJ。                         |
| \r                | 匹配一个回车符。等价于 \x0d 和 \cM。                         |
| \s                | 匹配任何空白字符，包括空格、制表符、换页符等等。等价于[ \f\n\r\t\v]。 |
| \S                | 匹配任何非空白字符。等价于[^ \f\n\r\t\v]。                   |
| \t                | 匹配一个制表符。等价于 \x09 和 \cI。                         |
| \v                | 匹配一个垂直制表符。等价于 \x0b 和 \cK。                     |
| \<                | 匹配单词开始位置，不支持。                                   |
| \>                | 匹配单词结束位置，不支持。                                   |
| \xn               | 匹配n，其中n为十六进制转义值。十六进制转义值必须为确定的**两个数字长**。例如，”\x41″匹配”A”。正则表达式中可以使用ASCII 编码。 |
| \int              | 反向引用，int为一个正整数。匹配之前获取的、表达式匹配的文本。例如：”(a)(b)(c)\1\2\3″能够匹配”abcabc”。**“\”元字符中，优先级为：反向引用>八进制Ascii值>原义字符****。** |
| \oct              | 标识一个八进制转义值或一个反向引用。如果之前至少 有otc个获取的子表达式，则otc为后向引用。否则，如果otc为八进制数字 (0-7)，则otc为一个八进制转义值（具体查看ASCII 编码）。 |
| \nm               | 标识一个八进制转义值或一个反向引用。如果 \nm 之前至少有nm个获取得子表达式，则nm为反向引用。如果 \nm之前至少有n个获取，则n为一个后跟文字m的反向引用。如果前面的条件都不满足，若n和m均为八进制数字(0-7)，则\nm将匹配八进制转义值nm（具体查看ASCII 编码）。 |
| \nml              | 如果 n 为八进制数字 (0-3)，且 m 和 l 均为八进制数字 (0-7)，则匹配八进制转义值 nml（具体查看ASCII 编码）。例如，”\101″能够匹配”A”。 |
| \un               | 匹配n，其中n是一个用四个十六进制数字表示的 Unicode字符。例如，”\u00A9″匹配版权符号”©”。 |
| [:flag:]          | 字符簇，如alnum、alpha、digit等，不支持。                    |
| \p{flag}          | Unicode属性、字母表和区块，如InHanzi、Letter、Number等，不支持。 |
| g、s、i、m、x、U  | 修饰符,用在正则表达式结尾，不支持。                          |
| 区分大小写        | 设置RegExp中可选参数为caseSensitive。                        |
| 多行匹配          | 当匹配的字符串是多行的时候，可设置RegExp可选参数multiLine。  |





# IO文件操作



文件操作
永远是最重要的内容之一

但看过文档，很多内容都是一目了然
这里简单的提一下目录、文件和链接（映射）的操作

这里补充一下同步和异步的内容
例子中的代码用到了async和await关键字
因为Directory.create函数是异步模式，返回值是Future

如果要等待函数执行完毕后，再执行之后的代码
那么一般有以下3种方法：
直接调用同步模式函数，如：Directory.createSync
将执行的之后的代码放到then函数中
使用关键字await，外层函数用async声明返回值为Future

举个例子，下面的代码中
fun1、fun2和fun3三个函数结果一样

```dart
import 'dart:io';

void main(){
  fun1();
  fun2();
  fun3();
}

void fun1() {
  var directory = new Directory("temp1");
  directory.createSync();
  //absolute返回path为绝对路径的Directory对象
  print(directory.absolute.path);
}

void fun2() {
  new Directory("temp2").create().then(
      (dir) => print(dir.absolute.path)
  );
}

//Dart中变量的类型可以省略，包括函数
fun3() async {
  var directory = await new Directory("temp3").create();
  print(directory.absolute.path);
}
```

运行结果：

```dart
E:\DartProject\Note16\temp1
E:\DartProject\Note16\temp2
E:\DartProject\Note16\temp3
```

注：
由于默认路径与使用dart命令的目录相关
建议在new Directory、File、Link或create link的时候
使用绝对路径，特别是项目中含有子目录的时候

下面的实例中使用的是第三种方法：
用await关键字来等待异步函数返回

另外，关于捕获文件操作过程中的异常
无非就是类似下面的用法
和通常的用法没有区别，这里不做讲解

```dart
final filePath = r"E:\back.txt";

try {
  File file = new File(filePath);
  file.writeAsString("$file");
} catch(e) {
  print(e);
}
```

## 1、目录

概要：

- 创建指定目录
- 重命名目录
- 删除目录
- 创建临时文件夹
- 获取父目录
- 列出目录的内容

```dart
import 'dart:io';
import 'dart:async';

void main() {
  handleDir();
}

handleDir() async {
  //可以用Platform.pathSeparator代替路径中的分隔符"/"
  //效果和"dir/subdir"一样
  //如果有子文件夹，需要设置recursive: true
  var directory = await new Directory("dir${Platform.pathSeparator}one").create(recursive: true);

  assert(await directory.exists() == true);
  //输出绝对路径
  print("Path: ${directory.absolute.path}");

  //重命名文件夹
  directory = await directory.rename("dir/subdir");
  print("Path: ${directory.absolute.path}\n");

  //创建临时文件夹
  //参数是文件夹的前缀，后面会自动添加随机字符串
  //参数可以是空参数
  var tempDir = await Directory.systemTemp.createTemp('temp_dir');
  assert(await tempDir.exists() == true);
  print("Temp Path: ${tempDir.path}");

  //返回上一级文件夹
  var parentDir = tempDir.parent;
  print("Parent Path: ${parentDir.path}");

  //列出所有文件，不包括链接和子文件夹
  Stream<FileSystemEntity> entityList = parentDir .list(recursive: false, followLinks: false);
  await for(FileSystemEntity entity in entityList) {

    //文件、目录和链接都继承自FileSystemEntity
    //FileSystemEntity.type静态函数返回值为FileSystemEntityType
    //FileSystemEntityType有三个常量：
    //Directory、FILE、LINK、NOT_FOUND
    //FileSystemEntity.isFile .isLink .isDerectory可用于判断类型
    print(entity.path);
  }

  //删除目录
  await tempDir.delete();
  assert(await tempDir.exists() == false);
}
```

运行结果：

```dart
Path: E:\DartProject\Note16\dir\one
Path: E:\DartProject\Note16\dir/subdir

Temp Path: C:\Users\King\AppData\Local\Temp\temp_dir7aa9c6f5-106b-11e6-a55f-ac220b7553ea
Parent Path: C:\Users\King\AppData\Local\Temp
C:\Users\King\AppData\Local\Temp\%@DH19{R%DFDKB85J)D~UR6.png
C:\Users\King\AppData\Local\Temp\0RCT3Y_IOUNT2`)27FJ9U`R.xml
C:\Users\King\AppData\Local\Temp\360newstmp.dat
……
```

## 2、文件

概要：

- 创建文件
- 将string写入文件
- 读取文件到String
- 以行为单位读取文件到List<String>
- 将bytes写入文件
- 读取文件到bytes
- 数据流Stream写入文件
- 数据流Stream读取文件
- 删除文件

```dart
import 'dart:io';
import 'dart:convert';
import 'dart:async';

void main() {
  //文件操作演示
  handleFile();
}

handleFile() async {
  //提示：pub中有ini库可以方便的对ini文件进行解析
  File file = new File("default.ini");

  //如果文件存在，删除
  if(!await file.exists()) {
    //创建文件
    file = await file.create();
  }

  print(file);

  //直接调用File的writeAs函数时
  //默认文件打开方式为WRITE:如果文件存在，会将原来的内容覆盖
  //如果不存在，则创建文件

  //写入String，默认将字符串以UTF8进行编码
  file = await file.writeAsString("[General]\nCode=UTF8");
  //readAsString读取文件，并返回字符串
  //默认返回的String编码为UTF8
  //相关的编解码器在dart:convert包中
  //包括以下编解码器：ASCII、LANTI1、BASE64、UTF8、SYSTEM_ENCODING
  //SYSTEM_ENCODING可以自动检测并返回当前系统编码
  print("\nRead Strings:\n${await file.readAsString()}");

  //以行为单位读取文件到List<String>，默认为UTF8编码
  print("\nRead Lines:");
  List<String> lines = await file.readAsLines();
  lines.forEach(
      (String line) => print(line)
  );

  //如果是以字节方式写入文件
  //建议设置好编码，避免汉字、特殊符号等字符出现乱码、或无法读取
  //将字符串编码为Utf8格式，然后写入字节
  file = await file.writeAsBytes(UTF8.encode("编码=UTF8"));
  //读取字节，并用Utf8解码
  print("\nRead Bytes:");
  print(UTF8.decode(await file.readAsBytes()));

//  //删除文件
//  await file.delete();
}
```

运行结果：

```dart
File: 'default.ini'

Read Strings:
[General]
Code=UTF8

Read Lines:
[General]
Code=UTF8

Read Bytes:
编码=UTF8
```

读写文件的话，常用的函数就是readAs和writeAs
但是如果我们要对某个字符进行处理，或读写某个区域等操作时
就需要用到open函数

open类型的函数有3个：
open({FileMode mode: FileMode.READ}) → Future<RandomAccessFile>
openRead([int start, int end]) → Stream<List<int>>
openWrite({FileMode mode: FileMode.WRITE, Encoding encoding: UTF8}) → IOSink

open和openSync一样，不过一个是异步、一个同步
可以返回RandomAccessFile类
openRead用于打开数据流
openWrite用于打开数据缓冲池
详细的内容可以查看API

## 3、链接

概要：

- 创建链接
- 获取链接文件的路径
- 获取链接指向的目标
- 重命名链接
- 删除链接

```dart
import 'dart:io';
import 'dart:async';

void main() {
  handleLink();
}

handleLink() async {
  //创建文件夹
  var dir = await new Directory("linkDir").create();
  //创建链接
  //Link的参数为该链接的Path，create的参数为链接的目标文件夹
  var link = await new Link("shortcut").create("linkDir");

  //输出链接文件的路径
  print(link.path);
  //输出链接目标的路径
  print(await link.target());

  //重命名链接
  link = await link.rename("link");
  print(link.path);

  //删除链接
  //link.delete();
}
```

运行结果：

```dart
shortcut
E:\DartProject\Note16\linkDir
link
```

需要说明的是，Dart中的Link
链接的是文件夹，而不能链接文件
并且，Dart中的链接和通常意义的快捷方式不同：

1. 首先，这里的链接只能指向文件夹
   快捷方式可以指向文件夹或文件
2. 链接不能剪切移动
   快捷方式可以剪切移动
3. 在命令行中，链接可以作为普通文件夹进行 cd、dir(ls)等操作
   快捷方式在命令行中可以看到，只是一个lnk文件，运行然后打开目标资源![img](http://www.cndartlang.com/wp-content/uploads/2017/08/180914rmbqnynv58qwqwy8.png)
4. 打开链接的时候，资源管理器的地址栏显示的是链接名
   而快捷方式打开的时候，资源管理器显示的是目标文件夹名
   ![img](http://www.cndartlang.com/wp-content/uploads/2017/08/181008uvnavwairrcljnn9.png)

## 4、数据流（读写文件实例：复制文件）

原本是准备在文件操作一节中提一下就完事的
但是测试了解下来，有点复杂，于是单独列一节

Stream是dart:async库中的类，并非dart:io
从它的位置可以看出，Stream是一个异步数据事件的提供者
它提供了一种接收事件序列（数据或错误信息）的方式

因此，我们可以通过listen来监听并开始产生事件
当我们开始监听Stream的时候，会接收到一个StreamSubscription对象
通过该对象可以控制Stream进行暂停、取消等操作

数据流Stream有两种类型：

- Single-subscription单一订阅数据流
- broadcast广播数据流

Stream默认关闭广播数据流，可以通过isBroadcast测试
如果要打开，需在Stream子类中重写 isBroadcast返回true
或调用asBroadcastStream

Single-subscription对象不能监听2次
**即使第1次的数据流已经被取消**

同时，为了保证系统资源被释放
在使用数据流的时候
必须等待读取完数据，或取消

关于数据流Stream，虽然抽象，但也不是不能理解
问题在于很多人不知道【Dart中】数据流的好处，何时该用
我所理解的是一般用于处理较大的连续数据，如文件IO操作

下面的实例是用数据流来复制文件，只能算是抛砖引玉吧！
File.copy常用来复制文件到某路径，但是看不到复制的过程、进度
这里用Stream来实现复制文件的功能，并添加进度显示的功能

```dart
import 'dart:io';
import 'dart:convert';
import 'dart:async';

void main() {
  //复制文件演示
  copyFileByStream();
}

copyFileByStream() async {
  //电子书文件大小：10.9 MB (11,431,697 字节)
  File file = new File(r"E:\全职高手.txt");
  assert(await file.exists() == true);
  print("源文件：${file.path}");

  //以只读方式打开源文件数据流
  Stream<List<int>> inputStream = file.openRead();
  //数据流监听事件，这里onData是null
  //会在后面通过StreamSubscription来修改监听函数
  StreamSubscription subscription = inputStream.listen(null);

  File target = new File(r"E:\全职高手.back.txt");
  print("目标文件：${target.path}");
  //以WRITE方式打开文件，创建缓存IOSink
  IOSink sink = target.openWrite();

  //常用两种复制文件的方法，就速度来说，File.copy最高效
//  //经测试，用时21毫秒
//  await file.copy(target.path);
//  //输入流连接缓存，用时79毫秒，比想象中高很多
//  //也许是数据流存IOSink缓存中之后，再转存到文件中的原因吧！
//  await sink.addStream(inputStream);

  //手动处理输入流
  //接收数据流的时候，涉及一些简单的计算
  //如：当前进度、当前时间、构造字符串
  //但是最后测试下来，仅用时68毫秒，有些不可思议

  //文件大小
  int fileLength = await file.length();
  //已读取文件大小
  int count = 0;
  //模拟进度条
  String progress = "*";

  //当输入流传来数据时，设置当前时间、进度条，输出信息等
  subscription.onData((List<int> list) {
    count = count + list.length;
    //进度百分比
    double num = (count*100)/fileLength;
    DateTime time = new DateTime.now();

    //输出样式：[1:19:197]**********[20.06%]
    //进度每传输2%，多一个"*"
    //复制结束进度为100%，共50个"*"
    print("[${time.hour}:${time.second}:${time.millisecond}]${progress*(num ~/ 2)}[${num.toStringAsFixed(2)}%]");

    //将数据添加到缓存池
    sink.add(list);
  });

  //数据流传输结束时，触发onDone事件
  subscription.onDone(() {
    print("复制文件结束！");
    //关闭缓存释放系统资源
    sink.close();
  });
}
```

运行结果：

```dart
源文件：E:\全职高手.txt
目标文件：E:\全职高手.back.txt
[1:19:177][0.57%]
[1:19:185][1.15%]
[1:19:186][1.72%]
[1:19:187]*[2.29%]
[1:19:187]*[2.87%]
……
[1:19:245]*************************************************[99.75%]
[1:19:245]**************************************************[100.00%]
复制文件结束！
```



# 数据库编程



Dart SDK中并没有原生的数据库驱动
但是无论是PostgreSQL还是SQLite还是MongoDB，都能找到对于的包
看了一下Pub和Github
不得不说，对数据库支持最好还是PostgreSQL和MongoDB



# Http响应和请求

dart:io中有Http请求的内容
这里再记录一下http这个包的简单用法

**pubspec.yaml**

```yaml
name: Note20
dependencies:
  http: any
```

**main.dart**

```dart
import 'package:http/http.dart' as Http;
import 'dart:convert';

main() async {
  /**
   * 向目标主机发送一次get请求
   * 自动创建临时Client，请求结束后自动删除
   * 如果要对主机发送多个请求，要手动new Client
   *
   * var client = new Client();
   * client.get(...)
   * client.put(...)
   *
   * 其他请求有：post put patch delete
   * 返回值为Future<Response>
   *
   * Http请求有可能返回Response，或者异常
   */
  String url = "http://www.cndartlang.com";
  Http.Response response = await Http.get(url);

  /**
   * 获取标签头字段的名称，并遍历输出
   * 获取标签头某个字段的值
   */
  response.headers.keys.forEach((str) => print(str));
  print("content-type = ${response.headers['content-type']}\n");

  /**
   * 获取响应对象的主体内容
   * Response.bodyBytes返回字节List
   *
   * Response.body返回String
   * 但仍然是通过对bodyBytes进行编码来实现
   * 调用Response.body的时候，默认以content-type的字段值来进行编码
   * 比如content-type = text/html; charset=utf-8，则采用UTF8进行编码
   *
   * 如果源码有无效的字符
   * 那么直接调用Response.body的时候出错，会显示错误：
   * FormatException: Bad UTF-8 encoding 0xfd
   * 那么我们就需要手动对字节进行编码
   * 设置allowMalformed为true，对无效或未完成的字符进行替换
   */
  List<int> bytes = response.bodyBytes;
  String utf8Body = UTF8.decode(bytes, allowMalformed: true).substring(0, 800);
  print(utf8Body + "\n");

  /**
   * read和readBytes作用为向主机发送get请求，但不返回Response
   * 返回值为Future<String>和Future<Uint8List>
   */
  print((await Http.read("http://www.baidu.com")).substring(0, 800) + "\n");

  /**
   * 设置请求的标签头
   * 通过修改User-Agent来模拟iPhone 6请求
   * 显示手机版百度首页
   *
   * 可以对比之前read的字符串
   */
  Http.get('http://www.baidu.com',
      headers: {'User-Agent': 'Mozilla/5.0 (iPhone; CPU iPhone OS 9_1 like Mac OS X) AppleWebKit/601.1.46 (KHTML, like Gecko) Version/9.0 Mobile/13B143 Safari/601.1'}).then((resp) {
    print(resp.body.substring(0, 800));
  });

}
```

**运行结果：**

略

# 获取环境变量及运行外部程序

本篇算是IO文件操作的补充
在写代码的时候，我们经常会用到一些路径或者系统值
也就是常说的环境变量
通常分为系统环境变量和用户环境变量

用户环境变量在Windows下面可以在系统属性中查看
这是用户自己设置的
常用的如：PATH、ANDROID_HOME等

也包含部分系统变量，如：windir、OS等
如果有相同的变量名，系统会先取用户变量、再取系统变量
如：TEMP、PATH等
详细的内容可以在命令行中用set命令查看
（Linux下为env和set命令）

注：
调用Ping的代码在Windwos中运行通过
Linux和Mac中未测试

**main.dart**

```dart
import 'dart:io';
import 'dart:convert';

main() async {
  print("操作系统：${Platform.operatingSystem}");
  print("CPU核数：${Platform.numberOfProcessors}");
  print("文件URI：${Platform.script}");
  print("文件路径：${Platform.script.toFilePath()}\n");

  if(!Platform.isWindows) {
    return;
  }

  //遍历所有环境变量
  Platform.environment.forEach((k, v) {
    print(k + "=" + v);
  });

  /**
   * 这里提一下start的命名可选参数：ProcessStartMode mode，有三个枚举值
   * ProcessStartMode.NORMAL，默认值
   * 新运行的程序作为主程序的子进程，并通过数据流stdin stdout stderr连接通信
   * ProcessStartMode.DETACHED
   * 创建一个独立的进程，与主进程无数据流连接，主进程只能获得子进程的pid
   * 关闭主进程后，对其没有影响
   * ProcessStartMode.DETACHED_WITH_STDIO
   * 创建一个独立的进程，但是与主进程可以通过数据流stdin stdout stderr连接
   *
   * Process.start的特点是可以通过数据流和子进程进行交互
   */

  Process.start("ping", ['www.baidu.com']).then((Process process) {
    // 如果原文有中文等特殊字符
    // 字节列表在转换为String的时候
    // 需要对字节列表进行重新编码
    process.stdout
        .transform(SYSTEM_ENCODING.decoder)
        //print(data)用于输出一行，stdout用于输出接收到的字节串
        .listen((data) => stdout.add(UTF8.encode(data)));
  });

  /**
   * 创建一个子进程，并且父进程和子进程不能交互
   * 之后返回运行结果
   * stdout和stderr默认编码为SYSTEM_ENCODING
   * 可在Process.run的命名可选参数中设置
   */
  Process.run('ping', ['www.cndartlang.com']).then((result) {
    stdout.write(result.stdout);
    stderr.write(result.stderr);
  });
}
```

**运行结果：**

略

具体代码如下：

```dart
import 'dart:io';

/**
* 2016.5.15 未按文件夹的命名来分析新版本
* v0.1.1 用文件最后修改时间来判断
*
* 包名可能有下面几种情况：
* _discoveryapis_commons-0.1.3
* route_hierarchical-0.6.1+1
* ace-0.5.10+20.12.14
* unittest-0.12.0-alpha.1
* webdriver-0.10.0-pre.10
* q-r.dart-master
*/

//保存最新版本包信息
Map<String, String> _saveMap = new Map();

//需要被删除包的信息
Map<String, List> _deleteMap = new Map();

//保存Pub缓存目录
String _cachePath = null;

String getPubPath() {
  var path = Platform.environment["PUB_CACHE"];

  if (path == null) {
    if (Platform.isWindows) {
      path = Platform.environment["APPDATA"];
      if(path == null) {
        noCache();
      }
      path = path + r"/pub/cache/hosted/pub.dartlang.org";
    } else {
      path = r"~/.pub-cache/hosted/pub.dartlang.org";
    }
  }

  if (!new Directory(path).existsSync()) {
    noCache();
  }

  print("[提示]Pub缓存目录：${path}");
  _cachePath = path;
  return path;
}

void noCache() {
  print(r"[错误]未找到Pub缓存目录！");
  exit(-1);
}

//判断包文件夹的名称是否规范
bool isValidFullName(String fullName) {
  if(new RegExp(r"^\D+$").hasMatch(fullName)
      || !new RegExp(r"(^.+)(-\d+\..*$)").hasMatch(fullName)) {
    return false;
  }

  return true;
}

//显示删除和保存包的分类信息
void showMaps() {
  print("------ Save ------");
  for(var key in _saveMap.keys) {
    print("${key}${_saveMap[key]}");
  }

  print("------ Delete ------");
  for(var key in _deleteMap.keys) {
    print("${key}${_deleteMap[key]}");
  }
}

/**
* 获取包的名称和版本号
* 包名称：list[0]
* 版本号：list[1]
*/
List<String> getLibNameAndVersion(String fullName) {
  return _getLibNameAndVersionByFullName(fullName);
}

List<String> _getLibNameAndVersionByFullName(String fullName) {
  Iterable<Match> matches = new RegExp(r"(^.+)(-\d+\..*$)").allMatches(fullName);
  for(var match in matches) {
    return [match.group(1), match.group(2)];
  }
}

List<String> _getLibNameAndVersionByYaml(String fullName) {

}

/**
* 默认通过pubspec.yaml的最后修改时间来判断
* 如果相同，则分析文件夹名称或解析yaml
*/
int compare(String name, String version) {
  int i =  _compareByModified(name, version);
  if(i == 0) {
    i = _compareByName();
  }
  if(i == 0) {
    i = _compareByYaml();
  }

  return i;
}

/**
* 对比最后修改时间
* 如果保存的版本比匹配到的新版本旧，那么用新版本覆盖
*/
int _compareByModified(String name, String version) {
  return new File(_cachePath + "/" + name + version + r"/pubspec.yaml")
      .lastModifiedSync()
      .compareTo(new File(_cachePath + "/" + name + _saveMap[name]).lastModifiedSync());
}

int _compareByYaml() {

}

int _compareByName() {

}

void manageMaps(String name, String version) {
  if (_saveMap[name] == null) {
    //如果没有该包的信息，直接保存
    _saveMap[name] = version;

  } else {
    //根据比较的结果，保存包信息
    int i = compare(name, version);
    if(_deleteMap[name] == null)
      _deleteMap[name] = new List();

    if( i > 0) {
      _deleteMap[name].add(_saveMap[name]);
      _saveMap[name] = version;
    } else {
      _deleteMap[name].add(version);
    }
  }
}

void deleteMaps() {
  _deleteMap.forEach((name, versions) {
    versions.forEach((version) {
      File file = new File(_cachePath + "/" + name + version);
      print("[Delete]${file.path}");
      file.delete(recursive: true);
    });
  });
}

void cleanCacheDir() {
  String path = getPubPath();
  Directory cacheDir = new Directory(path);

  //遍历所有文件夹
  List<FileSystemEntity> packageList = cacheDir.listSync();

  for (var i in packageList) {
    String packagePath = i.absolute.path;
    /**
     * 获取文件夹名称
     * FileUtils或Path中有方便的方法
     * String fullName = FileUtils.basename(packagePath);
     */
    String fullName = packagePath.substring(packagePath.lastIndexOf(new RegExp(r"[\/\\]")) + 1);

    if(!isValidFullName(fullName)) {
      //如果包没有版本数字或不按"-"+版本数字的规范命名
      //则直接将包保存到saveMap中
      _saveMap[fullName] = "";
    } else {
      List<String> name = getLibNameAndVersion(fullName);
      manageMaps(name[0], name[1]);
    }
  }

  showMaps();
  deleteMaps();
}

main() async {
  cleanCacheDir();
}
```



# Socket套接字

按照以往的经验，Socket可以简单的分为3类：
**流套接字（SOCK_STREAM）、数据包套接字（SOCK_DGRAM）**
**以及原始套接字（SOCK_RAW）**

其中**流套接字**使用了TCP协议
因此保证了面向连接、可靠的数据传输
实现数据的无差错、无重复发送，并按顺序接收

**数据包套接字**使用UDP协议
它提供了一种无连接的服务
但不能保证数据传输的可靠性，且无法保证顺序

**原始套接字**允许对较低层次的协议进行访问
比如IP、ICMP、IGMP协议
可以用来接收TCP/IP栈不能处理的IP包
或发送自定义包头、自定义协议的IP包
网络监听技术很大程度依赖原始套接字

接着，出于安全性方面的需要
在上面3类套接字之后，安全套接字应运而生

安全套接字使用TLS和SSL协议
始终对服务器进行认证，可选择的对客户端进行认证
使得客户端和服务器之间的的通信不会被攻击者嗅探、篡改
（早些年出现过利用DNS劫持再利用SSLStrip来进行嗅探）

要通过Internet进行通信，至少需要一对套接字
一个运行在服务器、一个运行在客户端
因此套接字也可简单的分为服务器端套接字和客户端套接字

大概提了一下之后，Dart中套接字的使用又如何呢？

在Dart中套接字主要有以下类：
Socket、ServerSocket
RawSocket、RawServerSocket
RawDatagramSocket

而下面的安全套接字则是继承上述对应的套接字
并且使用TLS和SSL协议
SecureSocket、SecureServerSocket
RawSecureSocket、RawSecureServerSocket
API很简单，就不一一写示例了

使用套接字进行数据处理有两种基本模式：同步和异步
而在Dart中使用异步编程非常简单、方便！

## 1、流套接字

Socket采用TCP协议
并且实现了Stream数据流和IOSink缓冲池的接口
使得Socket可以同其他的Stream一起使用，示例如下

注：
ServerSocket.bind返回的是Future<ServerSocket>
因此可以用下面的方法遍历连接到自身的所有套接字
await for(Socket socket in serverSocket)

**socket_server.dart**

```dart
import 'dart:io';
import 'dart:convert';

main() async {
  //绑定本地localhost的4041端口
  var serverSocket = await ServerSocket.bind(InternetAddress.LOOPBACK_IP_V4, 4041);

  //遍历所有连接到服务器的套接字
  await for(var socket in serverSocket) {
    //数据流Stream操作：监听接收到的数据
    socket.transform(UTF8.decoder).listen((data) {
      print("接收到来自Client的数据：" + data);
      print("向Client发送数据:Client 你好！");
      //缓冲池IOSink操作：写入数据
  socket.add(UTF8.encode('Client 你好！'));
    });
  }
}
```

**socket_client.dart**

```dart
import 'dart:io';
import 'dart:convert';

main() async {
  //连接服务器的4041端口
  var socket = await Socket.connect(InternetAddress.LOOPBACK_IP_V4, 4041);

  print("向Server发送数据:Server 你好！");
  socket.add(UTF8.encode('Server 你好！'));
  socket.transform(UTF8.decoder).listen((data) {
    print("接收到来自Server的数据：" + data);
  });
}
```

**运行结果：**

```bash
客户端
向Server发送数据:Server 你好！
接收到来自Server的数据：Client 你好！

服务端
接收到来自Client的数据：Server 你好！
向Client发送数据:Client 你好！
```

## 2、数据包套接字

RawDatagramSocket采用UDP协议
由系统暴露原始数据的事件信号
也就是说
数据包套接字不能监听数据，而是监听事件
当事件为RawSocketEvent.READ的时候
才能通过receive函数接收数据

同时，与一般编程语言不同的是
Dart中数据包套接字必须绑定地址和接口

注：
流套接字和原始套接字可以通过remoteAddress和remotePort获取地址和端口
因此客户端不用绑定地址和端口，仅通过服务器地址和端口创建套接字

**raw_datagram_socket_server.dart**

```dart
import 'dart:io';
import 'dart:convert';

main() async {
  /**
   * reuseAddress参数默认为true
   * 允许多个进程同时监听、绑定同一个端口
   */
  var rawDgramSocket = RawDatagramSocket.bind(InternetAddress.LOOPBACK_IP_V4, 4041);

  rawDgramSocket.then((socket) {
    //监听套接字事件
    socket.listen((event) {
      if(event == RawSocketEvent.READ) {
        print(UTF8.decode(socket.receive().data));
        socket.send(UTF8.encode("已收到！"), InternetAddress.LOOPBACK_IP_V4, 4042);
      }
    });
  });
}
```

**raw_datagram_socket_client.dart**

```dart
import 'dart:io';
import 'dart:convert';

main() async {
  var rawDgramSocket = RawDatagramSocket.bind(InternetAddress.LOOPBACK_IP_V4, 4042);

  rawDgramSocket.then((socket) {
    socket.send(UTF8.encode("你好！"), InternetAddress.LOOPBACK_IP_V4, 4041);

    socket.listen((event) {
      if(event == RawSocketEvent.READ) {
        print(UTF8.decode(socket.receive().data));
      }
    });
  });
}
```

**运行结果：**

```dart
客户端
已收到！

服务端
你好！
```

## 3、原始套接字

前面说过，原始套接字可以对低层的数据协议进行访问
比如IP、ICMP、IGMP协议
或发送自定义包头、自定义协议的IP包

这里就不举聊天的例子了
下面的实例是**获取、发送IP协议数据包**

**raw_server_socket.dart**

```dart
import 'dart:io';
import 'dart:convert';

/**
* 获取、发送IP协议数据包
* 浏览器访问http://localhost:4043
*/
main() async {
  var rawServerSocket = await RawServerSocket.bind(InternetAddress.LOOPBACK_IP_V4, 4043);

  rawServerSocket.listen((socket) {
    socket.listen((event) {
      if(event == RawSocketEvent.READ) {
        print(UTF8.decode(socket.read()));
        socket.write(UTF8.encode("""
        <!DOCTYPE html>
        <html>
          <head>
            <meta http-equiv="content-type" content="text/html;charset=utf-8">
          </head>
          
          <body>
            <h1>RawServerSocket 服务器</h1>
          </body>
        </html>
        """));
      }
    });
  });
}
```

浏览器访问http://localhost:4043

# Http Server

在dart:io中，HttpServer实现了一个基本的HTTP协议服务器
HTTP是基于请求与响应模式的、无状态的应用层协议

HTTP常基于TCP的连接方式
因此，可以通过HttpServer.listenOn(ServerSocket serverSocket)
来创建一个HttpServer用这种方法创建Http服务的时候
HttpServer.close并不会close ServerSocket

**hello_http_server.dart**

```dart
import 'dart:io';
import 'dart:convert';

main() async {
  //HttpServer对象绑定127.0.0.1的80端口
  var server = await HttpServer.bind(InternetAddress.LOOPBACK_IP_V4, 4041);
  //输出服务器地址端口
  print("Serving at ${server.address}:${server.port}");

  //HttpServer实现了Stream<HttpRequest>接口
  await for (HttpRequest request in server) {
    request.response
      /**
       * 设置ContentType
       * text/html 浏览器在获取文本后，自动调用html的解析器进行处理
       * text/plain 浏览器在获取文本后，不会对其进行处理
       *
       * 静态函数有：HTML TEXT JSON BINARY
       * ContentType.Text 等同 new ContentType("text", "plain", charset: "utf-8")
       */
      ..headers.contentType = new ContentType("text", "html", charset: "utf-8")
      ..write('Hello, ${await handleRequest(request) ?? 'name is null'}!')
      ..write('<br/><br/>--Dart语言中文社区')
      ..close();
  }
}

//获取表单的数据
handleRequest(HttpRequest request) async {
  //判断表单提交的方法
  switch(request.method) {
    case "GET":
    //Get方法比较简单，通过解析URI就可以得到
      return request.uri.queryParameters['name'];
    case "POST":
    /**
     * Post方法稍微麻烦一点
     * 首先，request传送的数据时经过压缩的
     * index.html中设置了utf8，因此需要UTF8解码器
     *
     * 表单提交的变量和值的字符串结构为：key=value
     * 如果表单提交了多个数据，用'&'对参数进行连接
     *
     * 对于提取变量的值，可以自行对字符串进行分析
     * 不过也有取巧的办法：
     * Uri.queryParameters(String key)能解析'key=value'类型的字符串
     *
     * Uri功能很完善，协议、主机、端口、参数都能简单地获取到
     * 其中，uri参数是用'?'连接的，例如：
     * http://www.baidu.com/s?wd=dart&ie=UTF-8
     * 因此，为了Uri类能正确解析，需要在表单数据字符串前加'?'
     */
      String strRaw = await request.transform(UTF8.decoder).join("&");
      String strUri = "?" + Uri.decodeComponent(strRaw);
      return Uri.parse(strUri).queryParameters['name'];
    default:
      return null;
  }
}
```

**web/index.html**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Note 24</title>
</head>
<body>
    <form action="http://localhost:4041" method="post">
        <input type="text" name="name"/>
        <input type="submit" value="发送">
    </form>
</body>
</html>
```

**运行结果：**

现实中的服务器并不是输出Hello World这么简单
还需要对流程进行控制，也就是路由功能
我们可以对HttpRequest.uri进行分析
当然，route包提供了更简洁的接口

**routing_requests.dart**

```dart
import 'dart:io';
import 'package:route/server.dart';
import 'package:route/url_pattern.dart';

/**
* UrlPattern是正则表达式的URL解析扩展类
* 用来对URI进行解析，其中参数分隔符默认为 '#'或'\'
* reverse函数可以生成对应参数的URI
*
* Examples:
*
*  var pattern = new UrlPattern(r'/app#profile/(\d+)');
*  pattern.matches('/app/profile/1234'); // true
*  pattern.matches('/app#profile/1234'); // true
*  pattern.reverse([1234], useFragment: true); // /app#profile/1234
*  pattern.reverse([1234], useFragment: false); // /app/profile/1234
*/

//查看全部文章
final postsUrl = new UrlPattern(r'/posts\/?');
//通过id查看某篇文章
final postUrl = new UrlPattern(r'/post/(\d+)\/?');

//查看全部文章请求的回调函数，如：/posts/
servePosts(req) {
  req.response.write("All blog posts");
  req.response.close();
}

//查看某篇文章的回调函数，如：/post/24
servePost(req) {
  var postId = postUrl.parse(req.uri.path)[0];
  req.response
    ..write('Blog post $postId')
    ..close();
}

//如果url不合法，返回Not found
serveNotFound(req) {
  req.response
    ..statusCode = HttpStatus.NOT_FOUND
    ..write('Not found')
    ..close();
}

main() async {
  var server = await HttpServer.bind(InternetAddress.LOOPBACK_IP_V4, 80);
  var router = new Router(server)
    /**
     * 连接HttpServer、URL表达式、回调函数
     * Router可以通过serve添加多个服务
     *
     * 但是如果UrlPattern相同，则后面监听的回调函数不会生效
     * ..serve(postUrl, method: 'GET').listen(servePost)  //有效
     * ..serve(postUrl, method: 'GET').listen(servePostTemp)  //无效
     */
    ..serve(postsUrl, method: 'GET').listen(servePosts)
    ..serve(postUrl, method: 'GET').listen(servePost)
    ..defaultStream.listen(serveNotFound);
}
```

**运行结果：**



# Http Client

## 1、HttpClient

在 [正则表达式实例](http://www.cndartlang.com/778.html) 那一节里面
使用dart:io中的HttpClient发送请求来采集数据，这一节再简单说一下
HTTP请求包括GET、PUT、POST、DELETE等
我们可以通过向服务器发送请求，来模拟登陆、上传、下载等操作

**upload_server.dart**

```dart
import 'dart:io';
import 'dart:convert';

main() async {
  var server = await HttpServer.bind(InternetAddress.LOOPBACK_IP_V4, 4049);

  await for (var req in server) {
    ContentType contentType = req.headers.contentType;

    if (req.method == 'POST' &&
        contentType != null &&
        contentType.mimeType == 'application/json') {
      try {
        var jsonString = await req.transform(UTF8.decoder).join();

        // 从URI中获取保存文件的文件名，file.txt
        var filename = req.uri.pathSegments.last;

        var file =  new File(filename);
        await file.writeAsString(jsonString, mode: FileMode.WRITE);

        //返回HTTP 200状态码
        req.response
          ..statusCode = HttpStatus.OK
          ..write("Server: 文件接收完毕！")
          ..close();

        print("文件保存成功：${file.absolute.path}");
      } catch (e) {
        req.response
          ..statusCode = HttpStatus.INTERNAL_SERVER_ERROR
          ..write("Server: 文件I/O操作出错: $e.")
          ..close();
      }
    } else {
      req.response
        ..statusCode = HttpStatus.METHOD_NOT_ALLOWED
        ..write("Server: 未支持的请求: ${req.method}.")
        ..close();
    }
  }
}
```

在HttpClient进行post、get等操作的时候
需要设置host、port和path
Http服务器一般http协议使用80端口，https协议使用443端口
Uri中具体的参数值可以参考下图
![img](http://www.cndartlang.com/wp-content/uploads/2017/08/201703x8cp8r8m0occ15k2.png)

**upload_client.dart**

```dart
import 'dart:io';
import 'dart:convert' show UTF8, JSON;

//需要以Post方式发送的数据
Map jsonData = {
  'name': 'Dart语言中文社区',
  'url': 'http://www.cndartlang.com',
  'date': new DateTime.now().toString(),
};

main() async {
  //path设置为'file.txt'，服务器中取值用作保存文件的文件名
  new HttpClient()
      .post(InternetAddress.LOOPBACK_IP_V4.host, 4049, '/file.txt')
      .then((HttpClientRequest request) {

    /**
     * 等同于以下写法：
     * request.headers.contentType = ContentType.parse("application/json; charset:utf-8");
     * request.headers.contentType =  new ContentType("application", "json", charset: "utf-8");
     * request.headers.contentType =  new ContentType("application", "json", charset: "utf-8");
     *
     * request.headers.add 作用为添加，如果变量名相同，会覆盖之前的值
     * request.headers.set 作用为清空headers，然后重新设置header
     */
    request.headers.contentType = ContentType.JSON;

    //写入数据
    request.write(JSON.encode(jsonData));

    return request.close();
  }).then((HttpClientResponse response) {

    if(response.statusCode == HttpStatus.OK) {
      print("Client: 文件上传成功！");
    }

    //接收服务器write的信息
    response.transform(UTF8.decoder).listen((String contents) {
      print(contents);
    });
  });
}
```

**运行结果：**
![img](http://www.cndartlang.com/wp-content/uploads/2017/08/201701uy1pyp7ydpi82ysp.png)
![img](http://www.cndartlang.com/wp-content/uploads/2017/08/201701oorukquuoupmolmj.png)

重要提示：
在使用HttpClient的时候
如果Response没有进行任何的读操作
如listen、transform、toList、toSet、any之类
那么程序会一直阻塞，Response不会被释放
如果不需要读取数据，可以调用drain函数将数据丢弃
（感谢 **雲，** 的测试 2016.5.26）

## 2、Http

除了SDK中的API，官方还出了http库
提供了常用方法来简化操作
特点是可以运行在客户端 package:http/browser_client.dart
以及服务器端 package:http/http.dart
感兴趣的可以查看pub或github

由于http库简化了操作
在细节处理方面就不及dart:io中的HttpClient了
下面通过http库和HttpClient来下载mp3文件
看两者有什么区别

mp3的地址随便找了一个
（资源不稳定，有可能请求失败，抛出异常，多请求几次试试）
http://www.baidu190.com/md5/A0D8D48BD70AC99C7C6E34B5A4B7C9FA.mp3
然后试着访问，看了一下发送的包
虽然源地址是以”.mp3″作为后缀，但是实际上并不是mp3文件
而是text/html数据，返回302状态码，经过了3次跳转
最后mp3的实际地址是
http://58.16.42.71:9999/other.web.re01.sycdn.kuwo.cn/bc9d5ab895cc4435fc977c7965f160e9/57457828/resource/n3/10/63/3640333498.mp3
通过Get请求，返回200状态码
接着分段获取数据，过程中返回206状态码
![img](http://www.cndartlang.com/wp-content/uploads/2017/08/201702jsi2i4kqw5500bw9.png)

这个跳转的过程让人有点烦躁
重要的是，如果要模拟发送请求
是按照跳转的顺序，不停的来回请求、响应
还是一步到位？看下面的代码

**http_download.dart**

```dart
import 'package:http/http.dart' as Http;
import 'dart:io';

void main() {
  //通过Http库提供的api来下载文件
  downloadByHttp();

  //通过HttpClient提供的api来下载文件
  downloadByHttpClient();
}

void downloadByHttp() {
  //创建File对象，如果文件已经存在，则删除
  var file = new File("Http_Download.mp3");
  if(file.existsSync()) {
    file.deleteSync();
  }
  //以FileMode.APPEND模式写入数据
  IOSink ios = file.openWrite(mode: FileMode.APPEND);

  /**
   * 请求的Header在get(.., headers:...)中进行设置
   * headers的类型为Map<String, String>
   */
  Http.get("http://www.baidu190.com/md5/A0D8D48BD70AC99C7C6E34B5A4B7C9FA.mp3")
      .then((Http.Response response) {

    //输出响应的状态码、Header以及数据大小
    print("State Code: ${response.statusCode}");
    print(response.headers);
    print("Date Size: ${response.bodyBytes.length}");

    //将接收到的数据写入缓冲池
    ios.add(response.bodyBytes);

    //响应结束后，关闭数据缓冲池
  }).whenComplete(() => ios.close());
}

void downloadByHttpClient() {
  var file = new File("IO_Download.mp3");
  if(file.existsSync()) {
    file.deleteSync();
  }
  IOSink ios = file.openWrite(mode: FileMode.APPEND);

  new HttpClient()
      .getUrl(Uri.parse("http://www.baidu190.com/md5/A0D8D48BD70AC99C7C6E34B5A4B7C9FA.mp3"))
      //不用进行设置请求的Header、写入数据等操作，因此直接close
      .then((HttpClientRequest request) => request.close())
      .then((HttpClientResponse response) {
    print(response.headers);
    response.listen((List event) {

      //接收到的数据为List<int>，写入缓冲池
      ios.add(event);
      //输出状态码和接收到的数据大小
      print("State Code: ${response.statusCode} Data Size: ${event.length}");

      //响应结束后，关闭数据缓冲池
    }, onDone: () => ios.close());
  });
}
```

**运行结果：**
![img](http://www.cndartlang.com/wp-content/uploads/2017/08/201702hnugkygkkzkxfm3h.png)
![img](http://www.cndartlang.com/wp-content/uploads/2017/08/201702d4zh94z4sc350f4d.png)

第一个是用http库来下载数据
第二个是用dart:io中的HttpClient下载数据

幸运的是，不管是http还是HttpClient
对于302、206等响应状态
都能自动进行跳转、接收数据等处理

不过还是有两点明显的区别：

**一是如果要关闭自动跳转**
http库中get、post等函数就实现不了
需要创建Http.Request，然后设置followRedirects
而HttpClient会返回HttpClientRequest，可以直接设置followRedirects

**二是在接收数据的时候**
http库中的get、post等函数是同步模式
会等待数据加载结束，再一次返回
而HttpClient是异步模式，会在每次加载数据之后调用回调函数
看运行结果就可以看出来

http库中如果要对每次请求的数据进行操作
需要创建Http.StreamedRequest，然后对变量stream数据流进行处理
而HttpClient如果要一次读取所有的数据
创建一个缓存存放临时数据，然后在onDone事件中读取
或者await IOSink.addStream(response)
如果不需要对每个响应的数据进行处理
IOSink.add(response)这句代码真的很简洁

总的来说，就普通Http的Server-side请求而言
两者区别不大
调用过程都很简单直接

**但是各项功能HttpClient实现起来更加的精练，代码更统一**
**http针对不同的环境会用到不同的类和方法，略显复杂**

不过在http/browser_client.dart中
http库提供了BrowserClient类，应用于Client-side浏览器客户端
实现了部分异步请求XMLHttpRequest的功能

至于取舍，哪个库方便好用
看你个人喜好，以及实际应用吧！



# WebSocket套接字

[Socket套接字](http://www.cndartlang.com/841.html)一节里面提到了常用的3种套接字
初显出Dart在服务器端的简单、高效以及功能强大
这里补充一下WebSocket的内容

WebSocket属于HTML5一种新的协议
实现了浏览器和服务器的全双工通信
而在这之前
真正的即时通信只有Flash Socket才能做到

在Dart中，有两个WebSocket
一个在dart:io中，运行在Dart VM虚拟机中
一个在dart:html中，运行在Web App浏览器中

因为一开始的握手要借助HTTP请求来完成
因此，WebSocket的服务端也就是一个Http Server

## 1、dart:io WebSocket

**websocket_server.dart**

```dart
import 'dart:io';

main() async {
  try {
    var server = await HttpServer.bind(InternetAddress.LOOPBACK_IP_V4, 4040);
    await for (HttpRequest req in server) {
      if (req.uri.path == '/ws') {
        // 将一个HttpRequest提升为WebSocket连接
        var socket = await WebSocketTransformer.upgrade(req);
        socket.listen((event) {
          print("接收到来自 ${req.connectionInfo.remoteAddress.address}:${req.connectionInfo.remotePort} 的消息：${event}");

          socket.add("数据已接收！");
        });
      }
    }
  } catch (e) {
    print(e);
  }
}
```

**websocket_client.dart**

```dart
import 'dart:io';

main() async {
  var socket = await WebSocket.connect('ws://127.0.0.1:4040/ws');
  socket.add('Hello, World!');
  socket.listen((event) {
    print("Server: $event");
  });
  socket.close();
}
```

**运行结果：**
![img](http://www.cndartlang.com/wp-content/uploads/2017/08/232938h4hdt4gyo5njksdk.png)
![img](http://www.cndartlang.com/wp-content/uploads/2017/08/232938a7y1z0m11d1zj1g7.png)

通过上面的代码可以看出，WebSocket的API并没有特别的地方
功能上也并不是很出彩，dart:io中套接字的接口已经很完善了
如果要使用WebSocket作为客户端运行在Dart VM虚拟机中
除非有特别的需求，否则实在想不出必须这样做的理由

## 2、dart:html WebSocket聊天室

dart:io中的WebSocket重点在于提供服务器端的功能API
dart:html中的WebSocket重点在于提供浏览器端的套接字接口
WebSocket的优势应该在于浏览器客户端

下面是聊天室的简单实例

**pubspec.yaml**

```yaml
name: Note26
dependencies:
  browser: any
```

**bin/ws_server.dart**

```dart
import 'dart:io';
import 'dart:convert';

main() async {
  //HTTP服务器，绑定IP和端口
  var server = await HttpServer.bind(InternetAddress.LOOPBACK_IP_V4, 4040);

  //存储客户端名称和套接字
  Map<String, WebSocket> chatClients = new Map<String, WebSocket>();

/////////////////////////////////////////////
  /**
   * 通过遍历Map
   * 向所有在线客户端发送最新的列表
   *
   * 数据使用Json格式
   * {
   *   'code': 命令
   *   'data': 在线列表的数据为List，接收客户端名称和发送消息为Map
   * }
   */
  void sendList() {
    chatClients.values.forEach((socket) {
      socket.add(JSON.encode({
        'code': "list",
        'data': chatClients.keys.toList()
      }));
    });
  }

  /**
   * 遍历Map，发送消息
   * 此时的String data已经经过JSON编码
   */
  void sendSpeak(String data) {
    chatClients.values.forEach((socket) {
      socket.add(data);
    });
  }
/////////////////////////////////////////////

/////////////////////////////////////////////
  /**
   * 处理服务器接收到的请求
   *
   * 主要流程：
   *   1、将HttpRequest请求提升为WebSocket链接
   *   2、监听客户端发送到服务器的数据（连接和发送消息）
   *   3、客户端关闭连接的时候，删除Map中存储的客户端数据，并更新最新列表
   */
  await for (HttpRequest req in server) {
    if (req.uri.path == '/ws') {
      // 将一个HttpRequest提升为WebSocket连接
      var socket = await WebSocketTransformer.upgrade(req);

      //昵称、IP、端口，用于服务器输出
      String name = '';
      String address = req.connectionInfo.remoteAddress.address;
      int port = req.connectionInfo.remotePort;

      //监听客户端发送过来的数据
      socket.listen((event) {
        var message = JSON.decode(event);

        //connect和speak数据都使用了Map
        if(message is! Map) {
          return;
        }

        switch(message['code']) {
          case 'connect':
            name = message['data']['name'];
            //保存套接字，更新在线列表
            chatClients[name] = socket;
            print("客户端连接 昵称：$name IP：$address 端口：$port");
            sendList();
            break;
          case 'speak':
            //发送消息
            print(message['data']);
            sendSpeak(event);
            break;
          default:
            print("接收到Message Data：${event.data}");
        }
      });

      //客户端关闭的时候，删除存储信息，更新在线列表
      socket.done.whenComplete(() {
        chatClients.remove(name);
        print("客户端连接断开连接 昵称：$name IP：$address 端口：$port");
        sendList();
      });
    }
  }
/////////////////////////////////////////////
}
```

**web/index.dart**

```dart
import 'dart:html';
import 'dart:convert';

/**
* 客户端类
*
* 主要流程：
*   1、查找HTML中各个DOM控件对象
*   2、监听昵称输入框（回车，并且值改变）
*     2.1 清空在线列表、消息输入框、消息显示框
*     2.2 连接WebSocket服务器，监听onOpen、onMessage、onClose事件
*     2.3 服务器发送来的Message有两个命令：list、speak，分别进行处理
*   3、监听消息输入框，向WebSocket服务器发送数据
*/
class Client {
  WebSocket ws;
/////////////////////////////////////////////
  //登陆界面
  DivElement enterScreen = querySelector("#enter_screen");
  //昵称输入框
  InputElement nameElement = querySelector("#name");

  //聊天界面
  DivElement chatScreen = querySelector("#chat_screen");
  //消息输入框
  InputElement inputElement = querySelector("#speak");
  //消息显示框
  DivElement context = querySelector("#context");
  //在线列表
  DivElement chatListElement = querySelector("#nameslist");
/////////////////////////////////////////////

  String chatName;

  Client() {
    //监听昵称输入框
    initNameListen();
    //监听消息输入框
    initInputListen();
  }

  void initNameListen() {
    nameElement.onChange.listen((e) {
      //取消Dom事件的默认动作
      e.preventDefault();

      //清空DOM控件
      addNames([]);
      context.children.clear();
      inputElement.value = "";

      //连接服务器
      ws = new WebSocket("ws://127.0.0.1:4040/ws");

      ws.onOpen.listen((e) {
        print("已连接服务器 ${ws.url}");
        //保存昵称
        chatName = nameElement.value;
        //显示聊天界面
        initHTML(true);

        //向服务器发送客户端信息
        var newUser = {'code': 'connect', 'data': {'name': chatName}};
        //消息输入框设置为可用
        inputElement.disabled = false;
        ws.send(JSON.encode(newUser));
      });

      //监听接收到的Message数据
      ws.onMessage.listen(onMessage);

      //关闭事件，显示登陆界面，隐藏聊天界面，清空昵称输入框
      ws.onClose.listen((e) {
        print("服务器连接已断开");
        initHTML(false);
        nameElement.value = '';
      });

      //终止事件的派发
      e.stopPropagation();
    });
  }

  void onMessage(MessageEvent event) {
    Map message = JSON.decode(event.data);

    switch(message['code']) {
      case 'speak':
        addText(message['data']['name'], message['data']['text']);
        break;
      case 'list':
        addNames(message['data']);
        break;
      default:
        print("接收到Message Data：${event.data}");
    }
  }

  void initInputListen() {
    //当输入框数据改变并回车的时候，触发onChange事件
    inputElement.onChange.listen((e) {
      //待发送的消息
      String value = inputElement.value;
      inputElement.value = '';

      print(value);
      var request = {
        'code': 'speak',
        'data': {
          'name': chatName,
          'text': value
        }
      };
      //发送数据
      ws.send(JSON.encode(request));
    });
  }

/////////////////////////////////////////////
  //刷新消息显示框
  void addText(String name, String text) {
    var result = new DivElement();
    result.innerHtml = "$name : $text";
    context.children.add(result);
  }

  //刷新在线列表
  void addNames(List names) {
    print("刷新在线用户列表：$names");

    chatListElement.children.clear();
    for(var name in names) {
      addName(name);
    }
  }

  void addName(String name) {
    var result = new DivElement();
    result.innerHtml = "$name";
    chatListElement.children.add(result);
  }
/////////////////////////////////////////////

  /**
   * isConnected
   *   true：隐藏登陆界面，显示聊天界面
   *   false：显示登陆界面，显示聊天界面
   */
  void initHTML(bool isConnected) {
    if(isConnected) {
      enterScreen.style.display = "none";
      chatScreen.style.display = "block";
      inputElement.focus();
    } else {
      enterScreen.style.display = "block";
      chatScreen.style.display = "none";
      nameElement.focus();
    }
  }
}

void main() {
  var client = new Client();
}
```

**web/index.html**

```html
<!DOCTYPE html>

<html>
  <head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">

    <title>Note 26</title>

    <script type="application/dart" src="index.dart"></script>
    <script src="packages/browser/dart.js"></script>

    <link rel="stylesheet" href="index.css">
  </head>

  <body>
    <h1>Note 26：WebSocket Chat Example</h1>

    <div id="enter_screen">
      <div class="box">
        <input class="form-control speak"  placeholder="请输入昵称" type="text" value="" id="name" />
      </div>
    </div>

    <div id="chat_screen" style="display: none;">
          <div class="box mheight">
            <h3>在线列表</h3>
            <div id="nameslist"></div>
          </div>

          <div id="context"></div>

          <input class="form-control speak" type="text" value="" id="speak" disabled/>
    </div>

  </body>
</html>
```

**web/index.css**

```css
#context-wrapper {
  min-height: 400px;
}

#context {
  min-height: 220px;
  bottom: 0;
}

.box {
  border: 1px solid #d8d8d8;
  border-radius: 3px;
  background-color: #fff;
  border-bottom-width: 2px;
  margin-right: 20px;
  padding: 20px 20px;
}

.mheight {
  min-height: 200px;
  float:left;
}
```

**运行结果：**
[![img](http://www.cndartlang.com/wp-content/uploads/2017/08/232940lxxkgd9ooxiq3vzd.gif)](http://www.cndartlang.com/wp-content/uploads/2017/08/232940lxxkgd9ooxiq3vzd.gif)

HTTP的问题主要在于基于请求—响应模式
服务端不能主动推送数据到客户端
为了保证数据实时同步，实际应用中常使用长轮询来工作
但每次通信都需要一个HTTP请求
如果数据交互频繁、服务器就需要处理很多的HTTP请求
而且，HTTP请求头中会发送Cookie等一些不必要的数据
往往造成Header比数据本身还多，大部分还是重复无用的
这使得效率偏低不说，还浪费大量的宽带

而WebSocket使得浏览器中也可以使用套接字
并且很友好的完成长连接的全双工通信
重要的是稳定，长轮询在遇到网络问题后
想要在不刷新页面的情况下恢复通信，很难
而WebSocket提供了onClose等事件，为通信提供了保障
不过WebSocket的最大问题还是浏览器的支持，兼容性是硬伤

如果你项目面向的用户群使用的浏览器大部分为低版本IE
就避免使用WebSocket
另外，WebSocket是长连接
如果客户端的程序没有数据实时同步的需求
也没有必要使用WebSocket
因为长连接会带来一定的服务器内存开销
然后，如果Isolate、Rpc或异步请求能轻松搞定问题的话
就完全没必要兴师动众的折腾WebSocket
（http库的BrowserClient提供有部分异步请求的功能）

最后，Force库是一个不错的Web实时通信框架
基于WebSocket和Polling长轮询实现
感兴趣的可以看看Pub或Github

dart:io这部分内容终于告一段落了，散花！

# 异步编程

Dart是一个单线程编程语言，如果进行I/O操作或等待一个耗时的操作时，程序会阻塞。异步编程让程序在运行的时候不会被阻塞卡死，而在Dart中则使用Future对象来实现异步操作。

1 Async和await
2 Async*，sync*，and all the rest
….2.1 同步生成器：sync*
….2.2 异步生成器：async*
….2.3 yield*
3 Future API

下面是一段同步代码，表示读取每日新闻摘要、彩票中奖号码、天气预报、棒球比分：

```dart
printDailyNewsDigest() {
  String news = gatherNewsReports(); // 需要花费一点时间
  print(news);
}

printWinningLotteryNumbers() {
  print('Winning lotto numbers: [23, 63, 87, 26, 2]');
}

printWeatherForecast() {
  print('Tomorrow\'s forecast: 70F, sunny.');
}

printBaseballScore() {
  print('Baseball score: Red Sox 10, Yankees 0');
}

main() {
  printDailyNewsDigest();
  printWinningLotteryNumbers();
  printWeatherForecast();
  printBaseballScore();
}
```

当收集新闻报道的时候，gatherNewsReports会使程序阻塞，无论花多长时间，必须取得返回值后才能继续运行，这使得用户被动等待，非常不友好，这时候就需要用到Future。

Future表示在将来的某个时候获取到一个值，当返回值为Future类型的函数被调用时，Dart会做两件事：
1、将要做的计算工作添加到函数队列中，并返回一个未完成的Future对象。
2、接下来，如果计算的值有效（包括异常），Future以该值完成计算。

要获取Future完成计算的值，可以使用下面2中方法：
1、使用async和await关键字
2、使用Future API

## 1、Async和await

从Dart 1.9开始，Dart添加了async、await关键字实现异步的功能。async和await关键字使得异步代码的编写非常简单，就和同步代码一样，并不使用Future API。如：

```dart
String getVersion() => '1.0.0';
```

改写为异步代码，需在大括号或 => 前添加async关键字。async关键字并不属于识别标志的一部分，它只是一个实现细节。从调用者的角度，调用async函数和传统函数并没有区别。对于函数return的类型并没有产生影响，最终都将封装为Future。同样，如果函数中throw异常，也会封装成Error Future。Dart中可以忽略函数返回值类型，但这并不推荐：

```dart
Future<String> getVersion() async => '1.0.0';
```

如果函数中，return的类型为T，那么函数的返回类型应该为Future<T>，或T的父类，否则会产生静态警告。如果函数中return的类型是Future<T>，那么函数的返回类型同样为Future<T>，而不是Future<Future<T>>。

之前的同步代码，输出函数可以如下进行改写：

```dart
Future printDailyNewsDigest() async {
  String news = await gatherNewsReports();
  print(news);
}

Future gatherNewsReports() async {
  String path =
      'https://www...';
  return (await HttpRequest.getString(path));
}
```

在main函数中，printDailyNewsDigest是第一个调用的函数，但由于异步运行，信息可能最后打印，下面是函数运行的顺序：
![img](http://www.cndartlang.com/wp-content/uploads/2017/08/233019lge0fwg55g03h993.png)

需注意的是，如果async函数没有return一个值，那么Dart将自动封装一个null值的Future。同时，await表达式可以使用多次，但只能在async标记的函数中使用。await声明的函数会进入阻塞状态，一直等待到完成计算，返回对应的值。使用await声明的表达式，等同于同步代码，可以用try-catch-finally捕获异常。

在await表达式中，表达式的值通常是Future，如果不是Future对象，则将该值自动封装为Future并添加到事件循环中，然后等待Future完成，如：await 1+1; 该特性使await表达式的行为拥有更好的可预见性，这也是Dart和其他语言类似功能的不同点之一。例如：如果一个循环中有一个无条件的await表达式，该特性可以确保表达式在每次循环的时候都被挂起。

在Stream数据流中，异步模式下，通常在then函数中对接收到的数据进行处理，如果要改为同步代码，除了调用同步模式的函数API外，还可以使用await for关键字，对数据进行遍历，如：

```dart
await for (var request in requestServer) {
  handleRequest(request);
}
```

## 2、Async*, sync*, and all the rest

前一节讨论了async函数和await表达式，这些功能是Dart中，初步支持异步编程和生成器的一部分。

在Dart 1.9中引入了函数生成器的概念，利用惰性函数计算结果序列，以提升性能。生成器有两种类型：同步生成器和异步生成器。同步生成器在需要的时候才生成值，然后使用者从生成器中拉取。异步生成器会以它自身的速度生成值，然后推送到使用者可以使用的地方。

为什么要支持生成器呢？有人可以自己实现，但那无疑是复杂而且冗长乏味的。Dart中内置的生成器使一切变得相当简单，而不用考虑去自定义可迭代类，实现moveNext()、current等成员函数、功能。

### 2.1 同步生成器：sync*

通过在函数主体前添加sync*可以快速标记该函数为同步生成器synchronous generator，解除很多手动定义可迭代类时复杂的公式化代码。假设我们要获取第一个自然数到n：

```dart
Iterable naturalsTo(n) sync* {
  print("Begin");

  int k = 0;
  while (k < n) yield k++;

  print("End");
}

main() {
  var it = naturalsTo(3).iterator;
  while(it.moveNext()) {
    print(it.current);
  }
}
```

当调用naturalsTo的时候，会立即返回Iterable（很像async函数立即返回Future），并且可以通过Iterable提取iterator。在有代码调用moveNext前，函数主体并不会执行。yield（生成）用于声明一个求值表达式，当第一次运行函数的时候，代码会执行到yield关键字声明的位置，并暂停执行，moveNext会返回true给调用者。函数会在下次调用moveNext的时候恢复执行。

**运行结果：**

```dart
Begin
0
1
2
End
```

当循环结束的时候，函数会隐式地执行return，这会使迭代终止，moveNext返回false。当然，作为一个专业的、实现了完整Iterable类API的迭代器和可迭代类来说，使用普通迭代器，这是非常冗长乏味的。

sync 和 * 可以分开，他们是不同的标记。如果你之前的代码使用了sync关键字，升级Dart并不会产生兼容性问题，sync并不是真正的预留字，并同样适用于async、await和yield。这些关键字仅在async函数和generator生成器函数中作为预留字（即：函数用async、sync*、async*标记）。

### 2.2 异步生成器：async*

异步生成器使用数据流来异步生成序列，通过在函数体前添加async*标识符来进行标记。这里同样用生成自然数来举例：

```dart
import 'dart:async';

Stream asynchronousNaturalsTo(n) async* {
  print("Begin");

  int k = 0;
  while (k < n) yield k++;

  print("End");
}

main() {
  asynchronousNaturalsTo(3).listen((v) {
    print(v);
  });
}
```

**运行结果：**

```dart
Begin
0
1
2
End
```

当运行asynchronousNaturalsTo的时候，会立即返回Stream，但函数体并不会执行，就像sync*生成器和async函数一样。一旦开始listen监听数据流，函数体会开始执行。当执行到yield关键字的时候，会将yield声明的求值表达式的计算结果添加到Stream数据流中。异步生成器没有必要暂停，因为数据流可以通过StreamSubscription进行控制。需注意的是，数据流不能重复监听。

```dart
import 'dart:async';

Stream get asynchronousNaturals async* {
  print("Begin");

  int k = 0;
  while (k < 3) {
    print("Before Yield");
    yield k++;
  }

  print("End");
}

main() {
  StreamSubscription subscription = asynchronousNaturals.listen(null);
  subscription.onData((value) {
    print(value);
    subscription.pause();
  });
}
```

**运行结果：**

```dart
Begin
Before Yield
0
Before Yield
```

上面的例子中，asynchronousNaturals用于获取小于3的自然数。async*等标识符同样适用于get函数。同时，与async*函数相关联的Stream可能会被pause暂停或cancel取消。如果被取消，控制权会转让给最近括起来从句末尾，即函数执行完毕。如果被暂停，函数会一直执行到yield声明的位置，然后暂停，直到Stream恢复。

### 2.3 yield*

之前的代码中，yield使虽然用起来确实有吸引力，但在写递归函数的时候，也可能遇到一些问题。下面是从大到小获取自然数的例子：

```dart
import 'dart:async';

Iterable naturalsDownFrom(n) sync* {
  if (n > 0) {
    yield n;
    for (int i in naturalsDownFrom(n-1)) { yield i; }
  }
}

main() {
  print(naturalsDownFrom(3));
}
```

**运行结果：**

```dart
(3, 2, 1)
```

由于每次调用sync*函数都会构建一个新的序列，因此在仅使用yield的情况下，只能遍历新构建的序列，通过yield将元素插入到当前序列中：

```dart
for (int i in naturalsDownFrom(n-1)) { yield i; }
```

上面的代码在功能上没什么问题，但我们来分析一下过程：
当**n=3**时，只执行了一次yield n;；
当**n=2**时，yield n;和yield i;分别执行了一次，并且执行的yield i;是上一层n=3时候的代码；
当**n=1**时，执行了一次yield n;，两次yield i;，执行的yield i;是n=2和n=3时候的代码。

在代码中添加提示信息可以看得更明白：

```dart
int level = 0;

Iterable naturalsDownFrom(n) sync* {
  level++;

  if (n > 0) {
    print("level: $level n:$n");
    yield n;

    for (int i in naturalsDownFrom(n-1)) {
      print("level: $level i:$i");
      yield i;
    }
  }
}
```

**运行结果：**

```dart
level: 1 n:3
level: 2 n:2
level: 2 i:2
level: 3 n:1
level: 3 i:1
level: 3 i:1
(3, 2, 1)
```

为避免和变量名混淆，用x表示自然数的位置（即第x个自然数），y表示yield i;执行的次数，即x=1时，y=0；x=2时，y=1；x=3时，y=2。y的值为0,1,2……x-1。

总的来说，因为递归将元素插入序列的原因，共执行了x(x-1)/2次yield i;，时间复杂度为O(n^2)，很明显地存在性能问题，而yield*则是为了解决此问题而设计的。yield*后面必须跟其他的（子）序列，并且会将后面（子）序列的所有元素插入到当前正在创建的序列中。

于是，代码可以修改如下：

```dart
Iterable naturalsDownFrom(n) sync* {
  if ( n > 0) {
    yield n;
    yield* naturalsDownFrom(n-1);
  }
}
```

在sync*函数中，子序列必须是一个Iterable可迭代对象；在async*函数中，子序列必须是一个Stream数据流。子序列可以为空，当为空的时候，yield*会跳过该表达式，并且不会暂停。

## 3、Future API

Future表示一个延迟的计算过程、任务，但并不立即执行。Future可以注册回调函数来处理计算的返回值或异常。API中关于Future的用法很多，通常使用构造函数来封装同步代码，如：

```dart
Future myFunc() {
  return new Future(() {
    //Do something
    return result;
  });
}
```

API中，Future的构造函数很丰富，如：

```dart
Future(dynamic computation())
Future.delayed(Duration duration, [dynamic computation()])
Future.sync(dynamic computation())
Future.value([value])
```

在网上的教程中，也有用Compeleter来实现异步函数的：

```dart
Future<String> myFunc() {
  Completer<String> comp = new Completer<String>();
  //Do something
  comp.complete("...");

  return comp.future;
}
```

但是并不推荐，通常Completer用于自定义Class中。如果你想从零开始创建一个Future，从基于回调函数的API转化成基于Future，可以如下使用Completer：

```dart
class AsyncOperation {
  Completer _completer = new Completer();

  Future<T> doOperation() {
    _startOperation();
    return _completer.future; // Send future object back to client.
  }

  // Something calls this when the value is ready.
  void _finishOperation(T result) {
    _completer.complete(result);
  }

  // If something goes wrong, call this.
  void _errorHappened(error) {
    _completer.completeError(error);
  }}
```

在使用Future API编写异步代码的时候，then函数用于注册Future完成计算时调用的回调函数，当Future完成计算时触发。如果有expensiveA、expensiveB、expensiveC三个异步函数，要实现三个函数依次运行，A执行完后运行B，B执行完后运行C，由于then函数返回值为Future，因此可以多次调用then形成Future链：

```dart
expensiveA()
  .then((_) => expensiveB())
  .then((_) => expensiveC());
```

注意：在then注册的回调函数的函数中，return的值会被封装为Future返回，如果没有return，会默认返回一个空值的Future。

上面的代码中，参数使用了下划线 _ ，其他语言可能表示占位符，但是在Dart中理解为占位符并不准确。应该说是习惯将 _ 视作占位符，Dart只是简单的将它视作一个变量，并可以在函数中使用：

```dart
print(_.runtimeType);
```

如果要调用多个异步函数，一并返回完成值，可以使用Future.wait，当所有Future完成后，返回List值列表：

```dart
Future.wait([expensiveA(), expensiveB(), expensiveC()])
      .then((List responses) => chooseBestResponse(responses));
```



# Event Loop事件循环

目录
1 基本概念
2 如何调度任务
….2.1 事件队列：new Future()
….2.2 微任务队列：scheduleMicrotask()
3 练习测试

本文将介绍Dart中的事件循环体系，你会从中了解到如何调度Future任务，以及预测任务执行的顺序，以便更好地编写异步代码，减少意外情况的出现。

## 1、基本概念

事件循环的任务是从事件队列中一次取出一项并处理，并且只要队列有内容，就会重复这两步工作。消息队列的内容可能是用户输入、文件I/O通知、定时器等等。而你可能是从其他的非Dart语言中，所了解熟悉的这些内容：
![img](http://www.cndartlang.com/wp-content/uploads/2017/08/220308nlfj4ux3c4kfuyk0.png)

在Dart中，略有区别。Dart应用程序在Main Isolate执行main函数之后开始运行，当main函数执行完毕退出后，Main Isolate的线程开始一个个的处理应用程序的事件队列中的内容。下图是简化的流程：
![img](http://www.cndartlang.com/wp-content/uploads/2017/08/220307iejjjl6d6oy9jpoy.png)

Dart应用程序仅有一个消息循环，以及两个队列：event事件队列和microtask微任务队列。事件队列包含所有的外部事件，如：I/O、鼠标事件、定时器、Isolate之间的消息等等。微任务队列是非常有必要的，因为在返回操作到事件循环前，事件处理代码有时候需要完成某个工作任务。例如，当一个可观察的对象发生改变的时候，它会将多个变化组织到一起，以异步的方式提交报告。微任务队列允许可观察对象在DOM能够显示不一致状态之前，提交改变。

事件队列包含Dart和系统产生的事件。目前，微任务队列的入口点仅来自Dart代码内部。如下图显示，当main函数exit后，事件循环开始工作。首先，按FIFO（先进先出）的方式执行所有微任务。接着，从事件队列中提取消息并一条条处理。然后再执行所有的微任务，以此循环，直到所有队列为空，没有新预计的事件发生为止。（如果Web应用的用户关闭窗口，Web应用可能在队列清空之前退出）
![img](http://www.cndartlang.com/wp-content/uploads/2017/08/220307oqz67vqin2piiz0c.png)

需要注意，当事件循环在处理微任务队列的时候，事件队列会被卡住，应用程序无法处理鼠标单击、I/O消息等事件。

虽然可以预测任务执行的顺序，但是我们无法预测事件循环什么时候会从队列中提取任务。Dart事件处理系统基于单线程循环，而不是基于时基（tick，系统的相对时间单位）或者其他的时间度量。例如，当你创建一个延迟任务的时候，事件会在你指定的时间插入到队列末尾，并且在队列中之前的事件未处理之前，会一直排队等待，微任务队列同样如此。

**通过Future链指定任务执行顺序**

如果你的代码有依赖关系，请显式地指定、明确他们执行的顺序，而不是依赖事件队列的顺序，避免出现意料之外的情况，因为Dart中事件队列的实现有可能发生变化。同时，也使其他开发者更容易理解你的代码。

下面是一段错误的代码：

```dart
// 错误，因为并没有明确设置和使用变量之间的依赖关系
future.then(...set an important variable...);
Timer.run(() {...use the important variable...});
```

应该用下面的方式代替，then()是更好的选择：

```dart
future.then(...set an important variable...)
  .then((_) {...use the important variable...});
```

如果你想执行代码，即使发生错误的时候，可以使用whenComplete()代替then()。如果使用变量比较耗时，可以考虑将代码放到新的Future中。

```dart
future.then(...set an important variable...)
  .then((_) {new Future(() {...use the important variable...})});
```

上面的代码使用新的Future来使用变量，给事件循环一个机会来处理事件队列中的其他事件。下一节会详述延迟运行调度代码的内容。

## 2、如何调度任务

当你需要指定稍后执行某段代码，可以使用dart:async包中提供的如下API：
1、Future类，可添加一个事件到事件队列的末尾；
2、顶层函数 scheduleMicrotask()，可添加一个任务到微任务队列的末尾。

一般情况下，我们通过Future来使用事件队列来调度任务。因为要处理完微任务队列之后才会处理事件队列，所以尽量使用事件队列可以使微任务队列更短，降低事件队列卡死的可能性。

如果一个任务必须在所有的事件队列之前处理，你应该立即执行该任务。如果不能立即执行，可以使用scheduleMicrotask()将代码添加到微任务队列。例如，在Web应用中，在事件队列之前执行微任务，来避免过早地释放或结束一个IndexedDB事务处理等等。
![img](http://www.cndartlang.com/wp-content/uploads/2017/08/220308xe9c7jc0j7quqacc.png)

### 2.1 事件队列：new Future()

插入事件队列可以使用new Future()或new Future.delayed()，这是dart:async中定义的两个Future的构造函数。

提示：你也可以Timer来调度任务，但是如果出现未捕获的异常，应用程序会立即退出。当然，更建议的是使用Future。Future是在Timer的基础上构建的，并添加了检测任务完成、响应错误异常等功能。

立即将一条内容添加到事件队列，使用new Future()：

```dart
// Adds a task to the event queue.
new Future(() {
  // ...code goes here...
});
```

在之后的某一时间将内容加入事件队列，使用new Future.delayed()：

```dart
// After a one-second delay, adds a task to the event queue.
new Future.delayed(const Duration(seconds:1), () {
  // ...code goes here...
});
```

虽然上面的代码在1s后将任务添加到了事件队列，但要等到main Isolate为空闲状态，微任务队列为空，且事件队列之前的任务执行完毕之后，该任务才会执行。例如，如果main函数或事件处理程序正在执行一个耗时的计算，任务并不会立即执行，直到计算完成。在这种情况下，延迟事件可能远远超过1s。

提示：如果你在Web应用中为动画绘制帧，不要使用Future、Timer或Stream。而是使用animationFrame，这是requestAnimationFrame的Dart接口。

**Future几点注意事项：**
1、当Future完成计算后，then()注册的回调函数会立即执行。需注意的是，then()注册的函数并不会添加到事件队列中，回调函数只是在事件循环中任务完成后被调用。
2、如果Future在then()被调用之前已经完成计算，那么任务会被添加到微任务队列中，并且该任务会执行then()中注册的回调函数。
3、Future()和Future.delayed()构造函数并不会立即完成计算。
4、Future.value()构造函数在微任务中完成计算，其他类似第2条。
5、Future.sync()构造函数会立即执行函数，并在微任务中完成计算，其他类似第2条。

关于第2条，可以参考下面的代码：

```dart
import 'dart:async';

main() async {
  Future f1 = new Future(() => null);
  Future f2 = new Future(() => null);
  Future f3 = new Future(() => null);

  f3.then((_) => print("f3 then"));

  f2.then((_) {
    print("f2 then");    
    new Future(() => print("new Future befor f1 then"));
    f1.then((_) {
      print("f1 then");
    });
  });
}
```

上面的代码中new Future(() => null)和new Future(null)有本质上的区别，一个上函数体为空，什么都不做，一个是参数为空，不存在函数。

**运行结果：**

```dart
f2 then
f1 then
f3 then
new Future befor f1 then
```

首先，f1、f2和f3会将任务添加到事件队列中，而且then()注册的函数并不会被添加到队列，也不会直接运行。接着完成计算，在f2.then中，new Future会将任务添加到事件队列，f1因为已经完成计算，因此f3.then会将任务添加到微任务队列，先于new Future打印信息。

### 2.2 微任务队列：scheduleMicrotask()

scheduleMicrotask()是dart:async中定义的顶层函数，可以调用如下：

```dart
scheduleMicrotask(() {
  // ...code goes here...
});
```

**如果有必要的话，使用Isolate和Worker**

要是你有一个计算密集型的任务需要运行怎么办？为了使应用程序保持响应，应该将任务放入Isolate或Worker中。Isolate可能运行在一个单独的进程或线程中，这取决于Dart的具体实现。并且，你可以在Dart Web应用中使用dart:html的Worker来添加一个JavaScript worker。

那么应该使用多少Isolate隔离区？对于计算密集型任务，一般隔离区的数量取决于你CPU有多少可用。如果只是纯粹地计算，很多额外的隔离区只是浪费资源。然而，如果隔离区执行异步调用，如执行I/O操作，并不会花费很多的时间在CPU上，那么比CPU更多的隔离区也是合情合理的。你可以使用比CPU数量更多的隔离区，如果你的应用有一个完善良好的体系结构。例如，你可能会为每一个功能创建一个单独的隔离区，或当你确保不需要共享数据的时候。

## 3、练习测试

**练习一**：下面代码的输出是什么？

```dart
import 'dart:async';
main() {
  print('main #1 of 2');
  scheduleMicrotask(() => print('microtask #1 of 2'));

  new Future.delayed(new Duration(seconds:1),
                     () => print('future #1 (delayed)'));
  new Future(() => print('future #2 of 3'));
  new Future(() => print('future #3 of 3'));

  scheduleMicrotask(() => print('microtask #2 of 2'));

  print('main #2 of 2');
}
```

**运行结果：**

```dart
main #1 of 2
main #2 of 2
microtask #1 of 2
microtask #2 of 2
future #2 of 3
future #3 of 3
future #1 (delayed)
```

上面的代码分三个顺序执行：
1、main()函数中的代码
2、微任务队列中的任务（scheduleMicrotask()）
3、事件队列中的任务（new Future() 或 new Future.delayed()）

注：在main()函数中，从开始到结束的调用都是同步模式，其中第一个调用的是print()，然后是scheduleMicrotask()、new Future.delayed()、new Future()……而scheduleMicrotask()、Future.delayed()、new Future()中作为参数的回调函数，会延迟调用。

**练习二**：接下来是一个稍复杂的例子，如果你能正确的预测代码的输出，奖励一朵小红花！

```dart
import 'dart:async';

main() {
  print('main #1 of 2');
  scheduleMicrotask(() => print('microtask #1 of 3'));

  new Future.delayed(new Duration(seconds:1),
      () => print('future #1 (delayed)'));

  new Future(() => print('future #2 of 4'))
      .then((_) => print('future #2a'))
      .then((_) {
    print('future #2b');
    scheduleMicrotask(() => print('microtask #0 (from future #2b)'));
  })
      .then((_) => print('future #2c'));

  scheduleMicrotask(() => print('microtask #2 of 3'));

  new Future(() => print('future #3 of 4'))
      .then((_) => new Future(
      () => print('future #3a (a new future)')))
      .then((_) => print('future #3b'));

  new Future(() => print('future #4 of 4'));
  scheduleMicrotask(() => print('microtask #3 of 3'));
  print('main #2 of 2');
}
```

**运行结果：**

```dart
main #1 of 2
main #2 of 2
microtask #1 of 3
microtask #2 of 3
microtask #3 of 3
future #2 of 4
future #2a
future #2b
future #2c
microtask #0 (from future #2b)
future #3 of 4
future #4 of 4
future #3a (a new future)
future #3b
future #1 (delayed)
```

就像之前说的，main()函数先执行，然后是微任务队列，以及事件队列。下面是3个要注意的地方：
1、Future 3的then函数中，回调函数返回新的Future，它将创建一个新任务#3a，并添加到事件队列中，因为之后的then执行的前提是Future完成计算，由于这时候微任务队列为空，因此#3a、#3b一次性执行完。
2、所有then()注册的回调函数，当Future完成后立即调用执行，因此Future 2、2a、2b、2c在返回控制权前一次性执行完。
3、如果将Future 3中then的代码由.then((_) => new Future( () => print(‘future #3a (a new future)’)))改为.then((_){ new Future( () => print(‘future #3a (a new future)’));})因为没有return语句，这时候回调函数返回的是Null Future，Future 3a会添加到事件队列，而不会立即执行。

下面是代码所属队列的图示：
![img](http://www.cndartlang.com/wp-content/uploads/2017/08/220308yxtxlz90xllb0vxx.png)

## 总结

当你调度任务的时候，尽量遵循下列规则：

- 尽量使用事件队列（new Future() 或new Future.delayed()）
- 当需要指定任务顺序的时候，使用Future.then()或whenComplete()
- 为避免事件队列“饥饿锁死”，尽量使微任务简短
- 为使应用程序保持响应，避免在事件循环中添加计算密集型代码
- 执行计算密集型代码的时候，另创建Isolate或Worker



# Future和异常处理



在旧式风格的同步代码中，你可以通过try-catch代码块捕获异常，始终保证程序不会完全地崩溃，例如：

```dart
try {
  throw new Exception("Oh, error!");
} catch (e) {
  logger.log(e); 
}
```

在Dart中，Future是一个特别重要的概念，并且无处不在。由于Future会将任务添加到事件队列，因此计算过程中产生的异常并不在当前代码块中，以致try-catch并不能捕获Future中的异常。

```dart
try {
  /**
  * 抛出异常，程序崩溃
  * try-catch不能捕获Future异常
  */
  new Future.error("Oh, error!"); 
} catch (e) {
  logger.log(e); 
}
```

当然，你可以使用Future.catchError来捕获信息。当调用Future，并且计算正常返回值的时候，then()注册的回调函数会被触发；如果计算或then()注册的回调函数返回异常，catchError()会被触发。

![img](http://www.cndartlang.com/wp-content/uploads/2017/08/140453eyey5yil555p9yly.png)

异常可能发生在计算过程或回调函数中，并且当计算的过程中出现异常的时候，then()注册的回调函数并不会被触发：

```dart
myFunc().then(processValue)
        .catchError(handleError);
```

我们可以通过myFunc().catchError()仅捕获计算过程中的异常，如果我们要处理计算返回的值，并且区分异常产生的位置，可以设置then()的onError参数：

```dart
myFunc()
  .then(successCallback, onError: (e) {
    ...
  })
  .catchError(handleError);
```

上面的代码中，计算过程中出现的异常由catchError注册的回调函数处理，而then()中产生的异常则由onError注册的回调函数处理。

对于捕获到的异常事件，可以在catchError注册的毁掉函数中进行判断，然后进行不同的操作。不过Future提供了更优雅的方式，通过可选参数bool test(Object error)来进行分类，当测试函数返回true的时候，执行对应的回调函数：

```dart
myFunc()
    .then(...)
    .catchError(handleFormatException,
                test: (e) => e is FormatException)
    .catchError(handleAuthorizationException,
                test: (e) => e is AuthorizationException);
```

如果说then().catchError()相当于try-catch的镜像函数的话，whenComplete()则等同于finally。无论是正常返回值还是出现异常，都会执行注册的回调函数：

```dart
var server = connectToServer();
server.post(myUrl, fields: {"name": "john", "profession": "juggler"})
      .then(handleResponse)
      .catchError(handleError)
      .whenComplete(server.close);
```

then()、catchError()、whenComplete()返回的值都是Future，因此可以继续注册then、catchError、whenComplete，形成Future链，如：

```dart
myFunc()
    .then((_) => new Future.error("Complete with Error #1"))
    .whenComplete(() => print("Done"))
    .then((_) => print("whenComplete future compelete"))
    .catchError((e) => print(e));
```

首先，第一个then会产生一个异常”Error #1“，并返回Future。由于注册了whenComplete，因此无论计算过程是否正常都会执行回调函数，在这个时候返回值仍然是**Error Future**，接下来，由于存在异常，之后的then并不会执行，最后由catchError捕获。输出结果为：

```bash
Done
Complete with Error #1
```

但在过程中可能又会产生新的异常，如果出现新的**Error Future**，则会以新的Error完成Future，即覆盖之前的Error：

```dart
myFunc()
    .then((_) => new Future.error("Complete with Error #1"))
    .whenComplete(() => new Future.error("Complete with Error #2"))
    .then((_) => print("whenComplete future compelete"))
    .catchError((e) => print(e));
```

输出结果为：

```dart
Complete with Error #2
```

**在处理异常的时候还需要意以下2种情况：**

### 情况一：异常的处理函数必须在Future完成之前注册，避免Future出现Error并抛出异常的时候，捕获异常的回调函数还没有注册成功

例如：

```dart
import 'dart:async';

myFunc() async {
  throw new Exception("Dart Exception");
  return "Dart";
}

main() {
  Future future = myFunc();

  new Future.delayed(const Duration(milliseconds: 500), () {
    future.then((value) => print(value))
        .catchError((e) => print(e));
  });
}
```

上面模拟注册延迟500毫秒，myFunc()被调用前，catchError并未成功注册，异常未正常捕获，如果将myFunc()放到Future.delayed()，问题解决：

```dart
main() {
  new Future.delayed(const Duration(milliseconds: 500), () {
    myFunc().then((value) => print(value))
        .catchError((e) => print(e));
  });
}
```

### 情况二：意外地混合同步类型和异步类型的异常

首先需要注意，throw new Exception产生的是同步类型的异常，如果只是字符串，等同于throw(“…”)或throw “…”，而new Future.error产生的是异步类型的异常。通常，我们写的是同步代码：

```dart
import 'dart:async';

String obtainFilePath(fileName) {
  return "D:/" + fileName;
}

String parseFileData(filePath) {
  return filePath + "\nFileData:CNDartLang";
}

Future parseAndRead() {
  String filePath = obtainFilePath("Temp.txt");
  return new Future(() {
    return print(parseFileData(filePath));
  });
}

main() {
  parseAndRead().catchError((e) {
      print(e);
    });
}
```

上面的代码中，obtainFilePath用于获取文件路径，parseFileData用于解析文件内容，两个都是同步函数，代码只有return，简单模拟一下过程。parseAndRead是异步函数，返回值是Future，因此main函数中，parseAndRead可以调用then、catchError等函数。

在parseAndRead函数返回值的时候，return print(…)代码中的return不能省略，逻辑为：**如果计算成功，则返回一个空值的**Future**，如果**print(…)**中抛出异常，则返回Error Future**。如果不return，则会返回一个空值的Future，外层的catchError不能捕获到异常信息。

运行结果：

```dart
D:/Temp.txt
FileData:CNDartLang
```

在obtainFilePath和parseFileData两个函数中，有可能抛出同步类型异常，因为parseFileData封装到了Future中，当出现异常的时候，会抛出Error Future，parseAndRead能够正常捕获到，封装到Future.then中同样会抛出Error Future，如：

```dart
String parseFileData(filePath) {
  throw("Parse FileData Error");
  return filePath + "\nFileData:CNDartLang";
}
```

但是如果异常出现在obtainFilePath中，则会出错，因为catchError不能捕获同步类型的异常：

```dart
String obtainFilePath(fileName) {
  throw("Obtain Error");
  return "D:/" + fileName;
}

//Unhandled exception:
//Obtain Error
//...
```

**解决方案有三个：**

### 1、使用Future.sync对代码进行封装

当回调函数返回值为非Future的时候，该函数返回对应值的Future；当回调函数抛出异常，该函数返回值为异常的Future；当回调函数返回Future的时候，包括Error Future，该函数返回Future。之前的代码中，parseAndRead可以封装如下：

```dart
Future parseAndRead() {
  return new Future.sync(() {
    String filePath = obtainFilePath("Temp.txt");
    return new Future(() {
      return print(parseFileData(filePath));
    });
  });
}
```

### 2、使用async关键字对函数进行声明，自动将同步类型代码封装为Future

```dart
parseAndRead() async {
  String filePath = obtainFilePath("Temp.txt");
  return new Future(() {
    return print(parseFileData(filePath));
  });
}
```

### 3、使用Zone对parseAndRead出现的异常进行捕获

下一篇文章会对Zone作详细介绍：

```dart
runZoned(() {
  parseAndRead().catchError((e) {
    print(e);
  });
}, onError: (e) {
  print(e);
});
```



# Zone分区

目录
1 基础知识
2 处理异步类型的错误信息
3 在分区中使用Stream数据流
4 存储zone-local value 分区本地值
….示例：在调试日志中使用分区本地值
5 Zone的重要功能
….示例一：重写print()函数
….示例二：委托父分区
….示例三：进入和离开分区时执行代码
….示例四：操作回调函数
6 总结

本文将以顶层函数runZoned()为焦点，介绍dart:async中和Zone有关的API。在这之前，你应该已经熟悉Future和异常处理。

目前，最常见的使用Zone的情况是在异步代码中处理错误或异常信息。下面的代码是一个简单的Http服务器的实现，通过在分区中运行Http服务器，确保程序继续运行，尽管未在异步代码中捕获错误消息：

```dart
runZoned(() {
  HttpServer.bind('0.0.0.0', port).then((server) {
    server.listen(staticFiles.serveRequest);
  });
},
onError: (e, stackTrace) => print('Oh noes! $e $stackTrace'));
```

通常上面的代码并不总是需要使用分区，而Zone分区可能在下面的任务中使用：
1、保护你的程序因为异步代码抛出的异常未捕获而退出，如前面的示例代码。
2、与其他分区关联数据（分区本地值，类似于静态变量）。
3、在部分或全部代码中，优先于一系列受限的函数（如print()、scheduleTask()等）。
4、在每次代码进入或退出分区的时候，执行某操作，如开始或停止定时器、保持堆栈信息等。

你可能在其他语言中遇到过类似Zone的东西，而Dart中Zone的灵感确实来自于Node.js中的Domain，和Java中的线程本地存储TLS也有很多相似的地方。

## 1、基础知识

分区表示一个调用的异步、动态的范围。它包含一个计算过程（注册的匿名函数），并作为调用的部分被执行，但是该任务并不添加到事件循环中。

例如，在Http服务器的例子中，bind()、then()和then()注册的回调函数在同一个分区中被执行，该分区通过runZoned()创建。

在下面的示例中，代码在3个不同的分区中执行：zone #1（root zone）、zone #2和zone #3。

```dart
import 'dart:async';

main() {
  foo();
  var future;
  runZoned(() {          // Starts a new child zone (zone #2).
    future = new Future(bar).then(baz);
  });
  future.then(qux);
}

foo() => ...foo-body...  // Executed twice (once each in two zones).
bar() => ...bar-body...
baz(x) => runZoned(() => foo()); // New child zone (zone #3).
qux(x) => ...qux-body...
```

下图显示了代码执行的顺序，以及代码执行的Zone分区：
![img](http://www.cndartlang.com/wp-content/uploads/2017/08/203439lcw3degnotxav3ca.png)

每次调用runZoned()都会创建一个新的分区，并执行分区中的代码。如果在代码中调度任务（如调用baz()），**任务会在被调度代码所在的分区中执行**。例如，在调用qux()（main函数中的最后一行）的时候，尽管then()附属于future，并且future在Zone #2中计算，但qux()依旧运行在Zone #1（根分区）中。

子分区并不会完全地替换父分区，新的分区会被嵌套进当前分区中。例如，Zone #2包含Zone #3，Zone #1（根分区）同时包含Zone #2和Zone #3。

需要注意的是，Dart代码虽然有可能在其他嵌套的子分区中执行，但是在最底层，未新建分区的时候，代码总是在root Zone根分区中运行。

## 2、处理异步类型的错误信息

开始的时候提到过，分区经常用来处理异步代码中的异常，这类似于同步代码中的try-catch。一般情况下，未捕获的异常是代码中throw语句主动地向上层抛出，由上层调用者处理。另一种未捕获的异常是通过new Future.error()或Completer的completeError()函数生成。

通常，我们使用runZoned()的命名参数onError来设置错误处理器，当分区中存在未捕获的异常时，异步错误处理器被调用。例如：

```dart
runZoned(() {
  Timer.run(() { throw 'Would normally kill the program'; });
}, onError: (error, stackTrace) {
  print('Uncaught error: $error');
});
```

上面的代码中，通过Timer.run()注册一个异步的回调函数并抛出异常 。正常情况下，如果没有设置异常处理器，该未捕获的异常会被抛出，直到顶层。在独立的Dart应用中，正在运行的进程会被Kill强制终止。

**需要注意try-catch和Zone异常处理器之间的区别**。Zone也可以捕获同步类型的异常，但是在Zone中，抛出异常位置之后的代码并不会执行，但是之前调度的异步回调函数会继续执行。因此，Zone的异常处理器可能会被调用多次。并且，处理的异常可能也来自与子分区或后代分区。为了明确异常的来源以及处理方式，建议使用catchError()捕获Future中的异常。如果异常到达Zone的边界而未被捕获，它将被视为未处理的异常被Zone抛出。

**重要提示一：异常不会由外部跨进到分区**

下面的代码中，第一行代码引发的异常信息不会跨越到分区中：

```dart
import 'dart:async';

main() {
  var f = new Future.error(499);
  f = f.whenComplete(() { print('Outside runZoned'); });
  f.catchError((e) => print(e));
  runZoned(() {
    f = f.whenComplete(() { print('Inside non-error zone'); });
  });
  runZoned(() {
    f = f.whenComplete(() { print('Inside error zone (not called)'); });
  }, onError: print);
}
```

**如果运行代码，会出现下面的提示信息：**

```dart
Outside runZoned
499
Inside non-error zone
Unhandled exception:
499
...stack trace...
```

如果删除第二个runZoned()的错误处理器onError，**运行结果如下**：

```dart
Outside runZoned
499
Inside non-error zone
Inside error zone (not called)
Unhandled exception:
499
...stack trace...
```

注意，由上面的代码可以观察到，删掉runZoned的错误处理器后，会导致异常继续传递。但是，如果runZoned设置了错误处理器，那么注册的回调函数并不会执行，并且立即抛出异常，最终打印堆栈的错误信息。造成的原因是因为异常发生在Zone外部，错误并不会由外部跨越进Zone分区中。如果你将整个代码段添加到带错误处理器的分区中，则可以避免出错。

**重要提示二：异常不会跳离出分区**

像之前代码所显示的，异常不会由外部跨进到分区。同样，错误也不会调理出包含错误处理器的分区：

```dart
import 'dart:async';

main() {
  var completer = new Completer();
  var future = completer.future.then((x) => x + 1);
  var zoneFuture;
  
  runZoned(() {
    zoneFuture = future.then((y) => throw 'Inside zone');
  }, onError: (error) {
    print('Caught: $error');
  });
  
  zoneFuture.catchError((e) { print('Never reached'); });
  completer.complete(499);
}
```

尽管Future链的末尾设置了catchError()，异步类型的异常也没有跳出带错误处理器的分区，而是由分区的错误处理器进行处理。因此，上面代码中的zoneFuture肯定不会完成，既不是Value Future，也不是Error Future。

## 3、在分区中使用Stream数据流

相对于Future来说，Stream在Zone中的使用更简单。在Zone中，数据流在监听的时候进行转换或执行回调函数。遵循该规则的数据流在listen监听前，应该没有任何副作用。该情况类似于同步代码中的Iterable，在你取值前，不会进行任何的求值计算。下面是通过runZoned使用数据流的例子：

```dart
var stream = new File('stream.dart').openRead()
    .map((x) => throw 'Callback throws');

runZoned(() { stream.listen(print); },
         onError: (e) { print('Caught error: $e'); });
```

上面的代码中，通过回调函数抛出异常，并且被runZoned的错误处理器捕获。map()回调函数与分区中的listen有关，至于在何处设置map()并没有影响。输出结果应该为：

```dart
Caught error: Callback throws
```

## 4、存储zone-local value分区本地值

如果你想使用静态变量，但是担心被多个并发运行的计算影响，可以考虑使用分区本地值。当然，也可以通过分区本地值来进行代码调试。另一个用途是处理HTTP请求，通过分区本地值中进行授权操作。

在runZoned()中，可以使用zoneValues参数来存储新建分区中使用的值，而zoneValues的类型为Map：

```dart
runZoned(() {
  print(Zone.current[#key]);
}, zoneValues: { #key: 499 });
```

要读取分区本地值，使用分区的索引操作符 [] 以及值的Key。只要对象重载了 == 操作符，并实现了hashCode的get方法，都可以作为Key，如字符串、数字等。如果未检索到[key]，则返回null。通常Key是一个原义符号Symbols：#identifier。

提示：Symbol 符号类表示一个标识符，用于代表Dart中定义的类、变量等等，#key等同于new Symbol(“key”)。对于混淆过的代码， Symbol 也返回混淆之前的标识符名字。后面的章节会有详细介绍。

分区中不能改变Key映射的值，但是你可以操作该对象。例如，下面的代码中，添加一条内容到分区本地值列表中：

```dart
runZoned(() {
  Zone.current[#key].add(499);
  print(Zone.current[#key]); // [499]
}, zoneValues: { #key: [] });
```

Zone可以从父分区中继承分区本地值，所以添加嵌套子孙分区并不会意外删除现有的值，并且在嵌套分区中可以改变，即重定义相同Key的值。建议使用唯一的对象作为Key，避免和其他库中的值发生冲突。

### 示例：在调试日志中使用分区本地值

如果有foo.txt和bar.txt两个文件，并且想输出所有行，代码如下：

```dart
import 'dart:async';
import 'dart:convert';
import 'dart:io';

Future splitLinesStream(stream) {
  return stream
      .transform(ASCII.decoder)
      .transform(const LineSplitter())
      .toList();
}

Future splitLines(filename) {
  return splitLinesStream(new File(filename).openRead());
}
main() {
  Future.forEach(['foo.txt', 'bar.txt'],
                 (file) => splitLines(file)
                     .then((lines) { lines.forEach(print); }));
}
```

上面的代码工作正常，但是假设你想知道每一行来自哪个文件。但是在splitLines()返回值为Stream，并没有恰当的方法在函数中合适的位置添加filename参数，而通过分区本地值，可以快速实现：

```dart
import 'dart:async';
import 'dart:convert';
import 'dart:io';

Future splitLinesStream(stream) {
  return stream
      .transform(ASCII.decoder)
      .transform(const LineSplitter())
      .map((line) => '${Zone.current[#filename]}: $line')    //Add
      .toList();
}

Future splitLines(filename) {
  return runZoned(() {    //Add
    return splitLinesStream(new File(filename).openRead());
  }, zoneValues: { #filename: filename });    //Add
}

main() {
  Future.forEach(['foo.txt', 'bar.txt'],
                 (file) => splitLines(file)
                     .then((lines) { lines.forEach(print); }));
}
```

新的代码中，并没有修改函数签名或在splitLines()与splitLinesStream()之间传递filename参数，而是使用分区本地值将数据流和filename相关联，仅添加3行代码就实现了该功能。

提示：函数由函数名、参数个数、参数类型、返回值组成，函数签名则是由函数名、参数个数与其类型组成。C++中，函数在重载时，利用函数签名的不同来区别到底该调用哪个方法。Dart中并不存在函数重载。

## 5、Zone的重要功能

在runZoned()中有一个命名参数zoneSpecification分区规范，类型为ZoneSpecification，可以通过该对象重写Zone管理的下列功能：

- Fork子分区
- 在分区中注册和运行回调函数
- 调度Microtask微任务和Timer定时器
- 处理未捕获的异步类型的异常
- Print输出

### 示例一：重写print函数

下面的代码，实现了忽略分区中print函数输出的一种方法：

```dart
import 'dart:async';

main() {
  runZoned(() {
    print('Will be ignored');
  }, zoneSpecification: new ZoneSpecification(
      print: (self, parent, zone, message) {
        // Ignore message.
      }));
}
```

在fork（复制新分区作为子分区）的分区中，print()通过指定的print拦截器重写，实现丢弃、忽略输出消息的功能。重写print()函数是被允许的，因为print()（像scheduleMicrotask以及Timer构造函数一样）使用当前分区来工作。

**关于参数中的拦截器和委托**

前面的示例中，Zone的print(String line)只有一个参数，而ZoneSpecification中定义的拦截器（PrintHandler，输出处理器）却有4个参数：print(Zone self, ZoneDelegate parent, Zone zone, String line)，并且前面3个参数总是以相同的顺序在ZoneSpecification中出现。

**self**
处理回调函数的分区

**parent**
ZoneDelegate分区委托表示父分区，通过它将操作转发给父分区。

**zone**
表示产生操作的位置。很多操作需要明确该操作是在哪个分区中被调用。例如，zone.fork(specification) 必须创建一个新的分区作为zone的子分区。另一个例子，即使你将scheduleMicrotask()委托给其他分区，微任务仍然在最初的分区中执行。

当拦截器将一个函数委托给parent的时候，函数的parent（ZoneDelegate）版本会添加一个额外的参数：zone。例如，在ZoneDelegate中print()的函数签名为print(Zone zone, String line)。

类似的，下面是可拦截函数scheduleMicrotask()的参数：
**定义的位置                 函数签名**
Zone                           void scheduleMicrotask(void f())
ZoneSpecification      void scheduleMicrotask(Zone self, ZoneDelegate parent, Zone zone, void f())
ZoneDelegate            void scheduleMicrotask(Zone zone, void f())

### 示例二：委托父分区

下面的示例将演示如何将任务委托给父分区执行：

```dart
import 'dart:async';

main() {
  runZoned(() {
    var currentZone = Zone.current;
    scheduleMicrotask(() {
      print(identical(currentZone, Zone.current));  // prints true.
    });
  }, zoneSpecification: new ZoneSpecification(
    scheduleMicrotask: (self, parent, zone, task) {
      print('scheduleMicrotask has been called inside the zone');
      // The origin `zone` needs to be passed to the parent so that
      // the task can be executed in it.
      parent.scheduleMicrotask(zone, task);
    }));
}
```

上面的代码中，分区中调度微任务的时候，会先打印提示信息print(‘scheduleMicrotask has been called inside the zone’);同样，上一个例子中，我们可以重新拼接字符串，在每次输出信息的时候打印日期：parent.print(zone, new DateTime.now().toString() + “\t” + message);

### 示例三：进入和离开分区时执行代码

如果你想知道异步代码执行所用时间，可以将代码放入分区中。当进入分区的时候，开始定时器；当离开分区的时候，停止定时器。关键的地方是，ZoneSpecification类提供了一套run*参数让分区执行指定的代码。

run*命名参数包括：run，runUnary和runBinary。作用为当Zone执行代码的时候，先执行指定代码。当回调函数的参数个数为0、1、2的时候，run、runUnary、runBinary分别被执行。同时，run参数也作用于初始化的时候，即调用runZoned()之后执行同步代码时。下面是使用run*分析代码的例子：

```dart
import 'dart:async';

main() {
  final total = new Stopwatch();
  final user = new Stopwatch();

  final specification = new ZoneSpecification(
      run: (self, parent, zone, f) {
        user.start();
        try { return parent.run(zone, f); } finally { user.stop(); }
      },
      runUnary: (self, parent, zone, f, arg) {
        user.start();
        try { return parent.runUnary(zone, f, arg); } finally { user.stop(); }
      },
      runBinary: (self, parent, zone, f, arg1, arg2) {
        user.start();
        try {
          return parent.runBinary(zone, f, arg1, arg2);
        } finally {
          user.stop();
        }
      });

  runZoned(() {
    total.start();
  // ... 执行同步代码...
  // ... 执行异步代码 ...
    new Future.delayed(new Duration(seconds: 1), () => null).then((_) {
      print(total.elapsedMilliseconds);
      print(user.elapsedMilliseconds);
    });
  }, zoneSpecification: specification);
}
```

**运行结果（和计算机性能相关，非固定值）：**

```dart
1008
10
```

在这个代码中，每个run*参数只是用于启动定时器，执行指定的功能，然后停止定时器。
注：以后，Zone可能会提供更简单的onEnter/onLeave API。

### 示例四：操作回调函数

ZoneSpecification中的register*Callback用于封装或修改回调函数，并且在Zone中异步执行。该参数与run*类似，register*Callback有三种形式：registerCallback (针对回调函数没有参数的情况)， registerUnaryCallback (回调函数是一个参数)，registerBinaryCallback (回调函数是两个参数)。

下面是例子是异步模式下，在Zone遇到未捕获异常退出前，输出保存堆栈信息：

```dart
import 'dart:async';

get currentStackTrace {
  try {
    throw 0;
  } catch(_, st) {
    return st;
  }
}

var lastStackTrace = null;

bar() => throw "in bar";
foo() => new Future(bar);

main() {
  final specification = new ZoneSpecification(
      registerCallback: (self, parent, zone, f) {
        var stackTrace = currentStackTrace;
        return parent.registerCallback(zone, () {
          lastStackTrace = stackTrace;
          return f();
        });
      },
      registerUnaryCallback: (self, parent, zone, f) {
        var stackTrace = currentStackTrace;
        return parent.registerUnaryCallback(zone, (arg) {
          lastStackTrace = stackTrace;
          return f(arg);
        });
      },
      registerBinaryCallback: (self, parent, zone, f) {
        var stackTrace = currentStackTrace;
        return parent.registerBinaryCallback(zone, (arg1, arg2) {
          lastStackTrace = stackTrace;
          return f(arg1, arg2);
        });
      },
      handleUncaughtError: (self, parent, zone, error, stackTrace) {
        if (lastStackTrace != null) print("last stack: $lastStackTrace");
        return parent.handleUncaughtError(zone, error, stackTrace);
      });

  runZoned(() {
    foo();
  }, zoneSpecification: specification);
}
```

运行代码，在调用foo()后，你会看到控制台输出”last stack”堆栈信息（lastStackTrace），接着输出来自异步代码环境的堆栈信息（stackTrace），该信息来自bar()而不是foo()。

需要注意，即使你正在实现一个异步API，你可能也完全不必处理分区的问题。例如，你可能期望通过dart:io库对当前分区进行记录，但反而依赖分区来处理dart:async中的类（如Future和Stream）。

如果你显式地操作分区，需要注册所有异步回调函数，并确保每个回调函数在最初注册的分区中被调用。Zone中的bind*Callback函数使操作更容易，该函数是由register*Callback 和run*组合的简化函数，以确保在分区中每个回调函数都被注册和运行。

如果你需要比bind*Callback更细致的操作，那么你需要分开单独使用register*Callback 和run*。另外，分区中的run*Guarded函数对代码进行封装，将调用放入try-catch代码块中，如果出现错误，会调用uncaughtErrorHandler。

## 6、总结

当异步代码中存在未捕获异常的时候，Zone是一种很好的保护代码的方式，并且可以做得更多。你可以将通过分区本地值关联Zone中的数据，也可以重写print、任务调度等核心功能。同时，Zone可以更好地对代码进行调试，并且提供Hook机制对功能进行分析。