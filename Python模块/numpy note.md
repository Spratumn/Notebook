## 创建数组

```python3
# numpy.empty(shape, dtype = float, order = 'C')
# numpy.zeros(shape, dtype = float, order = 'C')
# numpy.ones(shape, dtype = None, order = 'C')
# order有"C"和"F"两个选项,分别代表，行优先和列优先，在计算机内存中的存储元素的顺序。
x = np.empty([3, 2], dtype=int)
print(x)
# [[0 0]
#  [0 0]
#  [0 0]]

# 从已有的数组创建数组
# numpy.asarray(a, dtype = None, order = None)
# a： 列表或元组
x = [(1, 2, 3), (4, 5)]
a = np.asarray(x)
print(a)  # [(1, 2, 3) (4, 5)]

# numpy.frombuffer(buffer, dtype = float, count = -1, offset = 0)
# 用于实现动态数组
# buffer 是字符串的时候，Python3 默认 str 是 Unicode 类型，所以要转成 bytestring 在原 str 前加上 b
# buffer：可以是任意对象，会以流的形式读入
# count：读取的数据数量，默认为-1，读取所有数据。
s = b'Hello World'
a = np.frombuffer(s, dtype='S1')
print(a)  # [b'H' b'e' b'l' b'l' b'o' b' ' b'W' b'o' b'r' b'l' b'd']

# numpy.fromiter(iterable, dtype, count=-1)
# 从可迭代对象中建立 ndarray 对象，返回一维数组

# 使用 range 函数创建列表对象
lst = range(5)
it = iter(lst)
# 使用迭代器创建 ndarray
x = np.fromiter(it, dtype=float)
print(x)  # [0. 1. 2. 3. 4.]

# 从数值范围创建数组
# 根据 start 与 stop 指定的范围以及 step 设定的步长，生成一个 ndarray。
# numpy.arange(start, stop, step, dtype)
# 创建一个一维数组，数组是一个等差数列构成的
# np.linspace(start, stop, num=50, endpoint=True, retstep=False, dtype=None)
# 创建一个于等比数列
# np.logspace(start, stop, num=50, endpoint=True, base=10.0, dtype=None)

# 默认底数是 10
a = np.logspace(1.0,  2.0, num=10)
print(a)
# 将对数的底数设置为 2
a = np.logspace(0, 9, 10, base=2)
print(a)
```
## 切片和索引
```python3
# NumPy 切片和索引
a = np.arange(10)
s = slice(2, 7, 2)   # 从索引 2 开始到索引 7 停止，间隔为2
print(a[s])  # [2 4 6]
b = a[2:7:2]   # 从索引 2 开始到索引 7 停止，间隔为 2
print(b)  # [2 4 6]
# 冒号 : 的解释：如果只放置一个参数，如 [2]，将返回与该索引相对应的单个元素。
# 如果为 [2:]，表示从该索引开始以后的所有项都将被提取。
# 如果使用了两个参数，如 [2:7]，那么则提取两个索引(不包括停止索引)之间的项。

# 切片还可以包括省略号 …，来使选择元组的长度与数组的维度相同。
# 如果在行位置使用省略号，它将返回包含行中元素的 ndarray。
a = np.array([[1, 2, 3],
              [3, 4, 5],
              [4, 5, 6]])
print(a[..., 1])   # 第2列元素  # [2 4 5]
print(a[1, ...])   # 第2行元素  # [3 4 5]
print(a[..., 1:])  # 第2列及剩下的所有元素
# [[2 3]
#  [4 5]
#  [5 6]]
```
## 广播
广播的规则:
- 让所有输入数组都向其中形状最长的数组看齐，形状中不足的部分都通过在前面加 1 补齐。
- 输出数组的形状是输入数组形状的各个维度上的最大值。
- 如果输入数组的某个维度和输出数组的对应维度的长度相同或者其长度为 1 时，这个数组能够用来计算，否则出错。
- 当输入数组的某个维度的长度为 1 时，沿着此维度运算时都用此维度上的第一组值。

简单理解：对两个数组，分别比较他们的每一个维度（若其中一个数组没有当前维度则忽略），满足：
- 数组拥有相同形状。
- 当前维度的值相等。
- 当前维度的值有一个是 1。
若条件不满足，抛出 "ValueError: frames are not aligned" 异常。


```
a = np.array([[0, 0, 0],
              [10, 10, 10],
              [20, 20, 20],
              [30, 30, 30]])
b = np.array([1, 2, 3])
bb = np.tile(b, (4, 1))
print(bb)
# [[1 2 3]
#  [1 2 3]
#  [1 2 3]
#  [1 2 3]]
print(a + bb)
# [[ 1  2  3]
#  [11 12 13]
#  [21 22 23]
#  [31 32 33]]
```

