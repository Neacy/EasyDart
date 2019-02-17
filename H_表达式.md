## 表达式
你可以使用以下任何一种方法来控制Dart代码的流程:

* if and else
* for loops
* while and do-while loops
* break and continue
* switch and case
* assert

### If and else
最为常见：

```
if (isRaining()) {
  you.bringRainCoat();
} else if (isSnowing()) {
  you.wearJacket();
} else {
  car.putTopDown();
}
```
### for循环
普通形式：

```
var message = StringBuffer('Dart is fun');
for (var i = 0; i < 5; i++) {
  message.write('!');
}
```
in形式：

```
var collection = [0, 1, 2];
for (var x in collection) {
  print(x); // 0 1 2
}
```
### While 和 do-while
while：

```
while (!isDone()) {
  doSomething();
}
```
do-while:

```
do {
  printLine();
} while (!atEndOfPage());
```

### Break 和 continue
break：

```
while (true) {
  if (shutDownRequested()) break;
  processIncomingRequests();
}
```
continue:

```
for (int i = 0; i < candidates.length; i++) {
  var candidate = candidates[i];
  if (candidate.yearsExperience < 5) {
    continue;
  }
  candidate.interview();
}
```

### Switch 和 case
Dart中的Switch语句使用==比较整数、字符串或编译时常量。比较的对象必须都是相同类的实例(而不是它的任何子类型的实例)，并且该类不能覆盖==。枚举类型在switch语句中工作良好。

```
var command = 'OPEN';
switch (command) {
  case 'CLOSED':
    executeClosed();
    break;
  case 'PENDING':
    executePending();
    break;
  case 'APPROVED':
    executeApproved();
    break;
  case 'DENIED':
    executeDenied();
    break;
  case 'OPEN':
    executeOpen();
    break;
  default:
    executeUnknown();
}
```
当你在case中省略了break那么将会报错：

```
var command = 'OPEN';
switch (command) {
  case 'OPEN':
    executeOpen();
    // ERROR: Missing break

  case 'CLOSED':
    executeClosed();
    break;
}
```
而且case中是不允许存在空：

```
var command = 'CLOSED';
switch (command) {
  case 'CLOSED': // Empty case falls through.
  case 'NOW_CLOSED':
    // Runs for both CLOSED and NOW_CLOSED.
    executeNowClosed();
    break;
}
```
当然你可以使用continue来处理case空的情况：

```
var command = 'CLOSED';
switch (command) {
  case 'CLOSED':
    executeClosed();
    continue nowClosed;
  // Continues executing at the nowClosed label.

  nowClosed:
  case 'NOW_CLOSED':
    // Runs for both CLOSED and NOW_CLOSED.
    executeNowClosed();
    break;
}
```

### Assert
Assert常用判空处理：

```
// Make sure the variable has a non-null value.
assert(text != null);

// Make sure the value is less than 100.
assert(number < 100);

// Make sure this is an https URL.
assert(urlString.startsWith('https'));
```
需要注意的是assert只会在debug的情况下生效，release的情况下会自动忽略掉。