# 自动化任务
## 1. 模式匹配与正则表达式
Python 中所有正则表达式的函数都在 re 模块中。
### 1.1 Regex对象与Match对象
向 re.compile()传入一个字符串值，表示正则表达式，它将返回一个Regex模式对象（或者就简称为 Regex 对象）。

```python3
phoneNumRegex = re.compile(r'\d\d\d-\d\d\d-\d\d\d\d')

```
Regex 对象的 search()方法查找传入的字符串，寻找该正则表达式的所有匹配。如
果字符串中没有找到该正则表达式模式，search()方法将返回 None。如果找到了该模式，
search()方法将返回一个 **Match对象**。

**Match对象**有一个 group()方法，它返回被查找字符串中实际匹配的文本。

```python3
>>> phoneNumRegex = re.compile(r'\d\d\d-\d\d\d-\d\d\d\d')
>>> mo = phoneNumRegex.search('My number is 415-555-4242.')
>>> print('Phone number found: ' + mo.group())
Phone number found: 415-555-4242
```
虽然在 Python 中使用正则表达式有几个步骤，但每一步都相当简单。

1. 用 import re 导入正则表达式模块。
2. 用 re.compile()函数创建一个 Regex 对象（记得使用原始字符串）。
3. 向 Regex 对象的 search()方法传入想查找的字符串。它返回一个 Match 对象。
4. 调用 Match 对象的 group()方法，返回实际匹配文本的字符串。

```python3
graph LR
A[正则表达式模式]-->|调用re.compile|B[Regex 对象]
B[Regex 对象]-->|调用 search|c[Match 对象]
```
### 1.2 用正则表达式匹配更多模式
#### 1.2.1 利用括号分组
假定想要将区号从电话号码中分离。添加括号将在正则表达式中创建“分组”：
(\d\d\d)-(\d\d\d-\d\d\d\d)。然后可以使用 group()匹配对象方法，从一个分组中获取匹
配的文本。

正则表达式字符串中的第一对括号是第 1 组。第二对括号是第 2 组。向 group()
匹配对象方法传入整数 1 或 2，就可以取得匹配文本的不同部分。向 group()方法传
入 0 或不传入参数，将返回整个匹配的文本。

```python3
>>> phoneNumRegex = re.compile(r'(\d\d\d)-(\d\d\d-\d\d\d\d)')
>>> mo = phoneNumRegex.search('My number is 415-555-4242.')
>>> mo.group(1)
'415'
>>> mo.group(2)
'555-4242'
>>> mo.group(0)
'415-555-4242'
>>> mo.group()
'415-555-4242'
```
如果想要一次就获取所有的分组，请使用 groups()方法，mo.groups()返回多个值的元组。

```python3
>>> mo.groups()
('415', '555-4242')
>>> areaCode, mainNumber = mo.groups()
>>> print(areaCode)
415
>>> print(mainNumber)
555-4242
```
括号在正则表达式中有特殊的含义，但是如果你需要在文本中匹配括号，需要用倒斜杠对(和)进行字符转义。
#### 1.2.2 用管道匹配多个分组
字符|称为“管道”。正则表达式 r'Batman|Tina Fey'将匹配'Batman'或'Tina Fey'。

如果 Batman 和 Tina Fey 都出现在被查找的字符串中，第一次出现的匹配文本，
将作为 Match 对象返回。

```python3
>>> heroRegex = re.compile (r'Batman|Tina Fey')
>>> mo1 = heroRegex.search('Batman and Tina Fey.')
>>> mo1.group()
'Batman'
>>> mo2 = heroRegex.search('Tina Fey and Batman.')
>>> mo2.group()
'Tina Fey'
```
假设你希望匹配'Batman'、'Batmobile'、'Batcopter'和'Batbat'中任意一个。因为所有这
些字符串都以 Bat 开始，所以如果能够只指定一次前缀。

```python3
>>> batRegex = re.compile(r'Bat(man|mobile|copter|bat)')
>>> mo = batRegex.search('Batmobile lost a wheel')
>>> mo.group()
'Batmobile'
>>> mo.group(1)
'mobile'
```
mo.group()返回了完全匹配的文本'Batmobile'，而mo.group(1)只是返回第一个括号分组内匹配的文本'mobile'。
#### 1.2.3 用问号实现可选匹配
字符?表明它前面的分组在这个模式中是可选的,不论这段文本在不在，正则表达式都会认为匹配。

