## 关键字
下面这个表格列举了Dart中相关的关键字：

|||||
| ------ | ------ | ------ | ------ |
|abstract<sup>2</sup> |dynamic<sup>2</sup>|implements<sup>2</sup>|show<sup>1</sup>|
| as<sup>2</sup> | else | import<sup>2</sup> |static<sup>2</sup>|
| assert | enum | 	in |super|
| async<sup>1</sup> | 	export<sup>2</sup> | interface<sup>2</sup> |switch|
| await<sup>3</sup>  | extends | is |sync<sup>1</sup>|
| break | external<sup>2</sup>| library<sup>2</sup> |this|
| case | 	factory<sup>2</sup> | 	mixin<sup>2</sup> |throw|
| catch | false | 	new |	true|
| class | final | 	null |try|
| const | 	finally | on<sup>1</sup>  |	typedef<sup>2</sup> |
| continue | for | operator<sup>2</sup>  |	var|
| covariant<sup>2</sup>  | Function<sup>2</sup>  | part<sup>2</sup> |void|
| default | get<sup>2</sup>  | rethrow |while|
| deferred<sup>2</sup>  | hide <sup>1</sup>| return |with|
| do | if | set <sup>2</sup> |	yield<sup>3</sup> |

避免使用这些词作为标识符。但是，如果需要，用上标标记的关键字可以是标识符:  

* 上标1的单词是上下文关键字，只有在特定的地方才有意义。它们到处都是有效的标识符。
*  上标2的单词是内置标识符。为了简化将JavaScript代码移植到Dart的任务，这些关键字在大多数地方都是有效的标识符，但它们不能用作类名或类型名，也不能用作导入前缀。
*  上标3的单词是更新的、有限的保留单词，与Dart 1.0版本之后添加的异步支持相关。不能在任何使用async、async*或sync*标记的函数体中使用wait或yield作为标识符。

表中的所有其他单词都是保留单词，不能作为标识符也不能作为属性名。