

	let names = ["Chris", "Alex", "Ewa", "Barry", "Daniella"]
	
	
	func backwards(s1: String, s2: String) -> Bool {
	    return s1 > s2
	    }
	    var reversed = sorted(names, backwards)
	    // reversed 为 ["Ewa", "Daniella", "Chris", "Barry", "Alex"]


全局和嵌套函数实际上也是特殊的闭包，闭包采取如下三种形式之一：

    全局函数是一个有名字但不会捕获任何值的闭包
    嵌套函数是一个有名字并可以捕获其封闭函数域内值的闭包
    闭包表达式是一个利用轻量级语法所写的可以捕获其上下文中变量或常量值的匿名闭包

闭包表达式语法有如下一般形式：

{ (parameters) -> returnType in
    statements
}

闭包表达式语法可以使用常量、变量和inout类型作为参数，不提供默认值。 也可以在参数列表的最后使用可变参数。 元组也可以作为参数和返回值。

下面的例子展示了之前backwards函数对应的闭包表达式版本的代码：

	reversed = sorted(names, { (s1: String, s2: String) -> Bool in
	    return s1 > s2
	})

Swift可以推断其参数和返回值的类型

reversed = sorted(names, { s1, s2 in return s1 > s2 } )


单行表达式闭包可以通过隐藏return关键字来隐式返回单行表达式的结果
reversed = sorted(names, { s1, s2 in s1 > s2 } )


单行表达式闭包可以通过隐藏return关键字来隐式返回单行表达式的结果，如上版本的例子可以改写为：

reversed = sorted(names, { s1, s2 in s1 > s2 } )
Swift 自动为内联函数提供了参数名称缩写功能，您可以直接通过$0,$1,$2来顺序调用闭包的参数。

如果您在闭包表达式中使用参数名称缩写，您可以在闭包参数列表中省略对其的定义，并且对应参数名称缩写的类型会通过函数类型进行推断。 in关键字也同样可以被省略，因为此时闭包表达式完全由闭包函数体构成：

reversed = sorted(names, { $0 > $1 } )
M运算符函数（Operator Functions）

实际上还有一种更简短的方式来撰写上面例子中的闭包表达式。 Swift 的String类型定义了关于大于号 (>) 的字符串实现，其作为一个函数接受两个String类型的参数并返回Bool类型的值。 而这正好与sorted函数的第二个参数需要的函数类型相符合。 因此，您可以简单地传递一个大于号，Swift可以自动推断出您想使用大于号的字符串函数实现：

reversed = sorted(names, >)



## 尾随闭包（Trailing Closures）

如果您需要将一个很长的闭包表达式作为最后一个参数传递给函数，可以使用尾随闭包来增强函数的可读性。 尾随闭包是一个书写在函数括号之后的闭包表达式，函数支持将其作为最后一个参数调用。

func someFunctionThatTakesAClosure(closure: () -> ()) {
    // 函数体部分
    }

    // 以下是不使用尾随闭包进行函数调用
    someFunctionThatTakesAClosure({
        // 闭包主体部分
        })

        // 以下是使用尾随闭包进行函数调用
        someFunctionThatTakesAClosure() {
          // 闭包主体部分
          }

              注意： 如果函数只需要闭包表达式一个参数，当您使用尾随闭包时，您甚至可以把()省略掉。

              在上例中作为sorted函数参数的字符串排序闭包可以改写为：

              reversed = sorted(names) { $0 > $1 }

              当闭包非常长以至于不能在一行中进行书写时，尾随闭包变得非常有用。
