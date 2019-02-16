## 类
Dart是面向对象语言，所有对象都有一个类型<class>，且每一个类都有一个超类Object。
### 类前提知识
类的属性通常包含有变量以及函数，类的对象通过(.)来调用相关的属性或者方法。

```
var p = Point(2, 2);

// Set the value of the instance variable y.
p.y = 3;

// Get the value of y.
assert(p.y == 3);

// Invoke distanceTo() on p.
num distance = p.distanceTo(Point(4, 4));
```
当你担心类的实例可能为空，那么可以使用?.实现实例为空的话就不调用：

```
// If p is non-null, set its y value to 4.
p?.y = 4;
```
通常初始化一个对象的时候是通过调用构造函数，构造函数可以是类名或者类中的一个方法：

```
var p1 = Point(2, 2);
var p2 = Point.fromJson({'x': 1, 'y': 2});
```
下面的代码片段通过new来初始化对象，也是一样的效果：

```
var p1 = new Point(2, 2);
var p2 = new Point.fromJson({'x': 1, 'y': 2});

ps: new关键字在Dart2的时候变成是可选的。
```
有些类提供常量构造函数。要使用常量构造函数创建编译时常量，请在构造函数名称前加上const关键字:

```
var p = const ImmutablePoint(2, 2);
```
构造两个相同的编译时常量实例其实两个对象是一样的:

```
var a = const ImmutablePoint(1, 1);
var b = const ImmutablePoint(1, 1);

assert(identical(a, b)); // They are the same instance!
```
如果一个变量已经声明为const，那么常量对象就不需要再其前面加const：

```
// Lots of const keywords here.
const pointAndLine = const {
  'point': const [const ImmutablePoint(0, 0)],
  'line': const [const ImmutablePoint(1, 10), const ImmutablePoint(-2, 11)],
};

改成：
// Only one const, which establishes the constant context.
const pointAndLine = {
  'point': [ImmutablePoint(0, 0)],
  'line': [ImmutablePoint(1, 10), ImmutablePoint(-2, 11)],
};
```
如果常量构造函数位于常量上下文之外，并且在没有const的情况下被调用，那么它将创建一个非常量对象:

```
var a = const ImmutablePoint(1, 1); // Creates a constant
var b = ImmutablePoint(1, 1); // Does NOT create a constant

assert(!identical(a, b)); // NOT the same instance!
```
### 判断对象类型
想在运行时获取到对象的类型，可以通过runtimeType属性来获取：

```
print('The type of a is ${a.runtimeType}');

```

### 类的成员变量
可以如下代码声明类的成员变量：

```
class Point {
  num x; // Declare instance variable x, initially null.
  num y; // Declare y, initially null.
  num z = 0; // Declare z, initially 0.
}
```
所有未初始化的成员变量的值都是null。
所有的成员变量都会隐式的生成getter/setter方法，当然如果是final成员变量就没有setter方法：

```
class Point {
  num x;
  num y;
}

void main() {
  var point = Point();
  point.x = 4; // Use the setter method for x.
  assert(point.x == 4); // Use the getter method for x.
  assert(point.y == null); // Values default to null.
}
```
如果在声明实例变量的地方(而不是在构造函数或方法中)初始化实例变量，则在创建实例时设置该值，该实例在构造函数及其初始化器列表执行之前。
### 类的构造函数
类的构造函数都是通过声明一个函数跟类名一样的方式，常见的格式例如:

```
class Point {
  num x, y;

  Point(num x, num y) {
    // There's a better way to do this, stay tuned.
    this.x = x;
    this.y = y;
  }
}
```
其中的this代表着当前对象，如果变量名跟构造函数参数名不一致可以忽略this。

将构造函数参数赋值给实例变量的模式非常常见，Dart有语法上的优点可以简化这种模式：

```
class Point {
  num x, y;

  // Syntactic sugar for setting x and y
  // before the constructor body runs.
  Point(this.x, this.y);
}
```
### 默认构造函数
如果不声明构造函数，则会为您提供默认构造函数。默认构造函数没有参数，并调用超类中的无参数构造函数。
### 命名的构造函数
Dart不像Java没有多个跟类名一样的构造函数，所以可以通过命名的构造函数来提供多构造函数：

```
class Point {
  num x, y;

  Point(this.x, this.y);

  // Named constructor
  Point.origin() {
    x = 0;
    y = 0;
  }
}
```
请记住，构造函数不是继承的，这意味着超类的命名构造函数不是由子类继承的。如果您希望使用超类中定义的命名构造函数创建子类，则必须在子类中实现该构造函数。
### 调用非默认超类构造函数
默认情况下，子类中的构造函数调用超类的未命名的无参数构造函数。超类的构造函数在构造函数体的开头被调用。如果还使用初始化器列表，则在调用超类之前执行该列表。综上所述，执行顺序如下:

1. initializer list
2. superclass’s no-arg constructor
3. main class’s no-arg constructor

如果超类没有未命名的无参数构造函数，则必须手动调用超类中的一个构造函数。在构造函数主体(如果有的话)之前的冒号(:)后面指定超类构造函数。

