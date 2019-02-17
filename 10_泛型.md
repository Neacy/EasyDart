## 泛型
在Dart中，如果查看基本数组类型List的API文档，就会发现该类型实际上是List<E>。<…>符号将List标记为泛型(或参数化)，该类型具有正式的类型参数。按照惯例，大多数类型变量都有单个字母的名称，例如E、T、S、K和V。
### 为什么需要泛型
为了类型安全，通常需要泛型，但它们的好处不仅仅是允许代码运行:

* 正确地指定泛型类型可以生成更好的代码。
* 您可以使用泛型来减少代码重复。
如果希望列表只包含字符串，可以将其声明为list <String>(将其读作“String的列表”).这样子，当你往该list中加入非String类型的数据系统将会报错：

```
var names = List<String>();
names.addAll(['Seth', 'Kathy', 'Lars']);
names.add(42); //  加入非String类型将会报错
```
使用泛型的另一个原因是减少代码重复。泛型允许你在使用静态分析的同时，在许多类型之间共享单个接口和实现。例如，假设你创建了一个用于缓存对象的接口:

```
// 声明一个Object缓存
abstract class ObjectCache {
  Object getByKey(String key);
  void setByKey(String key, Object value);
}
// 当你需要一个String缓存对象的话那么需要再声明一个类
abstract class StringCache {
  String getByKey(String key);
  void setByKey(String key, String value);
}
```
那如果你还要一个整型类的缓存的话，自然还要再声明一个类，就会出现代码重复现象。

泛型就可以消除这种烦恼，相反你只需要声明一个带泛型的类即可：

```
abstract class Cache<T> {
  T getByKey(String key);
  void setByKey(String key, T value);
}
```
### 集合中的泛型
列表和映射文字可以参数化。参数化的文字就像你已经看到的文字一样，只不过在开始的括号之前添加了<type>(用于列表)或<keyType, valueType>(用于映射)。下面是使用类型化文字的例子:

```
var names = <String>['Seth', 'Kathy', 'Lars'];
var pages = <String, String>{
  'index.html': 'Homepage',
  'robots.txt': 'Hints for web robots',
  'humans.txt': 'We are people, not machines'
};
```
### 构造函数中的泛型
要在使用构造函数时指定一个或多个类型，请将类型放在类名后面的尖括号中(<…>)。例如:

```
var names = List<String>();
names.addAll(['Seth', 'Kathy', 'Lars']);
var nameSet = Set<String>.from(names);

// 同样可以为Map指定具体类型
var views = Map<int, View>();
```
### 泛型集合及其包含的类型
Dart泛型类型被具体化，这意味着它们在运行时携带类型信息。例如，你可以测试集合的类型:

```
var names = List<String>();
names.addAll(['Seth', 'Kathy', 'Lars']);
print(names is List<String>); // true

// 相反，Java中的泛型使用擦除，这意味着泛型类型参数在运行时被删除。在Java中，您可以测试一个对象是否是一个列表，但是你不能测试它是否是一个列表<String>。
```
### 限制参数化类型
在实现泛型类型时，你可能希望限制其参数的类型。你可以使用extends来实现这一点。

```
class Foo<T extends SomeBaseClass> {
  // Implementation goes here...
  String toString() => "Instance of 'Foo<$T>'";
}

class Extender extends SomeBaseClass {...}
```
可以使用SomeBaseClass或它的任何子类作为泛型参数:

```
var someBaseClassFoo = Foo<SomeBaseClass>();
var extenderFoo = Foo<Extender>();
```
同样可以给泛型类的时候可以不指定具体的类型：

```
var foo = Foo();
print(foo); // Instance of 'Foo<SomeBaseClass>'
```
但是如果声明一个Foo对象指定的参数非SomeBaseClass那么将会报错：

```
var foo = Foo<Object>();// 这样子将会报错
```
### 泛型方法
最初，Dart的通用支持仅限于类。一种新的语法，称为泛型方法，允许在方法和函数上使用类型参数:

```
T first<T>(List<T> ts) {
  // 早起这样子写是不被认可，将会报错的
  T tmp = ts[0];
  // 现在的Dart版本已经支持了
  return tmp;
}
```
在这里，first (<T>)上的泛型类型参数允许你在几个地方使用类型参数T:

* 在函数的返回类型(T)中。
* 在参数类型中(List<T>)。
* 在局部变量类型(T tmp)中。