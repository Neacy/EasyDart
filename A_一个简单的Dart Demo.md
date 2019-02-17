## 一个简单的Dart Demo
先简单的看一个Dart的例子，方便了解更多的Dart语言的特性：
  
```
// 声明一个函数
printInteger(int aNumber) {
  print('The number is $aNumber.'); // 打印输出
}

// Dart程序的主函数
main() {
  var number = 42; // 声明并初始化一个变量number
  printInteger(number); // 调用声明的函数
}
```
介绍一些Dart平时开发常见<使用率极高>的一些特性：

* 注解  

```
单行注解：  
// 这里一个单行注解
  
多行注解：  
/**
 * 这是一个多行注解
 */
 
另一种多行注解<当AS是黑色主题的时候会以绿色高亮的效果>
/// 这是一个多行注解
```
* 常见内置类型

```
int:整型
例如： int number = 42;

String:字符串, 通常用 '' 或者 ""
例如： String name = "Name"; 或者 String name = ‘Name’;
字符串拼接：
String name = 'Name is $Jayu';
或者
String name = 'Name is ${Jayu chou}';

List:列表 
ps: 例子后面再介绍

bool:布尔值
bool isName = true; 或者 bool isAge = false;
```
* 打印日志

```
print('这是一条日志输出');
```
* 变量声明

```
变量声明的方式：
可以统一通过var声明，如： var number = 42;
亦或者通过类型/类名来声明： 
String name = 'name'; 
ClassName cn = ClassName();
```
* main主函数

```
Dart类似Java有个主函数作为入口，叫做main();
```

