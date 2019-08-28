# Python编程基础
## 1 Python基础
### 1.1 字符串连接和复制

```python3
>>> 'Alice' + 'Bob'
'AliceBob'
```

```python3
>>> 'Alice' * 5
'AliceAliceAliceAliceAlice'
```
*操作符只能用于两个数字（作为乘法），或一个字符串和一个==整型==（作为字符
串复制操作符）。否则，Python 将显示错误信息。
### 1.2 变量名
- 只能是一个词。
- 只能包含字母、数字和下划线。
- 不能以数字开头

驼峰形式：lookLikeThis

PEP8：looking_like_this
### 1.3 str()、int()和 float()函数
str()、int()和 float()函数将分别求值为传入值的字符串、整数和浮点数形式。

**文本和数字相等判断**

数字的字符串值被认为与整型值和浮点型值完全不同，但整型值可以与
浮点值相等。

<、>、<=和>=操作符仅用于整型和浮点型值。
## 2 控制流
### 2.1 for 循环和 range()函数
range()的开始、停止和步长参数

```python3
for i in range(12, 16): # 两个参数（开始与结束不包括结束值）
print(i)
```
```python3
for i in range(0, 10, 2): # 三个参数（开始、结束与步长，不包括结束值）
print(i)
```
```python3
for i in range(5, -1, -1): # 递减
print(i)
```
### 2.2 导入模块
每个模块都是一个Python程序，包含一组相关的函数，可以嵌入你的程序之中。例如，math模块有数学运算相关的函数，random模块有随机数相关的函数，等等。

在开始使用一个模块中的函数之前，必须用 import 语句导入该模块。

import语句与from import 语句

```python3
import random, sys, os, math
from random import *。
```
## 3 函数
随着你获得更多的编程经验，常常会发现自己在为代码“消除重复”，即去除一些重复或复制的代码。消除重复能够使程序更短、更易读、更容易更新。
### 3.1 None 值
在 Python 中有一个值称为 None，它表示没有值。

有一个使用 None的地方就是print()的返回值。print()函数在屏幕上显示文本，但它不需要返回任何值，这和 len()或input()不同。但既然所有函数调用都需要求值为一个返回值，那么 print()就返回 None。
### 3.2 关键字参数和 print()
大多数参数是由它们在函数调用中的位置来识别的。但是，“关键字参数”是由函数调用时加在它们前面的关键字来识别的。关键字参数通常用于可选变元。

例如，print()函数有可选的变元end和sep，分别指定在参数末尾打印什么，以及在参数之间打印什么来隔开它们。

**代码对比**

1.修改end参数（默认为换行符）
```python3
print('Hello')
print('World')

print('Hello', end='')
print('World')
```
2.修改sep参数(默认为空格)
```python3
print('cats', 'dogs', 'mice')

print('cats', 'dogs', 'mice', sep=',')
```
### 3.3 局部和全局作用域
可以将“作用域”看成是变量的容器。当作用域被销毁时，所有保存在该作用域内的变量的值就被丢弃了。只有一个全局作用域，它是在程序开始时创建的。如
果程序终止，全局作用域就被销毁，它的所有变量就被丢弃了。

一个函数被调用时，就创建了一个局部作用域。在这个函数内赋值的所有变量，存在于该局部作用域内。该函数返回时，这个局部作用域就被销毁了，这些变量就丢失了。下次调用这个函数，局部变量不会记得该函数上次被调用时它们保存的值。

可以有一个名为 spam 的局部变量，和一个名为spam的全局变量。但是，踪某一个时刻使用的是哪个变量，可能比较麻烦。所以应该避免在不同作用域内使用相同变量名的原因。

**鼓励在编写函数时不使用全局变量**

**global 语句**

如果需要在一个函数内修改全局变量，就使用 global 语句。如果在函数的顶部有 global eggs这样的代码，它就告诉Python，“在这个函数中，eggs 指的是全局变量，所以不要用这个名字创建一个局部变量。”

```python3
def spam():
    global eggs
    eggs = 'spam'
eggs = 'global'
spam()
print(eggs)
```
### 3.4 异常处理
错误可以由 try 和 except 语句来处理。那些可能出错的语句被放在 try 子句中。
如果错误发生，程序执行就转到接下来的 except 子句开始处。