可以认为?是在说，“匹配这个问号之前的分组零次或一次”。
```python3
>>> batRegex = re.compile(r'Bat(wo)?man')
>>> mo1 = batRegex.search('The Adventures of Batman')
>>> mo1.group()
'Batman'
>>> mo2 = batRegex.search('The Adventures of Batwoman')
>>> mo2.group()
'Batwoman'
```
#### 1.2.4 用星号匹配零次或多次
*（称为星号）意味着“匹配零次或多次”，即星号之前的分组，可以在文本中出Python 编程快速上手——让繁琐工作自动化现任意次。

```python3
>>> batRegex = re.compile(r'Bat(wo)*man')
>>> mo1 = batRegex.search('The Adventures of Batman')
>>> mo1.group()
'Batman'
>>> mo2 = batRegex.search('The Adventures of Batwoman')
>>> mo2.group()
'Batwoman'
>>> mo3 = batRegex.search('The Adventures of Batwowowowoman')
>>> mo3.group()
'Batwowowowoman'
```
#### 1.2.5 用加号匹配一次或多次
+（加号）则意味着“匹配一次或多次”。加号前面的分组必须“至少出现一次”。

正则表达式 Bat(wo)+man 不会匹配字符串'The Adventures of Batman'，因为加号
要求 wo 至少出现一次。
#### 1.2.6 用花括号匹配特定次数
如果想要一个分组重复特定次数，就在正则表达式中该分组的后面，跟上花括号包围的数字。例如，正则表达式(Ha){3}将匹配字符串'HaHaHa'。

除了一个数字，还可以指定一个范围，即在花括号中写下一个最小值、一个逗号和
一个最大值。例如，正则表达式(Ha){3,5}将匹配'HaHaHa'、'HaHaHaHa'和'HaHaHaHaHa'。

也可以不写花括号中的第一个或第二个数字，不限定最小值或最大值。

(Ha){3,}将匹配 3 次或更多次实例，(Ha){,5}将匹配 0 到 5 次实例。
### 1.3 贪心和非贪心匹配
Python 的正则表达式默认是“贪心”的，这表示在有二义的情况下，它们会尽可能匹配最长的字符串。

花括号的“非贪心”版本匹配尽可能最短的字符串，即在结束的花括号后跟着一个问号。

```python3
>>> greedyHaRegex = re.compile(r'(Ha){3,5}')
>>> mo1 = greedyHaRegex.search('HaHaHaHaHa')
>>> mo1.group()
'HaHaHaHaHa'
>>> nongreedyHaRegex = re.compile(r'(Ha){3,5}?')
>>> mo2 = nongreedyHaRegex.search('HaHaHaHaHa')
>>> mo2.group()
'HaHaHa
```
**问号在正则表达式中可能有两种含义：==声明非贪心匹配==或==表示可选的分组==。这两种含义是完全无关的。**
### 1.4 findall()方法
除了search方法外，Regex对象也有一个findall()方法。

search()将返回==一个Match对象==，包含被查找字符串中的“第一次”匹配的文本 

findall()方法将返回==一组字符串==，包含被查找字符串中的所有匹配。

如果在正则表达式中有分组，那么 findall 将返回==元组的列表==。每个元组表示一个找
到的匹配，其中的项就是正则表达式中每个分组的匹配字符串。

```python3
import re


message = 'Cell: 415-555-9999 Work: 212-555-0000'
numberRex1 = re.compile(r'\d{3}-\d{3}-\d{4}')
numberRex2 = re.compile(r'(\d{3})-(\d{3}-\d{4})')
num = numberRex1.search(message)
print(num.group())
numList1 = numberRex1.findall(message)
numList2 = numberRex2.findall(message)
print(numList1)
print(numList2)
print(numList1[0])
print(numList2[0][0])
```
输出结果：

```
415-555-9999
['415-555-9999', '212-555-0000']
[('415', '555-9999'), ('212', '555-0000')]
415-555-9999
415

```
### 1.5 字符分类

