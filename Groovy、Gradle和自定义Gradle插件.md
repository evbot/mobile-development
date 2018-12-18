#### 一、摘要
Gradle目前作为Android的默认构建工具，Gradle通过提供DSL指令和Gradle插件等完成Android的编译。Gradle为了很好的描述构建，它提供了基于Groovy的DSL，但增加了一些特殊处理，以便在利用Groovy语言特性的之外，让它更像一个构建语言。如果想知道settings.gradle、build.gradle等的一些配置，需要了解Groovy知识。
#### 二、Groovy语法
[关于Groovy语法可以参考精通Groovy](https://www.ibm.com/developerworks/cn/education/java/j-groovy/j-groovy.html)<br />
##### 2.1 Groovy简介
Groovy语言对Java语言的进行了拓展，它提供了更简单、更灵活的语法，可以在运行时动态地进行类型检查。Java语言语法都适用于Groovy语言，Groovy也可以和Java进行混编。与Java相比，Groovy带来的快捷方式：
- Groovy 松散的Java语法允许省略分号和修改符。
- Groovy 所有内容都为public，除非另行指定。
- Groovy 允许定义简单脚本，同时无需定义正规的class对象。
- Groovy 在普通的常用Java对象上增加了一些独特的方法和快捷方式，使得它们更容易使用。
- Groovy 语法还允许省略变量类型。
从技术上讲，Groovy算是类型最松散的动态语言之一。从这个角度讲，Groovy与Java语言的区别很大，Java语言是一种固定类型语言。在Groovy中，类型是可选的。Groovy代码还能在运行时轻松地改变自己。这实际上意味着，能够在运行时轻松地为对象指定新方法和属性，这称为元编程。Groovy能够很好地支持这种编程方式。
##### 2.2 变量定义
```Java
  def var1 = 1 
  def var2 = 'abc'
  String var3 = "def"
  int int var4 = 2
```
##### 2.3 字符串处理
```Java
def str1 = 'abc $placeHoder' // 单引号不转义
def str2 = "mnp $placeHoder" // 双引号转义
println str1 // 输出“abc $placeHoder”
println str2 // 输出“mnp groovy”
```
##### 2.4 函数定义
```Java
// 无需指定参数类型
String testFunction(arg1, arg2){
  // ...
}
// Groovy中函数的返回值也可以是无类型的。无类型的函数定义，必须使用def关键字  
def nonReturnTypeFunc(){ 
  1000 // 最后一行代码的执行结果就是本函数的返回值 返回1000
}
// 如果指定了函数返回类型，则可不必加def关键字来定义函数  
String getString(){
   return "I am a string"  
}
```

##### 2.5 Groovy容器
- List：链表，其底层对应Java中的List接口，一般用ArrayList作为真正的实现类。
```Java
def aList = ['hello', 10, false, true, new testClass()]
println aList.size()  // 5
println aList[4].toString()  // testClass oh yeah
aList[100] = 'world'
println aList.size()  // 101
println aList.getClass().name // java.util.ArrayList

//list 遍历
aList = [ 1, 2, 3, 4, 5 ];
//遍历方法1
aList.each { it ->
    println it
}
//遍历方法2
aList.eachWithIndex { ele, inx ->
    println "${ele}: ${inx}"
}
```
- Map：键-值表，其底层对应Java中的LinkedHashMap。
```Java
def aMap = [key1: 'abc', key2: 2]
println aMap.key1  // abc
println aMap.key2  // 2
println aMap.keySet()  // [key1, key2]
def key3 = 'ooxx'
def aMap2 = [(key3): 'you are right', opq: 900]
println aMap2.opq   // 900
println aMap2.key3  // null
println aMap2.get('ooxx')  // you are right
println aMap2['ooxx']  // you are right
println aMap2.getClass().name // java.util.LinkedHashMap
//遍历方法1
aMap2.each { entry ->
    // entry.key, entry.value
}
//遍历方法2
aMap2.each { key, value ->
    // key, value
}
```
- Range：范围，它其实是List的一种拓展。
```Java
def aRange = 1..5
def bRange = 1..<5
println aRange  // [1, 2, 3, 4, 5]
println bRange  // [1, 2, 3, 4]
println aRange.from  // 1
println aRange.to    // 5
println aRange.getFrom()  // 1
println aRange.getTo()    // 5
```
##### 2.6 Grovvy闭包
groovy中的一个核心语法：closurs，也叫闭包。闭包在groovy中是一个处于代码上下文中的开放的，匿名代码块。它可以访问到其外部的变量或方法。
- 闭包定义
```Java
{ [closureParameters -> ] statements } //其中[]内是可选的闭包参数，可省略。当闭包带有参数，就需要->来将参数和闭包体相分离。
//闭包的具体例子
{ item++ }                                          
{ -> item++ }                                       
{ println it }                                      
{ it -> println it }                                
{ name -> println name }                            
{ String x, int y ->                                
    println "hey ${x} the value is ${y}"
}
{ reader ->                                         
    def line = reader.readLine()
    line.trim()
}
```
- 闭包也是对象
```Java
//闭包在groovy中是groovy.lang.Closure类的实例，这使得闭包可以赋值给变量或字段。
def listener = { e -> println "Clicked on $e.source" }      
assert listener instanceof Closure
Closure callback = { println 'Done!' }                      
Closure<Boolean> isTextFile = {
    File it -> it.name.endsWith('.txt')                     
}
```
- 闭包的调用
```Java
//闭包有两种调用方式：
def code = { 123 }
assert code() == 123 // 闭包名()来调用闭包
assert code.call() == 123 // 闭包名.call()来调用闭包
```
- 闭包的参数
```Java
// 正常参数，和函数的参数一致
// 参数列表，和函数的参数一致
// 含蓄的参数, 当闭包没有显式声明参数时，其默认包含一个隐式的参数it。
def greeting = { "Hello, $it!" }
assert greeting('Patrick') == 'Hello, Patrick!'
```
- 闭包的委托策略
```Java
this 表示定义闭包的外围类。
owner 表示定义闭包的直接外围对象，可以是类或者闭包。
delegate 表示一个用于处理方法调用和属性处理的第三方类。
```
##### 2.7 Groovy类