```python3
def spam(divideBy):
    try:
        return 42 / divideBy
    except ZeroDivisionError:
        print('Error: Invalid argument.')

print(spam(2))
print(spam(12))
print(spam(0))
print(spam(1))
```
如果在 try 子句中的代码导致一个错误，程序执行就立即转到except子句的代码。在运行那些代码之后，执行照常继续。

对比

```python3
def spam(divideBy):
    return 42 / divideBy
try:
    print(spam(2))
    print(spam(12))
    print(spam(0))
    print(spam(1))
except ZeroDivisionError:
    print('Error: Invalid argument.')
```
一旦执行跳到 except 子句的代码，就不会回到 try 子句。

正常情况下，int()函数在传入一个非整数字符串时，会产生 ValueError 错误，
比如 int('puppy')。

**内建错误类：**
```
+-- BaseException
     +-- SystemExit
     +-- KeyboardInterrupt
     +-- GeneratorExit
     +-- Exception
          +-- StopIteration
          +-- StopAsyncIteration
          +-- ArithmeticError
          |    +-- FloatingPointError
          |    +-- OverflowError
          |    +-- ZeroDivisionError
          +-- AssertionError
          +-- AttributeError
          +-- BufferError
          +-- EOFError
          +-- ImportError
          |    +-- ModuleNotFoundError
          +-- LookupError
          |    +-- IndexError
          |    +-- KeyError
          +-- MemoryError
          +-- NameError
          |    +-- UnboundLocalError
          +-- OSError
          |    +-- BlockingIOError
          |    +-- ChildProcessError
          |    +-- ConnectionError
          |    |    +-- BrokenPipeError
          |    |    +-- ConnectionAbortedError
          |    |    +-- ConnectionRefusedError
          |    |    +-- ConnectionResetError
          |    +-- FileExistsError
          |    +-- FileNotFoundError
          |    +-- InterruptedError
          |    +-- IsADirectoryError
          |    +-- NotADirectoryError
          |    +-- PermissionError
          |    +-- ProcessLookupError
          |    +-- TimeoutError
          +-- ReferenceError
          +-- RuntimeError
          |    +-- NotImplementedError
          |    +-- RecursionError
          +-- SyntaxError
          |    +-- IndentationError
          |         +-- TabError
          +-- SystemError
          +-- TypeError
          +-- ValueError
          |    +-- UnicodeError
          |         +-- UnicodeDecodeError
          |         +-- UnicodeEncodeError
          |         +-- UnicodeTranslateError
          +-- Warning
               +-- DeprecationWarning
               +-- PendingDeprecationWarning
               +-- RuntimeWarning
               +-- SyntaxWarning
               +-- UserWarning
               +-- FutureWarning
               +-- ImportWarning
               +-- UnicodeWarning
               +-- BytesWarning
               +-- ResourceWarning
```
## 4 列表

```python3
 spam = ['cat', 'bat', 'rat', 'elephant']
```
spam 变量只被赋予一个值：列表值。但列表值本身包含多个值。[]是一
个空列表，不包含任何值，类似于空字符串’’。
### 4.1 用下标取得列表中的单个值
如果使用的下标超出了列表中值的个数，Python 将给出 IndexError 出错信息。

整数值−1 指的是列表中的最后一个下标，−2 指的是列表中倒数第二个下标，以此类推。

列表也可以包含其他列表值。这些列表的列表中的值，可以通过多重下标来访问。
### 4.2 利用切片取得子列表
- spam[2]是一个列表和下标（一个整数）。
- spam[1:4]是一个列表和切片（两个整数）。

在一个切片中，第一个整数是切片开始处的下标。第二个整数是切片结束处的下标。切片向上增长，直至第二个下标的值，但不包括它。切片求值为一个新的列表值。

作为快捷方法，你可以省略切片中冒号两边的一个下标或两个下标。省略第一
个下标相当于使用 0，或列表的开始。省略第二个下标相当于使用列表的长度，意
味着分片直至列表的末尾。
### 4.3 用 len()取得列表的长度
len()函数将返回传递给它的列表中值的个数，就像它能计算字符串中字符的个
数一样。
### 4.4 用下标改变列表中的值
spam[1] = 'aardvark'意味着“将列表 spam 下标 1 处的值赋值为字符串'aardvark'。
### 4.5 列表连接和列表复制
- +操作符可以连接两个列表，得到一个新列表，就像它将两个字符串合并成一个新字符串一样。
- *操作符可以用于一个列表和一个整数，实现列表的复制。

