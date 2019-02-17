## 异步
Dart库中充满了返回Future或Stream对象的函数。这些函数是异步的:它们在设置可能耗时的操作(如I/O)之后返回，而不需要等待该操作完成。

async和wait关键字支持异步编程，允许您编写类似于同步代码的异步代码。

### Future
当你需要一个完整Future的结果时，你有两个选择:

* 使用异步并等待。
* 如库之旅所述，使用Future API。
使用async和await的代码是异步的，但它看起来很像同步代码。例如，下面是一些使用wait来等待异步函数结果的代码:

```
await lookUpVersion();
```
要使用await，代码必须在异步函数中—标记为async的函数:

```
Future checkVersion() async {
  var version = await lookUpVersion();
  // Do something with version
}
```
使用try、catch和finally来处理使用wait的代码中的错误和清理:

```
try {
  version = await lookUpVersion();
} catch (e) {
  // React to inability to look up the version
}
```
可以在异步函数中多次使用wait。例如，下面的代码等待三次函数的结果:

```
var entrypoint = await findEntrypoint();
var exitCode = await runExecutable(entrypoint, args);
await flushThenExit(exitCode);
```
在await expression中，expression的价值通常是Future;如果不是，那么该值将用Future自动包装。这个Future对象表示返回一个对象的承诺。wait表达式的值是返回的对象。wait表达式使执行暂停，直到该对象可用为止。

当你在编写await代码的时候，报了错误。那么需要查看一下是否await相关的代码是否写在async函数中。比如说，你想要main函数中编写await代码的话  那么就需要把main函数末尾加上async标识：

```
Future main() async {
  checkVersion();
  print('In main: version is ${await lookUpVersion()}');
}
```
### 定一个async函数
通过在函数的末尾加上async标识，如果没有加的话默认是同步函数：

```
String lookUpVersion() => '1.0.0';
```
所以需要再函数末尾加上async，由于async函数是一个异步操作，所以统统返回Future：

```
Future<String> lookUpVersion() async => '1.0.0';
```
如果async的函数无需返回值，那么只需要返回值改成：Future<void>
### Streams
如果你需要从Streams中获取值，你必须：

* 使用async 和异步for循环(await for)。
* 使用Streams API。
一个标准的异步循环格式为：

```
await for (varOrType identifier in expression) {
  // Executes each time the stream emits a value.
}
```
表达式的值必须具有类型流。执行进度如下:

* 等待流发出一个值。
* 执行for循环的主体，将变量设置为发出的值。
* 重复1和2，直到流关闭。
你可以通过break 或者 return表达式中断流。

如果在实现异步for循环时出现编译时错误，请确保等待在异步函数中。例如，要在应用程序的main()函数中使用异步For循环，main()的主体必须标记为async:

```
Future main() async {
  // ...
  await for (var request in requestServer) {
    handleRequest(request);
  }
  // ...
}
```
更多异步api可以查看dart:async 库。