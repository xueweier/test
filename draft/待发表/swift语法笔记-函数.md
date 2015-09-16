# Swift语法笔记 - 函数


多重输入参数（Multiple Input Parameters）
func halfOpenRangeLength(start: Int, end: Int) -> Int {
    return end - start
}
多重返回值函数
func count(string: String) -> (vowels: Int, consonants: Int, others: Int) {
	return (vowels, consonants, others)
}
无参函数无返回值函数
func sayGoodbye() {
    println("Goodbye!")
}


默认参数值
func join(string s1: String, toString s2: String, withJoiner joiner: String = " ") -> String {
    return s1 + joiner + s2
}

函数参数默认是常量,在参数名前加关键字 var 来定义变量参数.变量参数，仅仅能在函数体内被更改。
func alignRight(var string: String, count: Int, pad: Character) -> String {
return string
}

定义一个输入输出参数时，在参数定义前加 inout 关键字。
func swapTwoInts(inout a: Int, inout b: Int) {
    let temporaryA = a
    a = b
    b = temporaryA
}
var someInt = 3
var anotherInt = 107
swapTwoInts(&someInt, &anotherInt)