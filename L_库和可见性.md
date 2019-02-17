## 库和可见性
import和libary指令可以帮助您创建模块化和可共享的代码库。库不仅提供api，而且是一个隐私单元:以下划线(_)开头的标识符仅在库中可见。每个Dart应用程序都是一个库，即使它不使用库指令。

可以使用包分发库。有关SDK中包含的包管理器Pub的信息，请参见Pub包和资产管理器。

### 引入库
可用关键字import来导入一个库，比如：

```
import 'dart:html';
```
import操作只需要所需的惟一参数是指定库的URI，在Dart中如果是内置的库，一般是以dart:开头，而对于别的库那么是以package:开头。package:指定的库URI通常来自包管理器(如pub)提供的，例如：

```
import 'package:test/test.dart';
```
### 指定库前缀
如果你引入两个名字一样的库，那么会引起冲突，这时候你可以给其中一个库指定一个前缀，比如说：

```
import 'package:lib1/lib1.dart';
import 'package:lib2/lib2.dart' as lib2;

// Uses Element from lib1.
Element element1 = Element();

// Uses Element from lib2.
lib2.Element element2 = lib2.Element();
```
### 只导入库的一部分
如果只想使用库的一部分，可以有选择地导入库。例如:

```
// Import only foo.
import 'package:lib1/lib1.dart' show foo;

// Import all names EXCEPT foo.
import 'package:lib2/lib2.dart' hide foo;
```

### 延迟加载库
延迟加载(也称为延迟加载)允许应用程序在需要时按需加载库。以下是一些可能使用延迟加载的情况:

* 减少应用程序的初始启动时间。
* 执行A/B测试——例如尝试算法的其他实现。
* 加载很少使用的功能，如可选屏幕和对话框。
要延迟加载库，首先必须使用deferred as导入它。

```
import 'package:greetings/hello.dart' deferred as hello;
```
当你需要这个库时，使用这个库的标识符调用loadLibrary()。

```
Future greet() async {
  await hello.loadLibrary();
  hello.printGreeting();
}
```
在前面的代码中，await关键字暂停执行，直到加载库。

尽管你可能多次调用loadLibrary()方法，但是库只会加载一次。

在你使用延迟加载库的时候，需要谨记：

* 延迟库的常量不是导入文件中的常量。记住，在加载deferred库之前，这些常量不存在。
* 不能在导入文件中使用延迟库中的类型。相反，考虑将接口类型移动到由延迟库和导入文件导入的库中。
* Dart隐式地将loadLibrary()插入使用deferred作为命名空间定义的命名空间中。函数的作用是:返回一个Future。

更多操作参考： <https://www.dartlang.org/guides/libraries/create-library-packages>