```
class Person {
  String firstName;

  Person.fromJson(Map data) {
    print('in Person');
  }
}

class Employee extends Person {
  // Person 没有默认构造函数;
  // 你必须显示调用 super.fromJson(data).
  Employee.fromJson(Map data) : super.fromJson(data) {
    print('in Employee');
  }
}

main() {
  var emp = new Employee.fromJson({});

  // Prints:
  // in Person
  // in Employee
  if (emp is Person) {
    // Type check
    emp.firstName = 'Bob';
  }
  (emp as Person).firstName = 'Bob';
}
```
因为超类构造函数的参数是在调用构造函数之前计算的，所以参数可以是表达式，例如函数调用:

```
class Employee extends Person {
  Employee() : super.fromJson(getDefaultData());
  // ···
}
```
### 初始化器列表
除了调用超类构造函数外，还可以在构造函数体运行之前初始化实例变量。用逗号分隔初始化器。

```
// Initializer list sets instance variables before
// the constructor body runs.
Point.fromJson(Map<String, num> json)
    : x = json['x'],
      y = json['y'] {
  print('In Point.fromJson(): ($x, $y)');
}
```
在开发过程中，可以通过在初始化器列表中使用assert来验证输入。

```
Point.withAssert(this.x, this.y) : assert(x >= 0) {
  print('In Point.withAssert(): ($x, $y)');
}
```
### 重定向构造函数
有时构造函数的唯一目的是重定向到同一类中的另一个构造函数。重定向构造函数的主体为空，构造函数调用出现在冒号(:)之后。

```
class Point {
  num x, y;

  // The main constructor for this class.
  Point(this.x, this.y);

  // Delegates to the main constructor.
  Point.alongXAxis(num x) : this(x, 0);
}
```
### 常量构造函数
如果您的类生成永不更改的对象，则可以使这些对象成为编译时常量。为此，定义一个const构造函数，并确保所有实例变量都是final。

```
class ImmutablePoint {
  static final ImmutablePoint origin =
      const ImmutablePoint(0, 0);

  final num x, y;

  const ImmutablePoint(this.x, this.y);
}
```
### 构造函数工厂
在实现不总是创建类的新实例的构造函数时，请使用factory关键字。例如，工厂构造函数可能从缓存返回实例，也可能返回子类型的实例。

```
class Logger {
  final String name;
  bool mute = false;

  // _cache is library-private, thanks to
  // the _ in front of its name.
  static final Map<String, Logger> _cache =
      <String, Logger>{};

  factory Logger(String name) {
    if (_cache.containsKey(name)) {
      return _cache[name];
    } else {
      final logger = Logger._internal(name);
      _cache[name] = logger;
      return logger;
    }
  }

  Logger._internal(this.name);

  void log(String msg) {
    if (!mute) print(msg);
  }
}
```
然后调用方式：

```
var logger = Logger('UI');
logger.log('Button clicked');
```
### 类的方法
类方法可以访问对象的变量以及this：

```
class Point {
  num x, y;

  Point(this.x, this.y);

  num distanceTo(Point other) {
    var dx = x - other.x;
    var dy = y - other.y;
    return sqrt(dx * dx + dy * dy);
  }
}
```

### Getters / setters
Getters setters通常用于读取或者写入类的成员变量值，当然可以通过get/set关键字来创建额外的属性：

```
class Rectangle {
  num left, top, width, height;

  Rectangle(this.left, this.top, this.width, this.height);

  // Define two calculated properties: right and bottom.
  num get right => left + width;
  set right(num value) => left = value - width;
  num get bottom => top + height;
  set bottom(num value) => top = value - height;
}

void main() {
  var rect = Rectangle(3, 4, 20, 15);
  assert(rect.left == 3);
  rect.right = 12;
  assert(rect.left == -8);
}
```
### 抽象方法
实例、getter和setter方法可以是抽象的，可以定义接口，但将其实现留给其他类。抽象方法只能存在于抽象类中。

要使方法抽象，使用分号(;)而不是方法体:

```
abstract class Doer {
  // Define instance variables and methods...

  void doSomething(); // Define an abstract method.
}

class EffectiveDoer extends Doer {
  void doSomething() {
    // Provide an implementation, so the method is not abstract here...
  }
}
```
### 抽象类
通过abstract声明的类成为抽象类，抽象类是不能直接被初始化使用。抽象类通常用于定义接口，并让多个类去实现。如果你想要让你的抽象类可以被初始化使用，那么请声明一个工厂构造函数。

抽象类可以有多个抽象方法，例如：

```
abstract class AbstractContainer {
  // Define constructors, fields, methods...

  void updateChildren(); // Abstract method.
}
```
### 类的实现
每个类隐式地定义一个接口，其中包含类的所有实例成员及其实现的任何接口的所有实例成员。如果你希望创建一个不继承B的实现而支持B的API的类a，那么类a应该实现B接口。

类通过在implements子句中声明接口，然后提供接口所需的api来实现一个或多个接口。例如:

