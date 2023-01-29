# Swift

----

[toc]



## Overview

SwiftUI 是一个用户界面工具包, 允许我们以**声明性**方式设计应用程序.



## A Swift Tour

```swift
// 不需要导入单独库, 不需要程序入口 main() 函数
print("Hello, world!")
```

```swift
// var 用于变量, let 用于常量
var myVariable = 42
myVariable = 50
let myConstant = 42
```

```swift
// let 变量名 = 初始值							// 省略类型, 编译器会自动猜测
// let 变量名: 类型 = 初始值
// var 变量名: 类型? = 值						// 问号? 表示 值可选. 表示 这个变量要么有符合类型的值, 要么没有值, 即null/nil. 在强类型语言中, 有类型的值和空值是两个概念. 空值本身也是一种类型, 所以一个变量有两个个类型值, 那需要分别判断和定义, 为此发明‘可选值’这个概念, 并且简写为 ‘类型?’, 表示在类型值和空值之间二选一.

let implicitInteger = 70
let implicitDouble = 70.0
let explicitDouble: Double = 70
var optionalString: String? = "Hello"
```

```swift
// 强类型, 类型转换
// 变量的类型不会隐式转换, 变量类型必须显式的转换
let label = "The width is "
let width = 94
let widthLabel = label + String(width)		// String(94), 转换成 string
```

```swift
// \(), 在字符串里加入变量代换
let apples = 3
let oranges = 5
let appleSummary = "I have \(apples) apples."
let fruitSummary = "I have \(apples + oranges) pieces of fruit."
```

```swift
// """, 三个双引号, 多行的字符串
let quotation = """
I said "I have \(apples) apples."
And then I said "I have \(apples + oranges) pieces of fruit."
"""
```

```swift
// 数组
var fruits = ["strawberries", "limes", "tangerines"]
fruits[1] = "grapes"								// 赋值
fruits.append("blueberries")				// 追加
let emptyArray: [String] = []				// 创建空数组
fruits = []													// 编译器猜测数组元素类型

// 字典
var occupations = [
    "Malcolm": "Captain",
    "Kaylee": "Mechanic",
]
occupations["Jayne"] = "Public Relations"
let emptyDictionary: [String: Float] = [:]				// 创建空字典
occupations = [:]																	// 编译器猜测字典 key 和 value 的类型
```

```swift
// for - in, while, repeat-while
// for 下标 in 可遍历迭代器 {...}
let individualScores = [75, 43, 103, 87, 12]
var teamScore = 0
for score in individualScores {					// for 遍历时的当前元素 in 数组
    if score > 50 {
        teamScore += 3
    } else {
        teamScore += 1
    }
}

// for - in 遍历字典
let interestingNumbers = [
    "Prime": [2, 3, 5, 7, 11, 13],
    "Fibonacci": [1, 1, 2, 3, 5, 8],
    "Square": [1, 4, 9, 16, 25],
]
var largest = 0
for (_, numbers) in interestingNumbers {				// _, 站位和替代
    for number in numbers {
        if number > largest {
            largest = number
        }
    }
}
print(largest)
// Prints "25"

// while
var n = 2
while n < 100 {
    n *= 2
}
print(n)
// Prints "128"

// repeat - while
var m = 2
repeat {
    m *= 2
} while m < 100
print(m)
// Prints "128"
```

```swift
// if
if 布尔表达式 { ... }

// switch
let vegetable = "red pepper"
switch vegetable {
case "celery":
    print("Add some raisins and make ants on a log.")
case "cucumber", "watercress":
    print("That would make a good tea sandwich.")
case let x where x.hasSuffix("pepper"):
    print("Is it a spicy \(x)?")
default:
    print("Everything tastes good in soup.")
}
// Prints "Is it a spicy red pepper?"
```

```swift
// 处理 可选值, 有两种方法:
// 方法一: if 常规方法, 分支判断处理
var optionalString: String? = "Hello"
print(optionalString == nil)
// Prints "false"

var optionalName: String? = "John Appleseed"
var greeting = "Hello!"
if let name = optionalName {										// let name 局部变量仅在 if 范围内有效
    greeting = "Hello, \(name)"
}
// 方法二: ?? 空合 运算符
let informalGreeting = "Hi \(nickname ?? fullName)"		// nickname 可选, 如果 nickname 为空值, 那么返回 fullname
等同于
if let nickname {
    print("Hey, \(nickname)")
}
```

