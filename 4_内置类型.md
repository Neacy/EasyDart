## 内置类型
在Dart语言中内置了以下几个类型：

* numbers
* strings
* booleans
* lists (also known as arrays)
* maps
* runes (for expressing Unicode characters in a string)
* symbols

### Numbers
Dart中的Numbers有以下两种：
##### int
根据平台的不同，整数值不大于64位。在Dart VM上，值可以从-263到263 - 1。编译成JavaScript的Dart使用JavaScript数字，允许从-253到253 - 1的值。
#### double
64位(双精度)浮点数，由IEEE 754标准指定。

int和double都是num的子类型，num类型包括基本的运算符，如+、-、/和*，你还可以在其中找到abs()、ceil()和floor()等方法。(像>>这样的位操作符是在int类中定义的。)如果num及其子类型没有您要查找的内容，那么dart:math库可能有。 

整数是没有小数点的数。下面是一些定义整数文字的例子:

```
var x = 1;
var hex = 0xDEADBEEF;
```
如果一个数包含一个小数，它就是一个双精度数。下面是一些定义double的例子:

```
var y = 1.1;
var exponents = 1.42e5;
```
在Dart 2.1中，整数文字在必要时自动转换为双精度:

```
double z = 1; // 等于 double z = 1.0.
// 在Dart 2.1之前这么写将会报错的
```
下面是如何将字符串转换成数字，或者反过来:

```
// String -> int
var one = int.parse('1');
assert(one == 1);

// String -> double
var onePointOne = double.parse('1.1');
assert(onePointOne == 1.1);

// int -> String
String oneAsString = 1.toString();
assert(oneAsString == '1');

// double -> String
String piAsString = 3.14159.toStringAsFixed(2);
assert(piAsString == '3.14');
```
int类型指定传统的移位(<<，>>)和(&)以及(|)操作符。例如:

```
assert((3 << 1) == 6); // 0011 << 1 == 0110
assert((3 >> 1) == 1); // 0011 >> 1 == 0001
assert((3 | 4) == 7); // 0011 | 0100 == 0111
```
文字数字是编译时常量。许多算术表达式也是编译时常量，只要它们的操作数是计算为数字的编译时常量。

```
const msPerSecond = 1000;
const secondsUntilRetry = 5;
const msUntilRetry = secondsUntilRetry * msPerSecond;
```
### Strings
在Dart中可以使用双引号或者单引号来定义一个字符串：

```
var s1 = 'Single quotes work well for string literals.';
var s2 = "Double quotes work just as well.";
var s3 = 'It\'s easy to escape the string delimiter.';
var s4 = "It's even easier to use the other delimiter.";
```
可以使用${expression}将表达式的值放入字符串中。如果表达式是变量，可以跳过{}。要获取与对象对应的字符串，Dart调用对象的toString()方法。

```
var s = 'string interpolation';

assert('Dart has $s, which is very handy.' ==
    'Dart has string interpolation, ' +
        'which is very handy.');
assert('That deserves all caps. ' +
        '${s.toUpperCase()} is very handy!' ==
    'That deserves all caps. ' +
        'STRING INTERPOLATION is very handy!');

// ==运算符测试两个对象是否相等。如果两个字符串包含相同的代码单元序列，则它们是相等的。
```
你可以连接字符串使用相邻的字符串文字或+运算符:

```
var s1 = 'String '
    'concatenation'
    " works even over line breaks.";
assert(s1 ==
    'String concatenation works even over '
    'line breaks.');

var s2 = 'The + operator ' + 'works, as well.';
assert(s2 == 'The + operator works, as well.');
```
另一种创建多行字符串的方法是:   
使用带有单引号或双引号的三引号:

```
var s1 = '''
You can create
multi-line strings like this one.
''';

var s2 = """This is also a
multi-line string.""";
```
你可以创建一个“原始”字符串通过加前缀r:

```
var s = r'In a raw string, not even \n gets special treatment.';
```
文字字符串是编译时常量，只要任何插值表达式是计算为null或数字、字符串或布尔值的编译时常量。

```
// These work in a const string.
const aConstNum = 0;
const aConstBool = true;
const aConstString = 'a constant string';

// These do NOT work in a const string.
var aNum = 0;
var aBool = true;
var aString = 'a string';
const aConstList = [1, 2, 3];

const validConstString = '$aConstNum $aConstBool $aConstString';
// const invalidConstString = '$aNum $aBool $aString $aConstList';
```
### Booleans
Dart中使用bool来代表布尔值，常用于判断：

