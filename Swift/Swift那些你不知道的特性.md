### 声明特性

##### available 版本可用

@available(iOS 10.0, macOS 10.12, *)

##### discardableResult 

把这个特性用在函数或方法的声明中，当调用一个有返回值的函数或者方法却没有使用返回值时，编译器不会产生警告

##### dynamicCallable

为类、结构体、枚举或者协议添加这个特性来将这个类型的实例视为可调用函数

类型要么实现dynamicallyCall(withArguments:) 方法，要么实现 dynamicallyCall(withKeywordArguments:)

##### dynamicMemberLookup

应用这个特性到类、结构体、枚举或者协议来允许在运行时可通过名字查找成员。类型必须实现一个subscript(dynamicMemberLookup:) 下标脚本。

##### inlinable (函数，方法，计算属性，下表脚本，初始化器，私有属性或方法不可以)

暴露这个声明的实现作为模块的公开接口。这就允许编译器在调用时替换行内符号为这个符号具体实现的拷贝

##### nonobjc

废除一个隐式 objc 特性 ，一个标记为 nonobjc 特性的方法不能重写标为 objc 特性的方法。但是，一个标记为 objc 特性的方法可以重写一个标为nonobjc 特性的方法。同样，一个标为 nonobjc 特性的方法不能满足一个标为 objc 特性方法的协议需求

##### NSApplicationMain

应用于一个类中，来指明他是应用的委托，等同于调用调用 NSApplicationMain(_:_:) 

##### NSCopying

这个特性让属性值（由 copyWithZone(_:) 方法返回，而不是属性本身的值）的*拷贝*合成属性的setter。属性的类型必须遵循 NSCopying 协议

##### objc

把这个特性用到任何可以在 Objective-C 中表示的声明上——例如，非内嵌类，协议，非泛型枚举（原始值类型只能是整数），类和协议的属性、方法（包括 setter 和 getter ），初始化器，反初始化器，下标。 objc 特性告诉编译器，这个声明在 Objective-C 代码中是可用的	

objc会在下列情况隐士的添加 

- 声明是子类的重写，并且父类的声明有objc特性
- 声明满足的需求来自一个拥有objc特性的协议
- 声明有I，IBOutlet等特性 会自动添加objc

objc 特性可以接受一个特性实际参数，由一个标识符组成。当你想在 Objective-C 中为 objc 特性标记的实体暴露一个不同的名字时，用这个特性

##### objcMembers

给任何可以拥有 objc 特性的类声明添加这个特性。 objc 特性会隐式地添加到类的 Objective-C 兼容成员、扩展、子类以及它们所有的扩展。

### 类型特性

### Switch情况特性



### Swift知识小集

##### IOS安全体现在哪里 ：

- 可选值，
- 类型安全 一个String类型 不能赋值一个Int类型的值

在枚举中如果一个枚举需要一个参数也是枚举的情况 需要添加indirect关键字来告诉编译器这是个递归

```swift

/*或者在这里添加indirect*/enum ArithmeticExpression {
    case number(Int)
    indirect case addition(ArithmeticExpression, ArithmeticExpression)
    indirect case multiplication(ArithmeticExpression, ArithmeticExpression)
}
```



##### 字符串三个引号引用的字符串

```swift
let string = """
    This is a string.
		     noignore.
    ignore
	  """
//在第二个引号后面的空格有作用，而前面的是没有作用的
```

##### 字符串类型和数组类型相似也是可以通过`isEmpty` 来判断是否为空

##### 字符串可以通过append方法来添加一个Character类型 

```swift
///字符添加字符
let charString : Character = "a"
var cString = "bb"
cString.append(charString)
print(cString)
///bba

```

##### 用indices 循环来访问string索引

```swift
let idcString = "dicesString"
for index in idcString.indices {
    print(idcString[index])
}
//dicesString
```

> Tips:
>
> 你可以在任何遵循了` Indexable` 协议的类型中使用` startIndex` 和 `endIndex` 属性以及 `index(before:)` ， `index(after:)` 和`index(_:offsetBy:)` 方法。这包括这里使用的 String ，还有集合类型比如 Array ， `Dictionary` 和 `Set`

##### 用range删除字符串中的字符

```swift
var deleteString = "hello world"
let deleteRange  = deleteString.index(deleteString.startIndex, offsetBy: 5)..<deleteString.endIndex
deleteString.removeSubrange(deleteRange)
print(deleteString)
//hello
```



##### swift中的do-while

```swift
repeat {
    statements
} while condition
```



##### Switch case 中如果有多个case符合匹配，只匹配第一个值

```swift
 将会最先匹配 case(0,0) ，所以接下来的所有再匹配到的情况将被忽略
///使用Where来判断各种情况
let yetAnotherPoint = (1, -1)
switch yetAnotherPoint {
case let (x, y) where x == y:
    print("(\(x), \(y)) is on the line x == y")
case let (x, y) where x == -y:
    print("(\(x), \(y)) is on the line x == -y")
case let (x, y):
    print("(\(x), \(y)) is just some arbitrary point")
}
```

#### 函数的OC不同

##### swift形参可以省略

##### 可返回多值用元祖

##### 隐士返回值

如果一个函数是一个单一表达式，那么函数隐士返回这个表达式:

```swift
func greeting(_ person : String) -> String{
  "Hello " + person
}
//一样和下面的函数 
func greeting(_ persion : String) -> String{
  return  "Hello " + person
}
```

##### 内嵌函数

##### 可以省略实际参数标签

##### swift参数可以有默认值

##### swift 函数可变形式参数 

一个可变形参可以接受领个或者多个特定类型的值,类型是在形参后面插入三个点符号(...)

```swift
func add(_ nums : Int...) -> Int{
  var sum : Int = 0
  for num in nums {
    sum += num
  }
  return sum
}

add(2,3,4)
add(2,3,4,8,9,10)

```

##### swift中的函数类型 

```swift
func add(_ a : Int , _ b : Int) -> Int {
  
}

func sum(_ a : Int , _ b : Int) -> Int {
  
}

这两个函数都是(Int,Int)->Int类型的函数
```



##### 函数类型作为形参类型传入

```swift 
func processTwoNum(_ method : (Int,Int)->Int, _ a : Int , _ b : Int) -> Int {
  return method(a,b)
}

let a = 2
let b = 4
var method = add
let c = processTwoNum(method,a,b)
var method = sum
processTwoNum(sum,b,c)

```



#### Swift 中的闭包

1. 全局函数是一个有名字但不会补货任何值的闭包

2. 内嵌函数是一个有名字且能从其上层函数补货值的闭包

3. 闭包表达式是一个轻量级语法所写的可以在补货其上下文中常量或变量值的没有名字的闭包

   

Swift闭包的优化：

- 利用上下文推断形式参数和返回值的类型
- 单表达式的闭包可以隐士返回
- 简写实际参数名
- 尾随闭包语法
- 闭包是引用类型

##### 逃逸闭包

当闭包作为一个实际参数传递给一个函数的时候，我们就说这个闭包逃逸了，因为他是可以在函数返回之后被调用的 方法就是在函数前面添加 @escaping 

特点：

1. 被储存在电一函数外的变量里
2. 需要显示的引用self

##### 自动闭包

*自动闭包*是一种自动创建的用来把作为实际参数传递给函数的表达式打包的闭包。它不接受任何实际参数，并且当它被调用时，它会返回内部打包的表达式的值。这个语法的好处在于通过写普通表达式代替显式闭包而使你省略包围函数形式参数的括号