缩写字符分类 | 表示
---|---
\d | 0 到 9 的任何数字
\D | 除 0 到 9 的数字以外的任何字符
\w | 任何字母、数字或下划线字符（可以认为是匹配“单词”字符）
\W | 除字母、数字和下划线以外的任何字符
\s | 空格、制表符或换行符（可以认为是匹配“空白”字符）
\S | 除空格、制表符和换行符以外的任何字符

字符分类对于缩短正则表达式很有用。字符分类[0-5]只匹配数字 0 到 5，这比
输入(0|1|2|3|4|5)要短很多。

### 1.6 建立自己的字符分类
- 可以用方括号定义自己的字符分类。例如，字符分类[aeiouAEIOU]将匹配所有元音字符，不论大小写。
- 可以使用短横表示字母或数字的范围。例如，字符分类[a-zA-Z0-9]将匹配所
有小写字母、大写字母和数字。

请注意，在方括号内，==普通的正则表达式符号不会被解释==。这意味着，你不需
要前面加上倒斜杠转义.、*、?或()字符。例如，字符分类将匹配数字 0 到 5 和一个
句点。你不需要将它写成[0-5\\.]。

通过在字符分类的左方括号后加上一个插入字符（^），就可以得到“非字符类”。
非字符类将匹配不在这个字符类中的所有字符。

例如： consonantRegex = re.compile(r'[^aeiouAEIOU]')

不是匹配所有元音字符，而是匹配所有非元音字符。
### 1.7 插入字符和美元字符
在正则表达式的开始处使用插入符号（^），表明匹配必须发生在被查找文
本开始处。

在正则表达式的末尾加上美元符号（$），表示该字符串必
须以这个正则表达式的模式结束。

可以同时使用^和$，表明整个字符串必须匹配该模式。
### 1.8 通配字符
在正则表达式中，.（句点）字符称为“通配符”。它匹配除了换行之外的所有
字符。

**要记住，句点字符只匹配一个字符**

#### 1.8.1 用点-星匹配所有字符
用点-星（.*）表示“任意文本”。
```python3
>>> nameRegex = re.compile(r'First Name: (.*) Last Name: (.*)')
>>> mo = nameRegex.search('First Name: Al Last Name: Sweigart')
>>> mo.group(1)
'Al'
>>> mo.group(2)
'Sweigart'
```
点-星使用“贪心”模式：它总是匹配尽可能多的文本。

要用“非贪心”模式匹配所有文本，就使用点-星和问号 (.*?)。

```python3
>>> nongreedyRegex = re.compile(r'<.*?>')
>>> mo = nongreedyRegex.search('<To serve man> for dinner.>')
>>> mo.group()
'<To serve man>'
>>> greedyRegex = re.compile(r'<.*>')
>>> mo = greedyRegex.search('<To serve man> for dinner.>')
>>> mo.group()
'<To serve man> for dinner.>'
```
两个正则表达式都可以翻译成“匹配一个左尖括号，接下来是任意字符，接下
来是一个右尖括号”。
#### 1.8.2 用句点字符匹配换行
点-星将匹配除换行外的所有字符。通过传入 re.DOTALL 作为 re.compile()的第
二个参数，可以让句点字符匹配所有字符，包括换行字符。

```python3
>>> noNewlineRegex = re.compile('.*')
>>> noNewlineRegex.search('Serve the public trust.\nProtect the innocent.
\nUphold the law.').group()
'Serve the public trust.'
>>> newlineRegex = re.compile('.*', re.DOTALL)
>>> newlineRegex.search('Serve the public trust.\nProtect the innocent.\nUphold the law.').group()
'Serve the public trust.\nProtect the innocent.\nUphold the law.'
```
### 1.9 不区分大小写的匹配
通常，正则表达式用你指定的大小写匹配文本。

要让正则表达式不区分大小写，可以向re.compile()传入re.IGNORECASE或re.I，作为第二个参数。
### 1.10 用 sub()方法替换字符串
正则表达式不仅能找到文本模式，而且能够用新的文本替换掉这些模式。Regex
对象的 sub()方法需要传入两个参数。

