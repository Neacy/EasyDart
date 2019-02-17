## 异常
Dart代码可以抛出和捕获异常。异常是指示发生意外事件的错误。如果异常未被捕获，引发异常的隔离程序将被挂起，通常隔离程序及其程序将被终止。

对比Java，我们可以发现Dart中的函数并不会告诉你有哪些异常需要你去捕捉。

Dart中提供了Execeptions和Errors,以及一些预定义的子类型，当然也也可以自定义异常。

### Throw
常见的写法：

```
throw FormatException('Expected at least 1 section');
```
当然你也可以抛出任何对象：

```
throw 'Out of llamas!';
```
### Catch
通过catch来捕捉异常，防止代码奔溃。当然捕捉后你可以进一步处理：

```
try {
  breedMoreLlamas();
} on OutOfLlamasException {
  buyMoreLlamas();
}
```
要处理可以抛出不止一种异常类型的代码，可以指定多个catch子句。匹配抛出对象类型的第一个catch子句处理异常。如果catch子句没有指定类型，则该子句可以处理任何类型的抛出对象:

```
try {
  breedMoreLlamas();
} on OutOfLlamasException {
  // A specific exception
  buyMoreLlamas();
} on Exception catch (e) {
  // Anything else that is an exception
  print('Unknown exception: $e');
} catch (e) {
  // No specified type, handles all
  print('Something really unknown: $e');
}
```
正如前面的代码所示，您可以使用on、catch或两者兼用。在需要指定异常类型时使用on。当异常处理程序需要异常对象时，使用catch。

你也可以为catch()指定一个或者两个参数，第一个参数代表错误，第二个参数代表的调用栈：

```
try {
  // ···
} on Exception catch (e) {
  print('Exception details:\n $e');
} catch (e, s) {
  print('Exception details:\n $e');
  print('Stack trace:\n $s');
}
```
要在允许异常传播的同时部分处理异常，请使用rethrow关键字。

```
void misbehave() {
  try {
    dynamic foo = true;
    print(foo++); // Runtime error
  } catch (e) {
    print('misbehave() partially handled ${e.runtimeType}.');
    rethrow; // Allow callers to see the exception.
  }
}

void main() {
  try {
    misbehave();
  } catch (e) {
    print('main() finished handling ${e.runtimeType}.');
  }
}
```
### Finally
要确保无论是否抛出异常，一些代码都会运行，请使用finally子句。如果没有catch子句匹配异常，则在finally子句运行之后传播异常:

```
try {
  breedMoreLlamas();
} finally {
  // Always clean up, even if an exception is thrown.
  cleanLlamaStalls();
}
```
任何的catch回调运行之后仍然会执行finally回调：

```
try {
  breedMoreLlamas();
} catch (e) {
  print('Error: $e'); // Handle the exception first.
} finally {
  cleanLlamaStalls(); // Then clean up.
}
```