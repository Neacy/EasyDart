## 函数
Dart是属于面向对象的语言，所以函数其实也是对象，他的类型名为Function，意味着你可以声明一个Function变量也可以把一个Function作为参数传给另一个函数。

简单声明一个函数：

```
bool isNoble(int atomicNumber) {
  return _nobleGases[atomicNumber] != null;
}
```
虽然Effective Dart函数返回一个具体类型，但是如果不写返回类型也是可以的：

```
isNoble(atomicNumber) {
  return _nobleGases[atomicNumber] != null;
}
```
如果函数只有一行表达式，那你可以按如下的方式写：

```
bool isNoble(int atomicNumber) => _nobleGases[atomicNumber] != null;
```
其中=> expr是{ return expr; }的缩写表示，所以这种写法也称为箭头函数。

每个函数都可以包含有两种参数：可选 和 必需，首先列出必需的参数，然后列出任何可选参数。命名的可选参数也可以标记为@required 

### 函数参数
#### 可选参数
当你定义函数时，使用{param1, param2，…}指定命名参数:

```
void enableFlags({bool bold, bool hidden}) {...}
```
在调用函数时，可以使用paramName: value指定指定的参数。例如:

```
enableFlags(bold: true, hidden: false);
```
在Flutter中常见的Widget的构造函数参数都是比较多，所以使用这种方式能让代码看起来看浅显易懂：

```
const Scrollbar({Key key, @required Widget child})
```
当你在FLutter项目中使用到Scrollbar却缺少了child参数，那么系统就会报错。

当你把函数参数都放在[]中的话，那么代表它们是可选参数：

```
String say(String from, String msg, [String device]) {
  var result = '$from says $msg';
  if (device != null) {
    result = '$result with a $device';
  }
  return result;
}
```
不给可选参数赋值的调用方式：

```
assert(say('Bob', 'Howdy') == 'Bob says Howdy');
```
给函数可选参数赋值的调用方式：

```
assert(say('Bob', 'Howdy', 'smoke signal') ==
    'Bob says Howdy with a smoke signal');
```
#### 默认参数值
在Dart中可以直接在函数声明处直接给参数赋予默认值，默认值必须是编译时常量，当你的函数参数没有提供默认值，那么默认是为null。

```
/// 设置两个默认值
void enableFlags({bool bold = false, bool hidden = false}) {...}

// bold == true; hidden == false.
enableFlags(bold: true);

ps:
在就的Dart版本中可能存在(:) 代替 = 给函数参数提供默认值，由于版本的迭代推荐使用=给参数赋默认值。
```
通过一个例子如何给可选参数赋默认值：

```
String say(String from, String msg,
    [String device = 'carrier pigeon', String mood]) {
  var result = '$from says $msg';
  if (device != null) {
    result = '$result with a $device';
  }
  if (mood != null) {
    result = '$result (in a $mood mood)';
  }
  return result;
}

assert(say('Bob', 'Howdy') ==
    'Bob says Howdy with a carrier pigeon');
```
同样函数的参数如果是Map或者List一样可以赋默认值：

```
void doStuff(
    {List<int> list = const [1, 2, 3],
    Map<String, String> gifts = const {
      'first': 'paper',
      'second': 'cotton',
      'third': 'leather'
    }}) {
  print('list:  $list');
  print('gifts: $gifts');
}
```
### main() 主函数
main()作为一个app的入口函数，返回值为void且存在一个List<String> arguments类型的可选参数。

```
void main() {
  querySelector('#sample_text_id')
    ..text = 'Click me!'
    ..onClick.listen(reverseText);
}
```
当main()函数的可选参数可见的时候，可以看下下面的代码片段：

```
void main(List<String> arguments) {
  print(arguments);

  assert(arguments.length == 2);
  assert(int.parse(arguments[0]) == 1);
  assert(arguments[1] == 'test');
}
```
### 函数作为参数/变量
你可以给一个函数的参数传另一个函数进去，比如：

```
void printElement(int element) {
  print(element);
}

var list = [1, 2, 3];

// Pass printElement as a parameter.
list.forEach(printElement);
```
你还可以将函数赋值给变量，比如说：

```
var loudify = (msg) => '!!! ${msg.toUpperCase()} !!!';
assert(loudify('hello') == '!!! HELLO !!!');
```
### 匿名函数
匿名函数顾名思义是没有名字的不像main()这类函数有一个名字，匿名函数参数可有可无，甚至参数类型也是非必须的，大概的格式为：

```
([[Type] param1[, …]]) { 
  codeBlock; 
}; 
```
举个例子：

```
var list = ['apples', 'bananas', 'oranges'];
list.forEach((item) {
  print('${list.indexOf(item)}: $item');
});
```
如上面的代码片段中forEach中的匿名函数，item是没有指定类型。
如果匿名函数只有一行表达式的时候，可以使用箭头函数替代：

```
list.forEach(
    (item) => print('${list.indexOf(item)}: $item'));
```
### 函数作用域
函数中的变量是存在作用域的，先举个例子再解释：

```
bool topLevel = true;

void main() {
  var insideMain = true;

  void myFunction() {
    var insideFunction = true;

    void nestedFunction() {
      var insideNestedFunction = true;

      assert(topLevel);
      assert(insideMain);
      assert(insideFunction);
      assert(insideNestedFunction);
    }
  }
}
```
在nestedFunction() 函数中可以使用每一个变量，相反其内部的变量{}外面的却无法使用，作用域的意思完全跟Java中的一样理解即可。
### 闭包
闭包是一个函数对象，它可以访问其词法作用域内的变量，即使该函数是在其原始作用域之外使用的。

函数可以关闭在周围范围中定义的变量。在下面的示例中，makeAdder()捕获变量addBy。无论返回的函数走到哪里，它都会记住addBy。

```
/// Returns a function that adds [addBy] to the
/// function's argument.
Function makeAdder(num addBy) {
  return (num i) => addBy + i;
}

void main() {
  // Create a function that adds 2.
  var add2 = makeAdder(2);

  // Create a function that adds 4.
  var add4 = makeAdder(4);

  assert(add2(3) == 5);
  assert(add4(3) == 7);
}
```
### 函数默认返回值
每个函数都有一个返回值，当你没有返回一个值的话 那么默认返回的是null，例如：

```
foo() {}

assert(foo() == null);
```

