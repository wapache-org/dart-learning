

# 给Java开发者的Dart教程(精简版）


# 创建简单的Dart类

以下为一个带有三个属性的Bicycle类，和main函数。

```dart
class Bicycle {
  // Dart中没有 public, private, or protected关键字。默认都是public。
  int cadence;
  int speed;
  int gear;
  // 如果标识符前面加“_”，就是私有变量或者函数
  int _i_am_private;
}

// main()方法作为Dart的入口方法，这点大家都能快速理解。
void main() {

}
```

## 1. 定义构造函数

添加以下构造函数到Bicycle类中去

```dart

Bicycle(this.cadence, this.speed, this.gear);

// 上面的构造函数等同于下面的构造函数

Bicycle(int cadence, int speed, int gear) {
  this.cadence = cadence;
  this.speed = speed;
  this.gear = gear;
}

```

实例化Bicycle并打印

```dart
void main() {
  // 跟js类似, 可以用var来定义变量, 自动推算类型
  // 如果已知变量不会再改变，可以使用final来代替var
  var bike = new Bicycle(2, 0, 1);
  print(bike);
  
  // new 是可选关键字, 可以不写
  final finalBike = Bicycle(2, 0, 1);
}
```

```
Instance of 'Bicycle'
```

## 2. 增强输出显示

所有的Dart类同Java一样提供了重写toString()的方式，来增强类的输出。

```dart

@override
String toString() {
  // Dart中可以用单引号或双引号创建字符串，可以同时使用（栗子：双引号中包含单引号）。
  return 'Bicycle: '+speed+' mph';
}
// 使用 =>来代替return的写法，实现单行缩进效果，代码简洁有效。
@override
String toString() => 'Bicycle: '+speed+' mph';
// 使用${expression}在字符串中插入表达式
@override
String toString() => 'Bicycle: ${speed} mph';
// 如果是标识符，可以省略{ }。
@override
String toString() => 'Bicycle: $speed mph';
// 等java加入这几个语法糖已经等了N年了...
```
输出结果：
```
Bicycle: 0 mph
```

## 3. 添加只读变量

通常在Java中定义speed为只读变量 ——会将它申明为private并仅提供一个getter方法。接下来我们将实现相同的功能在Dart中。

Dart中定义私有标识，是让变量名以下划线 (_) 开头。在通过修改speed名字和添加getter方法，使其成为只读变量。

默认下，Dart为所有的public实例变量提供了getters 和 setters 。不需要定义getters 和 setters就可以随处使用变量，更新变量。除非你想指定变量的只读或者只写，才需要重新定义getters 和 setters。

最后的栗子🌰看起来跟Java非常类似。

```dart
class Bicycle {

  // 未初始化变量（包括numbers类型) 默认值都是null。
  int cadence;
  int gear;
  int _speed = 0;

  Bicycle(this.cadence, this.gear);
  
  // 添加getter方法
  int get speed => _speed;
  
  void applyBrake(int decrement) {
     _speed -= decrement;
  }

  void speedUp(int increment) {
    _speed += increment;
  }
  
  @override  
  String toString() => 'Bicycle: $_speed mph';
}

void main() {
  var bike = new Bicycle(2, 1);
  print(bike);
}
```

# 使用可选参数（替代重载）