第一个参数是一个字符串，用于取代发现的匹配。

第二个参数是一个字符串，即正则表达式。

sub()方法返回替换完成后的字符串。

```python3
>>> namesRegex = re.compile(r'Agent \w+')
>>> namesRegex.sub('CENSORED', 'Agent Alice gave the secret documents to Agent Bob.')
'CENSORED gave the secret documents to CENSORED.'

```

```python3
agentstring = 'Agent Alice told Agent Carol that Agent Eve knew that Agent Bob was a double agent.'
agentNamesRegex = re.compile(r'Agent (\w)\w*')
searchResult = agentNamesRegex.search(agentstring)
print(searchResult.group())
print(agentNamesRegex.findall(agentstring))
result = agentNamesRegex.sub(r'Agent \1****', agentstring)
print(result)
inputStr = '你好 小明,你来啦！'
replaceRex = re.compile(r'你好 \w+')
mo = replaceRex.search(inputStr)
print(mo.group())
print(replaceRex.sub(r'你好 小红', inputStr))

```
输出结果：
```
Agent Alice
['A', 'C', 'E', 'B']
Agent A**** told Agent C**** that Agent E**** knew that Agent B**** was a double agent.
你好 小明
你好 小红,你来啦！
```
### 1.11 管理复杂的正则表达式
匹配复杂的文本模式，可能需要长的、费解的正则表达式。你可以告诉re.compile()，忽略正则表达式字符串中的空白符和注释，从而缓解这一点。要实现这种详细模式，可以向 re.compile()传入变量 re.VERBOSE，作为第二个参数。

