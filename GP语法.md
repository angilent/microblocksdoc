- # GP 语法表示
  
  GP程序大部分时间都是在GP编程环境中作为可视化的块来编写、查看、编辑和调试的。然而，GP 程序的文本表示在以下几个方面很有用：
- 将 GP 程序存储在外部文件和版本历史中
- 表示基于文本的媒体中的程序，例如电子邮件、wiki 等。
- 引导 GP 编程环境
  
  GP 程序可以从块转换为文本，反之亦然。文本表示是人类可读的，但由于 GP 程序将以图形方式编写和查看（系统引导期间除外），因此不必针对手动编辑对其进行优化。
  
  本文的其余部分描述了 GP 的当前语法。它将随着语法的扩展而更新（例如添加对浮点数的支持）。
## 数字

GP目前只支持正负整数如：

```
123
-17
```

复制

在内部，整数是 31 位，所以它们的范围是 -1073741824 到 1073741823。
## 字符串

字符串用单引号括起来：

```
'Welcome to GP!'
```

复制

并且可以包含换行符：

```
'Welcome to GP:
a blocks-based programming language
for casual programmers'
```

复制

要在字符串中包含单引号字符，请将其加倍：

```
'It''s fun!'
```

复制

没有其他转义序列。字符串是 Unicode，编码为 UTF8。（7 位 ASCII 字符集是 UTF8 的子集。）
## 符号

一个符号就像一个没有引号的字符串：

```
print
```

复制

符号不能包含嵌入的空格，不能以数字或减号开头，也不能包含圆括号或花括号，但它可以包含符号字符：

```
+ - <= ||
```

复制

您可以从这些示例中猜到，符号用于函数名称和运算符。在内部，符号表示为字符串；GP 不像 Smalltalk-80 那样有单独的 Symbol 类。
## 布尔值和零

布尔值和 nil 表示为它们自己：

```
true
false
nil
```

复制

这些代表 GP 中的 true、false 和 nil 对象。因此，如果你想要一个包含这些词之一的字符串，你必须引用它：

```
'true'
```

复制
## 命令和表达式

命令是一个操作名称（符号），后跟零个或多个参数：

```
print 'GP rocks!'
```

复制

参数可以是文字值（数字、字符串、布尔值或 nil）、表达式或命令列表。

表达式是括在括号中的命令：

```
(mouseX)
(abs -10)
(at myArray 1)
(+ 3 4)
```

复制

请注意，表达式与命令一样，是一个操作名称，后跟任何参数。但是，也可以按照更熟悉的中缀顺序编写二元运算符，例如“+”：

```
(3 + 4)
```

复制

请注意，每个表达式，包括二进制表达式，都必须括在括号中；与许多其他语言不同，GP 解析器不会根据运算符优先级进行自动分组：

```
(3 + (2 + 2))   correct
(+ 3 2 2)       correct
(3 + 2 + 2)     syntax error!
```

复制
## 命令列表和注释

命令列表是一系列命令，每行一个命令，用花括号括起来：

```
{
  print 'How do elephants hide in cherry trees?'
  sleep 5000
  print 'They paint their toenails red'
}
```

复制

命令列表用于控制结构：

```
repeat 10 {
  print 'Hooray!'
}
```

复制

由于左大括号与重复命令在同一行，因此整个命令列表是重复命令的第二个参数。

通常，命令列表中的每个命令都独占一行，类似于可视块的布局。但是，为方便起见，可以将只有一个命令的命令列表写在一行中：

```
repeat 10 { print 'Hooray!' }
```

复制

您还可以通过用分号分隔将多个语句组合在一行中：

```
print 'Hello'; print 'World'
```

复制

但是，请注意，当文本 GP 程序转换为块然后再转换回文本时，它可能会重新格式化为每行只有一个语句，具体取决于漂亮打印机中内置的规则。
## 变量

命令或表达式的未加引号的字符串参数被解释为变量引用。因此，可以这样写：

```
print x 'squared is' (x * x)
```

复制

在内部，GP 将对变量“x”的引用表示为：

```
(v 'x')
```

复制

因此，变量引用只是一个普通的表达式，而上面的 print 语句只是一个更易读的等价物：

```
print (v x) 'squared is' ((v x) * (v x))
```

复制

GP 有两个操作符来设置和更改变量：

```
score = 0    set score to zero
score += 1   increment score by one
```

复制
## 评论

注释以一对斜杠字符开始，一直运行到行尾：

```
score = 0 // reset the score
```

复制

GP 解析器忽略注释。
## 可变命令和运算符

一些 GP 命令和运算符是*可变的*；也就是说，它们接受可变数量的参数。我们已经看到 print 命令可以接受任意数量的参数：

```
print '11 divided by 3 is' (11 / 3) 'with a remainder of' (11 % 3)
```

复制

+ 运算符也是可变的。虽然它是中缀形式，但总是恰好采用两个参数：

```
(1 + 2)
```

复制

它的前缀形式可用于总结任意数量的参数：

```
(+ 1 2)
(+ 1 2 3 4 5 6)
(+ 42)
(+) // the sum of an empty list, which is 0
```

复制

其他有用的可变参数运算符包括“*”，以及逻辑运算符“and”和“or”。系统库包括额外的可变参数函数，例如“min”和“max”。
## 如果语句

“if”命令也是可变的；它需要一对或多对（条件，语句列表）对，类似于许多其他语言中的“if ... else if ... else ...”语句。这是一个例子：

```
if (n > 0) {
  print 'n is positive'
} (n < 0) {
  print 'n is negative'
} true {
  print 'n is zero'
}
```

复制

或者，更简洁地说：

```
if (n > 0) { print 'n is positive'
} (n < 0) { print 'n is negative'
} true { print 'n is zero' }
```

复制

请注意，此形式中每个语句列表的右括号必须放在下一行，以告诉 GP 解析器语句继续到下一行。

虽然由于缺少额外的“else if”关键字，这看起来可能有点奇怪，但它实际上很容易阅读，并且在将此代码转换为块时会显示缺少的“else if”关键字。
## 获得帮助

不带参数的“帮助”命令将打印所有 GP 命令的列表。Help with argument 将为您提供特定命令的帮助——通常是一个简短的示例：

```
help +
```