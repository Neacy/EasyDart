## 默认值以及常量
### 默认值
在Dart语言中任何的变量没有初始化赋值的话默认都是为null值，甚至像数值类型都是默认为null。因为在Dart语言中一切皆对象

```
int lineCount;
assert(lineCount == null);

ps： assert()在正式环境中是会被忽略，
当方法中的判断语句为true的时候会抛出一个错误Exception
```
### 常量
如果你想要声明一个常量，那么只要使用final 或者 const。不管是用var或者别的类型常量只会被赋值一次，其中const表示的编译时常量(其实也就是个隐式fianl常量)。final声明的常量也是在使用的时候就要给予赋值否则会报错。
需要注意的是：实例变量不能声明为const只能用final，而且实例变量必须在构造函数体启动之前进行初始化——在变量声明处，通过构造函数参数，或者在构造函数的初始化器列表中。

举个例子：

```
final name = 'Bob'; // 没有声明类型的常量
final String nickname = 'Bobby';

当你想要修改name值的时候：
name = 'Alice';// 这是就会抛出错误：Error: a final variable can only be set once.
```
如果你想在编译的时候声明常量那么可以使用const，你可以给这个const常量赋值为数字、字符串、const常量或者对常量运行的结果，例如：

```
const bar = 1000000;
const double atm = 1.01325 * bar;
```
const这个关键字不仅仅可以用于声明一个常量，你还可以使用它来创建常量值，以及声明创建常量值的构造函数。任何变量都可以有一个常数值。例如：

```
var foo = const [];
final bar = const [];
const baz = []; // 等价于 `const []`
```
当然你可以对上面的代码中非const或者final的变量重新赋值，哪怕已经赋值一个常量。比如：

```
foo = [1, 2, 3]; // 之前赋值为 const []
```
但是你不能对一个常量重新赋值：

```
baz = [42]; // Error: Constant variables can't be assigned a value.
```
以上就是一些关于默认值和常量的内容。