### 4.6 用 del 语句从列表中删除值
del 语句将删除列表中下标处的值，表中被删除值后面的所有值，都将向前移动一个下标。

```python3
spam = ['cat', 'bat', 'rat', 'elephant']
del spam[2]
```
del 语句也可用于一个简单变量，删除它，作用就像是“取消赋值”语句。如果在删除之后试图使用该变量，就会遇到 NameError 错误，因为该变量已不再存在。

在实践中，你几乎永远不需要删除简单变量。del 语句几乎总是用于删除列表中的值。
### 4.7 列表用于循环
一个常见的 Python 技巧，是在 for 循环中使用 range(len(someList))，迭代列表
的每一个下标。

range(len(supplies))将迭代 supplies 的所有下标，无论它包含多少表项。
### 4.8 多重赋值技巧

```python3
cat = ['fat', 'black', 'loud']
size, color, disposition = cat
```
### 4.9 增强的赋值操作
在对变量赋值时，常常会用到变量本身。

```python3
spam = 42
spam = spam + 1
spam += 1 # 增强赋值操作符
```
+=操作符也可以完成字符串和列表的连接，*=操作符可以完成字符串和列表的
复制。

```python3
>>> spam = 'Hello'
>>> spam += ' world!'
>>> spam
'Hello world!'
>>> bacon = ['Zophie']
>>> bacon *= 3
>>> bacon
['Zophie', 'Zophie', 'Zophie']
```
### 4.10 方法
每种数据类型都有它自己的一组方法。例如，列表数据类型有一些有用的方法，
用来查找、添加、删除或操作列表中的值。
#### 4.10.1 用 index()方法在列表中查找值
如果该值存在于列表中，就返回它的下标。如果该值不在列表中，Python 就报 ValueError。
#### 4.10.2 用 append()和 insert()方法在列表中添加值
spam.append('moose')和 spam.insert(1, 'chicken')

方法属于单个数据类型。append()和 insert()方法是列表方法，只能在列表上调
用，不能在其他值上调用，例如字符串和整型。
#### 4.10.3 用 remove()方法从列表中删除值

```python3
>>> spam = ['cat', 'bat', 'rat', 'elephant']
>>> spam.remove('bat')
>>> spam
['cat', 'rat', 'elephant']
```
试图删除列表中不存在的值，将导致 ValueError 错误。

如果该值在列表中出现多次，只有第一次出现的值会被删除。

如果知道想要删除的值在列表中的下标，del语句就很好用。如果知道想要从列表中删除的值，remove()方法就很好用。
#### 4.10.4 用 sort()方法将列表中的值排序

```python3
>>> spam = [2, 5, 3.14, 1, -7]
>>> spam.sort()
>>> spam
[-7, 1, 2, 3.14, 5]
>>> spam = ['ants', 'cats', 'dogs', 'badgers', 'elephants']
>>> spam.sort()
>>> spam
['ants', 'badgers', 'cats', 'dogs', 'elephants']
```
不能对既有数字又有字符串值的列表排序

sort()方法对字符串排序时，使用“ASCII 字符顺序”，而不是实际的字
典顺序。这意味着大写字母排在小写字母之前。因此在排序时，小写的 a 在大写的
Z 之后。

如果需要按照普通的字典顺序来排序，就在 sort()方法调用时，将关键字参数
key 设置为 str.lower。

这将导致 sort()方法将列表中所有的表项当成小写，但实际上并不会改变它们
在列表中的值。

```python3
>>> spam = ['a', 'z', 'A', 'Z']
>>> spam.sort(key=str.lower)
>>> spam
['a', 'A', 'z', 'Z']
```
### 4.11 类似列表的类型：字符串和元组
#### 4.11.1 字符串
字符串是单个文本字符的列表。对列表的许多操作，也可以作用于字符
串：按下标取值、切片、用于 for 循环、用于 len()，以及用于 in 和 not in 操作符。

```python3
>>> name = 'Zophie'
>>> name[0]
'Z'
>>> name[-2]
'i'
>>> name[0:4]
'Zoph'
>>> 'Zo' in name
True
>>> 'z' in name
False
>>> 'p' not in name
False
>>> for i in name:
print('* * * ' + i + ' * * *')
* * * Z * * *
* * * o * * *
* * * p * * *
* * * h * * *
* * * i * * *
* * * e * * *
```
#### 4.11.2 可变和不可变数据类型
列表和字符串在一个重要的方面是不同的。列表是“可变的”数据类型，它
的值可以添加、删除或改变。但是，字符串是“不可变的”，它不能被更改。尝试
对字符串中的一个字符重新赋值，将导致 TypeError 错误。