```swift
// .., ..<, ..., range 操作符, 范围操作符, 范围比较
0..3, 0 至 3 之间, 不包含 0 和 3
0..<3, 包含 0 但不包含 3
0...3, 就表示从 0 开始到 3 为止并包含 3 这个数字的范围, 全闭合

"a"..."z", 表示全小写的26个字母
\0...~,  表示有效的 ASCII 字符(\0 和 ~ 分别是第一个和最后一个 ASCII 字符)
```

```swift
// func
// 函数 和 闭包

// 函数定义
func 函数名(参数01标签或者_ 形参01名字: 形参01类型, 参数02标签或者_ 形参02名字: 形参02类型) -> 返回类型 {...}
func 函数名(参数01标签或者_ 形参01名字: 形参01类型, 参数02标签或者_ 形参02名字: 形参02类型) -> (名称01: 元组成员01类型, 名称02: 元组成员02类型) {...}
func 函数名(参数) -> ((参数) -> 类型) {...}		// 函数返回值是函数
func 函数名(() -> 类型) -> 类型 {...}

// 函数调用
函数名(形参01名字或者参数01标签: 实参01, 形参02名字或者参数02标签: 实参02)

// 例如
func calculateStatistics(scores: [Int]) -> (min: Int, max: Int, sum: Int) {...}
let statistics = calculateStatistics(scores: [5, 3, 100, 3, 9])

// 函数可以嵌套
func returnFifteen() -> Int {
    var y = 10
    func add() {
        y += 5
    }
    add()
    return y
}
returnFifteen()

// 函数返回值是函数类型
func makeIncrementer() -> ((Int) -> Int) {
    func addOne(number: Int) -> Int {
        return 1 + number
    }
    return addOne
}
var increment = makeIncrementer()
increment(7)

// 函数的参数是函数
func hasAnyMatches(list: [Int], condition: (Int) -> Bool) -> Bool {
    for item in list {
        if condition(item) {
            return true
        }
    }
    return false
}
func lessThanTen(number: Int) -> Bool {
    return number < 10
}
var numbers = [20, 19, 7, 12]
hasAnyMatches(list: numbers, condition: lessThanTen)

// 闭包
// 闭包代码放在大括号中
// 函数作为参数, 写法有区别, 函数体的大括号{}用in替换掉
numbers.map({ (number: Int) -> Int in
    let result = 3 * number
    return result
})

// 单语句闭包
// 简写, 省略, 只需要提供参数, 可以省略参数类型和返回类型. 单语句闭包隐式返回它们唯一语句的值
let mappedNumbers = numbers.map({ number in 3 * number })
print(mappedNumbers)
// Prints "[60, 57, 21, 36]"

// 通过编号而不是名称来引用参数
// 当闭包是函数的唯一参数时, 您可以完全省略括号
let sortedNumbers = numbers.sorted { $0 > $1 }		// ({ $0 > $1 })
print(sortedNumbers)
// Prints "[20, 19, 12, 7]"
```

