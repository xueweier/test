
# 基本知识

数组

	var rooms = [Room]() // 空数组
	
可选链 ？

any类型 
	var things = [any]()


# 区别


### 元组、列表、字典

Swift 语言提供经典的数组和字典两种集合类型来存储集合数据。数组用来按顺序存储相同类型的数据。字典虽然无序存储相同类型数据值但是需要由独有的标识符引用和寻址（就是键值对）。

###### 元组（Tuple）

笛卡尔积中每一个元素（d1，d2，…，dn）叫作一个n元组（n-tuple）或简称元组。

元组是关系数据库中的基本概念，关系是一张表，表中的每行（即数据库中的每条记录）就是一个元组，每列就是一个属性。 在二维表里，元组也称为记录。

列表中的元素都是不变的


###### 列表（List），也就是数组吧

任意对象的`有序集合`；
可通过偏移存取，列表中的元素都是可变的；
长度可变，支持嵌套；

	var shoppingList: [String] = ["Eggs", "Milk"]
	var someInts = [Int]()
	
###### 字典（dictionary）

python里的字典就像java里的HashMap，以键值对的方式存在并操作，其特点如下
通过键来存取，而非偏移量；
键值对是无序的；
键和值可以是任意对象；
长度可变，任意嵌套；

	var airports: [String:String] = ["TYO": "Tokyo", "DUB": "Dublin"]
	var namesOfIntegers = Dictionary<Int, String>()
	
### 函数、方法

函数就是普通的函数。
方法，是类的函数、或者说成员函数。

Java中只有方法，C中只有函数，而C++里取决于是否在类中。

	func sayHello(personName: String) -> String {
	    let greeting = "Hello, " + personName + "!"
	    return greeting
	}
	
	func count(string: String) -> (vowels: Int, consonants: Int, others: Int) {
    var vowels = 0, consonants = 0, others = 0
    for character in string {
        switch String(character).lowercaseString {
        case "a", "e", "i", "o", "u":
            ++vowels
        case "b", "c", "d", "f", "g", "h", "j", "k", "l", "m",
          "n", "p", "q", "r", "s", "t", "v", "w", "x", "y", "z":
            ++consonants
        default:
            ++others
        }
    }
    return (vowels, consonants, others)
	}
	
	func swapTwoInts(inout a: Int, inout b: Int) {
	    let temporaryA = a
	    a = b
	    b = temporaryA
	}