“改变”一个字符串的正确方式，是使用切片和连接。构造一个“新的”字符
串，从老的字符串那里复制一些部分。

```python3
>>> name = 'Zophie a cat'
>>> newName = name[0:7] + 'the' + name[8:12]
>>> name
'Zophie a cat'
>>> newName
'Zophie the cat'

```
#### 4.11.3 元组数据类型
“元组”数据类型与列表数据类型的区别：
- 元组输入时用圆括号()，而不是用方括号[]。
- 元组像字符串一样，是不可变的。

如果元组中只有一个值，你可以在括号内该值的后面跟上一个逗号，表明这种
情况。否则，Python 将认为，你只是在一个普通括号内输入了一个值。

如果需要一个永远不会改变的值的序列，就使用元组。
#### 4.11.4 用 list()和 tuple()函数来转换类型

```python3
>>> tuple(['cat', 'dog', 5])
('cat', 'dog', 5)
>>> list(('cat', 'dog', 5))
['cat', 'dog', 5]
>>> list('hello')
['h', 'e', 'l', 'l', 'o']
```
如果需要元组值的一个可变版本，将元组转换成列表就很方便。
#### 4.11.5 引用
在变量必须保存可变数据类型的值时，例如列表或字典，Python就使用引用。对于不可变的数据类型的值，例如字符串、整型或元组，Python变量就保存值本身
```python3
>>> spam = 42
>>> cheese = spam
>>> spam = 100
>>> spam
100
>>> cheese
42
```
变量保存字符串和整数值。变量就像包含着值的盒子

spam 和 cheese是不同的变量，保存了不同的值。

但列表不是这样的。列表变量实际上没有包含列表，而是包含了对列表的“引用”。

当你将列表赋给一个变量时，实际上是将列表的“引用”
赋给了该变量。引用是一个值，指向某些数据。
```python3
>>> spam = [0, 1, 2, 3, 4, 5]
>>> cheese = spam
>>> cheese[1] = 'Hello!'
>>> spam
[0, 'Hello!', 2, 3, 4, 5]
>>> cheese
[0, 'Hello!', 2, 3, 4, 5]
```
当创建列表时，你将对它的引用赋给了变量。但下一行只是将 spam 中的列表引用拷贝到 cheese，而不是列表值本身。这意味着存储在 spam和cheese中的值，现在指向了同一个列表。
#### 4.11.6 传递引用
当函数被调用时，参数的值被复制给变元。对于列表，这意味着变元得到的是引用的拷贝。
#### 4.11.7 copy 模块的 copy()和 deepcopy()函数
Python 提供了名为 copy 的模块，其中包含 copy()和 deepcopy()函数。第一个函数
copy.copy()，可以用来复制列表或字典这样的可变值，而不只是复制引用。

```python3
>>> import copy
>>> spam = ['A', 'B', 'C', 'D']
>>> cheese = copy.copy(spam)
>>> cheese[1] = 42
>>> spam
['A', 'B', 'C', 'D']
>>> cheese
['A', 42, 'C', 'D']
```
如果要复制的列表中包含了列表，那就使用copy.deepcopy()函数来代替。deepcopy()函数将同时复制它们内部的列表。
## 5 字典和结构化数据
像列表一样，“字典”是许多值的集合。但不像列表的下标，字典的索引可以使用许多不同数据类型，不只是整数。

字典的索引被称为“键”，键及其关联的值称为“键-值”对。

在代码中，字典输入时带花括号{}。

```python3
>>> myCat = {'size': 'fat', 'color': 'gray', 'disposition': 'loud'}
>>> myCat['size']
'fat'
>>> 'My cat has ' + myCat['color'] + ' fur.'
'My cat has gray fur.'
```
因为字典是不排序的，所以不能像列表那样切片。

尝试访问字典中不存在的键，将导致 KeyError 出错信息。这很像列表的“越界”
IndexError 出错信息。

用in 关键字，可以看看输入的名字是否作为键存在于字典中，就像查看列表一样。

如果该名字在字典中，你可以用方括号访问关联的值。如果不在，你可以用同样的方括号语法和赋值操作符添加它
### 5.1 keys()、values()和 items()方法
它们将返回类似列表的值，分别对应于字典的键、值和键-值对。