```swift
// 对象和类
class Shape {
    var numberOfSides = 0
    func simpleDescription() -> String {
        return "A shape with \(numberOfSides) sides."
    }
}

// 调用, 通过点语法来访问
var shape = Shape()
shape.numberOfSides = 7
var shapeDescription = shape.simpleDescription()

// 构造函数
class NamedShape {
    var numberOfSides: Int = 0
    var name: String

    init(name: String) {				// 构造函数. 
        self.name = name
    }

    func simpleDescription() -> String {
        return "A shape with \(numberOfSides) sides."
    }
}

// 析构函数
deinit()

// 继承 和 覆盖
class Square: NamedShape {						// 继承 NamedShape 基类
    var sideLength: Double

    init(sideLength: Double, name: String) {
        self.sideLength = sideLength
        super.init(name: name)				// 构造函数. 调用基类构造函数 super.init()
        numberOfSides = 4
    }

    func area() -> Double {
        return sideLength * sideLength
    }

    override func simpleDescription() -> String {					// 覆盖基类的同名方法
        return "A square with sides of length \(sideLength)."
    }
}
let test = Square(sideLength: 5.2, name: "my test square")
test.area()
test.simpleDescription()

// 属性的 getter 和 setter
// 此类在实例化时, 会按顺序执行 3 个步骤:
// 1. 设置子类的属性值
// 2. 执行init()
// 3. 设置父类的属性值. 包括 setter, getter
class EquilateralTriangle: NamedShape {
    var sideLength: Double = 0.0

    init(sideLength: Double, name: String) {
        self.sideLength = sideLength
        super.init(name: name)
        numberOfSides = 3
    }

    var perimeter: Double {
        get {
            return 3.0 * sideLength
        }
        set {
            sideLength = newValue / 3.0
        }
    }

    override func simpleDescription() -> String {
        return "An equilateral triangle with sides of length \(sideLength)."
    }
}
var triangle = EquilateralTriangle(sideLength: 3.1, name: "a triangle")
print(triangle.perimeter)
// Prints "9.3"
triangle.perimeter = 9.9
print(triangle.sideLength)
// Prints "3.3000000000000003"

// willSet, didSet, 运行于实例初始化(以上3步骤)之外的变化时, 自动触发执行.
class TriangleAndSquare {
    var triangle: EquilateralTriangle {
        willSet {
            square.sideLength = newValue.sideLength
        }
    }
    var square: Square {
        willSet {
            triangle.sideLength = newValue.sideLength
        }
    }
    init(size: Double, name: String) {
        square = Square(sideLength: size, name: name)
        triangle = EquilateralTriangle(sideLength: size, name: name)
    }
}
var triangleAndSquare = TriangleAndSquare(size: 10, name: "another test shape")
print(triangleAndSquare.square.sideLength)
// Prints "10.0"
print(triangleAndSquare.triangle.sideLength)
// Prints "10.0"
triangleAndSquare.square = Square(sideLength: 50, name: "larger square")
print(triangleAndSquare.triangle.sideLength)
// Prints "50.0"

// ? 在等号右侧
let optionalSquare: Square? = Square(sideLength: 2.5, name: "optional square")
let sideLength = optionalSquare?.sideLength		// 这里的?问号有短路的意思, 当 optionalSquare 为 nil 时, 直接返回 nil, 不再往下执行了
```

```swift
// 枚举和结构体
// 枚举有方法
// 原始值(序号), 就是枚举的下标值(序号). 内置属性 rawValue 表示原始值
// case 声明的是 枚举元素的名字 ace(实际值). 枚举元素都有一个默认原始值, 原始值类似于下标(序号), 这个原始值默认从0开始. 可以重新指定从1开始也行. 在没有有意义的原始值的情况下, 完全不必提供原始值.
// 访问枚举中的元素, 通过‘.名字(实际值)’来访问: Rank.king, 得到的是序号(原始值)
enum Rank: Int {
    case ace = 1			// 默认枚举原始值从0开始, 可以指定从1开始.
    case two, three, four, five, six, seven, eight, nine, ten
    case jack, queen, king			// 实际值

    func simpleDescription() -> String {
        switch self {
        case .ace:
            return "ace"
        case .jack:
            return "jack"
        case .queen:
            return "queen"
        case .king:
            return "king"
        default:
            return String(self.rawValue)
        }
    }
}
let ace = Rank.ace					// 枚举类似类, 通过  ‘.实际值’ 访问
let aceRawValue = ace.rawValue

// 枚举类似 class 类, 也有构造函数 init()
if let convertedRank = Rank(rawValue: 3) {			// Rank(rawValue保留字原始值: 3)
    let threeDescription = convertedRank.simpleDescription()
}

// 缩写 与 全称, .xx 与 枚举名.xx 
enum Suit {
    case spades, hearts, diamonds, clubs

    func simpleDescription() -> String {
        switch self {
        case .spades:					// switch 内部 枚举 case 由缩写形式引用, 因为 self 已知的值是花色. 只要值的类型已知，您就可以使用缩写形式.
            return "spades"
        case .hearts:
            return "hearts"
        case .diamonds:
            return "diamonds"
        case .clubs:
            return "clubs"
        }
    }
}
let hearts = Suit.hearts		// let 声明 hearts 时没有指明类型, 所以 Suit.hearts 是全称.
let heartsDescription = hearts.simpleDescription()

// 如果在枚举声明时就声明了原始值, 那么不管这个枚举如何实例化, 他们都具有相同的原始值. 这就是某种关联关系.
// 另一个角度是说, 枚举实例化时的实际值是随实例化而变化的.

// 结构体
// 结构体和类相似, 都有方法, 构造函数. 区别: 结构体被传递时是复制拷贝副本, 而类实例是引用.
struct Card {
    var rank: Rank			// 字段名冒号
    var suit: Suit
    func simpleDescription() -> String {
        return "The \(rank.simpleDescription()) of \(suit.simpleDescription())"
    }
}
let threeOfSpades = Card(rank: .three, suit: .spades)
let threeOfSpadesDescription = threeOfSpades.simpleDescription()
```