## 高级索引
```python3
# NumPy 高级索引
# 1、整数数组索引
x = np.array([[1,  2],
              [3,  4],
              [5,  6]])
y = x[[0, 1, 2],
      [0, 1, 0]]  # 获取数组中(0,0)，(1,1)和(2,0)位置处的元素
print(y)  # [1 4 5]

x = np.array([[0,  1,  2],
              [3,  4,  5],
              [6,  7,  8],
              [9,  10,  11]])
print('我们的数组是：')
print(x)
print('\n')
rows = np.array([[0, 0],
                 [3, 3]])
cols = np.array([[0, 2],
                 [0, 2]])
y = x[rows, cols]  # 获取了 4X3 数组中的四个角的元素，行索引是 [0,0] 和 [3,3]，而列索引是 [0,2] 和 [0,2]
print('这个数组的四个角元素是：')
print(y)

# 可以借助切片 : 或 … 与索引数组组合
a = np.array([[1, 2, 3],
              [4, 5, 6],
              [7, 8, 9]])
b = a[1:3, 1:3]
c = a[1:3, [1, 2]]
d = a[..., 1:]
print(b)
# [[5 6]
#  [8 9]]
print(c)
# [[5 6]
#  [8 9]]
print(d)
# [[2 3]
#  [5 6]
#  [8 9]]

# 布尔索引
# 通过一个布尔数组来索引目标数组
x = np.array([[0,  1,  2],
              [3,  4,  5],
              [6,  7,  8],
              [9,  10,  11]])
print('我们的数组是：')
print(x)
print('\n')
# 现在我们会打印出大于 5 的元素
print('大于 5 的元素是：')
print(x[x > 5])  # [ 6  7  8  9 10 11]

# 使用 ~（取补运算符）来过滤 NaN
a = np.array([np.nan,  1, 2, np.nan, 3, 4, 5])
print(a[~np.isnan(a)])  # [1. 2. 3. 4. 5.]

# 从数组中过滤掉非复数元素
a = np.array([1,  2+6j,  5,  3.5+5j])
print(a[np.iscomplex(a)])  # [2. +6.j 3.5+5.j]

# 花式索引
# 根据索引数组的值作为目标数组的某个轴的下标来取值。
# 对于使用一维整型数组作为索引，
# 如果目标是一维数组，那么索引的结果就是对应位置的元素；
# 如果目标是二维数组，那么就是对应下标的行

# 传入顺序索引数组
x = np.arange(32).reshape((8, 4))
print(x)
print(x[[4, 2, 1, 7]])
# [[16 17 18 19]
#  [ 8  9 10 11]
#  [ 4  5  6  7]
#  [28 29 30 31]]

# 传入倒序索引数组
x = np.arange(32).reshape((8, 4))
print(x[[-4, -2, -1, -7]])
# [[16 17 18 19]
#  [24 25 26 27]
#  [28 29 30 31]
#  [ 4  5  6  7]]

# 传入多个索引数组
x = np.arange(32).reshape((8, 4))
print(x[np.ix_([1, 5, 7, 2],    # 先选取行
               [0, 3, 1, 2])])  # 再按列顺序排序
# [[ 4  7  5  6]
#  [20 23 21 22]
#  [28 31 29 30]
#  [ 8 11  9 10]]
```
## 迭代数组

```python3
a = np.arange(6).reshape(2, 3)
print(a)
# [[0 1 2]
#  [3 4 5]]
for x in np.nditer(a):
    print(x, end=", ")
print('\n')  # 0, 1, 2, 3, 4, 5,  选择的顺序是和数组内存布局一致的  默认是行序优先

for x in np.nditer(a.T):
    print(x, end=", ")
# 0, 1, 2, 3, 4, 5,   a 和 a.T 的遍历顺序是一样的
print('\n')

for x in np.nditer(a.T.copy(order='C')):
    print(x, end=", ")
# 0, 3, 1, 4, 2, 5,  a.T.copy(order = 'C')遍历顺序为列优先
print('\n')

# 可以通过显式设置，来强制 nditer 对象使用某种顺序：
a = np.arange(0, 60, 5)
a = a.reshape(3, 4)
print('原始数组是：')
print(a)
# [[ 0  5 10 15]
#  [20 25 30 35]
#  [40 45 50 55]]
print('\n')
print('以 C 风格顺序排序：')
for x in np.nditer(a, order='C'):
    print(x, end=", ")
# 0, 5, 10, 15, 20, 25, 30, 35, 40, 45, 50, 55,
print('\n')
print('以 F 风格顺序排序：')
for x in np.nditer(a, order='F'):
    print(x, end=", ")
# 0, 20, 40, 5, 25, 45, 10, 30, 50, 15, 35, 55,

# 修改数组中元素的值
# nditer 对象有另一个可选参数 op_flags。
# 默认情况下，nditer 将视待迭代遍历的数组为只读对象（read-only）
# 为了在遍历数组的同时，实现对数组元素值得修改，必须指定 read-write 或者 write-only 的模式
a = np.arange(0, 60, 5)
a = a.reshape(3, 4)
print('原始数组是：')
print(a)
print('\n')
for x in np.nditer(a, op_flags=['readwrite']):
    x[...] = 2*x
print('修改后的数组是：')
print(a)
# [[  0  10  20  30]
#  [ 40  50  60  70]
#  [ 80  90 100 110]]

# 广播迭代
a = np.arange(0, 60, 5)
a = a.reshape(3, 4)
print('第一个数组为：')
print(a)
print('\n')
print('第二个数组为：')
b = np.array([1,  2,  3,  4], dtype=int)
print(b)
print('\n')
print('修改后的数组为：')
for x, y in np.nditer([a, b]):
    print("%d:%d" % (x, y), end=", ")
# 0:1, 5:2, 10:3, 15:4, 20:1, 25:2, 30:3, 35:4, 40:1, 45:2, 50:3, 55:4,
```