这些方法返回的值不是真正的列表，它们不能被修改，没有append()方法。但这些数据类型（分别是 dict_keys、dict_values 和 dict_items）可以用于for 循环。

```python3
>>> spam = {'color': 'red', 'age': 42}
>>> for v in spam.values():
print(v)
red
42
```
### 5.2 检查字典中是否存在键或值
in 和 not in 操作符可以检查值是否存在于列表中。也可以利用这些操作符，检查某个键或值是否存在于字典中。

```python3
>>> spam = {'name': 'Zophie', 'age': 7}
>>> 'name' in spam.keys()
True
>>> 'Zophie' in spam.values()
True
>>> 'color' in spam.keys()
False
>>> 'color' not in spam.keys()
True
>>> 'color' in spam
False
```
'color' in spam 本质上是一个简写版本。相当于'color' in spam.keys()。

如果想要检查一个值是否为字典中的键，就可以用关键字 in（或 not in），作用于该字典本身。
### 5.3 get()方法
get()方法，它有两个参数：要取得其值的键，以及如果该键不存在时，返回的备用值。

```python3
>>> picnicItems = {'apples': 5, 'cups': 2}
>>> 'I am bringing ' + str(picnicItems.get('cups', 0)) + ' cups.'
'I am bringing 2 cups.'
>>> 'I am bringing ' + str(picnicItems.get('eggs', 0)) + ' eggs.'
'I am bringing 0 eggs.'
```
### 5.4 setdefault()方法
传递给该方法的第一个参数，是要检查的键。第二个参数，是如果该键不存在时要设置的值。如果该键确实存在，方法就会返回键的值。

```python3
>>> spam = {'name': 'Pooka', 'age': 5}
>>> spam.setdefault('color', 'black')
'black'
>>> spam
{'color': 'black', 'age': 5, 'name': 'Pooka'}
>>> spam.setdefault('color', 'white')
'black'
>>> spam
{'color': 'black', 'age': 5, 'name': 'Pooka'}

```
setdefault()方法是一个很好的快捷方式，可以确保一个键存在。

```python3
message = 'It was a bright cold day in April, and the clocks were striking thirteen.'
count = {}
for character in message:
    count.setdefault(character, 0)
    count[character] = count[character] + 1
print(count)
```
setdefault()方法调用确保了键存在于 count 字典中（默认值是 0），这样在执行 count[character] =count[character] + 1 时，就不会抛出 KeyError 错误。
### 5.5 漂亮打印
导入 pprint 模块，就可以使用pprint()和pformat()函数，它们将“漂亮打印”一个字典的字。

```python3
import pprint
message = 'It was a bright cold day in April, and the clocks were striking
thirteen.'
count = {}
for character in message:
count.setdefault(character, 0)
count[character] = count[character] + 1
pprint.pprint(count)
```
打印结果将按照键名排序后比较漂亮的打印出来。

如果字典本身包含嵌套的列表或字典，pprint.pprint()函数就特别有用。

如果希望得到漂亮打印的文本作为字符串，而不是显示在屏幕上，那就调用
pprint.pformat()。

```python3
str = pprint.pformat(someDictionaryValue)
```
写一个名为 addToInventory(inventory,addedItems)的函数，其中inventory参数是一个字典，表示玩家的物品清单（像前面项目一样），addedItems 参数是一个列表。
```
def addToInventory(inventory, addedItems):
    # your code goes here
    for i in range(len(addedItems)):
        inventory.setdefault(addedItems[i], 0)
        inventory[addedItems[i]] += 1


inv = {'gold coin': 42, 'rope': 1}
dragonLoot = ['gold coin', 'dagger', 'gold coin', 'gold coin', 'ruby']
addToInventory(inv, dragonLoot)
print(inv)
```
## 6 字符串操作
### 6.1 有用的字符串方法
**原始字符串**

可以在字符串开始的引号之前加上r，使它成为原始字符串。“原始字符串”完全忽略所有的转义字符，打印出字符串中所有的倒斜杠。

**字符串方法 upper()、lower()、isupper()和 islower()**

在程序中加入代码，处理多种用户输入情况或输入错误，诸如大小写不一致，这会让程序更容易使用，且更不容易失效。

如果字符串至少有一个字母，并且所有字母都是大写或小写，isupper()和islower()方法就会相应地返回布尔值 True。否则，该方法返回 False。