```swift
// 并发
func 函数名(标签 参数名: 参数类型) async -> 返回类型 {...}		 // 使用 async 标记

func fetchUserID(from server: String) async -> Int {			// 使用 async 标记 fetchUserID() 为异步执行
    if server == "primary" {
        return 97
    }
    return 501
}

func fetchUsername(from server: String) async -> String {
    let userID = await fetchUserID(from: server)					// 在调用异步函数的前面用 await 标记
    if userID == 501 {
        return "John Appleseed"
    }
    return "Guest"
}

func connectUser(to server: String) async {
    async let userID = fetchUserID(from: server)					// async let 调用 fetchUserID() 这个异步函数, 让 userID 这个变量平行调用运行于 fetchUserID() 这个异步函数返回. 用异步变量接收异步函数的返回
    async let username = fetchUsername(from: server)
    let greeting = await "Hello \(username), user ID \(userID)"				// 在使用异步变量返回时, 用 await 标记
    print(greeting)
}

// 使用 Task 在同步代码中调用异步函数, 而无需等待他们返回.
Task {
    await connectUser(to: "primary")
}
// Prints "Hello Guest, user ID 97"
```

```swift
// 协议(接口)和扩展
protocol ExampleProtocol {									// 声明协议
    var simpleDescription: String { get }
    mutating func adjust()
}
// 类、枚举和结构体都可以采用协议
// mutating 关键词用于在结构体里表示此结构体的方法可以修改本结构体. 但类方法不要加这个关键词因为类的方法永远可以修改类本身.
class SimpleClass: ExampleProtocol {
    var simpleDescription: String = "A very simple class."
    var anotherProperty: Int = 69105
    func adjust() {
        simpleDescription += "  Now 100% adjusted."
    }
}
var a = SimpleClass()
a.adjust()
let aDescription = a.simpleDescription

struct SimpleStructure: ExampleProtocol {
    var simpleDescription: String = "A simple structure"
    mutating func adjust() {
        simpleDescription += " (adjusted)"
    }
}
var b = SimpleStructure()
b.adjust()
let bDescription = b.simpleDescription

// extension 扩展用于向任何类型增加 方法和属性. 也可以扩展协议
extension Int: ExampleProtocol {
    var simpleDescription: String {
        return "The number \(self)"
    }
    mutating func adjust() {
        self += 42
    }
}
print(7.simpleDescription)
// Prints "The number 7"

// 协议相当于类型. 可以表示/创建具有不同类型但都符合某一协议的对象集合
// 当一个值的类型是协议类型时, 协议定义之外的方法不可用. 即便这个值运行时类型时其他类型, 只要有协议类型这一项, 那么就要严格符合协议类型, 其他不在协议类型中的定义均不支持.
let protocolValue: ExampleProtocol = a
print(protocolValue.simpleDescription)
// Prints "A very simple class.  Now 100% adjusted."
// print(protocolValue.anotherProperty)  // Uncomment to see the error
```

```swift
// 错误处理
```

```swift
// 泛型
// 尖括号里放个名字就是函数或和类型的泛型
func makeArray<Item>(repeating item: Item, numberOfTimes: Int) -> [Item] {
    var result: [Item] = []
    for _ in 0..<numberOfTimes {
        result.append(item)
    }
    return result
}
makeArray(repeating: "knock", numberOfTimes: 4)

// 泛型涉及函数, 方法, 类, 枚举, 结构体
// Reimplement the Swift standard library's optional type
enum OptionalValue<Wrapped> {
    case none
    case some(Wrapped)
}
var possibleInteger: OptionalValue<Int> = .none				// 一个类型, 枚举类型
possibleInteger = .some(100)

// 泛型是不确定的类型. 泛型是类型的参数.
```









## FAQ





## See Also

https://docs.swift.org/swift-book/GuidedTour/GuidedTour.html		A Swift Tour