```
// Check for an empty string.
var fullName = '';
assert(fullName.isEmpty);

// Check for zero.
var hitPoints = 0;
assert(hitPoints <= 0);

// Check for null.
var unicorn;
assert(unicorn == null);

// Check for NaN.
var iMeantToDoThis = 0 / 0;
assert(iMeantToDoThis.isNaN);
```
### Lists
也许几乎每一种编程语言中最常见的集合是数组或有序对象组。在Dart中，数组是列表对象，所以大多数人只称它们为列表。

Dart列表看起来像JavaScript数组很像:

```
var list = [1, 2, 3];
// Dart VM会默认当前list的类型为int，当你这时候加入非int类型的值那么将会报错。
```
List是是从0开始算起，你可以像Java或者JS语言类似的使用它：

```
var list = [1, 2, 3];
assert(list.length == 3);
assert(list[1] == 2);

list[1] = 1;
assert(list[1] == 1);
```
可以通过const关键字使这个List变成“常量”List:

```
var constantList = const [1, 2, 3];
// constantList[1] = 1; // 是个常量数组 这时候赋值就会报错
```
### Maps
通常，map是一个将键和值关联起来的对象。键和值可以是任何类型的对象。每个键只出现一次，但是您可以多次使用相同的值:

```
var gifts = {
  // Key:    Value
  'first': 'partridge',
  'second': 'turtledoves',
  'fifth': 'golden rings'
};

var nobleGases = {
  2: 'helium',
  10: 'neon',
  18: 'argon',
};
// 再Dart VM中跟List类似这里的gifts的是Map<String, String>,nobleGases是Map<int, String>
```
当然你也可以使用Map的构造函数来初始化一个Map对象：

```
var gifts = Map();
gifts['first'] = 'partridge';
gifts['second'] = 'turtledoves';
gifts['fifth'] = 'golden rings';

var nobleGases = Map();
nobleGases[2] = 'helium';
nobleGases[10] = 'neon';
nobleGases[18] = 'argon';
```
也可以像JS语言那样直接往Map对象中添加键值对：

```
var gifts = {'first': 'partridge'};
gifts['fourth'] = 'calling birds';
```
一样可以像JS从Map对象中取出值：

```
var gifts = {'first': 'partridge'};
assert(gifts['first'] == 'partridge');
```
如果根据一个key值从Map对象中取值如果不存在该Key那么返回null：

```
var gifts = {'first': 'partridge'};
assert(gifts['fifth'] == null);
```
可以通过.length取出Map对象的长度：

```
var gifts = {'first': 'partridge'};
gifts['fourth'] = 'calling birds';
assert(gifts.length == 2);
```
一样可以使用const创建一个常量Map对象：

```
final constantMap = const {
  2: 'helium',
  10: 'neon',
  18: 'argon',
};
// constantMap[2] = 'Helium';  // 修改常量的Map将会报错
```
### Runes
在Dart中 runes是一个UTF-32编码的字符串。  
Unicode为世界上所有的书写系统中使用的每个字母、数字和符号定义一个惟一的数字值。由于Dart字符串是UTF-16代码单元的序列，因此在字符串中表示32位Unicode值需要特殊的语法。

表达Unicode编码点的常用方法是\uXXXX，其中XXXX是一个4位十六进制值。例如,字符(♥)\ u2665核心。若要指定多于或少于4位十六进制数字，请将值放在大括号中。对于example,笑emoji(😆)\u{1f600}.

String类有几个属性可以用来提取rune信息。codeUnitAt和codeUnit属性返回16位代码单元。使用runes属性获取字符串的runes。

```
main() {
  var clapping = '\u{1f44f}';
  print(clapping);
  print(clapping.codeUnits);
  print(clapping.runes.toList());

  Runes input = new Runes(
      '\u2665  \u{1f605}  \u{1f60e}  \u{1f47b}  \u{1f596}  \u{1f44d}');
  print(new String.fromCharCodes(input));
}
```
### Symbols
Symbols对象表示在Dart程序中声明的操作符或标识符。你可能永远不需要使用Symbols，但是对于按名称引用标识符的api来说，它们是非常宝贵的，因为缩小会更改标识符名称，但不会更改标识符符号。

若要获取标识符的符号，请使用符号文字，即在标识符后面加#:

```
#radix
#bar
```
Symbols是编译时常量。