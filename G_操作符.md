## 操作符
Dart中的操作跟Java/JS还是比较相像，基本上可以按Java/JS的思想来对待Dart中的操作符，整体的表格如下：

|描述|操作符|
| ------ | ------ |
|一元后缀|expr++ expr--    ()    []    .    ?.|
|一元前缀|-expr    !expr    ~expr    ++expr    --expr|
|乘除|*    /    %  ~/|
|加减|+    -|
|位移|<<    >>|
|位与|&|
|位非|^|
|位或|\||
|关系类型检验|>=    >    <=    <    as    is    is!|
|相等比较|==    != |
|逻辑与|&&|
|逻辑或|\|\||
|非空|??|
|条件表达式|expr1 ? expr2 : expr3|
|级联|..|
|赋值|=    *=    /=    ~/=    %=    +=    -=    <<=    >>=    &=    ^=    |=    ??=|

操作符都是使用在表达式中：

```
a++
a + b
a = b
a == b
c ? a : b
a is T
```
### 算术表达式
|操作符|描述|
| ------ | ------ |
|+|加|
|-|减|
|-expr|一元减号，也称为否定(表达式的符号反转)|
|\*|乘|
|/|除|
|~/|除法，返回一个整数结果|
|%|求余|
举例：

```
assert(2 + 3 == 5);
assert(2 - 3 == -1);
assert(2 * 3 == 6);
assert(5 / 2 == 2.5); // Result is a double
assert(5 ~/ 2 == 2); // Result is an int
assert(5 % 2 == 1); // Remainder

assert('5/2 = ${5 ~/ 2} r ${5 % 2}' == '5/2 = 2 r 1');
```
同样也支持 前缀++/前缀--/后缀++/后缀--

|操作符|描述|
| ------ | ------ |
|++var|var = var + 1(结果返回var + 1)|
|var++|var = var + 1(结果返回var)|
|--var|var = var - 1(结果返回var - 1)|
|var--|var = var - 1(结果返回var)|
举例：

```
var a, b;

a = 0;
b = ++a; // Increment a before b gets its value.
assert(a == b); // 1 == 1

a = 0;
b = a++; // Increment a AFTER b gets its value.
assert(a != b); // 1 != 0

a = 0;
b = --a; // Decrement a before b gets its value.
assert(a == b); // -1 == -1

a = 0;
b = a--; // Decrement a AFTER b gets its value.
assert(a != b); // -1 != 0
```
### 等式和关系运算符
|操作符|描述|
| ------ | ------ |
|==|相等|
|!=|不等|
|>|大于|
|<|小于|
|>=|大于等于|
|<=|小于等于|
举例：

```
assert(2 == 2);
assert(2 != 3);
assert(3 > 2);
assert(2 < 3);
assert(3 >= 3);
assert(2 <= 3);
```
### 类型操作符
|操作符|描述|
| ------ | ------ |
| as |定型|
|is|类型属于|
|is!|类型不属于|
举例：

```
if (emp is Person) {
  emp.firstName = 'Bob';
}

(emp as Person).firstName = 'Bob';
```
### 赋值操作符
相关操作符可以查看上面的表格，举例：

```
// 赋值
a = value;
// 非空赋值
b ??= value;
```
### 位移以及逻辑操作符
相关操作符见上方表格，举例：

```
final value = 0x22;
final bitmask = 0x0f;

assert((value & bitmask) == 0x02); // AND
assert((value & ~bitmask) == 0x20); // AND NOT
assert((value | bitmask) == 0x2f); // OR
assert((value ^ bitmask) == 0x2d); // XOR
assert((value << 4) == 0x220); // Shift left
assert((value >> 4) == 0x02); // Shift right
```
### 条件表达式
Dart中条件操作符有两种：

condition ? expr1 : expr2 当条件为true的时候返回expr1否则返回expr2

expr1 ?? expr2  当expr1为true的时候返回expr1否则返回expr2

```
var visibility = isPublic ? 'public' : 'private';

String playerName(String name) => name ?? 'Guest';
```

### 级联表达式(..)
级联(..)允许对同一对象执行一系列操作。除了函数调用之外，还可以访问同一对象上的字段。这通常会节省创建临时变量的步骤，并允许你编写更流畅的代码。

```
querySelector('#confirm') // Get an object.
  ..text = 'Confirm' // Use its members.
  ..classes.add('important')
  ..onClick.listen((e) => window.alert('Confirmed!'));
```
下面更直观的展示一下级联表达式的好处：
```
var button = querySelector('#confirm');
button.text = 'Confirm';
button.classes.add('important');
button.onClick.listen((e) => window.alert('Confirmed!'));
```
原先的调用得分为好几个步骤，改用级联的表达式：

```
final addressBook = (AddressBookBuilder()
      ..name = 'jenny'
      ..email = 'jenny@example.com'
      ..phone = (PhoneNumberBuilder()
            ..number = '415-555-0100'
            ..label = 'home')
          .build())
    .build();
```
在返回实际对象的函数上构造级联时要小心。例如，以下代码失败:

```
var sb = StringBuffer();
sb.write('foo')
  ..write('bar'); // Error: method 'write' isn't defined for 'void'.
```