```python3
phoneRegex = re.compile(r'''(
(\d{3}|\(\d{3}\))?              # area code
(\s|-|\.)?                      # separator
\d{3}                           # first 3 digits
(\s|-|\.)                       # separator
\d{4}                           # last 4 digits
(\s*(ext|x|ext.)\s*\d{2,5})?    # extension
)''', re.VERBOSE)

```
前面的例子使用了三重引号('")，创建了一个多行字符串。这样就可以将正则表达式定义放在多行中，让它更可读。

表示正则表达式的多行字符串中，多余的空白字符也不认为是要匹配的文本模式的一部分。
### 1.12 组合使用 re.IGNOREC ASE、re.DOTALL 和 re.VERBOSE
re.compile()函数只接受一个值作为它的第二参数。

当希望在正则表达式中使用re.VERBOSE来编写注释，同时还希望使用re.IGNORECASE来忽略大小写，可以使用管道字符（|）将变量组合起来，从而绕过这个限制。管道字符在这里称为“按位或”操作符。

```python3
someRegexValue = re.compile('foo', re.IGNORECASE | re.DOTALL)
```
## 2. 读写文件
### 2.1 文件与文件路径
在 Windows 上，路径书写使用倒斜杠作为文件夹之间的分隔符。

在 OS X 和Linux 上，使用正斜杠作为它们的路径分隔符。

如果需要创建文件名称的字符串，os.path.join()函数就很有用。这些字符串将传
递给几个文件相关的函数。

```python3
>>> myFiles = ['accounts.txt', 'details.csv', 'invite.docx']
>>> for filename in myFiles:
print(os.path.join('C:\\Users\\asweigart', filename))
C:\Users\asweigart\accounts.txt
C:\Users\asweigart\details.csv
C:\Users\asweigart\invite.docx
```
#### 2.1.1 当前工作目录
每个运行在计算机上的程序，都有一个“当前工作目录”，或 cwd。

利用 os.getcwd()函数，可以取得当前工作路径的字符串，并可以利用 os.chdir()改变它。

```python3
>>> import os
>>> os.getcwd()
'C:\\Python34'
>>> os.chdir('C:\\Windows\\System32')
>>> os.getcwd()
'C:\\Windows\\System32'
```
如果要更改的当前工作目录不存在，Python 就会显示一个错误。
#### 2.1.2 绝对路径与相对路径
有两种方法指定一个文件路径。
- “绝对路径”，总是从根文件夹开始。
- “相对路径”，它相对于程序的当前工作目录。

单个的句点（“点”）用作文件夹目名称时，是“这个目录”的缩写。两个句点（“点点”）意思是父文件夹。

相对路径开始处的.\是可选的。例如，.\spam.txt 和 spam.txt 指的是同一个文件。
####  2.1.3 用os.makedirs()创建新文件夹

```python3
>>> import os
>>> os.makedirs('C:\\delicious\\walnut\\waffles')
```
这不仅将创建 C:\delicious 文件夹，也会在 C:\delicious 下创建 walnut 文件夹，
并在 C:\delicious\walnut 中创建 waffles 文件夹。目的是确保完整路径名存在。
#### 2.1.4 os.path 模块
os.path 模块包含了许多与文件名和文件路径相关的有用函数。

例如：os.path.join()来构建所有操作系统上都有效的路径。
#### 2.1.5 处理绝对路径和相对路径
os.path 模块提供了一些函数，返回一个相对路径的绝对路径，以及检查给定的
路径是否为绝对路径。

- os.path.isabs(path) 是绝对路径，就返回 True
- os.path.abspath(path) 将相对路径转换为绝对路径
- os.path.relpath(path, start) 返回从 start 路径到 path 的相对路径的字符串

基本名称跟在路径中最后一个斜杠后，它和文件名一样，目录名称是最后一个斜杠之前的所有内容
- os.path.dirname(path) 返回目录名称
- os.path.basename(path) 返回基本名称

```python3
>>> path = 'C:\\Windows\\System32\\calc.exe'
>>> os.path.basename(path)
'calc.exe'
>>> os.path.dirname(path)
'C:\\Windows\\System32'
```
如果同时需要一个路径的目录名称和基本名称，就可以调用 os.path.split()，获
得这两个字符串的元组

```python3
>>> calcFilePath = 'C:\\Windows\\System32\\calc.exe'
>>> os.path.split(calcFilePath)
('C:\\Windows\\System32', 'calc.exe')
```
#### 2.1.6 查看文件大小和文件夹内容
os.path 模块提供了一些函数，用于查看文件的字节数以及给定文件夹中的文件和子文件夹。
- 调用 os.path.getsize(path)将返回 path 参数中文件的字节数。
- 调用 os.listdir(path)将返回文件名字符串的列表，包含 path 参数中的每个文件

如果想知道这个目录下所有文件的总字节数，就可以同时使用 os.path.getsize()和 os.listdir()。
```python3
>>> totalSize = 0
>>> for filename in os.listdir('C:\\Windows\\System32'):
totalSize = totalSize + os.path.getsize(os.path.join('C:\\Windows\\System32', filename))
>>> print(totalSize)
1117846456

```
#### 2.1.7 检查路径有效性
- os.path.exists(path)  path参数所指的文件或文件夹存在，将返回 True
- os.path.isfile(path)  path 参数存在，并且是一个文件，将返回 True
- os.path.isdir(path)  path 参数存在，并且是一个文件夹，将返回 True

### 2.2 文件读写过程
- “纯文本文件”只包含基本文本字符，不包含字体、大小和颜色信息。带有 .txt扩展名的文本文件以及带有.py 扩展名的Python脚本文件，都是纯文本文件的例子。
- “二进制文件”是所有其他文件类型，诸如字处理文档、PDF、图像、电子表格
和可执行程序。

在 Python 中，读写文件有 3 个步骤：

1. 调用 open()函数，返回一个 File 对象。
2. 调用 File 对象的 read()或 write()方法。
3. 调用 File 对象的 close()方法，关闭该文件。

#### 2.2.1 用 open()函数打开文件
open(path)函数返回一个 File 对象。

path既可以是绝对路径，也可以是相对路径。
```python3
>>> helloFile = open('C:\\Users\\your_home_folder\\hello.txt')
```
读模式是默认的模式。

File 对象代表计算机中的一个文件，它只是Python中另一种类型的值，就像你已熟悉的列表和字典。

#### 2.2.2 读取文件内容
有了一个 File 对象，就可以开始从它读取内容。
- read()方法将文件的内容看成是单个大字符串
- readlines()方法从文件取得一个字符串的列表。列表中的每个字符串就是文本中的每一行。

**与单个大字符串相比，字符串的列表通常更容易处理。**
#### 2.2.3 写入文件
需要以“写入纯文本模式”或“添加纯文本模式”打开该文件，或简称为“写模式”和“添加模式”。

write():将字符串写入文件，并返回写入的字符个数，包括换行符。然后用close()关闭该文件。

- 写模式:(将'w'作为第二个参数传递给 open())将覆写原有的文件，从头开始
- 添加模式:(将'a'作为第二个参数传递给 open())将在已有文件的末尾添加文本。

如果传递给 open()的文件名不存在，写模式和添加模式都会创建一个新的空文
件。在读取或写入文件后，调用 close()方法，然后才能再次打开该文件。
### 2.3 用 shelve 模块保存变量
将 Python 程序中的变量保存到二进制的 shelf 文件中

```python3python3
import shelve


shelfFile = shelve.open('mydata')
cats = ['Zophie', 'Pooka', 'Simon']
shelfFile['cats'] = cats
shelfFile.close()
shelfFile = shelve.open('mydata')
print(shelfFile['cats'])
shelfFile.close()
```
shelf 值不必用读模式或写模式打开，因为它们在打开后，既能读又能写。

就像字典一样，shelf值有keys()和values()方法，返回shelf中键和值的类似列表的值。

所以应该将它们传递给 list()函数，取得列表的形式。

创建文件时，如果你需要在 Notepad 或 TextEdit 这样的文本编辑器中读取它们，
纯文本就非常有用。但是，如果想要保存 Python 程序中的数据，那就使用 shelve 模块。
### 2.4 用 pprint.pformat()函数保存变量
假定你有一个字典，保存在一个变量中，你希望保存这个变量和它的内容，以便将来使用。pprint.pformat()函数将提供一个字符串，你可以将它写入.py文件。该文件将成为你自己的模块，如果你需要使用存储在其中的变量，就可以导入它。

创建自定义数据模块myCats
```python3
>>> import pprint
>>> cats = [{'name': 'Zophie', 'desc': 'chubby'}, {'name': 'Pooka', 'desc': 'fluffy'}]
>>> pprint.pformat(cats)
"[{'desc': 'chubby', 'name': 'Zophie'}, {'desc': 'fluffy', 'name': 'Pooka'}]"
>>> fileObj = open('myCats.py', 'w')
>>> fileObj.write('cats = ' + pprint.pformat(cats) + '\n')
83
>>> fileObj.close()
```
使用自定义模块myCats

```python3
>>> import myCats
>>> myCats.cats
[{'name': 'Zophie', 'desc': 'chubby'}, {'name': 'Pooka', 'desc': 'fluffy'}]
>>> myCats.cats[0]
{'name': 'Zophie', 'desc': 'chubby'}
>>> myCats.cats[0]['name']
'Zophie'
```
只有==基本数据类型==，诸如整型、浮点型、字符串、列表和字典，可以作为简单文本写入一个文件。例如，File 对象就不能够编码为文本。
## 3.调试
调试器是 IDLE 的一项功能，它可以一次执行一条指令，在代码运行时，让你有机会检查变量的值，并追踪程序运行时值的变化。这比程序全速运行要慢得多，但可以帮助你查看程序运行时其中实际的值，而不是通过源代码推测值可能是什么。

### 3.1 抛出异常
抛出异常相当于是说：“停止运行这个函数中的代码，将程序执行转到 except 语句 ”。

如果没有 try 和 except 语句覆盖抛出异常的 raise 语句，该程序就会崩溃，并显
示异常的出错信息。
- 抛出异常使用 raise 语句
- 调用Exception()函数 (将包含有用的出错信息的字符串传递给Exception()函数)

通常是调用该函数的代码知道如何处理异常，而不是该函数本身。所以你常常
会看到 raise 语句在一个函数中，try 和 except 语句在调用该函数的代码中。
```python3
def boxPrint(symbol, width, height):
    if len(symbol) != 1:
        raise Exception('Symbol must be a single character string.')
    if width <= 2:
        raise Exception('Width must be greater than 2.')
    if height <= 2:
        raise Exception('Height must be greater than 2.')
    print(symbol * width)
    for i in range(height - 2):
        print(symbol + (' ' * (width - 2)) + symbol)
    print(symbol * width)


for sym, w, h in (('*', 4, 4), ('O', 20, 5), ('x', 1, 3), ('ZZ', 3, 3)):
    boxPrint(sym, w, h)  # 程序崩溃
    try:
        boxPrint(sym, w, h)
    except Exception as err:
        print('An exception happened: ' + str(err))
```
如果没有 try 和 except 语句覆盖抛出异常的 raise 语句，该程序就会崩溃，并显
示异常的出错信息。

使用 try 和 except 语句，你可以更优雅地处理错误，而不是让整个程序崩溃。
### 3.2 取得反向跟踪的字符串
反向跟踪包含了出错消息、导致该错误的代码行号，以及导致该错误的函数调用的序列。这个序列称为“调用栈”。

在从多个位置调用函数的程序中，调用栈就能帮助你确定哪次调用导致了错误。

只要抛出的异常没有被处理，Python 就会显示反向跟踪。

使用Python的traceback模块将反向跟踪信息写入一个日志文件，并让程序继续运行。稍后，在准备调试程序时，可以检查该日志文件。

```python3
import traceback

def spam():
    bacon()

def bacon():
    raise Exception('This is the error message.')

try:
    spam()
except:
    errorFile = open('errorInfo.txt', 'a')
    errorFile.write(traceback.format_exc() + '\n' + '\n')
    errorFile.close()
    print('The traceback info was written to errorInfo.txt.')

```
write() 方法的返回值是 116，因为 116 个字符被写入到文件中。反向跟踪文本
被写入 errorInfo.txt。
### 3.3 断言
断言针对的是程序员的错误，而不是用户的错误。对于那些可以恢复的错误（诸如
文件没有找到，或用户输入了无效的数据），请抛出异常，而不是用 assert 语句检测它。

```python3
>>> podBayDoorStatus = 'open'
>>> assert podBayDoorStatus == 'open', 'The pod bay doors need to be "open".'
>>> podBayDoorStatus = 'I\'m sorry, Dave. I\'m afraid I can't do that.''
>>> assert podBayDoorStatus == 'open', 'The pod bay doors need to be "open".'
Traceback (most recent call last):
 File "<pyshell#10>", line 1, in <module>
 assert podBayDoorStatus == 'open', 'The pod bay doors need to be "open".'
AssertionError: The pod bay doors need to be "open".
```
在使用这个变量的程序中，基于这个值是'open'的假定，我们可能写下了大量的代码，即这些代码依赖于它是 'open'，才能按照期望工作。所以添加了一个断言，确保假定 podBayDoorStatus 是 'open' 是对的。


```python3
def switchLights(stoplight):
    for key in stoplight.keys():
        if stoplight[key] == 'green':
            stoplight[key] = 'yellow'
        elif stoplight[key] == 'yellow':
            stoplight[key] = 'red'
        elif stoplight[key] == 'red':
            stoplight[key] = 'green'
    assert 'red' in stoplight.values(), 'Neither light is red! ' + str(stoplight)


market_2nd = {'ns': 'green', 'ew': 'red'}
switchLights(market_2nd)
```
运行结果：

```
Traceback (most recent call last):
  File "F:/Open/Project/Python/untitled/pythontest.py", line 13, in <module>
    switchLights(market_2nd)
  File "F:/Open/Project/Python/untitled/pythontest.py", line 9, in switchLights
    assert 'red' in stoplight.values(), 'Neither light is red! ' + str(stoplight)
AssertionError: Neither light is red! {'ns': 'yellow', 'ew': 'green'}
```


这里重要的一行是 AssertionError。虽然程序崩溃并非如你所愿，但它马上指
出了心智正常检查失败：两个方向都没有红灯，这意味着两个方向的车都可以走。
在程序执行中尽早快速失败，可以省去将来大量的调试工作。

在运行 Python 时传入-O 选项，可以禁用断言。

断言是针对开发的，不是针对最终产品。当你将程序交给其他人运行时，它应该没有缺陷，不需要进行心智正常检查。
### 3.4 日志
记日志是一种很好的方式，可以理解程序中发生的事，以及事情发生的顺序。Python 的 logging 模块使得你很容易创建自定义的消息记录。这些日志消息将描述程序执行何时到达日志函数调用，并列出你指定的任何变量当时的值。另一方面，缺失日志信息表明有一部分代码被跳过，从未执行。
#### 3.4.1 使用日志模块
将下面的代码复制到程序顶部（但在 Python 的#!行之下）：
```python3
import logging
logging.basicConfig(level=logging.DEBUG, format=' %(asctime)s - %(levelname)s- %(message)s')
```
当 Python 记录一个事件的日志时，它会创建一个LogRecord对象，保存关于该事件的信息。logging 模块的函数让你指定想看到的这个LogRecord对象的细节，以及希望的细节展示方式。

```python3
import logging
logging.basicConfig(level=logging.DEBUG, format=' %(asctime)s - %(levelname)s- %(message)s')
logging.debug('Start of program')


def factorial(n):
    logging.debug('Start of factorial(%s%%)' % (n))
    total = 1
    for i in range(1, n + 1):
        total *= i
        logging.debug('i is ' + str(i) + ', total is ' + str(total))
        logging.debug('End of factorial(%s%%)' % (n))
    return total


print(factorial(5))
logging.debug('End of program')
```
debug() 函数将调用 basicConfig()，打印一行信息。这行信息的格式是我们在 basicConfig()函数中指定的，并且包括我们传递给 debug() 的消息。

运行结果：

```
120
 2018-07-23 21:18:29,402 - DEBUG- Start of program
 2018-07-23 21:18:29,402 - DEBUG- Start of factorial(5%)
 2018-07-23 21:18:29,402 - DEBUG- i is 1, total is 1
 2018-07-23 21:18:29,402 - DEBUG- End of factorial(5%)
 2018-07-23 21:18:29,402 - DEBUG- i is 2, total is 2
 2018-07-23 21:18:29,402 - DEBUG- End of factorial(5%)
 2018-07-23 21:18:29,402 - DEBUG- i is 3, total is 6
 2018-07-23 21:18:29,402 - DEBUG- End of factorial(5%)
 2018-07-23 21:18:29,402 - DEBUG- i is 4, total is 24
 2018-07-23 21:18:29,402 - DEBUG- End of factorial(5%)
 2018-07-23 21:18:29,402 - DEBUG- i is 5, total is 120
 2018-07-23 21:18:29,402 - DEBUG- End of factorial(5%)
 2018-07-23 21:18:29,402 - DEBUG- End of program
```

logging.debug() 调用不仅打印出了传递给它的字符串，而且包含一个时间戳和单词 DEBUG。

日志消息是给程序员的，不是给用户的。

对于用户希望看到的消息，例如“文件未找到”或者“无效的输入，请输入一个数字”，应该使用 print() 调用。

我们不希望禁用日志消息之后，让用户看不到有用的信息。
#### 3.4.2 日志级别
“日志级别”提供了一种方式，按重要性对日志消息进行分类。

从最不重要到最重要。利用不同的日志函数，消息可以按某个级别记入日志。

级别 | 日志函数 | 描述
---|---|---
DEBUG | logging.debug() | 最低级别，用于小细节
INFO | logging.info() | 用于记录程序中一般事件的信息
WARNING | logging.warning() | 用于表示可能的问题
ERROR | logging.error() | 用于记录错误，它导致程序做某事失败
CRITICAL  | logging.critical()|用于表示致命的错误，导致程序完全停止工作


你可能只对错误感兴趣。在这种情况下，可以将 basicConfig() 的 level 参数设置为 logging.ERROR，这将只显示 ERROR和 CRITICAL 消息，跳过 DEBUG、INFO 和 WARNING 消息。

logging.disable() 函数禁用了这些消息，这样就不必进入到程序中，手工删除所有的日志调用。只要向logging.disable()传入一个日志级别，它就会禁止该级别和更低级别的所有日志消息。
#### 3.4.3 将日志记录到文件
logging.basicConfig() 函数接受 filename 关键字参数

```python3
import logging
logging.basicConfig(filename='myProgramLog.txt', level=logging.DEBUG, format='%(asctime)s - %(levelname)s - %(message)s')
```