接下去练习定义一个 [Rectangle class](https://docs.oracle.com/javase/tutorial/java/javaOO/objectcreation.html), Java教程的一个示例。

Java中构造方法可通过重载，实现不同参数数量或类型的方法定义。而Dart中不支持重载构造函数，处理这种状态，正如本节中你会发现的一样。

[在DartPad中打开 Rectangle 示例](https://dartpad.dartlang.org/68fb4923eee0e9e3d3af42730c1d3503)

### 1. 添加Rectangle构造方法

Dart中一个空构造方法来实现Java四个重载构造方法。
```dart
Rectangle({
    this.origin = const Point(0, 0), 
    this.width = 0, 
    this.height = 0
});
```
该构造使用了可选命名参数。
 * this.origin、this.width、 this.height 使用简写技巧在构造函数的声明中为实例变量赋初始值。
 * this.origin、this.width、 this.height 都是可选命名参数。命名参数都闭包于大括号中 ({})。
 * 其中 this.origin = const Point(0, 0)为origin实例变量指定了默认值——Point(0,0)。指定的默认值必须是编译时常量。 该构造函数为三个实例变量都提供默认值。

### 2. 增强输出

· 添加toString()方法，到Rectangle类中：
```dart
@override
String toString() => 'Origin: (${origin.x}, ${origin.y}), width: $width, height: $height';
```

### 3. 使用构造方法

根据你需要的参数，在main()方法中去验证Rectangle实例。
```dart
main() {
  print(new Rectangle(origin: const Point(10, 20), width: 100, height: 200));
  print(new Rectangle(origin: const Point(10, 10)));
  print(new Rectangle(width: 200));  print(new Rectangle());
}
```
如上Dart使用单行的构造函数，实现了Java16行代码的等效功能。

### 4. 运行实例

你会看到如下的输出：

```
Origin: (10, 20), width: 100, height: 200
Origin: (10, 10), width: 0, height: 0
Origin: (0, 0), width: 200, height: 0
Origin: (0, 0), width: 0, height: 0
```

# 创建一个工厂

工厂模式作为Java中常见的一种设计模式，相比直接实例化对象存在几个优势。例如：隐藏实例的细节、能够提供工厂返回类型的子类型，并且可选地返回现有对象而不是新对象。

这里演示两种实现创建shape-creation工厂的方法：

 * 方式1：创建一个顶级函数
 * 方式2：创建一个工厂构造方法

在本练习中，将使用Shapes示例，该示例将实例化形状并打印其计算区域：

```dart

// Dart也是使用import关键字引入依赖, 但引入的是语法不同

// dart.math是Dart's 的核心库之一。其他核心库还包括： dart:core, dart:async, dart:convert, and dart:collection。
import 'dart:math';

// Dart 支持抽象（ abstract）类 classes。
// 支持在一个文件中可定义多个类。
abstract class Shape {
  num get area;
}

class Circle implements Shape {
  final num radius;  
  Circle(this.radius);
  // 重写get方法
  num get area => PI * pow(radius, 2);
}

class Square implements Shape {
  final num side;  
  Square(this.side);
  // 重写get方法
  num get area => pow(side, 2);
}

main() {
  final circle = new Circle(2);
  final square = new Square(2);
  print(circle.area);  
  print(square.area);
}
```
[在DartPad中的Shapes实例](https://dartpad.dartlang.org/a98247f43448f5e0eac4c7f483c173a5)

你可以在控制台看到打印结果：
```
12.566370614359172
4
```

### 方式1：创建一个顶级函数

 Implement a factory as a top-level function by adding the following function at the highest level (outside of any class):

通过在顶层添加以下方法（类的外层），来实现一个工厂作为顶层方法。

```
Shape shapeFactory(String type) {
  if (type == 'circle') return new Circle(2);
  if (type == 'square') return new Square(2);
  throw 'Can\'t create $type.';
}
```

在main()函数中调调用工厂方法：

```dart
final circle = shapeFactory('circle');
final square = shapeFactory('square');
```

### 运行实例

输出结果与之前相同。

观察结论：

 * 如果函数调用传入 'circle' 或 'square'外的参数，将抛出异常。
 *  Dart SDK中已有定义多个通用的异常类，或者你也可以继承这些异常类来创建一个你指定的异常。在本例中你也可以抛出一个描述异常原因的字符串。
 * 对于DartPad中未捕获的异常。可以通过try-catch语句包裹，并打印异常来观察。可选练习 [DartPad example](https://dartpad.dartlang.org/56381ac0a35594844919150039dcf214).
 * 在单引号字符串中使用单引号需要加上(\)来转义 ('Can\'t create $type.') 或者使用双引号嵌套 ("Can't create $type.")。

### 方式2：创建一个工厂构造方法

使用Dart的`factory`关键字来创建工厂构造方法

在Shape抽象类中添加工厂的构造方法：
```dart
abstract class Shape {
  factory Shape(String type) {
    if (type == 'circle') return new Circle(2);
    if (type == 'square') return new Square(2);
    throw 'Can\'t create $type.';
  }
  num get area;
}
```

main()函数中改为：

```dart
final circle = new Shape('circle');
final square = new Shape('square');
```

删除之前添加的`shapeFactory()`方法。

# 实现一个接口

Dart语言不包含`interface`关键字，因为每个类都隐式地定义了一个接口。

[在DartPad中打开Shapes示例](https://dartpad.dartlang.org/8e2ad295a1b7c8254414afb43fc7d280)


加入一个CircleMock类，并继承Circle类：
```dart
class CircleMock implements Circle {}
```
这时候你应该会看到 "Missing concrete implementations" 错误。在类中定义area和radius来修复这个错误吧。
```dart
class CircleMock implements Circle {
  num area;
  num radius;
}
```

# 六、在Dart上函数式编程


跟Java 8 的函数式编程类似, 但Dart支持更完整的函数式编程

Dart比Java 8 强的地方有:

1. 有顶级函数, Java只能静态方法来实现

```dart

// ${}里可以直接写表达式
String scream(int length) => "A${'a' * length}h!";

main() {
  final values = [1, 2, 3, 5, 10, 50];
  // 过程式写法
  for (var length in values) {
    print(scream(length));
  }
  
  // 函数式写法
  // values.map(scream).forEach(print);
  
  // 跟Java的lambda是不是很像^_^
  values.skip(1).take(3).map(scream).forEach(print);
  
}

```

```
Aah!
Aaah!
Aaaah!
Aaaaaah!
Aaaaaaaaaaah!
Aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaah!

Aaah!
Aaaah!
Aaaaaah!

```

# 七、恭喜你

你可能已经入坑了