upper()和 lower()字符串方法返回一个新字符串，其中原字符串的所有字母都被
相应地转换为大写或小写。字符串中非字母字符保持不变。

请注意，这些方法没有改变字符串本身，而是返回一个新字符串。如果你希望改
变原来的字符串，就必须在该字符串上调用 upper()或 lower()，然后将这个新字符串
赋给保存原来字符串的变量。

即
```python3
>>> spam = 'Hello world!'
>>> spam = spam.upper()
>>> spam
'HELLO WORLD!
```
**isX 字符串方法**

如果需要验证用户输入，isX 字符串方法是有用的。

- isalpha()返回 True，如果字符串只包含字母，并且非空；
- isalnum()返回 True，如果字符串只包含字母和数字，并且非空；
- isdecimal()返回 True，如果字符串只包含数字字符，并且非空；
- isspace()返回 True，如果字符串只包含空格、制表符和换行，并且非空；
- istitle()返回 True，如果字符串仅包含以大写字母开头、后面都是小写字母的单词。

**字符串方法 startswith()和 endswith()**

startswith()和 endswith()方法返回 True，如果它们所调用的字符串以该方法传入的字符串开始或结束。否则，方法返回 False。

**字符串方法 join()和 split()**

join()方法在一个字符串上调用，参数是一个字符串列表，返回一个字符串。返回的字符串由传入的列表中每个字符串连接而成。

```python3
>>> ', '.join(['cats', 'rats', 'bats'])
'cats, rats, bats'
>>> ' '.join(['My', 'name', 'is', 'Simon'])
'My name is Simon'
>>> 'ABC'.join(['My', 'name', 'is', 'Simon'])
'MyABCnameABCisABCSimon
```
split()方法做的事情正好相反：它针对一个字符串调用，返回一个字符串列表。

```python3
string = 'I,am,a,boy'
s = string.split(',') # 默认参数为空格
print(s)
```
一个常见的 split()用法，是按照换行符分割多行字符串。

```python3
>>> spam = '''Dear Alice,
How have you been? I am fine.
There is a container in the fridge
that is labeled "Milk Experiment".
Please do not drink it.
Sincerely,
Bob'''
>>> spam.split('\n')
['Dear Alice,', 'How have you been? I am fine.', 'There is a container in the
fridge', 'that is labeled "Milk Experiment".', '', 'Please do not drink it.',
'Sincerely,', 'Bob']
```
**用 rjust()、ljust()和 center()方法对齐文本**

rjust()和 ljust()字符串方法返回调用它们的字符串的填充版本，通过插入空格来
对齐文本。这两个方法的第一个参数是一个整数长度，用于对齐字符串。

```python3
>>> 'Hello'.rjust(10)
' Hello'
>>> 'Hello'.rjust(20)
' Hello'
>>> 'Hello World'.rjust(20)
' Hello World'
>>> 'Hello'.ljust(10)
'Hello '
```
rjust()和 ljust()方法的第二个可选参数将指定一个填充字符，取代空格字符。

```python3
>>> 'Hello'.rjust(20, '*')
'***************Hello'
>>> 'Hello'.ljust(20, '-')
'Hello---------------'
```
center()字符串方法与 ljust()与 rjust()类似，但它让文本居中，而不是左对齐或
右对齐。

```python3
>>> 'Hello'.center(20)
' Hello '
>>> 'Hello'.center(20, '=')
'=======Hello========'
```
**用 strip()、rstrip()和 lstrip()删除空白字符**

strip()字符串方法将返回一个新的字符串，它的开头或末尾都没有空白字符。

lstrip()和 rstrip()方法将相应删除左边或右边的空白字符。

有一个可选的字符串参数，指定==两边==的哪些字符应该删除。

```python3
>>> spam = 'SpamSpamBaconSpamEggsSpamSpam'
>>> spam.strip('ampS')
'BaconSpamEggs'
```
传入 strip()方法的字符串中，字符的顺序并不重要

**用 pyperclip 模块拷贝粘贴字符串**
pyperclip 模块有 copy()和 paste()函数，可以向计算机的剪贴板发送文本，或从
它接收文本。将程序的输出发送到剪贴板，使它很容易粘贴到邮件、文字处理程序
或其他软件中。

```python3
>>> import pyperclip
>>> pyperclip.copy('Hello world!')
>>> pyperclip.paste()
'Hello world!'

```

