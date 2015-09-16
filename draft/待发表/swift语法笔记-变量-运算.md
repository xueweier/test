# Swift语法笔记 - 变量类型、运算符、控制流

曾经在去年接触过 Object-C ，尝试着将斯坦福大学的公开课看完，并且按着公开课把他们的代码敲了一遍。代码可以看[这里](https://github.com/kelvinblood/iOS_Demo_CS193P_2013)。最近看了一遍Swift中文翻译组的[翻译](http://numbbbbb.gitbooks.io/-the-swift-programming-language-/)，做了这篇一个小笔记。翻译的很好，感谢他们的辛勤劳动！因为以前是c语言入门，所以文章会略过很多与c语言相同的内容



### 简介

Swift 是一门进行 iOS 和 OS X 应用开发的新语言。

Swift 包含了 C 和 Objective-C 上所有基础数据类型，Int表示整型值；Double和Float表示浮点型值；Bool是布尔型值；String是文本型数据。Swift 还提供了两个基本的集合类型，Array和Dictionary。

Swift 增加了 Objective-C 中没有的高阶数据类型比如元组（Tuple）。可以使用元组创建或者传递一组数据，比如作为函数的返回值时，可以用一个元组可以返回多个值。

Swift 还增加了可选（Optional）类型，用于处理值缺失的情况。可选表示“那儿有一个值，并且它等于 x ”或者“那儿没有值”。可选有点像在 Objective-C 中使用nil，但是它可以用在任何类型上，不仅仅是类。可选类型比 Objective-C 中的nil指针更加安全也更具表现力，它是 Swift 许多强大特性的重要组成部分。



### 基础

	var x = 0.0, y = 0.0, z = 0.0
	var welcomeMessage: String //类型标注
	let maximumNumberOfLoginAttempts = 10

常量和变量`必须`在使用前声明,尝试更改常量会导致编译时报错。

编译器可以在编译代码的时候自动推断出变量/表达式的类型，不需要每次进行变量声明。

### 字符串和字符

	println("The current value of friendlyWelcome is \(friendlyWelcome)")
	// 输出 "The current value of friendlyWelcome is Bonjour!

Swift 中的字符串是否可以修改仅通过定义的是变量还是常量来决定.

Swift 的String类型是值类型。 如果您创建了一个新的字符串，那么当其进行常量、变量赋值操作或在函数/方法中传递时，会进行值拷贝。 任何情况下，都会对已有字符串值创建新副本，并对该新副本进行传递或赋值操作。在实际编译时，Swift 编译器会优化字符串的使用，使实际的复制只发生在绝对必要的情况下，这意味着您将字符串作为值类型的同时可以获得极高的性能。

全局countElements函数计算字符串字符数量。

	println("unusualMenagerie has \(countElements(unusualMenagerie)) characters")

连接字符串和字符可以用+、+=、append。

Swift 提供了三种方式来比较字符串的值：字符串相等==、前缀相等hasPrefix属性和后缀相等hasSuffix属性。

	var act1SceneCount = 0
	for scene in romeoAndJuliet {
	    if scene.hasPrefix("Act 1 ") {
	        ++act1SceneCount
	    }
	}

	uppercaseString、lowercaseString属性 大小写

字符串字面量可以包含以下特殊字符：

* 转义字符\0(空字符)、\\(反斜线)、\t(水平制表符)、\n(换行符)、\r(回车符)、\"(双引号)、\'(单引号)。
* Unicode 标量，写成\u{n}(u为小写)，其中n为任意的一到八位十六进制数。

### 运算和运算符

	let minValue = UInt8.min  // minValue 为 0，是 UInt8 类型的最小值
	let maxValue = UInt8.max  // maxValue 为 255，是 UInt8 类型的最大值

Swift 提供了8，16，32和64位的有符号和无符号整数类型。

整数字面量可以被写作：

	一个十进制数，没有前缀
	一个二进制数，前缀是0b
	一个八进制数，前缀是0o
	一个十六进制数，前缀是0x

整数和浮点数的转换必须显式指定类型

	Double(three)
	Int(pi)

类型别名（type aliases）

	for index in 1...5 {
	    println("\(index) * 5 = \(index * 5)")
	}
赋值运算符、算术运算符、求余运算符、自增和自减运算、复合赋值、比较运算符、三目运算符、逻辑运算、使用括号来明确优先级
运算符和c相同。

多出了浮点数求余计算、一元负号运算符/一元正号运算符（对称美= =）、空合运算符（有点复杂暂时不管）
闭区间运算符（a...b）定义一个包含从a到b(包括a和b)的所有值的区间。
半开区间（a..<b）定义一个从a到b但不包括b的区间。

Swift 有两个布尔常量，true和false

### 集合

* 数组（Arrays）
* 字典（Dictionaries）

// 数组

	var shoppingList: [String] = ["Eggs", "Milk"]
	shoppingList.isEmpty
	shoppingList.append("Flour")
	shoppingList += ["Chocolate_Spread","Cheese","Butter"]
	var firstItem = shoppingList[0]
	shoppingList[4...6] = ["Bananas", "Apples"]
	shoppingList.insert("Maple Syrup", atIndex: 0)
	let mapleSyrup = shoppingList.removeAtIndex(0)
	let apples = shoppingList.removeLast()
	for item in shoppingList {
	    println(item)
	}
	for (index, value) in enumerate(shoppingList) {
	    println("Item \(index + 1): \(value)")
	}

	var someInts = [Int]()
	someInts.append(3)
	someInts = []
	var threeDoubles = [Double](count: 3, repeatedValue:0.0)
	var anotherThreeDoubles = Array(count: 3, repeatedValue: 2.5) // 类型推断
	var sixDoubles = threeDoubles + anotherThreeDoubles
	// sixDoubles 被推断为 [Double], 等于 [0.0, 0.0, 0.0, 2.5, 2.5, 2.5]

// 字典

	var airports: [String:String] = ["TYO": "Tokyo", "DUB": "Dublin"]
	println("The dictionary of airports contains \(airports.count) items.")
	airports.isEmpty
	airports["LHR"] = "London"
	if let oldValue = airports.updateValue("Dublin_Internation", forKey: "DUB") {
	    println("The old value for DUB was \(oldValue).")
	}
	// 输出 "The old value for DUB was Dublin."（DUB原值是dublin）
	for (airportCode, airportName) in airports {
	    println("\(airportCode): \(airportName)")
	}

	for airportCode in airports.keys {
	    println("Airport code: \(airportCode)")
	}
	for airportName in airports.values {
	    println("Airport name: \(airportName)")
	}
	使用keys或者values属性直接构造一个新数组
	let airportCodes = Array(airports.keys)
	let airportNames = Array(airports.values)

###### 元祖

	let http404Error = (404, "Not Found")
	let (statusCode, statusMessage) = http404Error
		println("The status code is \(statusCode)")
		println("The status code is \(http404Error.0)")
	let (justTheStatusCode, _) = http404Error
	let http200Status = (statusCode: 200, description: "OK")
		println("The status code is \(http200Status.statusCode)")

元组（tuples）把多个值组合成一个复合值。元组内的值可以是任意类型，并不要求是相同类型。

只需要一部分元组值，使用下划线。还可以通过下标来访问元组中的单个元素，下标从零开始。在定义元组的时候给单个元素命名。

	let somePoint = (1, 1)
	switch somePoint {
	case (0, 0):
	    println("(0, 0) is at the origin")
	case (_, 0):
	    println("(\(somePoint.0), 0) is on the x-axis")
	case (0, _):
	    println("(0, \(somePoint.1)) is on the y-axis")
	case (-2...2, -2...2):
	    println("(\(somePoint.0), \(somePoint.1)) is inside the box")
	default:
	    println("(\(somePoint.0), \(somePoint.1)) is outside of the box")
	}

###### 可选类型

###### 断言

	let age = -3
	assert(age >= 0, "A person's age cannot be less than zero")

断言的适用情景：

整数类型的下标索引被传入一个自定义下标脚本实现，但是下标索引值可能太小或者太大。
需要给函数传入一个值，但是非法的值可能导致函数不能正常执行。
一个可选值现在是nil，但是后面的代码运行需要一个非nil值。

### 控制流

for

	for index in 1...5 {
	    println("\(index) times 5 is \(index * 5)")
	}
	for _ in 1...power {
	    answer *= base
	}
	let names = ["Anna", "Alex", "Brian", "Jack"]
	for name in names {
	    println("Hello, \(name)!")
	}
	let numberOfLegs = ["spider": 8, "ant": 6, "cat": 4]
	for (animalName, legCount) in numberOfLegs {
	    println("\(animalName)s have \(legCount) legs")
	}
	for character in "Hello" {
	    println(character)
	}


	for var index = 0; index < 3; ++index {
	    println("index is \(index)")
	}

while

	while square < finalSquare {

	}
	do {
	statements
	} while condition

switch

不存在隐式的贯穿

	let someCharacter: Character = "e"
	switch someCharacter {
	case "a", "e", "i", "o", "u":
	    println("\(someCharacter) is a vowel")
	case "b", "c", "d", "f", "g", "h", "j", "k", "l", "m",
	"n", "p", "q", "r", "s", "t", "v", "w", "x", "y", "z":
	    println("\(someCharacter) is a consonant")
	default:
	    println("\(someCharacter) is not a vowel or a consonant")
	}
	// 输出 "e is a vowel"

Where

	case 分支的模式可以使用where语句来判断额外的条件。

	下面的例子把下图中的点(x, y)进行了分类：

	let yetAnotherPoint = (1, -1)
	switch yetAnotherPoint {
	case let (x, y) where x == y:
	    println("(\(x), \(y)) is on the line x == y")
	case let (x, y) where x == -y:
	    println("(\(x), \(y)) is on the line x == -y")
	case let (x, y):
	    println("(\(x), \(y)) is just some arbitrary point")
	}
	// 输出 "(1, -1) is on the line x == -y"

值绑定

	let anotherPoint = (2, 0)
	switch anotherPoint {
	case (let x, 0):
	    println("on the x-axis with an x value of \(x)")
	case (0, let y):
	    println("on the y-axis with a y value of \(y)")
	case let (x, y):
	    println("somewhere else at (\(x), \(y))")
	}
	// 输出 "on the x-axis with an x value of 2"

###### 控制转移语句（Control Transfer Statements

	continue - 一个循环体立刻停止本次循环迭代，重新开始下次循环迭代。
	breakbreak - 语句会立刻结束整个控制流的执行。当你想要更早的结束一个switch代码块或者一个循环体时，你都可以使用break语句。
	fallthrough - c风格的switch，自动进入下一个switch）
	return

###### 其它

注释风格和c语言一样。

Swift 并不强制要求你在每条语句的结尾处使用分号（;）
