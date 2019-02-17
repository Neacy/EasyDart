## 更多Dart重要知识
### Generator
当您需要惰性地生成一个值序列时，请考虑使用生成器函数。Dart内置对两种生成器函数的支持:

* 同步生成器:返回一个可迭代对象。
* 异步生成器:返回流对象。

要实现同步生成器函数，请将函数体标记为sync*，并使用yield语句返回值:

```
Iterable<int> naturalsTo(int n) sync* {
  int k = 0;
  while (k < n) yield k++;
}
```
同样，要像实现异步生成器函数，那么把函数标记为async*，也是通过yield返回值：

```
Stream<int> asynchronousNaturalsTo(int n) async* {
  int k = 0;
  while (k < n) yield k++;
}
```
如果你的生成器是递归的，你可以使用yield*来提高它的性能:

```
Iterable<int> naturalsDownFrom(int n) sync* {
  if (n > 0) {
    yield n;
    yield* naturalsDownFrom(n - 1);
  }
}
```
### 可调用的类
要允许像函数一样调用Dart类，请实现call()方法，例如：

```
class WannabeFunction {
  call(String a, String b, String c) => '$a $b $c!';
}

main() {
  var wf = new WannabeFunction();
  var out = wf("Hi","there,","gang");
  print('$out');
}
```
如上代码，声明了WannabeFunction类，并实现了call函数<需要三个参数>。

### Isolates
大多数计算机，甚至在移动平台上，都有多核cpu。为了利用所有这些核心，开发人员通常使用并发运行的共享内存线程。然而，共享状态并发很容易出错，并可能导致复杂的代码。

所有Dart代码都运行在isolates中，而不是线程中。每个隔离程序都有自己的内存堆，确保没有隔离程序的状态可以从任何其他隔离程序访问。
### Typedefs
在Dart中，函数是对象，就像字符串和数字是对象一样。typedef或函数类型别名为函数类型提供了一个名称，可以在声明字段和返回类型时使用。类型定义在将函数类型分配给变量时保留类型信息。

考虑下面的代码，它不使用类型定义:

```
class SortedCollection {
  Function compare;

  SortedCollection(int f(Object a, Object b)) {
    compare = f;
  }
}

// Initial, broken implementation.
int sort(Object a, Object b) => 0;

void main() {
  SortedCollection coll = SortedCollection(sort);

  // All we know is that compare is a function,
  // but what type of function?
  assert(coll.compare is Function);
}
```
在分配f进行比较时，类型信息将丢失。f的类型(对象,对象)→int(→意味着回报),然而,类型的比较函数。如果我们更改代码以使用显式名称并保留类型信息，开发人员和工具都可以使用这些信息。

```
typedef Compare = int Function(Object a, Object b);

class SortedCollection {
  Compare compare;

  SortedCollection(this.compare);
}

// Initial, broken implementation.
int sort(Object a, Object b) => 0;

void main() {
  SortedCollection coll = SortedCollection(sort);
  assert(coll.compare is Function);
  assert(coll.compare is Compare);
}

```