```
// A person. The implicit interface contains greet().
class Person {
  // In the interface, but visible only in this library.
  final _name;

  // Not in the interface, since this is a constructor.
  Person(this._name);

  // In the interface.
  String greet(String who) => 'Hello, $who. I am $_name.';
}

// An implementation of the Person interface.
class Impostor implements Person {
  get _name => '';

  String greet(String who) => 'Hi $who. Do you know who I am?';
}

String greetBob(Person person) => person.greet('Bob');

void main() {
  print(greetBob(Person('Kathy')));
  print(greetBob(Impostor()));
}
```
### 类的继承
可以通过extends类继承一个类，然后super可以指向父类：

```
class Television {
  void turnOn() {
    _illuminateDisplay();
    _activateIrSensor();
  }
  // ···
}

class SmartTelevision extends Television {
  void turnOn() {
    super.turnOn();
    _bootNetworkInterface();
    _initializeMemory();
    _upgradeApps();
  }
  // ···
}
```
### 重载成员变量或者方法
通过@override标识，即可实现重载父类中相关成员或者方法：

```
class SmartTelevision extends Television {
  @override
  void turnOn() {...}
  // ···
}
```
### 重载操作符
一下表格内的操作符可以重载：

|||||
| ------ | ------ | ------ | ------ |
| <	 |+|\||[]|
| >	 |/|^|[]=|
| <	= |~/|&|~|
| >= |*|<<|==|
| - |%|>>||
看个例子：

```
class Vector {
  final int x, y;

  Vector(this.x, this.y);

  Vector operator +(Vector v) => Vector(x + v.x, y + v.y);
  Vector operator -(Vector v) => Vector(x - v.x, y - v.y);

  // Operator == and hashCode not shown. For details, see note below.
  // ···
}

void main() {
  final v = Vector(2, 3);
  final w = Vector(2, 2);

  assert(v + w == Vector(4, 5));
  assert(v - w == Vector(0, 1));
}
```
### noSuchMethod()
要在代码试图使用不存在的方法或实例变量时检测或响应，可以覆盖noSuchMethod():

```
class A {
  // Unless you override noSuchMethod, using a
  // non-existent member results in a NoSuchMethodError.
  @override
  void noSuchMethod(Invocation invocation) {
    print('You tried to use a non-existent member: ' +
        '${invocation.memberName}');
  }
}
```
你不能调用未实现的方法，除非下列情况之一为真:

* 接收器具有静态类型动态。
* 接收方有一个静态类型，它定义了未实现的方法(抽象就可以了)，而接收方的动态类型有一个noSuchMethod()的实现，它不同于类对象中的方法。
### 枚举类
枚举类型，通常称为枚举或枚举，是一种特殊类型的类，用于表示固定数量的常量值。
声明一个枚举类：

```
enum Color { red, green, blue }
```
每一个枚举都有一个索引值从0开始，并存在一个索引的getter方法：

```
assert(Color.red.index == 0);
assert(Color.green.index == 1);
assert(Color.blue.index == 2);
```
如果想要枚举类的所有值,可以调用枚举类的values：

```
List<Color> colors = Color.values;
assert(colors[2] == Color.blue);
```
你可以在switch语句中使用枚举，如果你不处理所有枚举的值，你会得到一个警告:

```
var aColor = Color.blue;

switch (aColor) {
  case Color.red:
    print('Red as roses!');
    break;
  case Color.green:
    print('Green as grass!');
    break;
  default: // Without this, you see a WARNING.
    print(aColor); // 'Color.blue'
}
```
枚举类有以下两个限制：

* 你不能子类化、混合或实现枚举
* 不能显式实例化枚举。
### 向类中添加属性：mixin
mixins是一种复用类的方式。

要使用mixin，请使用with关键字后面跟着一个或多个mixin名称。下面的例子展示了两个使用mixin的类:

```
class Musician extends Performer with Musical {
  // ···
}

class Maestro extends Person
    with Musical, Aggressive, Demented {
  Maestro(String maestroName) {
    name = maestroName;
    canConduct = true;
  }
}
```
要实现mixin，创建一个扩展对象且不声明构造函数的类。除非希望mixin作为常规类使用，否则使用mixin关键字而不是class。例如:

```
mixin Musical {
  bool canPlayPiano = false;
  bool canCompose = false;
  bool canConduct = false;

  void entertainMe() {
    if (canPlayPiano) {
      print('Playing piano');
    } else if (canConduct) {
      print('Waving hands');
    } else {
      print('Humming to self');
    }
  }
}
```
mixin可以通过on来实现继承的操作：

```
mixin MusicalPerformer on Musician {
  // ···
}
```
### 类的静态属性和静态方法
静态方法或者静态属性都是通过static关键字声明：

```
// 静态属性
class Queue {
  static const initialCapacity = 16;
  // ···
}

// 静态方法
class Point {
  num x, y;
  Point(this.x, this.y);

  static num distanceBetween(Point a, Point b) {
    var dx = a.x - b.x;
    var dy = a.y - b.y;
    return sqrt(dx * dx + dy * dy);
  }
}
```