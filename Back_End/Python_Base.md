# python 基础

> python执行过程：
> 	源代码 –编译--> 字节码(特定python的表现形式.pyc) –解释--> 机器码
> 	|—————————————————— 1 次 ————————————————|

## 基础语法

### 变量

1.  定义：关联一个对象的标识
2.  命名：
    -   必须为字母数或下划线，后面跟着字母，数字，下划线。
    -   不能使用保留关键字
3.  语法：
    -   变量名1 = 变量名2 = 表达式
    -   变量名1，变量名2  =表达式1，表达式2
        -   交换值（Python独有）
        -   变量名1，变量名2  =变量名2，变量名1

#### 数据类型

-   变量没有类型，关联的对象有类型。
-   可以通过 type 方法，判断对象的类型。
-   语法：变量 = type(数据)

1.  None

    -   表示不存在的特殊对象
    -   变量解除绑定

2.  整形 int

    -   整数. 包含正数，负数，0.
    -   字面值：
        -   默认十进制
        -   二进制（0,1）：0b0,0b1,0b10,0b11, 0b100 （使用二进制在前面加0b）
        -   八进制(0-7)：0o0,0o7,0o10（使用八进制在前面加0o）
        -   十六进制(0-9,a(10)-f(15))：0x0,0xa（使用十六进制在前面加0x）
            -   9, a, b, c ,d ,e ,f, 10 (9, 10, 11, 12, 13, 14, 15)
    -   小整数对象池：CPython 中整数-5 到 256，永远存在小整数对象池中，不会释放
        -   对象池：缓存的机制。
        -   使用 `id()`(返回变量存储的对象地址)，在交互式中可以体现，文件式中进行了优化

3.  浮点数 float

    -   小数 (包含正数，负数，0.0)
    -   科学计数法：e/E => 10 的 x 幂
        -   123.2e2

4.  字符串 str

    -   用于记录文本信息, `""` 双引号包裹。

5.  复数 complex

    -   虚部以j或J结尾
        -   5 + 3j

6.  布尔 bool
    -   True / False

#### 数据类型转换

-   int() / float() / str() / complex()
-   bool()
    -   False: 0, 0.0, "", None
-   混合类型自动升级
-   1 + 2.14 => 3.14

### 运算符

-   `+`  `-`  `*`
-   除
    -   小数除: `/`   (结果为float类型)
    -   底板除：`//`  (向下取整)
    -   取余数: `%`
-   幂运算:  `**`

-   优先级

    -   `**`
    -   `()`
    -   `*` `/` `%` `//`
    -   `+` `-`

-   round()  => (数据，精度)：四舍五入，精度如果省略，则默认为0.

#### 增强运算符

`x += 1 => x = x + 1`

-   `-=`
-   `*=`
-   `/=`
-   `//=`
-   `%=`
-   `**=`

#### 比较运算符

`>`
`<`
`==`
`>=`
`<=`
`!=`

#### 逻辑运算符

-   与 `and` => 一假皆假
-   或 `or` => 一真俱真
-   非 `not`

#### 短路逻辑

-   如果ａｎｄ前面的表达式结果为false，后面的表达式不再执行
    -   建议：将耗时的判断尽量放在后面
-   如果or前面的表达式结果为True，后面的表达式不再执行
    -   建议：将耗时的判断尽量放在后面

#### 身份运算符

-   判断两个对象是不是同一个对象
-   x is y => id(x) == id(y)
-   x is not y => id(x) != id(y)

#### 优先级

-   从高到低：
-   算数运算符
-   比较运算符
-   增强运算符
-   逻辑运算符

-   小括号最高

#### del

1.  作用： del 语句可以删除变量
2.  语法： del 变量名1，变量名2
3.  对象引用计数：当对象绑定给变量时，计数增加1，当变量解除绑定时，计数减少

    -   待计数为0时，对象自动释放。

    ![Python_RC](http://localhost:9710/blog/Python/Python_RC.jpg)

### 语句

1.  行

    -   物理行：程序员编写代码时的行。
    -   逻辑行：python解释器需要执行的指令。

        -   建议一个物理行对应一个逻辑行。
        -   如果在一个物理行使用多个逻辑行，使用分号隔开`；`
        -   `print('1');print('2')`

    -   如果逻辑行过长，可以使用隐式换行或显示换行
        -   隐式换行：所有括号的内容换行。`() [] {}`
        -   显示换行：通过折行符换行`\`

2.  选择

```python
if condition:
    print("...")
elif condition:
    print("...")
else:
    print("...")
```

-   真值表达式：
    -   if 条件可以为 非布尔变量，python 自动转化为 bool
-   条件表达式：
    -   语法：`variable` = `result1` if `condition` else `result2`

3.  循环

**while 语句**

```py
# while True => 死循环，后续代码不再执行
while condition:
    result
    if exit_condition:
        break # 退出循环体
else
    # while 条件结束执行，break后不执行 else
```

**for 语句**

-   用来遍历可迭代对象的数据元素
-   可迭代对象：能一次获取数据元素的对象

```py
for  变量列表 in 可迭代对象:
    语句
else
    语句
```

range() 函数

-   创建一个生成一系列整数的可迭代对象
-   range(开始，结束，间隔)
    -   返回的数字不包含结束点
    -   开始默认为 0
    -   间隔默认为 1

> for 语句 与 range() 函数 结合做预定次数的循环

4.  跳转语句

break:

-   跳出循环体，后面的代码不再执行。
-   可以让while语句的else部分不执行。

ontinue: 跳过本次循环，继续下次循环。

### 容器

#### 字符串 str

> 由一系列字符组成的**不可变序列**容器，存储的是字符编码值。

##### 编码

-   字节Byte：计算机存储的最小单位,等于8位bit（位）。
-   字符：单个的数字，文字，符号。
-   字符集(码表)：存储字符与二进制序列的对应关系。
-   编码：将字符转换为对应的二进制序列的过程。
-   解码：将二进制序列转换为对应的字符的过程。
-   编码方式：
    1.  ASCII编码：包含英文、数字等字符。每个字符1个字节。
    2.  GBK编码：兼容ASCII，包含21003个中文。每个英文1个字节，汉字2个字节。
    3.  Unicode字符集：国际统一编码，旧字符集每个字符2个字节，新字符集每个字符4字节。
    4.  UTF – 8编码：Unicode的存储和传输方式。英文1个字节，中文3个字节。

##### 相关函数

-   ord() ： 参数：字符串，返回 该字符串 的Unicode码
-   chr() ： 参数：Unicode码，返回 该字符串

##### 字面值

单引号和双引号（无区别）

-   单引号内的双引号不算结束符
-   双引号内的单引号不算结束符

三引号（即 js 中的模板字符串） =>  `"""str"""`/`'''str'''`
	可以包含单双引号
	可见即所得，常常作为文档注释

转义符

-   定义：可以改变原有字符含义的特殊字符
-   常用的：
    -   `\n` => 换行
    -   `\0` => 空字符
    -   `\t` => 水平制表格(即：tab)
-   原始字符：
    -   字符串前 加 `r` => 取消转义
    -   str = r"str"
-   字符串格式化:

    -   生成一定格式的字符串
    -   "str"%(变量列表)
        -   `"my name is %s. age is %d. score is %f" % (name, age, score)`
    -   格式：`%[- + 0 宽度.精度]类型码`

        -   `-` ： 左对齐(默认就是右对齐) => print("num is %-5d" % (num))
        -   `+` ：显示正号 => print("num is %-5d" % (num))
        -   0：左侧空白位置补零 => print("num is %05d" % (num))
        -   宽度：整个数据输出的宽度(字符大小) => print("num is %5d" % (num))
        -   精度：保留小数点后多少位 => print("num is %.2f" % (float_num))
            -   与 round() 的区别 功能相同
            -   round() 改变数值

    -   类型码: s(字符串) d(整数) f(float)

#### 通用操作

##### 数学运算符

-   `+`：拼接两个容器
-   `+=`：用原容器与右侧容器拼接，并重新绑定变量
-   `*`：重复生成容器元素
-   `*=`：用原容器生成重复元素，并重新绑定变量
-   `<  >  >=  <=  ==  !=`：依次比较两个容器中元素的编码值，一但不同则返回结果。

##### 成员运算符

-   数据  in  容器
-   数据  not in  容器
-   判断容器中是否包含指定数据，返回bool类型。

##### 索引

作用：访问容器元素
语法：容器[整数]

-   索引不能越界

正向索引从0开始，最后一个是`len(字符串) -1`
反向索引从-1开始，-1代表最后一个，第一个是 `- len(字符串)`

##### 切片

1.  作用：从容器中取出相应的元素重新组成一个容器。
2.  语法：容器[开始索引:结束索引:步长]
3.  说明：

-   结束索引不包含该位置元素
-   步长是切片每次获取完当前元素后移动的偏移量
-   省略开始索引，默认为0.
-   省略结束索引，默认最后。
-   即使越界也不会报错

4.  特殊

```py
"abcde"[::] # "abcde"
"abcde"[::-1] # "edcba"
"abcde"[-2:-5:-1] # "dcb"
"abcde"[3:1] # 空
"abcde"[3:1:-1] # "dc"
"abcde"[-2:] # "de"
"abcde"[1:1] # 空
```

##### 内建函数

-   len(容器)  返回容器长度
-   sum(容器)  返回容器所有元素的累加和（元素必须是数值）
-   max(容器)  返回容器中最大的元素
-   min(容器)  返回容器中最小的元素

#### 列表 list

> 由一系列变量组成的**可变序列**容器。

-   创建：
    -   创建空元组：list01 = \[] / list()
    -   创建有默认值的元组：list01 = [1,2,3] / list("agasdg")
        -   [元素]
        -   list(可迭代对象)
-   添加元素
    -   追加：list01.append(元素)
    -   插入：list01.inset(index, 元素)
-   删除元素
    -   移除指定元素：list01.remove(元素)
        -   删除一个建议使用
    -   删除指定索引元素：del list01[0]
        -   删除多个建议使用，从后往前删不会出错
            -   删除以后，后面的索引会前进一位，导致错误
-   修改元素、获取元素 => 使用 索引、切片 定位到元素操作
-   遍历使用 for 循环结合 range
-   排序：list01.sort()
    -   指定范围排序：
    ```python
    temp = list01[:4]
    temp.sort()
    list01[:4] = temp
    ```

```python
listtext = [1, 2, 3, "a", True, "d"]
listtext[:3] = ["x", "y", "z", "z", "f"] # ['x', 'y', 'z', 'z', 'f', 'a', True, 'd']
listtext[:3] = ["x"] # ['x', 'a', True, 'd']

# 正
for i in range(len(listtext)):
    print(listtext[i])
# 跳
for i in range(0, len(listtext), 2):
    print(listtext[i])
# 倒
for i in range(len(listtext) - 1, -1, -1):
    print(listtext[i])
```

**经典案例：找最大/小值**

```python
list_num = [23, 123, 44, 1, 4, 2, 55, 234, 123, 532, 23]

max_num = list_num[0]

for i in range(1, len(list_num)):
    if list_num[i] > max_num:
        max_num = list_num[i]
print(max_num)
```

##### 与字符串联系

1.  列表和字符串都是序列，元素之间有先后顺序。
2.  字符串时不可变的序列，列表可变。
3.  字符串中每个元素存储字符，而列表可以存储任意类型对象。
4.  列表和字符串都属于可迭代对象。
5.  关联：

    -   将多个字符串拼接为一个。
        -   result = "连接符".join(列表)
    -   将一个字符串拆分为多个。
        -   列表 = "a-b-c-d".split("分隔符")

    ```python
    # 此方式耗内存,耗能
    result = ""
    for item in range(10):
        result += str(item)
    print(result)

    # 最优解
    list_result = []
    for item in range(10):
        list_result.append(str(item))
    str_result = "-".join(list_result)
    print(str_result)
    ```

##### 列表操作原理（内存图）

```python
list01 = [0, 1]
list02 = list01
list01[0] = 100
print(list02[0])
```

**内存图**

![Python-MemoryAllocationMap-\_List01](http://localhost:9710/blog/Python/Python-MemoryAllocationMap-_List01.jpg)

```python
list01 = [0, 1]
list02 = list01
list01 = [100]
print(list02[0])
```

**内存图**

![Python-MemoryAllocationMap-\_List02](http://localhost:9710/blog/Python/Python-MemoryAllocationMap-_List02.jpg)

```python
list01 = [0, 1]
list02 = list01[:] # list02 = list01.copy()
list01[0] = 100
print(list02[0])
```

**内存图**

![Python-MemoryAllocationMap-\_List03](http://localhost:9710/blog/Python/Python-MemoryAllocationMap-_List03.jpg)

##### 深拷贝和浅拷贝

>   浅拷贝：在复制过程中，只复制一层变量。不会复制深层变量绑定的对象。
>   深拷贝：复制整个依赖的变量。

**浅拷贝内存图**

![Python-MemoryAllocationMap-\_List04](http://localhost:9710/blog/Python/Python-MemoryAllocationMap-_List04.jpg)

**深拷贝使用**

```python
import copy

list01 = [1, [2, 3]]
list02 = copy.deepcopy(list01)
list01[1][0] = 200
print(list02[1][0])
```

##### 列表推导式

1.  语法：

    -   变量 = [表达式 for 变量 in 可迭代对象]
    -   变量 = [表达式 for 变量 in 可迭代对象 if 条件]

2.  作用：使用简易方法，将可迭代对象转生成为列表

```python
# 示例
list01 = [2, 5, 6, 7, 8, 9]
# list02 = []
# for item in list01:
#     if not item % 2:
#         list02.append(item**2)
list03 = [item**2 for item in list01 if not item % 2]
```

3.  列表推导式嵌套

```python
list01 = ["a", "b", "c"]
list02 = ["A", "B", "C"]
list03 = []
# 全排列
# for i in list01:
#     for j in list02:
#         list03.append(i + j)
list03 = [i + j for i in list01 for j in list02]
print(list03)
```

##### 列表扩容原理

-   开辟更大的空间
-   将原数据拷贝过来
-   替换表头地址

![Python-MemoryAllocationMap-\_List05](http://localhost:9710/blog/Python/Python-MemoryAllocationMap-_List05.jpg)

#### 元组 tuple

> 由一系列变量组成的**不可变序列**容器。

1.  创建空元组：t01 = () / tuple()
2.  创建有默认值的元组：t01 = (1,2,3) / tuple("agasdg")
    -   (元素)
    -   tuple(可迭代对象)
3.  作用
    -   元组与列表都可以存储一系列变量。
    -   列表会预留内存空间，所以可以增加。
    -   元组会按需分配内存，建议变量数据固定时使用元组，因为通常占用空间小。
4.  如果元组只有一个元素必须在元素后加一个","，否则视为普通对象不是元组对象。

#### 字典 dict

![Python-MemoryAllocationMap-\_List06](http://localhost:9710/blog/Python/Python-MemoryAllocationMap-_List06.jpg)

1.  定义

-   由一系列键值对组成的可变映射容器。
-   映射(哈希算法)：通过键对应值，每条记录无序。
-   键必须唯一且不可变对象(字符串、数字、元组)，值没有限制。

2.  创建

-   创建空字典：d01 = {} / dict()
-   创建有默认值的字典：d01 = {"a": "A", "b": "B"} / dict([(1, 2), (3, 4)])
    -   {键1：值1，键2：值2}
    -   dict(可迭代对象)

3.  添加/修改：

-   字典[键] = 表达式
-   说明：键不存在则创建，键存在则修改。

4.  获取值：

-   字典[键]
-   说明：如果不存在键，则错误。
    -   在字典中读取元素，先判断存在，再读取

5.  删除：

-   del 字典[键]

6.  获取字典所有记录

```python
# for item in d01.items():
#     print(item[0]) # key
#     print(item[1]) # value

# 获取字典所有记录
for k, v in d01.items():
    print(k) # key
    print(v) # value

# 获取字典所有键
for k in d01.keys():
    print(k) # key

# 获取字典所有值
for v in d01.values():
    print(k) # key
```

7.  示例：

```python
input_string = input("")
d01 = {}
for item in input_string:
    if item not in d01:
        d01[item] = 1
    else:
        d01[item] += 1
print(d01)
```

8.  字典推导式

```python
dic01 = {}
# for item in range(10):
#     if not item % 2:
#         dic01[item] = item**2

dic01 = {item: item**2 for item in range(10) if not item % 2}
# 键值反转，值不重复
dic02 = {values: keys for keys, values in dic01.items()}
# 键值反转，值重复，不希望被覆盖
dic03 = [(values, keys) for keys, values in dic01.items()]
```

#### 集合 set

> 由一系列不重复的不可变类型变量组成的可变映射容器
> 相当于只有键没有值的字典

1.  创建

-   创建空集合：s01 = set()
-   创建有默认值的集合：s01 = {"a", "b"} / set("abcde")
    -   {元素1, 元素2}
    -   set(可迭代对象) => 其他容器 -> 集合 （去重）

2.  增：
    s01.add(元素)
3.  删：
    -   s01.remove(元素)
    -   s01.discard(元素) => `if 元素 in s01: s01.remove(元素)` (元素不存在不会报错)
4.  查：
    -   `for item in s01:`
    -   查找全部，只能使用 `for` , 不能修改 （原因是映射）
5.  运算：
    -   交集: s01 & s02
    -   并集: s01 | s02
    -   补集: s01 ^ s02
    -   对称补集: s01 - s02
    -   子集: s02 &lt; s01
    -   超集: s01 > s02
6.  集合推导式：
    -   {表达式 for item in s01 if 条件}

#### 固定集合 frozenset

> 不可变的集合 (没有预留空间，不能增，不能删)

-   创建空集合：frozenset()
-   创建具有默认值的集合：frozenset(可迭代对象)

#### 双层 for 循环

```python
for row in range(3):
    for column in range(5):
        print("*", end="")  # 在一行输出
    print()  # 换行

# ------- 提升
num = input("")
status = False
for i in range(len(num) - 1):
    for j in range(i + 1, len(num)):
        if num[i] == num[j]:
            status = True
if status:
    print("no")
else:
    print("yes")

# ------- 排序
list01 = [4, 2, 5, 1, 22, 59, 21, 42, 20]
for i in range(len(list01) - 1):
    for j in range(i + 1, len(list01)):
        if list01[i] < list01[j]:
            list01[i], list01[j] = list01[j], list01[i]
print(list01)
```

### 函数 function

1.  定义：用于封装一个特定的功能，表示一个功能或行为

    -   函数是可以重复执行的语句块，可以重复调用。

2.  作用：

    -   提高代码的可重用性，可维护性（代码层次结构更清晰）

3.  定义函数：

    1.  语法：

        ```python
        def 函数名(形参列表):
        函数体
        ```

    2.  说明：

        -   def 关键字，全称 define ，意为”定义”。
        -   函数名：对函数体中语句的描述，命名规则与变量相同，建议使用动词。
        -   形参列表：形式参数
            -   方法定义者 要求 调用者提供 的信息
        -   函数体：完成该功能的语句。

    3.  函数的第一行语句可以选择性的使用文档字符串存储函数与参数的说明。

4.  函数调用：

    -   语法：函数名(实参列表)
    -   说明：根据形参传递内容。

5.  返回值：

    1.  定义：方法定义者告诉调用者的结果。
    2.  语法：在方法体中，通过return关键字指明结果。
        -   return 结果
    3.  说明：
        -   不写 return 关键字，相当于返回 None
        -   return 还意味着方法结束。

**实例以及规范写法**

```python
def get_prime_number(star_num, end_num):
    """
        生成指定范围内的所有素数
    :param star_num: 开始
    :param end_num: 结束
    :return: 范围内的所有素数
    """
    list_prime_number = []
    for number in range(star_num, end_num + 1):
        if is_prime(number):
            list_prime_number.append(number)
    return list_prime_number


def is_prime(number):
    """
        判断是否为素数
    :param number: 需要判断的数
    :return: bool
    """
    if number < 2:
        return False
    for i in range(2, number):
        if number % i == 0:
            return False
    return True


print(get_prime_number(1, 101))
```

#### 函数传参：

1.  实参传递方式 argument

    1.  位置传参:
        -   实参与形参的位置依次对应
        -   序列传参
            -   实参用 \* 将序列拆解后与形参的位置依次对应
            -   可以在运行时，根据某些逻辑决定传入的数据（列表）
    2.  关键字传参：
        -   实参根据形参的名字进行对应 `fun01(a=1, b=2, c=3)`
        -   字典传参
            -   实参用 \*\* 将字典拆解与形参的名字进行对应。
            -   可以在运行时，根据某些逻辑决定传入的数据（字典）

2.  形参传递方式 parameter

    -   默认(缺省)参数：让调用者可以有选择性的传递需要的信息

        1.  语法：
            def 函数名(参数1 = 默认值1):
                函数体
        2.  说明：
            -   缺省参数必须自右至左依次存在，如果一个参数有默认值，则右侧所有参数必须都有默认值。
            -   缺省参数可以有0个或多个

    -   位置形参

        -   星号元组形参
            1.  语法：
                def 函数名(\*args):
                    函数体
            2.  作用：收集多余的位置形参
            3.  说明：
                -   一般命名为 \*args
                -   形参列表最多只能有一个
                -   在方法内部，就是元组
                -   对调用者而言，**可以传递数量无限的位置实参**

    -   命名关键字形参

        1.  语法：
            def 函数名(\_,参数名):
                函数体
            def 函数名(\_args,参数名):
                函数体
        2.  作用：强制实参使用关键字传递
        3.  双星号字典形参

            1.  语法：
                def 函数名(\*\*kwargs):
                    函数体
            2.  作用：收集多余的关键字传参
            3.  说明：
                -   一般命名 kwargs
                -   形参列表中最多只能有一个
                -   在方法内部，就是字典
                -   对调用者而言，**可以传递数量无限的关键字实参**

            ```python
            def keywords(**kwargs):
                print(kwargs)

            keywords(a="c", b=1, c=23)
            ```

3.  参数自左至右的顺序

    位置形参 --> 星号元组形参 --> 命名关键字形参 --> 双星号字典形参

4.  可变/不可变类型在传参时的区别：

    1.  不可变类型参数：
        -   数值型：整数、浮点数、复数
        -   布尔值 bool
        -   空值 None
        -   字符串 str
        -   元组  tuple
        -   固定集合 frozenset
    2.  可变类型参数：
        -   列表 list
        -   字典 dict
        -   集合 set
        -   字节数组 bytearray
    3.  传参说明：
        -   不可变类型的数据传参时，函数内部不会改变原数据的值。
        -   可变类型的数据传参时，函数内部可能改变原数据的值。
    4.  内存图：

        **不可变对象传参**

        ```python
        def func01(num01):
            num01 = 2
            print("num01: " + str(num01))

        number01 = 1
        # 调用方法在内存中开辟空间（栈帧）
        # 栈帧中定义该方法内部创建的变量
        # 方法执行完毕后，栈帧立即释放
        func01(number01)
        print("number01: " + str(number01))
        ```

        ![Python-MemoryAllocationMap-\_Fun01](http://localhost:9710/blog/Python/Python-MemoryAllocationMap-_Fun01.jpg)

        **可变对象传参**

        ```python
        def func02(list_target):
            list_target[0] = 2
            print("list_target[0]: " + str(list_target[0]))

        list_number = [1, 2]
        func02(list_number)
        print("list_number[0]: " + str(list_number[0]))
        ```

        ![Python-MemoryAllocationMap-\_Fun02](http://localhost:9710/blog/Python/Python-MemoryAllocationMap-_Fun02.jpg)

#### 作用域 LEGB

1.  作用域：变量起作用的范围

    -   Local 局部作用域：函数内部。
    -   Encolsing 外部嵌套作用域：函数嵌套
    -   Global 全局作用域：模块(.py)文件内部
    -   Builtins 内建模块作用域：builtins.py

2.  变量名查找规则：L -> E -> G -> B

3.  全局变量：

    -   定义在函数外部，模块(.py文件)内部的变量
    -   函数体内部可以访问，但是不能直接修改（需使用global语句声明才能修改）
    -   global 语句

        1.  作用：
            -   在函数内部修改全局变量
            -   在函数内部定义全局变量（全局声明）
        2.  语法：global 变量1，变量2
        3.  说明：
            -   在函数内部直接为全局变量赋值，视为创建新的局部变量
            -   不能先声明局部变量，再用 global 声明为全局变量

        ```python
        g01 = 100

        def fun():
            # 方法内部可以读取全局变量
            # print(g01)

            # 在方法内部创建了局部变量 g01,没有修改全局变量 g01
            # g01 = 300
            # print(g01)

            # 若需要在方法内部，修改全局变量
            global g01
            g01 = 400

            # 在局部作用域中，创建全局变量
            global g02
            g02 = 500

        fun()
        print(g01)
        print(g02)
        ```

4.  局部变量

    -   在方法体内部定义的变量
    -   调用函数时才被创建，函数结束后自动销毁
    -   在方法内部创建的变量，只能在方法内部使用
    -   局部变量可以访问外部嵌套变量,修改需要使用 `nonlocal` 关键字

    ```py
        def fun01():
            # 外部嵌套作用域
            a = 1

            def fun02():
                b = 2
                # 可以访问外部嵌套变量 a
                # 没有修改外部嵌套变量，而是创建了新的局部变量
                # a = 3
                # print(a)
                nonlocal a  # 生命外部嵌套变量 a
                a = 3
                print(a)

            fun02()
            print(a)

        fun01()
    ```

## 面向对象 Object Oriented

### 类和对象

#### 类和对象的浅析

1.  类：一个抽象的概念，即生活中的”类别”

    -   例如：学生、水果。

2.  对象：类的具体实例，即归属于某个类别的”个体”

    -   例如：张三同学、苹果

3.  类是创建对象的”模板”.

    -   数据成员：名词性的状态。例如：姓名
    -   方法成员：动词性的行为。例如：学习

4.  类与类的行为不同，对象与对象的数据不同。

#### 类和对象的用法

1.  定义类

    ```python
    class 类名:

    """文档说明"""

    def  __init__(self,参数列表):

    self.实例变量 = 参数
    ```

2.  方法成员

    -   类名所有单词首字母大写

    -   **init**叫做构造函数,创建对象时被调用，也可以省略。

    -   self变量绑定的是被创建的对象，名字通常叫做”self”。

#### 类和对象的语法

1.  创建对象(实例化对象)

    -   变量 = 构造函数(参数列表)
    -   构造函数的self参数会自动绑定对象地址，不用传递

2.  实例成员：
    1.  实例变量：
        -   语法：
            -   定义：对象地址.变量名称
            -   调用：对象地址.变量名称
        -   说明
            -   首次通过对象赋值为创建变量，再次赋值为修改。
                ```python
                w01 = Car()
                w01.name = "bm" # 创建
                w01.name = "bc" # 修改
                ```
            -   通常在构造函数(**init**)中创建.
                ```python
                w01 = Car("bm")
                w01.name = "bc" # 修改
                ```
            -   每个对象存储一份，通过对象地址访问。
            -   **dict**: 对象的属性，用于存储自身是实例变量的字典
                -   `w01.__dict__`
    2.  实例方法
        -   语法：
            -   定义：
                ```python
                def 方法名称(self,参数):
                    方法体
                ```
            -   调用：
                -   对象地址.实例方法名称(参数)
                    -   通过对象地址调用实例方法，会自动传递对象地址
                -   不建议的用法：
                    -   可以通过类名访问，并传递对象地址。
                    -   类名.实例方法名(对象地址,参数)
                    -   通过类名调用实例方法，需手动传递
            -   说明：
                -   至少有一个形参，用于绑定调用该方法的对象，一般命名为"self"。
                -   无论创建多少个对象，方法只有一份，且**实例方法被所有对象共享.**
                -   作用：表示对象的行为。
    3.  内存图

```python
class Car:
    # self 调用当前方法的对象
    def __init__(self, name, model, price):
        self.name = name
        self.model = model
        self.price = price

    def travel(self):
        print(self.name, "travel")

# 对象的地址
w01 = Car("bm", 12, 10000)
# 通过对象地址, 调用对象方法,会自动传递对象地址
w01.travel()
```

![Python-MemoryAllocationMap-\_OOP01](http://localhost:9710/blog/Python/Python-MemoryAllocationMap-_OOP01.jpg)

3.  类成员

    1.  类变量

        -   语法
            -   定义：在类中，方法外定义变量
            ```py
            class 类名:
                类变量名 = 数据
            ```
            -   调用：类名.类变量名
                -   不建议使用对象.类变量名.
            -   说明
                -   存储在类中。
                -   只有一份，被所有对象共享。
                -   描述所有对象的共有数据。

    2.  类方法

        -   语法
            -   定义
            ```py
            @classmethod
            def 方法名称(cls,参数):
                方法体
            ```
            -   调用：类名.方法名(参数)
                -   不建议使用对象.类方法名
            -   说明
                -   至少有一个形参，用于绑定调用该方法的类，一般命名为"cls"。
                -   使用@classmethod修饰的目的是调用方法时隐式传递类。
                -   类方法不能访问实例成员，实例方法可以访问类成员。
            -   作用：操作类变量

    3.  内存图

```py
class ICBC:
    moneys = 9999999

    @classmethod
    def print_total_money(cls):
        print(cls.moneys)

    def __init__(self, name, money):
        self.name = name
        self.money = money
        ICBC.moneys -= money


i01 = ICBC("zfzhihang", 100000)
# 调用类变量
# print(ICBC.moneys)
# 调用类方法，此时会自动传递类名进入方法
ICBC.print_total_money()

i02 = ICBC("cfzhihang", 100000)
ICBC.print_total_money()
```

![Python-MemoryAllocationMap-\_OOP02](http://localhost:9710/blog/Python/Python-MemoryAllocationMap-_OOP02.jpg)

4.  静态方法

    1.  语法
        -   定义：
            ```py
            @staticmethod
            def 方法名称(参数):
                方法体
            ```
        -   调用：类名.方法名称(参数)
            -   不建议使用对象.静态方法名.
        -   说明：
            -   使用 @staticmethod 修饰的目的是该方法不需要隐式传递参数。
            -   静态方法不能访问实例成员和类成员
        -   作用：
            -   统一管理函数(定义在 .py 文件中的函数)
            -   表达不需要使用实例成员和类成员时，使用静态方法。
            -   将函数转移到类中，即静态方法
    2.  实例

    ```py
        class Vector2:
            """
                向量
            """

            def __init__(self, x=0, y=0):
                self.x = x
                self.y = y

            @staticmethod
            def right():
                return Vector2(0, 1)

            @staticmethod
            def up():
                return Vector2(-1, 0)

        class DoubleListHelper:
            """
                二维列表助手:
                    在开发过程中，所有对二维列表的常用操作
            """

            @staticmethod
            def get_elements(list_target, v_pos, v_dir, count):
                get_list = []
                for i in range(count):
                    v_pos.x += v_dir.x
                    v_pos.y += v_dir.y
                    get_list.append(list_target[v_pos.x][v_pos.y])
                return get_list
        #
        # 测试
        # list01 = [
        #     ["00", "01", "02", "03"],
        #     ["10", "11", "12", "13"],
        #     ["20", "21", "22", "23"],
        #     ["30", "31", "32", "33"],
        # ]
        #
        # re = DoubleListHelper.get_elements(list01, Vector2(1, 0), Vector2.right(), 3)
        # print(re)
    ```

### 封装

封装数据的优势：
    \-   符合人类的思考方式，
    \-   将数据与对数据的操作封装起来。

#### 封装的定义

    1.  从数据角度讲：
        -   将一些基础变量复合为一个自定义类型。
        -   不但可以准确的描述事物，还可以体现该事物的行为。

    2.  从行为角度讲：
        -   向类外提供必要的功能，隐藏实现的细节。
        -   使用者可以不必操心实现过程。

    3.  从设计角度讲：

        -   分而治之
            -   将一个大的需求分解为许多类，让每个类处理一个独立的功能。
            -   优点：便于分工，便于复用，可扩展性强。
        -   封装变化
            -   需求可能会变化的功能要单独封装，避免影响其他类。
            -   模块化编程
        -   高内聚
            -   类中各个方法都在完成一项任务
        -   低耦合
            -   类与类的关联性与依赖度要低，让一个类变化，尽少影响其他的类。

#### 私有成员

1.  语法：命名使用双下划线开头
2.  作用：修改变量名，让外界”不能直接访问”
3.  本质：障眼法，也可以访问：`对象._类名__成员名`

```python
class Enemy:
    def __init__(self, name="", atk_speed=0):
        self.set_name(name)
        self.set_atk_speed(atk_speed)

    def get_name(self):
        return self.__name

    def set_name(self, value):
        self.__name = value

    def get_atk_speed(self):
        return self.__atk_speed

    def set_atk_speed(self, value):
        if 0 <= value <= 10:
            self.__atk_speed = value
        else:
            print("err")
```

#### 属性

> 公开的实例变量，缺少逻辑验证。私有的实例变量与两个公开的方法相结合，又显得调用者的操作略显复杂。而属性可以将两个方法的使用方式像操作变量一样方便

1.  定义

    ```python
    @property  # 读取数据时执行
    def 属性名(self):
    return  self.__ 属性名    # 私有的实例变量

    @属性名.setter  # 写入数据时执行
    def 属性名(self,参数):
    self.__ 属性名 = 参数
    ```

2.  调用：

    -   对象地址.属性名 = 数据
    -   变量 = 对象地址.属性名

3.  说明：

    -   通常两个公开属性，保护一个私有的变量。
    -   @property  负责处理读取逻辑
    -   @属性名.setter 负责处理写入逻辑

    ```python
    class Student:
        def __init__(self, name="", age=0):
            # self.set_name(name)
            self.name = name  # 调用 @name.setter 修饰的方法
            # self.set_age(age)
            self.age = age  # 调用 @age.setter 修饰的方法

        # def get_name(self):
        #     return self.__name

        # 本质：创建 property 对象， name 存储对象地址
        # 注意：创建对象时，会指定读取方法
        # 相当于：name = property(get_name， None)
        @property  # 拦截读取变量的操作
        def name(self):  # get_name()
            return self.__name

        @name.setter  # 拦截写入变量的操作 本质：name.setter(写入方法)
        def name(self, value):  # set_name()
            self.__name = value

        @property
        def age(self):
            return self.__age

        @age.setter
        def age(self, value):
            if 7 <= value <= 22:
                self.__age = value
            else:
                self.__age = 0
                print("err")
    ```

4.  属性本质

    ```python
    class Student:
        def __init__(self, name=""):
            self.name = name

        def get_name(self):
            print("get")
            return self.__name

        def set_name(self, value):
            print("set")
            self.__name = value
        # 拦截对变量 name 的读写操作
        # 创建 property 对象， name 存储的是对象地址
        # 注意：创建对象时，需要传递读写方法
        name = property(get_name, set_name)
    ```

5.  只读属性

    ```python
    class Student:
        def __init__(self, name=""):
            self.__name = name

        # 只读属性
        # 本质：创建 property 对象， name 存储对象地址
        # 注意：创建对象时，会指定读取方法
        # 相当于：name = property(get_name， None)
        @property
        def name(self):
            return self.__name
    ```

6.  只写属性

    ```python
    class Student:
        def __init__(self, name=""):
            self.name = name

        def set_name(self, value):
            self.__name = value

        name = property(None, set_name)

    s01 = Student("zfzf")
    print(s01._Student__name)
    ```

7.  拓展（只读属性返回可变对象时）

    ```python
    class Person:
        def __init__(self, name):
            self.name = name
            self.__skills = []

        def teach(self, person_other, str_skill):
            person_other.__skills.append(str_skill)
            print(self.name, person_other.name, str_skill)

        @property
        def name(self):
            return self.__name

        @name.setter
        def name(self, value):
            self.__name = value

        @property
        def skills(self):
            # 返回可变对象地址，意味着在类外仍可以操作可变对象
            # return self.__skills
            # 返回新的可变对象，意味着在类外操作的是新可变对象，不影响原对象
            return self.__skills[:]
            # 备注：每次通过切片返回的新对象，都会另外开辟空间创建新对象，占用过多内存
    ```

8.  `__slots__`

    -   作用：限制一个类创建的对象只能有固定的实例变量，不能再额外添加。
    -   语法：在类中定义 `__slots__ = (“变量名1”,”变量名2”)`
    -   说明
        -   含有`__slots__`属性的类所创建的对象没有`__dict__`属性。
        -   优点：防止因写错属性名称而发生的错误。
        -   缺点：丧失了动态语言可以在运行时为对象添加变量的灵活性。

### 继承

1.  语法：

    ```python
    class 子类名称(父类名称):
        def __init__(self,父类参数,自身参数):
            super().__init__(父类参数)
            self.自身实例变量 =自身参数
    ```

    1.  说明：

        -   子类如果没有构造函数，将自动执行父类的，但如果有构造函数将覆盖父类的。
        -   此时必须通过super()函数调用父类的构造函数，以确保父类数据成员被正常创建。

    2.  内置函数：

        -   isisntance（对象，类型）函数:返回对象是否兼容类型。
        -   issubclass（类型，类型元组）函数:返回类型是否兼容类型元组。
            -   第二个参数只有一个，不需要使用元组，直接写类型即可
            -   类型元组只要有一个兼容该类型即返回 True

2.  定义：重用现有类的功能与概念，并在此基础上进行扩展。

    -   重用现有类的功能，并在此基础上进行扩展 => 代码的复用

3.  说明：

    -   子类拥有父类所有成员：子类直接具有父类的成员，还可以具有自己的功能。
    -   事物具有一定的层次、渊源，继承可以统一概念。

4.  优势：

    -   一种代码的复用方式。
    -   以层次化的方式管理类。

5.  劣势：

    -   子类与父类耦合度高(父类构造函数/成员的变化，直接影响所有子类)
    -   切换不灵活

6.  作用：

    -   隔离客户端代码与功能的实现方式

7.  适用性

    -   多个类在概念上是一致的，且需要进行统一的处理。

8.  相关概念

    -   父类(基类/超类)、子类(派生类).

    -   父类相对于子类更抽象，使用范围更宽泛；

    -   子类相对于父类更具体，使用范围更狭小。

    -   单继承：父类只有一个(例如：java，c#,……)

    -   多继承：父类有多个(例如：python、c++)

    -   Object类(万类之祖)：任何类都直接或者间接继承自Object类。

#### 多继承

> 定义：一个子类继承两个或两个以上的基类，父类中的成员同时被子类继承下来。

**同名方法的解析顺序（MRO）：C3算法模拟的广度优先。**

-   类自身 --> 父类继承列表(由左至右)  --> 再上层父类

```py
    class A:
        def m(self):
            print("a--m")


    class B(A):
        def m(self):
            print("b--m")


    class C(A):
        def m(self):
            print("c--m")


    class D(B, C):
        def m(self):
            super().m()
            C.m(self) # 通过类名调用实例方法，传递对象地址
            print("d--m")


    d01 = D()
    d01.m()
    print(D.mro())
```

### 多态

1.  定义：

    -   调用父类同一个方法，但在不同子类间，有不同的表现。（设计思路：调用父类，执行子类）

2.  作用

    -   继承：将相关概念的共性进行抽象，多态：在共性的基础上，体现子类的个性化。
    -   增强程序扩展性，体现开闭原则。

3.  重写

    -   子类实现了父类中相同(名称、参数)的方法，在调用该方法时，实际执行的是子类的方法。

4.  内置可重写函数

> 在python中，以双下划线开头和结尾的函数--内建函数（系统定义的成员）。可以在自定义类中对其重写，从而改变其行为。

1.  对象转换为字符串：
    -   `__str__`:重写的时候，返回给人看的友好的支持。
        ```python
            def __str__(self):
                return "id is %d, name is %s, age is %d, score is %d" % (self.id, self.name, self.age, self.score)
        ```
    -   `__repr__`:重写的时候，返回解释器可以识别(eval)的字符串。
        ```python
            def __repr__(self):
                return 'StudentModel("%s", %d, %d)' % (self.name, self.age, self.score)
        ```
2.  算数运算符重载：让自定义类生成的对象（实例）能够使用运算符进行操作，本质就是调用内建函数

    ```py
      def __add__(self, other):
          return Vector(self.x + other)

      v01 = Vector(10)
      v02 = v01 + 5
      print(v02)
    ```

**算数运算符重载：(仅支持 对象在左边)**

| 方法名                   | 运算符和表达式 | 说明   |
| :----------------------- | :------------- | :----- |
| `__add__`(self,rhs)      | self + rhs     | 加     |
| `__sub__`(self,rhs)      | self - rhs     | 减     |
| `__mul__`(self,rhs)      | self \* rhs    | 乘     |
| `__truediv__`(self,rhs)  | self / rhs     | 除     |
| `__floordiv__`(self,rhs) | self // rhs    | 地板除 |
| `__mod__`(self,rhs)      | self % rhs     | 取余   |
| `__pow__`(self,rhs)      | self \*\* rhs  | 幂     |

**反向算数运算符重载：(仅支持 对象在右边)**

| 方法名                    | 运算符和表达式 | 说明   |
| :------------------------ | :------------- | :----- |
| `__radd__`(self,lhs)      | lhs + self     | 加     |
| `__rsub__`(self,lhs)      | lhs - self     | 减     |
| `__rmul__`(self,lhs)      | lhs \* self    | 乘     |
| `__rtruediv__`(self,lhs)  | lhs / self     | 除     |
| `__rfloordiv__`(self,lhs) | lhs // self    | 地板除 |
| `__rmod__`(self,lhs)      | lhs % self     | 取余   |
| `__rpow__`(self,lhs)      | lhs \*\* self  | 幂     |

**复合运算符重载：**

| 方法名                    | 运算符和表达式 | 说明   |
| :------------------------ | :------------- | :----- |
| `__iadd__`(self,rhs)      | self += rhs    | 加     |
| `__isub__`(self,rhs)      | self -= rhs    | 减     |
| `__imul__`(self,rhs)      | self \*= rhs   | 乘     |
| `__itruediv__`(self,rhs)  | self /= rhs    | 除     |
| `__ifloordiv__`(self,rhs) | self //= rhs   | 地板除 |
| `__imod__`(self,rhs)      | self %= rhs    | 取余   |
| `__ipow__`(self,rhs)      | self \*\*= rhs | 幂     |

```py
def __iadd__(self, other):
    self.x += other
    return self
```

**比较运算符重载：**

| 方法名             | 运算符和表达式 | 说明     |
| :----------------- | :------------- | :------- |
| `__lt__`(self,rhs) | self &lt; rhs  | 小于     |
| `__le__`(self,rhs) | self &lt;= rhs | 小于等于 |
| `__gt__`(self,rhs) | self > rhs     | 大于     |
| `__ge__`(self,rhs) | self >= rhs    | 大于等于 |
| `__eq__`(self,rhs) | self == rhs    | 等于     |
| `__ne__`(self,rhs) | self != rhs    | 不等于   |

### 类与类的关系

1.  泛化(继承)

    -   子类与父类的关系，概念的复用，耦合度最高；

    -   B类泛化A类，意味着B类是A类的子类。

    -   做法：B类继承A类。

    -   > 父类 向 子类 的演变 => 特化

2.  关联(聚合/组合)

    -   部分与这个整体的关系，耦合度要低于泛化。

    -   A类关联B类，意味着B是A的一部分。

    -   做法：在A类中包含B类的成员。

3.  依赖

    -   合作关系，耦合度最低。

    -   A类依赖B类，意味着A类的某个功能以依靠B类实现。

    -   做法：在A类的某个方法中，将B类作为参数。

**综合实例**

```python
  class Employee:
      """
          普通员工类 即 父类
      """

      def __init__(self, name, job):  # Employee 关联 job
          self.name = name
          self.job = job

      def calcular_salary(self):
          return self.job.get_salary()


  class Job:
      def __init__(self, basic_salary):
          self.basic_salary = basic_salary

      def get_salary(self):
          return self.basic_salary


  class Programer(Job):  # Programer 泛化 Job（隔离变化）
      """
          程序员类
      """

      def __init__(self, basic_salary, extra):
          super().__init__(basic_salary)
          self.extra = extra

      def get_salary(self):
          return super().get_salary() + self.extra


  class salesmen(Job):
      """
          销售类
      """

      def __init__(self, basic_salary, extra):
          super().__init__(basic_salary)
          self.extra = extra

      def get_salary(self):
          return super().get_salary() + self.extra


  lw = Employee("lw", Programer(100, 10))
  print(lw.calcular_salary())
  lw.job = salesmen(100, 1)
  print(lw.calcular_salary())
```

### 设计原则

1.  开闭原则(目标)

    -   对扩展开放，对修改关闭。
    -   允许增加新功能，不修改客户端(使用者)代码。

2.  类的单一职责(类的定义)

    -   一个类有且只有一个改变的原因。
    -   外界一个需求的变化，内部一个改变的类。

3.  依赖倒置

    -   客户端代码尽量依赖(使用)抽象(父)的组件。
    -   抽象的是稳定的，实现是多变的。
    -   使用抽象（父类），而不使用具体（子类）
    -   隔离 调用 与 定义

4.  组合复用原则(少用继承)

    -   如果仅仅为了代码复用优先选择组合关系，而非继承关系。
    -   组合的耦合度低于继承，灵活度高于继承。

5.  里氏替换(重写注意事项)

    -   父类出现的地方可以被子类替换，在替换后依然保持原有功能。
    -   从内存角度解释：父类(成员少)   子类(成员多)
    -   子类在重写父类方法时，尽量选择扩展重写(先调父类同名方法)，不要改变原有功能。

6.  迪米特法则(低耦合)

    -   原话：不要和陌生人说话。
    -   类与类交互时，在满足功能的基础上，传递的数据量越少越好。

**总结：**

1.  主要思想：封装变化、隔离变化、执行变化
2.  三大特性：
    -   封装：分而治之、封装变化、高内聚、低耦合。
    -   继承：重用现有类功能和概念，在此基础上进行扩展。
    -   多态：继承体现的是共性，多态体现的是个性。
3.  三大关系：
    -   泛化：父子关系
    -   关联：成员关系
    -   依赖：协作关系(做成参数)
4.  六大原则：
    -   开闭原则：对扩展开放，对修改关闭。
    -   单一原则：一个类有且只有一个改变的原因。
    -   依赖倒置：使用抽象(父类),不使用具体(子类).**抽象不依赖于具体.**
    -   组合复用：使用关联关系，不使用泛化关系。（是一种，有一个）
    -   里氏替换：父类出现的地方，可以被子类替换，替换后可以保持原功能。
    -   迪米特法则：低耦合。
5.  优势：通俗的讲，灵活。
    -   高复用：没有重复的代码。
    -   高扩展：开闭原则。
    -   高维护：逻辑清晰，结构规整。

## 模块 Module

1.  定义：包含一系列代码(数据/函数/类/语句)的文件，通常以.py结尾。
2.  作用：让一些相关的代码，有逻辑的组织在一起，使结构更加清晰，有利于团队开发。
3.  导入：

    -   import

        1.  语法：`import 模块名 [as 别名]`
            -   将该模块作用域 赋值给 变量：模块名
        2.  作用：将某个模块整体导入到当前模块
        3.  本质：使用变量名(模块名)关联指定模块代码。
        4.  使用：模块名.成员

    -   from import

        1.  语法：`from 模块名 import 成员 [as 别名]`
            -   将该模块指定成员 赋值给 变量：成员（多个成员逗号隔开）
        2.  作用：将模块内的指定的成员导入到当前模块作用域中。
        3.  使用：成员
        4.  特殊：模块名 用 `.` 代替表示相对导入，当前包导入

    -   from import \*

        1.  语法：`from 模块名 import *`
        2.  作用：将模块内的所有成员导入到当前模块作用域中。
        3.  注意：
            -   小心重名
            -   模块中以下划线(\_)开头的属性，不会被导入，通常称这些成员为隐藏成员。

### 模块变量

-   `__all__`变量：定义可导出成员，仅对 `from xxx import *` 语句有效
    -   `__all__ = ["fun01", "Class01"]`
-   `__doc__`变量：读取文档字符串（`"""`注释是文档字符串）
    -   使用：第一种导入语法 可以用 该模块变量 导出 文档字符串
-   `__file__`变量：模块对应的文件路径名
-   `__name__`变量：模块自身名字，可以判断是否为主模块。
    -   当此模块作为主模块（第一个运行的模块）运行时，`__name__`绑定`__main__`,不是主模块，而是被其他模块导入时，存储模块名。
    -   作用：在if代码块中定义只能从当前模块开始执行，才调用的代码。
    -   `if __name__ == "__main__":`
    -   `测试代码`

### 加载过程

> 在模块导入时，模块的所有语句都会执行。
> 如果一个模块已经导入，则再次导入时不再重复执行语句。

### 分类

1.  内置模块(builtins): 在解释器内部可以直接使用。

2.  标准库模块：安装python时自带的，经过导入后可直接使用。

    ```py
        # 标准库模块 - 时间
        import time

        # 返回时间戳（1970年后经过的浮点秒数）
        print(time.time())
        # 时间戳 -> 时间元组（ 年 月 日 时 分 秒 星期 一年的第几天 夏令时 ）
        print(time.localtime(1590313878.4446368))
        # 时间元组 -> 时间戳
        print(time.mktime(time.localtime()))
        # 时间元组 -> 字符串 （时间的格式化）
        print(time.strftime("%Y %m %d %H:%M:%S", time.localtime()))
        # 字符串 -> 时间元组
        print(time.strptime("2020 05 26", "%Y %m %d"))
    ```

3.  第三方模块：需要自行下载，再导入使用。

4.  自定义模块：经过导入后可直接使用。

### 包 package

1.  Python 程序结构

    ```py
        包
            模块
                类
                    函数()/方法()
                        语句
    ```

2.  定义: 将模块以文件夹的形式进行分组管理。

    -   必须包含`__init__`.py

3.  作用: 让一些相关的模块有逻辑的组织在一起。

4.  使用:

    1.  导入：`import 包.模块`, 使用：`包.模块.成员`
    2.  导入：`from 包 import 模块`, 使用：`模块.成员`
    3.  导入：`from 包 import *` , 使用：`模块.成员`
        -   必须在`__init__.py`文件中，定义`__all__ = [“模块名”]`
    4.  导入：`from 包.模块 import */成员` , 使用：`成员` （推荐）

5.  搜索顺序

    -   搜索内建模块(builtins)

    -   sys.path 提供的路径

        -   第一个路径是运行模块所在路径

        -   如果希望从项目其他模块运行，那么必须在pycharm中设置项目根目录。

        -   `sys.path.append('url')`

### 异常处理

#### 异常

1.  定义：运行时检测到的错误。

2.  现象：当异常发生时，程序不再向下继续执行，而转到函数调用的语句，直到遇到异常处理语句。

3.  常见异常：

    -   名称异常（NameError）：变量未定义

    -   类型异常（TypeError）：不用类型数据进行计算

    -   索引异常（IndexError）：索引越界

    -   属性异常（AttributeError）:对象没有相应属性

    -   未实现异常（NotImplementedError）: 尚未实现的方法

    -   键异常（KeyError）：没有对应名称的键

    -   值异常（ValueError）：值错误

    -   异常基类（Exception）

#### 处理

1.  语法：

    ```py
        try:

            可能发生错误的代码

        except 异常类型1 [as 变量名]:

             处理逻辑1

        except 异常类型2 [as 变量名]:

             处理逻辑2
        else:

            未发生异常执行的代码
        finally:

            无论是否异常，一定执行的代码
    ```

2.  作用：将程序由异常状态转换为正常流程

3.  说明：

    -   as 子句：用于绑定错误对象的变量，可省略
    -   except 子句可以有一个或多个，用来捕获某种类型的错误
    -   else 子句最多只能有一个
    -   finally 子句最多只能有一个，如果没 except 子句必须存在，如有异常没有被捕获到，会向上层（调用处）继续传递，直到程序终止运行。

```py
    def get_score():
        while True:
            try:
                num = int(input("score:"))
            except:
                print("err: input int")
                continue
            if 1 <= num <= 100:
                return num

            print("err: 1-100")
```

#### raise 语句

1.  作用：抛出一个错误，让程序进入异常状态。

2.  目的：在程序调用层数较深时，向主程序传递错误信息要层层return比较麻烦，所以建议使用人为抛出异常。

#### 自定义异常类

1.  作用：封装需要传递的错误信息。

2.  定义：

    ```py
        class 类名Error(Exception):
            def __init__(self,参数):
                super().__init__(参数)
                self.数据 = 参数
    ```

3.  调用：

    ```py
        try:
            ...
            raise 自定义异常类名（参数）
            ...
        except 自定义异常类 as 变量名：
            变量名.数据
    ```

### 迭代

> 每一次对过程的重复称之为”迭代一次”，而每一次迭代得到的结果会作为下一次迭代的初始值。例如：循环获取容器中的元素。

#### 可迭代对象 iterable

> 可迭代对象：具有 `__iter__()`方法，可以返回迭代器的对象
> 能被for循环的条件：可迭代对象（具有`__iter__()`方法的对象）

1.  语法：

    -   创建：
        ```py
            class 可迭代对象类名:
                def __iter__(self):
                    return 迭代器
        ```
    -   使用：
        ```py
            for 变量名 in 可迭代对象:
                语句
        ```

2.  原理：

```py
    # for 循环原理
    # 1.  获取迭代器对象
    iterator = list01.__iter__()

    while True:
        try:  # 如果获取所有元素，则执行 except
            # 2.  获取下一个元素（迭代过程：调用迭代器的__next__方法）
            item = iterator.__next__()
            # 3.  停止迭代：捕获 StopIteration 异常
        except StopIteration:
            break  # 跳出循环
```

#### 迭代器 iterator

> 能够被`__next__()`方法的调用并返回下一个值的对象（具有`__next__()`函数的对象，可以获取下一个元素。 ）

1.  语法：

    ```py
        class 迭代器名称:
            def __init__(self,聚合对象):
                self.聚合对象 = 聚合对象
            def __next__():
                if 没有元素:
                    raise StopIteration()  # 停止迭代
                return 聚合对象元素  #聚合对象通常是容器对象
    ```

2.  作用：使用者只需要通过一种方式(`__next__`)，便可简单明了的获取聚合对象中的各个元素，无需了解其内部结构。

3.  原理：

    ```py
        class Skill:
            pass

        class SkillIterator:
            def __init__(self, target):
                self.target = target
                self.index = 0

            def __next__(self):
                # 索引越界抛出异常
                if self.index >= len(self.target):
                    raise StopIteration()

                # 返回下一个元素
                item = self.target[self.index]
                self.index += 1
                return item

        class SkillManager:
            def __init__(self, skills):
                self.skills = skills

            def __iter__(self):
                # 创建迭代器对象 传递 需要迭代的数据
                return SkillIterator(self.skills)

        manager = SkillManager([Skill(), Skill(), Skill()])

        for item in manager:  # 获取 manager 对象中集合（聚合）类型对象元素
            print(item)

        # iterator = manager.__iter__()

        # while True:
        #     try:
        #         item = iterator.__next__()
        #         print(item)
        #     except:
        #         break
    ```

4.  设计思想：

    ![Python-MemoryAllocationMap-\_OOP03](http://localhost:9710/blog/Python/Python-MemoryAllocationMap-_OOP03.jpg)

### 生成器 generator

1.  定义：能够动态(循环一次计算一次返回一次)提供数据的可迭代对象。

2.  作用：在循环过程中，按照某种算法推算数据，不必创建容器存储完整的结果，从而节省内存空间。数据量越大，优势越明显。

3.  惰性(延迟)操作：通俗的讲在需要的时候才计算结果。

#### 生成器函数

1.  定义：含有 yield 关键字的函数，返回值是生成器对象。

2.  语法：

    -   创建

        ```py
            def 方法名():
             …
             yield 数据
             …
        ```

    -   使用

        ```py
            for 变量名 in 方法名():

                语句
        ```

3.  本质

    -   创建

        ```py
        class 生成器名称:
            def __iter__(self):
                return 迭代器对象
        ```

    -   使用

        ```py
            生成器变量名 = 生成器名称()

            迭代器变量名 =变量名.__iter__()

            while True:

                try:

                    print(迭代器变量名.__next__())

                except:

                    break
        ```

4.  语法说明：

    -   调用生成器函数返回迭代器对象，不执行函数体。
    -   执行过程：
        1.  调用生成器函数，自动创建迭代器对象。
        2.  调用迭代器对象的`__next__()`方法才执行生成器函数体。
        3.  每次执行到yield关键字时返回数据，暂时离开。
        4.  待下次调用`__next__()`方法继续执行。
        5.  再次调用 next 方法时，从上次离开的代码开始执行，到 yield 语句暂时离开
        6.  待执行完方法体，抛出 StopIteration 异常

5.  生成迭代器对象原理：

    -   将yield关键字以前的代码放到`__next__()`方法中。
    -   将yield关键字以后的数据作为`__next__()`方法返回值。

```py
    # class MyRange:
    #     def __init__(self, stop):
    #         self.stop = stop

    #     def __iter__(self):
    #         index = 0
    #         while True:
    #             if index < self.stop:
    #                 yield index
    #                 index += 1

    def my_range(stop):
    index = 0
    while True:
        if index < stop:
            yield index
            index += 1

    for item in my_range(5):
    print(item)
```

```py
    def get_even(list_target):
        for item in list_target:
            if item % 2 == 0:
                yield item

    for item in get_even(list01):
        print(item)
    # 方法/函数需要向外返回多个结果时，使用生成器函数
```

#### 内置生成器

1.  枚举函数

    -   语法：

        ```py
            for 变量 in enumerate(可迭代对象)：
                语句
            for (索引，元素) in enumerate(可迭代对象)：
                语句
        ```

    -   作用：遍历可迭代对象时，可以将索引与元素组合为一个元组。

2.  zip

    -   语法：

        ```py
            for item in zip(可迭代对象1,可迭代对象2 ...):
                语句
        ```

    -   作用：将多个可迭代对象中对应的元素组合成一个个元组，生成元组的个数由长度最短的可迭代对象决定

#### 生成器表达式

    1.  语法：(表达式 for 变量 in 可迭代对象 [if 真值表达式])

    2.  定义：用推导式语法创建的生成器对象。

    ```py
        # def fun(list01):
        #     for item in list01:
        #         yield item ** 2

        # for item in fun(list01):
        #     print(item)

        list02 = (item ** 2 for item in list01)
        for item in list02:
            print(item)
    ```

### 函数式编程

1.  定义：用一系列函数解决问题。
    -   函数可以赋值给变量，赋值后变量绑定函数。
    -   允许将函数作为参数传入另一个函数。
    -   允许函数返回一个函数。
2.  高阶函数：将函数作为参数或返回值的函数。

#### 函数作为参数

> 将核心逻辑传入方法体，使该方法的适用性更广，体现了面向对象的开闭原则。

```py
    def fun01():
        print("fun01")

    # 将方法作为方法的参数进行传递

    def fun02(func):
        # 对于 fun02 的定义者而言，不知道也不需要知道func的具体逻辑
        print("fun02")
        func()

    # 调用 fun01 ，并将返回值 赋值给 变量a
    # a = fun01()
    # 将函数赋值给 变量a（没有执行函数）
    # a = fun01
    # a()  # 调用变量a，间接执行 函数fun01

    fun02(fun01)

    def find_list(target, condition):
        for item in target:
            # 使用形参 condition 将不变与变化隔离开
            if condition(item):
                yield item

    print(list(find_list(list01, lambda item: item > 5)))
    # 生成器直接转换成列表 使用 set() 可以去重...
```

##### lambda 表达式

-   定义：是一种匿名方法。

-   作用：作为参数传递时语法简洁，优雅，代码可读性强。随时创建和销毁，减少程序耦合度。

-   语法

    -   定义：

        ```py
            变量 = lambda 形参: 方法体
        ```

    -   调用：变量(实参)

    -   说明：

        -   形参没有可以不填
        -   方法体只能有一条语句，且不支持赋值语句。

##### 内置高阶函数

-   map（函数，可迭代对象）：使用可迭代对象中的每个元素调用函数，将返回值作为新可迭代对象元素；返回值为新可迭代对象。（ListHelp.select)

-   filter(函数，可迭代对象)：根据条件筛选可迭代对象中的元素，返回值为新可迭代对象。（ListHelp.find_all）

-   sorted(可迭代对象，key = 函数,reverse = bool值)：排序，返回值为排序结果。

-   max(可迭代对象，key = 函数)：根据函数获取可迭代对象的最大值。

-   min(可迭代对象，key = 函数)：根据函数获取可迭代对象的最小值。

#### 函数作为返回值

> 逻辑连续，当内部函数被调用时，不脱离当前的逻辑。

##### 闭包

1.  三要素：

    -   必须有一个内嵌函数。

    -   内嵌函数必须引用外部函数中变量。

    -   外部函数返回值必须是内嵌函数。

2.  定义： 在一个函数内部的函数,同时内部函数又引用了外部函数的变量。

3.  本质： 闭包是将内部函数和外部函数的执行环境绑定在一起的对象。

4.  优点：

    -   内部函数可以使用外部变量。
    -   闭包使逻辑连续

5.  缺点： 外部变量一直存在于内存中，不会在调用结束后释放，占用内存。

6.  作用：实现python装饰器

```py
    def fun01():
        print("fun01")
        a = 1

        def fun02():
            print("fun02")
            print(a)

        return fun02

    result = fun01()
    # 调用内部函数（内部函数使用了外部变量 - 闭包）
    result()  # 可以使用外部函数说明外部函数在调用后，没有释放
```

##### 函数装饰器 decorators

1.  定义： 在不改变原函数的调用以及内部代码情况下，为其添加新功能的函数。

2.  语法：

    ```py
        def 函数装饰器名称(func):

            def 内嵌函数(*args, **kwargs):

                需要添加的新功能

                return func(*args, **kwargs)

        return 内嵌函数
    ```

```py
    def print_func_name(func):
        # 包装修旧功能
        def wrapper(*args, **kwargs):
            # 增加新功能
            print(func.__name__)
            # 旧功能
            return func(*args, **kwargs)

        return wrapper  # 返回包装器

    @print_func_name  # 原理 say_hello = print_func_name(say_hello)
    def say_hello():
        print("say hello")
        return 123

    print(say_hello())
```

3.  本质： 使用 `@函数装饰器名称` 修饰原函数，等同于： `原函数名称 = 函数装饰器名称（原函数名称）` 创建与原函数名称相同的变量，关联内嵌函数；故调用原函数时执行内嵌函数。

4.  装饰器链： 一个函数可以被多个装饰器修饰，执行顺序为从近到远

5.  实战

    ```py
        import time

        def print_execute_time(func):
            def wrapper(*args, **kwargs):
                old_time = time.time()
                result = func(*args, **kwargs)
                time.time() - old_time
                print(time.time() - old_time)
                return result

            return wrapper

        class Student:
            def __init__(self, name):
                self.name = name

            @print_execute_time
            def study(self):
                print("study")
                time.sleep(2)

        s01 = Student("zf")
        s01.study()
    ```

## 数据结构

1.  数据：信息的载体，能输入到计算机中并且能被计算机识别/存储/处理的符号总称。
2.  数据元素：数据的基本单位，又称记录（Record）, 一般数据元素由若干基本项（段/域/属性）组成
3.  数据结构：数据元素及数据元素之间的关系，或组成数据的形式。

### 数据结构之间的关系

1.  逻辑结构

    -   数据之间的抽象关系（邻接关系/从属关系），按每个元素可能具有的直接前趋数和直接后趋数将逻辑结构分为 线性结构 和 非线性结构。
    -   与计算机无关，对现实事物进行数学描述

2.  存储结构

    -   逻辑结构在计算机中的具体实现方式，分为顺序存储方法，链接存储方法，索引存储方法，散列存储方法
    -   计算机中组成数据的数据元素按 xx 结构存储。

#### 逻辑结构

1.  特点：

    -   只是描述数据结构中数据元素之间的联系规律
    -   从具体问题抽象出来的数学模型，是独立计算机存储器的（与机器无关）。

2.  逻辑结构分类

    -   线性结构（n个数据元素的有序集合体）
        -   集合中必须存在唯一的 一个 "第一个元素"
        -   集合中必须存在唯一的 一个 "最后的元素"
        -   除最后元素之外，其他数据元素均具有唯一的 "后继"
        -   除第一个元素之外，其他数据元素均具有唯一的 "前趋"
        -   即一个挨着一个
    -   树形结构（层次结构）
        -   数据元素间存在着 一对多 树形关系的数据结构。是一类重要的非线性数据结构。
        -   在树形结构中，树根节点没有前趋节点，其余每个节点有且只有一个前趋节点，子叶节点没有后继节点，其余每个节点的后续节点数可以是一个也可以是多个
    -   图状结构（网状结构）
        -   在图状结构中任意两个数据元素间都可能存在关系。（多对多）
    -   其他结构

#### 存储结构

1.  特点

    -   数据的逻辑结构在计算机存储器中的映象（表示）
    -   存储结构是通过计算机程序实现的，故依赖具体的计算机语言

2.  分类

    -   顺序存储
        -   将数据结构中各元素按照其逻辑顺序存放于存储器一片连续的存储空间中
    -   链式存储
        -   将数据结构中的各元素分布到存储器的不同点，用记录下一个结点位置的方式建立他们之间的联系
    -   索引存储
        -   存储数据的同时，建立一个附加的索引表，即索引存储结构 = 数据文件 + 索引表
    -   散列存储（hash）
        -   根据数据元素的特殊字段（关键字 key），计算数据元素的存放地址，然后数据元素按地址存放

3.  顺序结构 和 链式结构
    -   顺序结构适合遍历，不适合插入修改
    -   链式结构适合插入操作/大量数据的分散存储，但遍历操作稍微复杂

### 线性表

> 线性表的定义：描述其逻辑结构，通常会在线性表上进行的查找/插入/删除等操作。
>
> 线性表作为一种基本的数据结构类型，在计算机存储器中的映象一般有两种形式：顺序映象/链式映象

#### 线性表的顺序存储结构

1.  定义：将线性表的 L = （a0,a1,...an-1）中的各元素依次存储与计算机一片连续的存储空间
2.  特点：
    -   逻辑上相邻的元素，存储位置也相邻
    -   存储密度高，便于对数据的遍历查找
    -   对表的 插入/删除 等的运算效率较差
3.  程序实现：list 存放一片单一连续的内存块，list 即可描述顺序存储结构

#### 线性表的链式存储结构

1.  定义：将线性表的 L = （a0,a1,...an-1）中的各元素分布在存储器的不同存储块，称为结点，每个结点（尾部节点除外）都持有一个指向下一个结点的引用。
2.  特点：
    -   逻辑上相邻的元素，存储位置不一定相邻。
    -   存储稀疏，不必开辟整块存储空间。
    -   对表的 插入/删除 等的运算效率较高
    -   逻辑结构复杂不利于遍历
3.  程序实现:

    ```py
        class Node(object):
            def __init__(self, val, next=None):
                self.val = val
                self.next = next

        class LinkList(object):

            def __init__(self):
                self.head = Node(None)

            def init_link(self, data):
                """
                    初始化链式表
                @param data:
                @return:
                """
                p = self.head  # 可移动变量
                for i in data:
                    p.next = Node(i)
                    p = p.next

            def show(self):
                """
                    控制台输出链表
                @return:
                """
                p = self.head.next
                while p:
                    print(p.val, end=" ")
                    p = p.next
                print()

            def append(self, val):
                """
                    尾部插入新结点
                @param val:
                @return:
                """
                p = self.head
                while p.next:
                    p = p.next
                p.next = Node(val)

            def get_length(self):
                """
                    获取链表长度
                @return:
                """
                n = 0
                p = self.head
                while p.next:
                    n += 1
                    p = p.next
                return n

            def is_empty(self):
                """
                    链表是否为空
                @return:
                """
                if self.get_length():
                    return False
                return True

            def clear(self):
                """
                    清空链表
                @return:
                """
                self.head.next = None

            def get_item(self, index):
                """
                    获取索引值
                @param index:
                @return:
                """
                p = self.head.next
                n = 0

                while p:
                    if index >= self.get_length() or index < 0:
                        raise IndexError("list index out of range")
                    elif index == n:
                        return p
                    p = p.next
                    n += 1
                # while n < index and p:
                #     n += 1
                #     p = p.next
                # if not p:
                #     raise IndexError("list index out of range")
                # return p

            def insert(self, index, val):
                """
                    根据索引位置插入数据(前插)
                @param index:需要插入的索引位置
                @param val:需要插入的数值
                @return:
                """
                if index < 0 or index > self.get_length():
                    raise IndexError("list index out of range")
                p = self.head
                i = 0
                while i < index:
                    p = p.next
                    i += 1
                node_val = Node(val)
                node_val.next = p.next
                p.next = node_val
                # node_val = Node(val)
                # node_val.next = self.get_item(index) if index != self.get_length() else None
                # if index == 0:
                #     self.head.next = node_val
                # else:
                #     self.get_item(index - 1).next = node_val

            def delete(self, val):
                p = self.head
                # while p.next.val != val:
                #     if not p.next:
                #         raise IndexError("list index out of range")
                #     p = p.next
                # p.next = p.next.next
                while p.next:
                    if p.next.val == val:
                        p.next = p.next.next
                        break
                    p = p.next
                else:
                    raise ValueError("x not in list")
    ```

### 栈和队列

#### 栈

1.  定义：栈是限制在一端进行插入操作和删除操作的线性表（堆栈），允许进行操作的一端栈顶，另一固定端为栈底，栈中没有元素称之为 空栈。

2.  特点：

    -   只能在一端进行数据操作
    -   栈模型具有后进先出（的规律）

        ![Python-Net_stack](http://localhost:9710/blog/Python/Python-Net_stack.jpg)

3.  程序实现：

    -   栈的顺序存储结构

        ```py
            class StackError(Exception):
                pass

            class SequenceStack(object):
                """
                    栈的顺序存储结构
                """

                def __init__(self):
                    self._elements = []

                def top(self):
                    if not self._elements:
                        raise StackError("Stack is empty")
                    return self._elements[-1]

                def is_empty(self):
                    return self._elements == []

                def push(self, val):
                    """
                        入栈
                    @param val:
                    @return:
                    """
                    self._elements.append(val)

                def pop(self):
                    """
                        出栈
                    @return:
                    """
                    if not self._elements:
                        raise StackError("Stack is empty")
                    return self._elements.pop()
        ```

        -   应用：一段文字的括号是否匹配

            ```py
                parens = "()[]{}"
                left_paren = "([{"
                # 使用字典做判断的介质
                opposite = {")": "(", "]": "[", "}": "{"}

                def match_parent(text):
                    i, text_len = 0, len(text)
                    while True:
                        while i < text_len and text[i] not in parens:
                            i += 1
                        if i >= text_len:
                            return
                        else:
                            yield text[i], i
                            i += 1

                def verify(data):
                    stack = SequenceStack()
                    for item, index in match_parent(data):
                        if item in left_paren:
                            stack.push((item, index))
                        elif stack.is_empty() or opposite[item] != stack.pop()[0]:
                            return "Unmatching is found at %d for %s" % (index, item)
                    else:
                        if stack.is_empty():
                            return "All parentheses are matched"
                        else:
                            e = stack.pop()
                            return "Unmatching is found at %d for %s" % (e[1], e[0])

                # def is_match(left_bracket, right_bracket):
                #     if left_bracket == "(" and right_bracket == ")":
                #         return True
                #     elif left_bracket == "[" and right_bracket == "]":
                #         return True
                #     elif left_bracket == "{" and right_bracket == "}":
                #         return True
                #     else:
                #         return False
                #
                # def detection(data):
                #     stack = SequenceStack()
                #     left_brackets = ["(", "[", "{"]
                #     right_brackets = [")", "]", "}"]
                #     for item in data:
                #         if item in left_brackets:
                #             stack.push(item)
                #         if item in right_brackets:
                #             if not is_match(stack.pop(), item):
                #                 return "error"
                #     if stack.is_empty():
                #         return "ok"
                #     return "error"
            ```

    -   栈的链式存储结构

        ```py
            class LinkStack(object):
                """
                    栈的链式存储结构
                """

                def __init__(self):
                    # 标记栈顶位置
                    self._top = None

                def is_empty(self):
                    """
                        非空验证
                    @return:
                    """
                    return not self._top

                def push(self, val):
                    """
                        入栈
                    @param val:
                    @return:
                    """
                    self._top = Node(val, self._top)
                    # node_val = Node(val)
                    # node_val.next = self._top
                    # self._top = node_val

                def pop(self):
                    """
                        弹栈
                    @return:
                    """
                    if not self._top:
                        raise StackError("stack is empty")
                    p = self._top
                    self._top = p.next
                    return p.val

                def get_top(self):
                    """
                        获取栈顶元素值
                    @return:
                    """
                    if self.is_empty():
                        raise StackError("stack is empty")
                    return self._top.val
        ```

        -   应用：逆波兰表达式

            ```py
                while True:
                    str_expression = input("dc: ")
                    list_exp_element = str_expression.split(" ")
                    for item in list_exp_element:
                        if item == "p":
                            break
                        elif item not in "+-":
                            stack.push(item)
                        else:
                            first_pop = stack.pop()
                            last_pop = stack.pop()
                            stack.push(str(eval("%s%s%s" % (last_pop, item, first_pop))))

                    print(stack.get_top())
                    stack.clear()
            ```

#### 队列

1.  定义：队列是限制在两端进行插入操作和删除操作的线性表，允许进行存入操作的一端称为"队尾"， 允许进行删除操作的一端称为"队头"。

2.  特点：

    -   队列只能在队头和队尾进行数据操作。
    -   栈模型具有先进先出/后进后出的规律

        ![Python-Net_queue](http://localhost:9710/blog/Python/Python-Net_queue.jpg)

3.  程序实现

    -   队列的顺序存储结构

        ```py
            class SequenceQueue:
                """
                    队列的顺序存储结构
                """

                def __init__(self):
                    self._element = []

                def is_empty(self):
                    return self._element == []

                def append(self, val):
                    self._element.append(val)

                def pop(self):
                    if not self._element:
                        raise QueueError("Queue is empty")
                    return self._element.pop(0)
        ```

    -   队列的链式存储结构

        ```py
            class LinkQueue:
                """
                    队列的链式存储结构
                        左侧对头，右侧队尾
                """

                def __init__(self):
                    # Node(None) 可以是任意结点（无任何作用），一整条队列
                    # front 的作用 类似一个指针，指向队尾的前一个结点
                    self._front = self._rear = Node(None)

                def is_empty(self):
                    """
                        非空验证
                    @return:
                    """
                    return self._front is self._rear

                def append(self, val):
                    """
                        入队
                    @param val:
                    @return:
                    """
                    self._rear.next = Node(val)
                    self._rear = self._rear.next

                def pop(self):
                    """
                        出队
                    @return:
                    """
                    if self._front is self._rear:
                        raise QueueError("Queue is empty")
                    self._front = self._front.next
                    return self._front.val

                def clear(self):
                    self._front = self._rear
        ```

### 树形结构

1.  定义：

    -   树（Tree）是n（n>=0）个节点的有限集合T，它满足两个条件：有且仅有一个特定的称为根（Root）的节点，其余的节点可以分为m（m>=0）个互不相交的有限集合T1.T2 ... .Tm,其中每一个集合又是一棵树，并称为其根的子树（Subtree）。

        ![Python-Net_tree01](http://localhost:9710/blog/Python/Python-Net_tree01.jpg)

2.  基本概念：

    -   一个节点的子树的个数称为该节点的度数（多少个分叉），一棵树的度数是指该树中节点的最大度数。
    -   度数为零的节点称为树叶或终端节点，度数不为零的节点称为分支节点，除根节点外的分支节点称为内部节点。
    -   一个节点的子树之根节点称为该节点的子节点，该节点称为它们的父节点，同一节点的各个子节点之间称为兄弟节点。一棵树的根节点没有父节点，叶节点没有子节点。
    -   一个节点系列k1,k2,...ki,ki+1,...kj，并满足ki是ki+1的父节点，就称为一条从k1到kj的路径，路径的长度为j-1，即路径中的边数，路径中前面的节点是最后节点的祖先，后面的节点是前面节点的子孙。
    -   节点的层数等于父节点的层数加一，根节点的层数定义为一，树中节点层数的最大值称为该树的高度或深度。
    -   m(m>=0)棵互不相交的树的集合称为森林，树去掉根节点就成为森林，森林加上一个新的根节点就成为树。

        ![Python-Net_tree02](http://localhost:9710/blog/Python/Python-Net_tree02.jpg)

#### 二叉树

1.  定义：

    -   二叉树（Binary Tree）是n(n>=0)个节点的有限集合，它或者是空集(n=0),或是由一个根节点以及两棵互不相交的，分别称为左子树和右子树的二叉树组成。二叉树与普通有序树不同，二叉树严格区分左孩子和右孩子，即使只有一个子节点也要区分左右。

2.  特点：

    -   特殊的树形结构
    -   度数最多为2
    -   严格区分左子树，右子树

3.  特征：

    -   二叉树第i(i>=1)层上的节点最多为 2 的 i-1 次方个。
    -   深度为k(k>=1)的二叉树最多有 2 的 k 次方 -1 个节点。
    -   在任意一棵二叉树中，树叶的数目比度数为2的节点的数目多一。
    -   满二叉树： 深度为k(k>=1)时有 2 的 k 次方 -1 个节点的二叉树。
    -   完全二叉树：只有最下面两层有度数小于2的节点，且最下面一层的叶节点集中在最左边的若干位置上。

        ![Python-Net_tree03](http://localhost:9710/blog/Python/Python-Net_tree03.jpg)

4.  遍历：沿某条搜索路径周游二叉树，对树中的每个节点访问一次且仅访问一次。

    -   先序遍历：先访问树根，再访问左子树，最后访问右子树；
    -   中序遍历：先访问左子树，再访问树根，最后访问右子树；
    -   后序遍历：先访问左子树，再访问右子树，最后访问树根；
    -   层次遍历：从根节点开始，逐层从左向右进行遍历。

        ```bash
            # 遇到没有遍历过的节点当作根看待
            # 遍历过程中遍历到根即写出根的值

            树形结构：
                    1
                 2     3
               4     5   6
                      7

            先根 1 2 4 3 5 7 6
            中根 4 2 1 5 7 3 6
            后根 4 2 7 5 6 3 1

            树形结构：
                    1
                 2     3
               4  5      6
                    7

            先根 1 2 4 5 7 3 6
            中根 4 2 5 7 1 3 6
            后根 4 7 5 2 6 3 1

            先根 F C A D B E H G M
            中根 A C B D F H E M G

            树形结构

                     F
                 C        E
               A   D   H    G
                  B        M
            后根 A B D C H M G E F
        ```

#### 递归思想

1.  定义

    -   所谓递归函数是指一个函数的函数体中直接调用或间接调用了该函数自身的函数。这里的直接调用指一个函数的函数体中含有调用自身的语句。间接调用是指一个函数在函数体里有调用了其他函数而其他函数又反过来调用了该函数的情况。

2.  递归函数调用的执行过程分为两个阶段

    -   递推阶段：从原问题出发，按递归公式递推从未知到已知，最终达到递归终止条件
    -   回归阶段：按递归终止条件求出结果，逆向逐步代入递归公式，回归到原问题求解

3.  优缺点

    -   优：递归可以把问题简单化，让思路更为清晰，代码更清洁。
    -   缺：递归因系统环境影响大，当递归深度太大时，可能会得到不可预知的结果。

4.  补充：

    -   递归函数要有终止条件，且终止条件要在调用自身之前。

    ```py
        def recursion(n):
            if n <= 1:  # 终止条件，一定在函数的调用之前
                return 1
            return n * recursion(n - 1)
        # 递归思想：
        # 5 * r(4)                      ↓  递推
        #       4 * r(3)
        #           3 * r(2)
        #               2 * r(1) >>> 1  ↑  回归
    ```

#### 二叉树的代码实现

1.  二叉树顺序存储

    -   使用列表，如果二叉树比较稀疏，比较浪费空间

        ```py
            """
                    A
                B      C
                     D   E
                   F G  H I
            """

            # None 为空节点
            # [根，左，右]
            list_binary_tree = [
                "A",
                ["B", None, None],
                ["C", ["D", ["F", None, None], ["G", None, None]], ["E", ["H", None, None], ["I", None, None]]]
            ]
        ```

2.  二叉树链式存储

**程序实现：**

```py
    class TreeNode:
        """
            二叉树节点类
        """

        def __init__(self, root_node=None, left_subtree=None, right_subtree=None):
            self.root_node = root_node
            self.left_subtree = left_subtree
            self.right_subtree = right_subtree

    class BinaryTree:
        """
            二叉树类
        """

        def __init__(self, root_node=None):
            self.root_node = root_node

        def is_empty(self):
            if not self.root_node:
                return True
            return False

        def pre_order(self, node):
            """
                先序遍历
            @param node:
            @return:
            """
            if not node:
                return
            print(node.root_node, end=" ")
            self.pre_order(node.left_subtree)
            self.pre_order(node.right_subtree)

        def infix_order(self, node):
            """
                中序遍历
            @param node:
            @return:
            """
            if not node:
                return
            self.infix_order(node.left_subtree)
            print(node.root_node, end=" ")
            self.infix_order(node.right_subtree)

        def post_order(self, node):
            """
                后续遍历
            @param node:
            @return:
            """
            if not node:
                return
            self.post_order(node.left_subtree)
            self.post_order(node.right_subtree)
            print(node.root_node, end=" ")

        def level_order(self):
            """
                层次遍历
            @param node:
            @return:
            """
            queue = SequenceQueue()
            queue.enqueue(self.node)
            while not queue.is_empty():
                node = queue.dequeue()
                print(node.root_node, end=" ")
                if node.left_subtree:
                    queue.enqueue(node.left_subtree)
                if node.right_subtree:
                    queue.enqueue(node.right_subtree)

    """
            A
        B      C
             D   E
           F G  H I
    """

    if __name__ == "__main__":
        # 后序遍历增加节点 b f g d h i e c a
        b = TreeNode("B")
        f = TreeNode("F")
        g = TreeNode("G")
        d = TreeNode("D", f, g)
        h = TreeNode("H")
        i = TreeNode("I")
        e = TreeNode("E", h, i)
        c = TreeNode("C", d, e)
        a = TreeNode("A", b, c)
        binary = BinaryTree(a)
        print(binary.is_empty())
        binary.pre_order(binary.root_node)
        print("==========")
        binary.infix_order(binary.root_node)
        print("==========")
        binary.post_order(binary.root_node)
        print("==========")
        binary.level_order()
```

## 算法基础

### 基础概念特征

1.  定义

    -   算法（algorithm）是一个有穷规则（或语句/指令）的有序集合，它确定了解决某一问题的一个运算序列，对于问题的初始输入，通过算法有限步的运行，产生一个或多个输出。

2.  数据的逻辑结构与存储结构密切相关

    -   算法设计：取决于选定的逻辑结构。
    -   算法实现：依赖于采用的存储结构。

3.  算法的特性

    -   有穷性 —— 算法执行的步骤（或规则）是有限的
    -   确定性 —— 每个计算步骤无二义性
    -   可行性 —— 每个计算步骤能在有限的时间内完成
    -   输入/输出 —— 存在数据的输入和输出

4.  评价算法好坏的方法

    -   正确性：运行正确是一个算法的前提
    -   可读性：容易理解/容易编程和调试/容易维护
    -   健壮性：考虑情况全面，不容易出现运行错误
    -   时间效率高：算法消耗的时间少
    -   存储量低：占用较少的存储空间。

### 时间复杂度计算

算法效率 —— 用依据该算法编制的程序在计算机上执行所消耗的时间来度量。“O” 表示一个数量级的概念。根据算法中的语句执行的最大次数（频度）来估算一个算法执行时间的数量级。

-   计算方法：

    -   写出程序中所有运算语句执行的次数，进行加和。
    -   如果得到的结果是常量则时间复杂度为1
    -   如果得到的结果中存在变量n ，则取n 的最高次幂作为时间复杂度。

        **随着问题规模n 增大，算法执行时间增长率**

        ![Python-Net_algorithm](http://localhost:9710/blog/Python/Python-Net_algorithm.png)

### 排序和查找

#### 排序

> 排序（sort）是将无序的记录序列（或称文件）调整成有序的序列

-   常见排序方法

    -   冒泡排序(Bubble Sort)：它重复地走访过要排序的元素列，依次比较两个相邻的元素，如果顺序（如从大到小、首字母从Z到A）错误就把他们交换过来。走访元素的工作是重复地进行直到没有相邻元素需要交换，也就是说该元素列已经排序完成。

        ![Python-Net_Sort_Bubble](http://localhost:9710/blog/Python/Python-Net_Sort_Bubble.gif)

    -   选择排序(Selection Sort)：首先在未排序序列中找到最小（大）元素，存放到排序序列的起始位置，然后，再从剩余未排序元素中继续寻找最小（大）元素，然后放到已排序序列的末尾。以此类推，直到所有元素均排序完毕。

        ![Python-Net_Sort_Selection](http://localhost:9710/blog/Python/Python-Net_Sort_Selection.gif)

    -   插入排序(Insertion Sort)：在待排序的元素中，假设前面n-1(其中n>=2)个数已经是排好顺序的，现将第n个数插到前面已经排好的序列中，然后找到合适自己的位置，使得插入第n个数的这个序列也是排好顺序的。按照此法对所有元素进行插入，直到整个序列排为有序的过程，称为插入排序

        ![Python-Net_Sort_Insertion](http://localhost:9710/blog/Python/Python-Net_Sort_Insertion.gif)

    -   快速排序（Quick Sort）：从数列中挑出一个元素，称为“基准”（pivot），重新排序数列，所有元素比基准值小的摆放在基准前面，所有元素比基准值大的摆在基准的后面（相同的数可以摆在任一边）.在这个分区退出之后，该基准就处于数列的中间位置，这个称为分区（partition）操作。递归的把小于基准值元素的子数列和大于基准值元素的子数列排序。

        ![Python-Net_Sort_Quick](http://localhost:9710/blog/Python/Python-Net_Sort_Quick.gif)

        ```py
            class Sort:
                def __init__(self, list_target):
                    self.list_target = list_target

                def bubble(self):
                    """
                        冒泡排序
                    @return:
                    """
                    for i in range(len(self.list_target) - 1):  # 比较多少轮
                        for j in range(len(self.list_target) - i - 1):  # 每轮比较多少次
                            if self.list_target[j] > self.list_target[j + 1]:
                                self.list_target[j], self.list_target[j + 1] = self.list_target[j + 1], self.list_target[j]

                def selection(self):
                    """
                        选择排序
                    @return:
                    """
                    for i in range(len(self.list_target) - 1):
                        target_min = i
                        for j in range(i + 1, len(self.list_target)):
                            if self.list_target[i] > self.list_target[j]:
                                # self.list_target[i], self.list_target[j] = self.list_target[j], self.list_target[i]
                                target_min = j
                        if target_min != i:
                            self.list_target[i], self.list_target[target_min] = self.list_target[target_min], self.list_target[i]

                def insertion(self):
                    """
                        插入排序
                    @return:
                    """
                    for i in range(1, len(self.list_target)):
                        mark = self.list_target[i]
                        j = i
                        while j > 0 and self.list_target[j - 1] > mark:
                            self.list_target[j] = self.list_target[j - 1]
                            j -= 1
                            self.list_target[j] = mark

                def quick(self, low, high):
                    """
                        快速排序
                            low 列表开头元素索引
                            high 列表结尾元素索引
                    @return:
                    """
                    if low < high:
                        key = self.sub_quick(low, high)
                        self.quick(0, key - 1)
                        self.quick(key + 1, high)

                def sub_quick(self, low, high):
                    """
                        单次快速排序
                    @param low:
                    @param high:
                    @return:
                    """
                    key = self.list_target[low]
                    while low < high:
                        while low < high and self.list_target[high] >= key:
                            high -= 1
                        self.list_target[low] = self.list_target[high]
                        while low < high and self.list_target[low] < key:
                            low += 1
                        self.list_target[high] = self.list_target[low]
                    self.list_target[low] = key
                    return low
        ```

#### 查找

> 查找（或检索）是在给定信息集上寻找特定信息元素的过程。

-   二分法查找

    -   当数据量很大适宜采用该方法。采用二分法查找时，数据需是排好序的。

        ```py
            def search(target, key):
                low = 0
                high = len(target) - 1
                while low <= high:
                    mid = (high + low) // 2
                    if target[mid] < key:
                        low = mid + 1
                    elif target[mid] > key:
                        high = mid - 1
                    else:
                        return mid
                return
        ```

# 网络编程

## 网络IO

### IO

1.  定义：

> 在内存中存在数据交换的操作认为是IO操作，比如和终端交互，和磁盘交互，和网络交互等。

_ps. 磁盘：能够长久保持数据的存储器统称为磁盘_

_存储器分为寄存器，主存储器，外存储器_
_寄存器：辅助cpu 运行/计算_
_主存储器：内存，程序运行过程中进行数据临时存储，空间分配_
_外存储器：硬盘_

2.  程序分类

    -   IO密集型程序：在程序执行中有大量的IO操作，而cpu运算较少，消耗cpu较少，耗时长
    -   计算密集型程序：程序运行中计算较多，IO操作相对较少，cpu消耗多，执行速度块，几乎没有阻塞。

#### 文件

> 文件是保存在持久化存储设备（硬盘/U盘/光盘）上的一段数据，从功能角度分为文本文件（打开后会自动解码为字符）/二进制文件（视频/音频等）。在 Python 中把文件视作一种类型的对象，类似 其他类型。

##### 字节串（bytes）

> 在 Python 3 中引入了字节串的概念，与 str 不同，字节串以字节序列值表达数据，更方便用来处理二进制数据。在 Python3 中字节串是常见的二进制数据展现方式。

-   普通的ASCII编码字符串可以在前面加 b 转换为字节串。`eg. b"hello"`
-   通过 字节串转换函数 将 数据类型 强转 字节串 `bytes(num_)`
    -   参数：整数(n 初始化一个长度为n的列表序列)/字符串/可迭代对象

**常用函数**

-   字符串转换为字节串方法：`str_.encode()`
-   字节串转化为字符串方法：`bytes_.decode()`

##### 打开文件

`file_object = open(file_name, access_mode = "r",buffering = -1)`

-   功能：打开一个文件，返回一个文件对象。
-   参数：
    -   file_name —— 路径+文件名
    -   access_mode —— 打开文件的方式，如果不写默认为“r”
        -   r ：以读方式打开，文件必须存在
        -   w ：以写方式打开，如果待打开文件已存在，会先清空，不存在则创建
        -   a ：以追加模式打开，如果文件已存在，不会清空，接着往后追加
        -   r+：读写，文件必须存在
        -   w+：读写，如果待打开文件已存在，会先清空，不存在则创建
        -   a+：读写，如果文件已存在，不会清空，接着往后追加
        -   rb ：以二进制读模式打开
        -   wb ：以二进制写模式打开
        -   ab ：以二进制追加模式打开
        -   rb+：以二进制读写模式打开
        -   wb+：以二进制读写模式打开
        -   ab+：以二进制读写模式打开
    -   buffering —— 参数 0 表示无缓冲，1 表示有行缓冲，如果是大于1 标识直接指明缓冲区大小。如果不写或为负数，则采用系统自己指定的缓冲.返回值:成功返回文件流对象，失败得到 IOError.
        -   buffer = 1 表示行缓存则在写入数据时遇到 \\n 自动刷新缓冲区
        -   采用系统默认缓存需要缓存区满后才能自动写入磁盘
        -   无论什么缓冲，当程序结束或者文件被关闭时都会将缓冲区内容写入磁盘

> 缓冲：系统自动的在内存中为每一个正在使用的文件开辟一个缓冲区，从内存向磁盘输出数据必须先送到内存缓冲区，装满缓冲区在一起送到磁盘中去，从磁盘中读数据，则一次从磁盘文件将一批数据读入到内存缓冲区中，然后再从缓冲区逐个的将数据送到程序的数据区。

_缓冲区作用：减少磁盘的交互次数，提高磁盘的读写效率_

_文件流：_
_流(stream):所有的 I/O 操作仅是简单的从程序移进或者移出，这种字节流，就称为流。所以打开一个文件有时也称为打开一个文件流对象。_
_系统已经默认为我们打开了三个流，在sys模块中：_
_标准输入 —— sys.stdin_
_标准输出 —— sys.stdout_
_标准错误 —— sys.stderr_

##### 文件读写

1.  读取文件

    -   read([size])

        -   定义：用来直接读取字节到字符串中，最多读取给定数目个字节，如果没有给定size参数（默认值为-1）或者size值为负，文件将被读取直至末尾，文件过大时候建议在 non-blocking(非阻塞模式) 模式下使用。
        -   读取到文件结尾在读取，得到空字符串
        -   实际读取的内容可以小于等于指定 size 大小

    -   readline([size])

        -   定义：读取打开文件的一行(读取下个行结束符(换行符)之前的所有字节）整行(包括行结束符，作为字符串返回。)和 read() 相同,它也有一个可选的 size 参数，默认为-1，代表读至行结束符。如里提供了该参数，则只读取size 大小的内容。

    -   readlines([size])

        -   定义：该方法并不像其它两个输入方法一样返回一个字符串。它会读取所有(剩余的)行然后把它们作为一个字符串列表返回。它的可选参数 size 代表返回的最大字节大小，

    -   文件对象本身也是一个迭代器，在 for 循环中可以迭代文件的每一行。

2.  写入文件

    -   write(string)
        -   功能与read0和 readline0)相反,它把含有文本数据或二进制数据块的字符串写入到文件中去。
        -   若需要换行需人为添加换行符（"\\n"）
        -   open() 使用 "wb" 打开的文件、写入内容时必须用字节串。
    -   writelines(str_list)
        -   和 readlines()一样, writelines(方法是针对列表的操作,它接受一个字符串列表作为参数,将它们写入文件。行结束符并不会被自动加入,所以如果需要的话,你必须在调用 writelines0前给每行结尾加上行结束符

3.  关闭文件

    -   打开一个文件后我们就可以通过文件对象对文件进行操作了,当操作结束后使用close()关闭这个对象可以防止一些误操作,也可以节省资源
        -   `file_object.close()`

4.  with 操作

> python 中的 with 语句使用于对资源进行访问的场合,保证不管处理过程中是否发生错误或者异常都会执行规定的"清理"操作,释放被访问的资源,比如有文件读写后自动关闭、线程中锁的自动获取和释放等

**with语句的语法格式如下**

```py
    with context_expression [as target(s)]:
        with-body  # 语句块
```

_通过with方法可以不用close().因为with生成的对象在语句块结束后会自动处理，所以也就不需要close(),但是这个文件对象只能在with 语句块内使用_

```py
    with open("file", "r+") as f:
        f.read()
```

**实例**

```py
    def copy_rename_file(file_rename):
        file_name = input("file name: ")
        try:
            file_obj = open(file_name, "rb")
        except FileNotFoundError as e:
            print(e)
        else:
            target_obj = open(file_rename, "wb")
            while True:
                data = file_obj.read(1024)
                if not data:
                    break
                target_obj.write(data)

            file_obj.close()
            target_obj.close()

    copy_rename_file("zf")
```

5.  其他操作

    -   刷新缓冲区 flush()

        -   该函数调用后会进行一次磁盘交互，将缓冲区中的内存写入到磁盘。

    -   文件偏移量

        1.  定义

            -   打开一个文件进行操作时系统会自动生成一个记录,记录中描述了我们对文件的一系列操作,其中包括每次操作到的文件位置。文件的读写操作都是从这个位开始进行的

        2.  功能:代表文件的当前读写操作位置,随读写操作移动

            -   r,w方式打开文件此时偏移量在开头
            -   以a方式打开此时偏移量在末尾

        3.  基本操作

            -   tell()：获取文件偏移量
            -   seek(offset[,whence])：移动文件偏移量位置
                -   offset —— 字节代表相对于某个位置偏移量,可以是负数表示向前移动
                -   whence —— 是基准位置的默认值为0代表从文件开头算起,1代表从当前位置算起,2代表从文件末尾算起

    -   文件描述符

        1.  定义：在系统中每一个IO操作都会分配一个整数作为编号，该整数即这个IO操作的文件描述符。（不会重复）
        2.  获取文件描述符
            -   fileno() —— 通过IO对象获取对应的文件描述符

#### 文件管理函数

> 先导入 os 模块
> 函数的参数均是 路径/文件名字符串

1.  os.path.getsize(str_url): 获取文件大小
2.  os.listdir(str_url)：查看文件列表
3.  os.path.exists(str_url)：查看文件是否存在
4.  os.path.isfile(str_url)：判断文件类型
5.  os.path.isdir(str_url)：判断文件类型

    -   `ls -l` 第一个字母 `-`：普通文件，`d`：目录
        -   Linux 文件分类：b(块设备)、c(字符设备)、d(目录)、-(普通文件)、l(链接文件)、s(套接字文件)、p(管道文件)

6.  os.remove(str_url)：删除文件

### 网络编程基础

>   计算机网络功能主要包括实现资源共享，实现数据信息的快速传递。

#### OSI七层模型

> 制定组织: ISO (国际标准化组织)
>
> 作用 ： 使网络通信的工作流程标准化
>
> 应用层：提供用户服务，具体功能由应用程序实现
> 表示层：数据的压缩优化加密
> 会话层：建立用户级的连接，选择适当的传输服务
> 传输层：提供传输服务，进行流量监控
> 网络层：路由选择，网络互联
> 链路层：进行数据交换，控制具体数据发送
> 物理层：提供数据传输的硬件保证，网卡接口，传输介质

-   优点：

    1.  建立了统一的工作流程
    2.  各部分功能清晰，各司其职
    3.  降低耦合度，方便开发

#### 四层模型（TCP/IP模型）

背景 ： 实际工作中七层模型太过细致，难以实践，逐渐演化为实际工作中应用的四层

![Python-Net_Net_Base01](http://localhost:9710/blog/Python/Python-Net_Net_Base01.png)

#### 数据传输过程

1.  发送端由应用程序发送消息,逐层添加首部信息,最终在物理层发送消息包。
2.  发送的消息经过多个节点(交换机,路由器)传输,最终到达目标主机
3.  目标主机由物理层逐层解析首部消息包,最终到应用程序呈现消息。

![Python-Net_Net_Base02](http://localhost:9710/blog/Python/Python-Net_Net_Base02.png)

#### 网络协议

> 定义： 在网络数据传输中，都遵循的规定，规则。包括建立什么样的数据结构，什么样的特殊标志等

#### 网络概念

1.  网络主机（host）

    -   功能：标识一台主机在网络中的位置（地址）

        -   本地地址：'localhost', '127.0.0.1'
        -   网络地址：'172.40.91.112'
        -   自动获取合适网卡地址： '0.0.0.0'

> Linux: ifconfig

2.  IP地址

    -   功能：确定一台主句的网络路由位置。
    -   结构：
        -   IPv4 点分十进制标识 172.40.91.112，每部分取值范围0--255。（32位）
        -   IPv6 128位 fe80::5fa6:69ac:689c:f844 扩大了地址范围
    -   特殊IP ：
        -   127.0.0.1   本地测试IP
        -   0.0.0.0   自动获取合适网卡IP
        -   172.40.91.0  表示网段
        -   172.40.91.1  表示网关
        -   172.40.91.255 广播地址

3.  域名

    -   定义： 给网络服务器地址起的名字

    -   作用： 方便记忆，表达一定的含义

    -   -   ping  [ip] ： 测试和某个主机的连接情况

4.  端口 （port）

    -   作用： 是网络地址的一部分，用于区分主机上不同的网络应用。

    -   特点： 一个系统中的网络应用监听端口不重复

    -   取值范围：1---65535
        -   1--1023 系统应用或者大众程序使用的端口
        -   1024 -- 65535  自用端口

#### 传输层服务

##### 面向连接的传输服务 （基于TCP协议的数据传输）

1.  传输特征：提供了可靠的数据传输，可靠性指的是在数据传输中，无丢失，无失序，无差错，无重复。
2.  实现手段：在通信前需要建立数据连接，通信结束需要正常断开连接。

    1.  三次握手（建立连接的过程）

        -   客户端向服务端发送报文请求连接
        -   服务器接收到请求后返回报文进行确认
        -   客户端得到回复发送最终连接报文进行连接

        ![Python-Net_ThreeHandshake](http://localhost:9710/blog/Python/Python-Net_ThreeHandshake.png)


    2.  四次挥手 （断开连接的过程）
        -   主动方发送报文请求断开
        -   被动方接收请求，返回报文确定开始断开
        -   被动方准备就绪后，再次发送报文，表示可以断开
        -   主动方收到确认，最终发送报文断开连接

        ![Python-Net_FourWave](http://localhost:9710/blog/Python/Python-Net_FourWave.png)

3.  适用情况：对数据传输可靠性要求高，传输文件大。比如：文件传输，网页访问等

##### 面向无连接的传输服务（基于UDP协议的数据传输）

1.  传输特点：不保证传输的可靠性，传输过程没有连接和断开，数据收发自由。
2.  适用情况： 网络较差，对可靠性要求不高的传输。比如：网络视频，群聊，广播

#### socket套接字编程

1.  套接字：实现网络编程进行数据传输的一种技术手段

2.  python实现套接字编程 `import socket`

3.  套接字类型

    -   流式套接字(SOCK_STREAM)：以字节流的方式进行数据传输，实现TCP网络传输方案(面向连接-tcp协议-可靠地-流式套接字)。

    -   数据报套接字(SOCK_DGRAM): 以数据报形式传输数据，实现UDP网络传输方案(面向无连接-udp协议-不可靠-数据报套接字)。

##### TCP套接字编程/通信

> 服务器：socket()-->bind()-->listen()-->accept()-->recv/send-->close()
>
> 客户端：socket() --> connect() --> send/recv-->close()

###### 服务端流程

![Python-Net_TCP_Client](http://localhost:9710/blog/Python/Python-Net_TCP_Server.png)

1.  创建套接字对象

`sockfd = socket.socket(socket_family=AF_INET, socket_type = SOCK_STREAM, proto = 0)`

    -   功能: 创建套接字对象
    -   参数：
        -   socket_family 网络地址类型 AF_INET ==> IPv4
        -   socket_type 套接字类型
            -   SOCK_STREAM  流式套接字
            -   SOCK_DGRAM  数据报套接字
        -   proto  子协议选择，通常为0
    -   返回值： 套接字对象

2.  绑定地址:`sockfd.bind(addr)`

    -   功能： 绑定本机网络地址
    -   参数： 二元元组(ip,port)   ('0.0.0.0',8888)

3.  设置监听:`sockfd.listen(n)`

    -   功能：将套接字设置为监听套接字，创建监听队列
    -   参数：监听队列大小

4.  等待处理客户端请求:`connfd,addr = sockfd.accept()`

    -   功能：阻塞等待处理客户端请求
    -   返回值：
        -   connfd  客户端连接套接字
        -   addr   连接的客户端地址

> 阻塞函数：程序运行过程中遇到阻塞函数则暂停执行，直到达到某种条件后继续运行。

5.  收发消息

    -   `data = connfd.recv(buffersize)`

        -   功能: 接收客户端消息
        -   参数：每次最多接收消息的大小
        -   返回值：收到的消息内容

    -   `n = connfd.send(data)`
        -   功能：发送消息
        -   参数：要发送的内容  bytes格式
        -   返回值：发送了多少字节

6.  关闭套接字:`sockfd.close()`

    -   功能: 关闭套接字

```py
    import socket

    sockfd = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

    sockfd.bind(("0.0.0.0", 12016))

    sockfd.listen(3)

    print("waiting for connect...")

    connfd, addr = sockfd.accept()

    data = connfd.recv(1024)

    print("receive message", data)

    n = connfd.send(b"Receive message")

    print("send", n)

    connfd.close()
    sockfd.close()
```

###### 客户端流程

![Python-Net_TCP_Client](http://localhost:9710/blog/Python/Python-Net_TCP_Client.png)

1.  创建套接字

    -   只有相同类型套接字才可以通信

2.  请求连接：`sockfd.connect(addr)`

    -   功能： 连接服务端
    -   参数： 元组， 服务端地址

3.  消息收发

    -   防止两端都阻塞，消息的收发要配合

4.  关闭套接字

```py
    from socket import *

    sockfd = socket()

    server_addr = ("127.0.0.1", 12016)

    sockfd.connect(server_addr)

    sockfd.send(b"answer-zf")

    data = sockfd.recv(1024)

    print(data)

    sockfd.close()
```

##### tcp套接字数据传输特点

1.  tcp连接中当一端退出，另一端如果阻塞在recv，则recv会立即返回一个空字串

2.  tcp连接中如果另一端已经不存在，再试图使用send向其发送内容时会出现 BrokenPipeError

##### 网络收发缓冲区

1.  缓冲区有效的协调了消息的收发速度
2.  send，recv实际是向缓冲区发送接收消息，当缓冲区不为空的时候recv就不会阻塞

##### 粘包问题

-   原因：tcp以字节流方式传输数据，没有消息边界，多次发送的内容如果被一次接收就会形成粘包
-   影响：如果每次发送的内容是需要独立解析的含义，此时粘包会对消息的解析产生影响
-   处理：
    1.  人为添加消息边界
    2.  控制发送速度

```py
    ################### tcp_server ###################

    import socket

    sockfd = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    sockfd.bind(("0.0.0.0", 12016))
    sockfd.listen(3)

    while True:
        print("waiting for connect...")
        try:
            connfd, addr = sockfd.accept()
            print(addr)
        except KeyboardInterrupt:
            break
        while True:
            data = connfd.recv(5)
            if not data:
                break
            print("receive message", data.decode())
            n = connfd.send(b"Receive message")
            print("send", n)
        connfd.close()
    sockfd.close()

    ################### tcp_client ###################

    from socket import *

    sockfd = socket()
    server_addr = ("127.0.0.1", 12016)
    sockfd.connect(server_addr)

    while True:
        message_client = input("message: ")
        if not message_client:
            break
        sockfd.send(message_client.encode())
        data = sockfd.recv(1024)
        print(data.decode())

    sockfd.close()
```

##### UDP套接字编程/通信（udp、不可靠、面向无连接）

###### 服务端流程

1.  创建套接字: `sockfd = socket(AF_INET,SOCK_DGRAM)`

2.  绑定地址：`sockfd.bind(addr)`

3.  收发消息:

    -   `data,addr = sockfd.recvfrom(buffersize)`
        -   功能: 接收UDP消息
        -   参数： 每次最多接收多少字节内容
        -   返回值：
            -   data 接收到的消息
            -   addr 消息发送方地址
    -   `n = sockfd.sendto(data,addr)`
        -   功能: 发送UDP消息
        -   参数：
            -   data 发送的内容  bytes格式
            -   addr 目标地址

4.  关闭套接字:`sockfd.close()`

###### 客户端流程

1.  创建udp套接字
2.  发送接收消息
3.  关闭套接字

```py
    ################### udp_server ###################
    from socket import *

    sockfd = socket(AF_INET, SOCK_DGRAM)

    server_addr = ("0.0.0.0", 12016)
    sockfd.bind(server_addr)

    while True:
        data, addr = sockfd.recvfrom(1024)
        print("receive: ", data.decode())
        sockfd.sendto(b"answer-zf", addr)
    sockfd.close()

    ################### udp_client ###################

    from socket import *

    HOST = "127.0.0.1"
    POST = 12016
    ADDR = (HOST, POST)

    sockfd = socket(AF_INET,SOCK_DGRAM)

    while True:
        input_message = input(">>>")
        if not input_message:
            break
        n = sockfd.sendto(input_message.encode(), ADDR)
        data, addr = sockfd.recvfrom(1024)
        print("receive", data.decode())

    sockfd.close()
```

##### tcp套接字和udp套接字编程区别

1.  流式套接字式以字节流方式传输数据，数据报套接字以数据报形式传输
2.  tcp套接字会有粘包问题，udp套接字有消息边界不会粘包
3.  tcp套接字保证消息的完整性，udp不保障
4.  tcp套接字依赖listen accept完成连接才能进行数据收发，udp套接字不需要
5.  tcp使用send recv收发消息，udp使用sendto,recvfrom

##### socket模块方法和socket套接字属性

1.  部分socket模块方法

    -   获取计算机名： `socket.gethostname()`

    -   通过主机名获取ip地址： `socket.gethostbyname('www.baidu.com')`

    -   通过服务名称获取监听端口： `socket.getservbyname('mysql')`

    -   通过端口获取服务名称： `socket.getservbyport(3306)`

    -   将IP地址转换为字节串： `socket.inet_aton('192.168.1.2')`

    -   将字节串转换为IP： `socket.inet_ntoa(b'\xc0\xa8\x01\x02')`

2.  套接字属性

    1.  sockfd.family  地址类型
    2.  sockfd.type  套接字类型
    3.  sockfd.getsockname() 获取套接字绑定地址
    4.  sockfd.fileno() 获取文件描述符
        -   文件描述符：系统中每一个IO操作都会分配一个整数作为编号，该整数即这个IO的文件描述符。
        -   特点：每个IO的文件描述符不会重复
    5.  getpeername() 获取连接套接字客户端地址
    6.  setsockopt(level,option,value)

        -   功能：设置套接字选项
        -   参数：
            -   level 选项类别  SOL_SOCKET
            -   option  具体选项内容
            -   value  选项值

        _SOL_SOCKET option 属性_

        ![Python-Net_setsockopt](http://localhost:9710/blog/Python/Python-Net_setsockopt.png)

_应用：上传文件_

```py
    ################### TCP server file upload ###################

    from socket import *

    sockfd = socket()

    sockfd.bind(("0.0.0.0", 12016))
    sockfd.listen(3)
    connfd, addr = sockfd.accept()
    print("connect >>>  ", addr)

    fd = open("zf.jpg", "wb")
    while True:
        data = connfd.recv(1024)
        if not data:
            break
        fd.write(data)
    fd.close()
    connfd.close()
    sockfd.close()

    ################### TCP client file upload ###################

    from socket import *

    sockfd = socket()

    sockfd.connect(("127.0.0.1", 12016))

    fd = open("text.jpg", "rb")
    while True:
        data = fd.read(1024)
        if not data:
            break
        sockfd.send(data)
    fd.close()
    sockfd.close()
```

##### UDP套接字广播

-   广播定义：一端发送，多端接收
-   广播地址：每个网段内的最大地址，向该地址发送则网段内所有的主机都能接收。

```py
    ################### UDP BROADCAST client ###################
    from socket import *

    sockfd = socket(AF_INET, SOCK_DGRAM)

    # 设置套接字 允许接受广播
    sockfd.setsockopt(SOL_SOCKET, SO_BROADCAST, 1)

    sockfd.bind(("0.0.0.0", 9999))

    while True:
        try:
            data, addr = sockfd.recvfrom(1024)
        except KeyboardInterrupt:
            break
        else:
            print(data.decode())

    sockfd.close()

    ################### UDP BROADCAST server ###################

    from socket import *
    from time import sleep

    dest = ("192.168.1.255", 9999)

    sockfd = socket(AF_INET, SOCK_DGRAM)
    sockfd.setsockopt(SOL_SOCKET, SO_BROADCAST, 1)

    data = """
        *******************
        this.is.test broadcast ......
        *******************
    """

    while True:
        sleep(2)
        sockfd.sendto(data.encode(),dest)
    sockfd.close()
```

##### TCP套接字之HTTP传输

1.  HTTP协议（超文本传输协议）

    -   用途：网页获取，数据传输
    -   特点:
        -   应用层协议，传输层使用tcp服务
        -   简单，灵活，无状态(协议不记录传输内容)
        -   http1.1 支持持久连接，丰富请求类型。

2.  网页请求过程

    1.  客户端(浏览器)通过tcp传输,发送http请求给服务端
    2.  服务端接收到http请求后进行解析
    3.  服务端处理请求内容,组织响应内容
    4.  服务端将响应内容以http响应格式发送给浏览器
    5.  浏览器接收到响应内容,解析展示

    ![Python-Net_Web_Access](http://localhost:9710/blog/Python/Python-Net_Web_Access.jpg)

###### HTTP请求 request

1.  请求行 ： 具体的请求类别和请求内容
    -   格式：`GET    /    HTTP/1.1`
        -   `GET`:请求类别
        -   `/`:请求内容 （`/` 代表主页）
        -   `HTTP/1.1`:协议版本
    -   请求类别：每个请求类别表达不同的请求方式和行为
        -   GET  : 获取网络资源
        -   POST ：提交一定的信息，得到反馈
        -   HEAD ：只获取网络资源响应头
        -   PUT  ：更新服务器资源
        -   DELETE ： 删除服务器资源
        -   CONNECT
        -   TRACE ： 测试
        -   OPTIONS ： 获取服务器性能信息
2.  请求头 ： 对请求的进一步描述和解释
    -   `Accept-Encoding: gzip`
3.  空行
4.  请求体: 请求参数或者提交内容

###### HTTP响应 response

1.  响应行：反馈基本的响应情况
    -   `HTTP/1.1    200    OK`
        -   `HTTP/1.1`:协议版本
        -   `200`:响应码
            -   1xx 提示信息，请求被接受
            -   2xx 响应成功
            -   3xx 响应需要进一步操作，重定向
            -   4xx 客户端错误
            -   5xx 服务端错误
        -   `OK`:附加信息
2.  响应头 ： 对响应内容的描述
    -   `Content-Type:text/html`
3.  空行
4.  响应体：响应的主体内容信息

_http server_

```py
    from socket import *

    def handle(conn_fd):
        print("request from ", conn_fd.getpeername())
        request = conn_fd.recv(4096)
        # 防止客户端断开
        if not request:
            return
        request_line = request.splitlines()[0].decode()  # 将请求按行分割取第一行
        info = request_line.split(" ")[1]
        if info == "/":
            fd = open("index.html", "r")
            response_msg = "HTTP/1.1 200 OK\r\n"
            response_msg += "Content-Type: text/html\r\n"
            response_msg += "\r\n"
            response_msg += fd.read()
        else:
            response_msg = "HTTP/1.1 200 OK\r\n"
            response_msg += "Content-Type: text/html\r\n"
            response_msg += "\r\n"
            response_msg += "<h1> 404 </h1>"
        conn_fd.send(response_msg.encode())

    def main():
        sock_fd = socket()
        sock_fd.setsockopt(SOL_SOCKET, SO_REUSEADDR, 1)
        sock_fd.bind(("0.0.0.0", 12016))
        sock_fd.listen(3)
        print("Listen the port 12016")
        while True:
            conn_fd, addr = sock_fd.accept()
            handle(conn_fd)
            conn_fd.close()
```

#### struct 模块的使用(打包、解包)

1.  原理：将一组简单数据进行打包，转换为 bytes 格式，或者将一组 bytes格式数据进行解析。

2.  接口使用：

        -   Structural(format)
            -   功能：生成结构化对象
            -   参数：format 定制的数据结构

                ![Python-Net_Struct](http://localhost:9710/blog/Python/Python-Net_Struct.png)

        -   st.pack(v1,v2,v3...)
            -   功能：将一组数据按照指定格式打包转换为 bytes
            -   参数：要打包的数据
            -   返回值： bytes 字节串
        -   st.unpack(bytes_data)
            -   功能：将 bytes字节串按照指定的格式解析
            -   参数：要解析的字节串
            -   返回值：解析后的内容

        ```py
            import struct
            st = struct.Struct("i8sif")
            data = st.pack(1,b"zhangsan",16,78.5)
            d = st.unpack(data)
            # 也可以使用 struct 模块直接调用
            # struct.pack(format,v1,v2,v3...)
            # struct.unpack(format,bytes_data)
            zf_data = struct.pack("i18sif",1,b"zhangsan",16,22.3)
            un_zf_data = struct.unpack("i18sif", zf_data)
        ```

```py
    ################### UDP struct server ###################

    import struct
    from socket import *

    sock_fd = socket(AF_INET, SOCK_DGRAM)

    sock_fd.bind(("0.0.0.0", 12016))

    st = struct.Struct("i32sif")
    file_desc = open("struct.txt", "a")

    while True:
        data, addr = sock_fd.recvfrom(1024)
        tuple_write_msg = st.unpack(data)
        write_msg = "%d %s %d %.2f\n" % (tuple_write_msg[0],
                                         tuple_write_msg[1].decode(),
                                         tuple_write_msg[2],
                                         tuple_write_msg[3])
        file_desc.write(write_msg)
        file_desc.flush()

    file_desc.close()
    sock_fd.close()

    ################### UDP struct client ###################

    from socket import *
    import struct

    sock_fd = socket(AF_INET, SOCK_DGRAM)

    ADDR = ("127.0.0.1", 12016)
    st = struct.Struct("i32sif")

    while True:
        print("====================")
        input_id = int(input("user id: "))
        input_name = input("user name: ").encode()
        input_age = int(input("user age: "))
        input_score = float(input("user score: "))
        send_msg = st.pack(input_id, input_name, input_age, input_score)
        sock_fd.sendto(send_msg, ADDR)

    sock_fd.close()
```

### 并发编程

#### 多任务并发编程

1.  意义： 充分利用计算机资源，同时处理多个任务，提高程序的运行效率

2.  并行和并发

    -   并行：多个任务利用计算机多核资源在同时执行，此时多个任务间是并行关系
    -   并发：同时处理多个任务，内核在任务间不断的切换达到很多任务都被同时处理的效果，实际每个时刻只有一个任务在被执行（占有内核）。

3.  实现方法：多进程，多线程

#### 进程 （process）

1.  定义： 程序在计算机中的一次运行过程

    -   程序是一个可行行的文件，是静态的只占磁盘
    -   进程是一个动态的过程，占有计算机运行资源，有一定的生命周期

2.  如何产生一个进程

    1.  用户空间通过调用程序接口或者命令发起请求
    2.  操作系统接收请求，开始调用系统接口创建进程
    3.  操作系统调配计算机硬件资源，整合进程状态等进程创建工作
    4.  操作系统将创建的进程提供给用户使用

3.  进程概念

-   cpu时间片：如果一个进程占有cpu则称这个进程在cpu时间片上

-   PCB(进程控制块)：在内存中开辟的一块空间，用于存放进程信息，也用于操作系统对进程的调配（查找识别）

-   进程ID（PID）：系统为每个进程分配的一个大于0的整数，作为进程的ID标志

    -   Linux命令 ： ps  -aux

-   父子进程：系统中每一个进程（除了初始进程）都有唯一的父进程，可以有0个或者多个子进程

    -   命令 ： pstree

-   进程状态

    -   三态：

        -   就绪态： 进程具备执行条件，等待分配cpu资源
        -   运行态： 进程占有cpu时间片正在运行
        -   阻塞态： 进程处理阻塞暂停状态，让出cpu

        ![Python-Net_3State](http://localhost:9710/blog/Python/Python-Net_3State.png)

    -   五态 （增加新建和终止）：

        -   新建：创建进程获取资源
        -   终止：进程结束释放资源

        ![Python-Net_5State](http://localhost:9710/blog/Python/Python-Net_5State.png)

    -   查看状态命令 ： ps  -aux => STAT 列

        > -   S 等待态
        > -   R 执行态
        > -   D 等待态（不可终端等待）
        > -   T 等待态
        > -   Z 僵尸
        >
        > -   &lt; 有较高优先级
        > -   N 优先级较低
        > -   \+ 前台进程（在终端有现象显示的）
        > -   s 会话组组长
        > -   l 有多线程的

    -   进程的运行特征

        1.  进程可以使用计算机多核资源
        2.  进程是计算机分配资源的最小单位
        3.  进程之间的运行互不影响,各自独立
        4.  每个进程拥有独立的空间,各自使用自己空闫资源

##### 基于fork的多进程创建

`pid = os.fork()`

-   功能： 创建新的进程

-   返回值 ： 整数，如果创建进程失败返回一个负数(-1)，如果成功则在原有进程返回新进程的PID，在新进程中返回0.

    -   fork 函数调用，创建新进程
    -   系统中产生新的进程空间
    -   新进程原样拷贝父进程的内存空间
    -   此时在系统中有两个同样的进程在运行
    -   在原有进程中 fork 的返回值是新进程的PID
    -   在新的进程中 fork 的返回值是0

    ```py
        import os

        pid = os.fork()

        if pid < 0:
            print("Create Process Failed")
        elif pid == 0:
            print("the new process")
        else:
            print("the old process")

        print("fork test over")
    ```

-   总结：
    -   子进程会复制父进程的全部内存空间(包括执行记录以及执行效果{变量赋值..})，故从fork的下一句开始执行（不会再产生子进程，循环递归下去）
    -   父子进程各自运行不影响，顺序不确定
    -   利用父子进程中fork返回值的差异，配合if语句让父子进程执行不同内容几乎是固定搭配
    -   父进程fork之前开辟的空间，子进程也会拥有，父子进程在各自空间操作不会相互影响
    -   父子进程有各自特有内容，PID ,PCB..

##### 进程相关函数

1.  `os.getpid()`

    -   功能: 获取一个进程的PID值
    -   返回值 ： 返回当前进程的PID

2.  `os.getppid()`

    -   功能：获取父进程的PID号
    -   返回值： 父进程pid

3.  `os._exit(status)`

    -   功能: 结束一个进程
    -   参数：进程结束状态 整数

4.  `sys.exit([status])`
    -   功能: 结束一个进程
    -   参数：默认为0，整数表示退出状态，字符串表示退出时打印内容

##### 孤儿和僵尸

1.  孤儿进程 ： 父进程先于子进程退出，此时子进程成为孤儿进程。

    -   特点 ： 孤儿进程会被系统进程收养，此时系统进程会成为孤儿进程新的父进程。孤儿进程退出时，系统进程会处理退出行为

2.  僵尸进程 ： 子进程先于父进程退出，父进程没有处理子进程退出行为，此时子进程就会成为僵尸进程

    -   特点 ：僵尸进程虽然已经结束，但是会存留部分进程信息在内存中，大量的僵尸会浪费系统内存。

3.  僵尸进程处理

    1.  使用wait()函数处理子进程退出

        -   `pid,status = os.wait()`

            -   功能： 阻塞等待子进程退出
            -   返回值：
                -   pid  退出的子进程PID
                -   status 子进程的退出状态

        -   `pid,status = os.waitpid(pid,option)`

            -   功能： 阻塞等待子进程退出
            -   参数：
                -   pid
                    -   `-1` 等待任意子进程退出
                    -   `>0` 表示等待指定子进程
                -   option
                    -   0  表示阻塞
                    -   os.WNOHANG 表示非阻塞
            -   返回值：

                -   pid  退出的子进程PID
                -   status 子进程的退出状态

                ```py
                    import os
                    pid = os.fork()

                    if pid < 0:
                        print("error")
                    elif pid == 0:
                        print("child pid", os.getpid())
                        os._exit(2)
                    else:
                        # pid, status = os.wait()  # 等待处理僵尸 => 阻塞（弊端）
                        pid, status = os.waitpid(-1, os.WNOHANG)  # 非阻塞
                        print("pid", pid)
                        print("status", status)  # 2 x 256 / os.WEXITSTATUS() => 2
                        while True:
                            pass
                ```

    2.  创建二级子进程（缺点浪费进程）

        1.  父进程创建子进程wait等待回收
        2.  子进程创建二级子进程后退出
        3.  二级子进程成为孤儿，和父进程一起完成事件

        ```py
            def f1():
                for i in range(4):
                    sleep(2)
                    print("answer-zf")

            def f2():
                for i in range(5):
                    sleep(1)
                    print("zzzzzzzzzzzzzz")

            pid = os.fork()

            if pid < 0:
                print("Error")
            elif pid == 0:
                pid_c = os.fork()
                if pid_c == 0:
                    f2()
                else:
                    os._exit(0)
            else:
                os.wait()
                f1()
        ```

    3.  信号处理

        -   原理 ： 子进程退出会发送信号给父进程，如果父进程忽略子进程信号，则系统会自动处理子进程退出

        -   方法：

            -   `import  signal`
            -   `signal.signal(signal.SIGCHLD,signal.SIG_IGN)`
                -   参数：
                    -   信号类型：子进程退出信号
                    -   信号处理方法：子进程发出退出信号后，父进程进行忽略

        -   特点：
            -   非阻塞
            -   使用该方法，可以处理所有子进程退出

        ```py
            import os
            import signal

            signal.signal(signal.SIGCHLD, signal.SIG_IGN)

            pid = os.fork()

            if pid < 0:
                pass
            elif pid == 0:
                print(os.getpid())
            else:
                while True:
                    pass
        ```

##### 基于 fork 的 聊天室 实例

```py
    ################### Chat Room Client ###################

    from socket import *
    import os, sys

    # 服务端地址
    ADDR = ("127.0.0.1", 12017)

    def send_message(sock_fd, name):
        while True:
            try:
                str_input_msg = input("pl. input message: ")
            except KeyboardInterrupt:
                str_input_msg = "quit"
            if str_input_msg == "quit":
                str_send_msg = "Q " + name
                sock_fd.sendto(str_send_msg.encode(), ADDR)
                sys.exit()
            str_send_msg = "C %s %s" % (name, str_input_msg)
            # print(str_send_msg)
            sock_fd.sendto(str_send_msg.encode(), ADDR)

    def recv_message(sock_fd):
        while True:
            data, addr = sock_fd.recvfrom(1024)
            if data.decode() == "quit":
                sys.exit()
            print(data.decode() + "\npl. input message: ", end="")

    def main():
        sock_fd = socket(AF_INET, SOCK_DGRAM)
        while True:
            str_name = input("pl. input your name: ")
            str_send_info = "L " + str_name
            sock_fd.sendto(str_send_info.encode(), ADDR)
            data, addr = sock_fd.recvfrom(2048)

            if data.decode() == "OK":
                break
            else:
                print(data.decode())

        print("Now You Have Entered Chat Room")

        pid = os.fork()
        if pid < 0:
            sys.exit("Error!")
        elif pid == 0:
            send_message(sock_fd, str_name)
        else:
            recv_message(sock_fd)

    if __name__ == "__main__":
        main()

    ################### Chat Room Server ###################

    from socket import *
    import os, sys

    # 地址
    ADDR = ("0.0.0.0", 12017)

    # 用户信息
    dict_user_info = {}

    def do_quit(sock_fd, name):
        str_msg = "%s out chat room" % name
        for item in dict_user_info:
            if item != name:
                sock_fd.sendto(str_msg.encode(), dict_user_info[item])
            else:
                sock_fd.sendto(b"quit", dict_user_info[item])
        # 删除用户信息
        del dict_user_info[name]

    def do_chat(sock_fd, name, msg):
        str_send_msg = "\n%s : %s" % (name, msg)
        for item in dict_user_info:
            if item != name:
                sock_fd.sendto(str_send_msg.encode(), dict_user_info[item])

    def do_login(sock_fd, name, addr):
        if name in dict_user_info or "admin" in name:
            sock_fd.sendto(b"\nNAME EXIST", addr)
            return

        # 发送返回信息
        sock_fd.sendto(b"OK", addr)
        # 记录用户信息
        dict_user_info[name] = addr

        # 向其他用户发送通知
        send_msg = "\nWelcome to %s enter chat room" % name
        for item in dict_user_info:
            if item != name:
                sock_fd.sendto(send_msg.encode(), dict_user_info[item])

    def do_request(sock_fd):
        while True:
            data, addr = sock_fd.recvfrom(1024)
            list_recv_info = data.decode().split(" ")
            if list_recv_info[0] == "L":
                do_login(sock_fd, list_recv_info[1], addr)
            elif list_recv_info[0] == "C":
                chat_msg = " ".join(list_recv_info[2:])
                do_chat(sock_fd, list_recv_info[1], chat_msg)
            elif list_recv_info[0] == "Q":
                if list_recv_info[1] not in dict_user_info:
                    sock_fd.sendto(b"quit", addr)
                    continue
                do_quit(sock_fd, list_recv_info[1])

    def super_admin_rights(sock_fd):
        """
            超级管理员发言
                核心：由于子进程的数据 父进程不能共享
                    采用 父进程像客户端一样，向子进程发送消息，
                        子进程统一处理请求的方案解决
        :param sock_fd:
        :return:
        """
        while True:
            str_msg = input("admin message: ")
            str_admin_msg = "C admin " + str_msg
            sock_fd.sendto(str_admin_msg.encode(), ADDR)

    def main():
        sock_fd = socket(AF_INET, SOCK_DGRAM)
        # 绑定地址
        sock_fd.bind(ADDR)

        pid = os.fork()
        if pid < 0:
            sys.exit("Error!")
        elif pid == 0:
            # 处理请求
            do_request(sock_fd)
        else:
            super_admin_rights(sock_fd)

    if __name__ == "__main__":
        main()
```

##### 基于 multiprocessing 模块创建进程

1.  创建流程

    -   将需要子进程处理的事件封装为函数
    -   通过Process类创建进程对象，关联函数
    -   可以通过进程对象对进程进行属性设置
    -   通过start启动进程
    -   通过join回收进程

2.  接口使用

    1.  Process()

        -   功能: 创建进程对象
        -   参数：
            -   target 绑定要执行的目标函数
            -   args 元组 给target函数位置传参
            -   kwargs 字典  给target函数关键字传参

    2.  p.start()

        -   功能 : 启动进程
            -   此时进程产生，将p绑定函数作为新进程的执行内容。

    3.  p.join([timeout])

        -   功能 ：阻塞等待回收进程
        -   参数 : 超时时间

    ```py
       # 基本使用
       import multiprocessing
       from time import sleep

       def func(sec,name):
           print("%s start"%name)
           sleep(sec)
           print("%s complete"%name)

       # p = multiprocessing.Process(target=func,args=(2,"zf"))
       p = multiprocessing.Process(target=func,kwargs={"name":"zf","sec":2})

       p.start()

       sleep(2)
       print("parent process")

       p.join()

       # 等价于 fork()
       # if pid == 0:
       #     func()
       #     os._exit()
       # else:
       # sleep(2)
       # print("parent process")
       #     wait()
    ```

3.  总结：

    -   multiprocessing创建进程同样是复制父进程的空间代码段，父子进程运行互不影响
    -   子进程只执行target绑定函数，其余均父进程执行
    -   Process创建进程中，往往父进程只用来创建和回收进程，具体事件由子进程完成
    -   multiprocessing创建的子进程中不能使用标准输入（无法使用 input... ）

    ```py
        # 同时开辟多个子进程
        from multiprocessing import Process
        from time import sleep
        import os

        def th1/2/3():
            sleep(3/2/4)
            print("th1/2/3")
            print(os.getppid(), "------------", os.getpid())

        things = [th1, th2, th3]
        jobs = []
        for th in things:
            p = Process(target=th)
            jobs.append(p)
            p.start()

        for i in jobs:
            i.join()
    ```

4.  进程对象属性

    -   p.name   获取进程名称
        -   `Process(..,name="..")` 可以在创建时传递name参数设置进程名称
    -   p.pid   进程PID号
    -   p.is_alive()  进程是否在生命周期
    -   p.daemon  设置父子进程的退出关系
        -   将该属性设置为True则父进程退出，其子进程也会退出
        -   要求必须在start()前设置
        -   不会和join一起使用

###### 进程池技术

1.  必要性

    -   进程的创建和销毁过程消耗的计算机资源较多
    -   当任务量众多，每个任务又比较小的时候，需要频繁创建销毁进程，对计算机压力较大
    -   进程池技术很好的解决了上述问题

2.  原理： 创建一定数量的进程来处理事件，事件处理完进程不退出而是继续处理其他事件，直到所有事件都处理完毕再一同销毁。增加进程的复用性，降低资源消耗。

3.  进程池实现

    -   from  multiprocessing  import  Pool

    -   pool = Pool(processes)

        -   功能： 创建进程池对象
        -   参数： 指定创建进程数量，默认根据系统自动判定

    -   pool.apply_async(func,args,kwds):

        -   功能 ： 将进程事件加入进程池
        -   参数 ：
            -   func 进程事件函数
            -   args  给进程事件元组传参
            -   kwds  给进程事件字典传参

    -   pool.close()

        -   功能: 关闭进程池
            -   不能将进程事件加入进程池

    -   pool.join()
        -   功能：回收进程池
            -   阻塞等待现有的进程池事件执行完毕

    ```py
        from multiprocessing import Pool
        from time import ctime, sleep

        def worker(msg):
            sleep(2)
            print(msg)

        # 主进程结束，子进程直接销毁 与 Process 不同
        pool = Pool()

        for i in range(20):
            msg = "this is %d" % i
            pool.apply_async(func=worker, args=(msg,))

        pool.close()
        pool.join()
    ```

###### 进程间通信（IPC）

1.  必要性： 进程空间独立，资源不共享，此时在进程间数据传输时，需要一定的方法手段

2.  进程间通信方法:

    -   管道  消息队列  共享内存  信号  信号量  套接字

3.  管道通信

    1.  通信原理：在内存中开辟管道空间，生成管道对象，多个进程使用同一管道进行读写操作

    2.  实现方法

        -   from  multiprocessing import Pipe

        -   fd1,fd2 = Pipe(duplex = True)

            -   功能: 创建管道
            -   参数：默认表示双向管道，False表示单向管道
            -   返回值：

                -   表示管道两端读写对象
                -   如果双向管道均可读写
                -   单向管道fd1只读(recv())，fd2只写(send())

        -   fd.recv()

            -   功能: 从管道读取内容
            -   返回值：读取到的内容

        -   fd.send(data)
            -   功能：向管道写入内容
            -   参数： 要写入的数据

    ```py
        from multiprocessing import Pipe, Process
        import os, time

        fd1, fd2 = Pipe()

        def fun(name):
            time.sleep(2)
            fd1.send({name: os.getpid()})

        jobs = []
        for i in range(5):
            p = Process(target=fun, args=(i,))
            jobs.append(p)
            p.start()

        for i in range(5):
            data = fd2.recv()
            print(data)

        for i in jobs:
            i.join()
    ```

4.  消息队列

    1.  通信原理：在内存中建立队列模型，进程通过队列存取消息实现通信

    2.  实现方法

        -   `from multiprocessing import Queue`

        -   q = Queue(maxsize = 0)

            -   功能 ： 创建消息队列
            -   参数 ： 最多存放多少个消息

        -   q.put(data,[block,timeout])

            -   功能： 向队列存入消息
            -   参数：
                -   data  要存入的内容
                -   block
                    -   False 为非阻塞
                    -   timeout  超时时间

        -   data = q.get([block,timeout])

            -   功能: 从队列中取出消息
            -   参数：
                -   block
                    -   False 为非阻塞
                    -   timeout  超时时间

        -   q.full() 判断队列是否为满
        -   q.empty()  判断队列是否为空
        -   q.qsize()  获取队列中消息个数
        -   q.close()  关闭队列

    ```py
        from multiprocessing import Queue, Process
        from random import randint
        from time import sleep

        q = Queue(5)

        def request():
            for i in range(20):
                x = randint(0, 100)
                y = randint(0, 100)
                q.put((x, y))

        def handle():
            while True:
                sleep(0.5)
                try:
                    x, y = q.get(timeout=3)
                except:
                    break
                else:
                    print("%d + %d = %d" % (x, y, x + y))

        p1 = Process(target=request)
        p2 = Process(target=handle)

        p1.start()
        p2.start()
        p1.join()
        p2.join()
    ```

5.  共享内存

    1.  通信原理：在内存中开辟一块空间，进程可以写入和读取内容，但是每次写入内容都会覆盖之前内容

    2.  实现方法

        -   `from multiprocessing import Value,Array`

        -   obj = Value(ctype,data)

            -   功能: 创建共享内存
            -   参数:

                -   ctype  共享内存类型  'i'  'f'  'c'(字符，只能存字节串)
                -   data   共享内存初始数据

                ![Python-Net_Ctype](http://localhost:9710/blog/Python/Python-Net_Ctype.png)

            -   obj.value 对该属性修改查看即共享内存读写


        -   obj = Array(ctype,data)
            -   功能： 创建共享内存
            -   参数：
                -   ctype 共享内存类型
                -   data  其他数据类型表示开辟空间存放的初始化数据，整数表示共享内存开辟数据元素个数

            -   obj进行遍历或者索引方式获取值，也可以通过索引直接赋值
            -   obj.value 用于整体打印共享内存中字节串

            ```py
                from multiprocessing import Process, Array
                import time
                import random

                shall_mer = Array("i", 5 )

                def fun1():
                    for i in shall_mer:
                        print(i)

                p = Process(target=fun1)

                g.start()
                p.join()

                print(money.value)
            ```

6.  本地套接字

    1.  功能:用于本地两个程序之间进行数据的收发
    2.  套接字文件:用于本地套接字之间通信时,进行数据传输的介质。
    3.  创建本地套接字流程
        1.  创建本地套接字：`sock_fd = socket(AF_UNIX, SOCK_STREAM)`
        2.  绑定套接字文件：`sock_fd.bind(url_file)`
        3.  监听、接收客户端连接，收发消息
            -   `listen() -> accept() -> recv() / send()`

    ```py
        ################### recv ###################

        from socket import *
        import os

        sock_file = "./sock"

        if os.path.exists(sock_file):
            os.remove(sock_file)

        sock_fd = socket(AF_UNIX, SOCK_STREAM)
        sock_fd.bind(sock_file)

        sock_fd.listen(3)

        while True:
            conn_fd, addr = sock_fd.accept()
            while True:
                data = conn_fd.recv(1024)
                if not data:
                    break
                print(data.decode())
            conn_fd.close()
        sock_fd.close()

        ################### send ###################

        from socket import *

        sock_file = "./sock"
        sock_fd = socket(AF_UNIX, SOCK_STREAM)
        sock_fd.connect(sock_file)

        while True:
            msg = input(">>>")
            if not msg:
                break
            sock_fd.send(msg.encode())
        sock_fd.close()
    ```

7.  信号量（信号灯集）

    1.  通信原理：给定一个数量对多个进程可见，多个进程都可以操作数量的增减，并根据数量决定行为

    2.  实现方法

        -   from multiprocessing import  Semaphore

        -   sem = Semaphore(num)

            -   功能：创建信号量对象
            -   参数： 信号量初始值
            -   返回值： 信号量对象

        -   sem.acquire()  消耗一个信号量，当信号量为0时会阻塞

        -   sem.release()  增加一个信号量

        -   sem.get_value()  获取信号量值

    ```py
        from multiprocessing import Semaphore, Process
        from time import sleep
        import os
        # 服务器程序最多允许三个进程同时执行事件
        sem = Semaphore(3)

        def fun():
            print("%d ready" % os.getpid())
            sem.acquire()
            print("%d start" % os.getpid())
            sleep(3)
            print("%d end" % os.getpid())
            sem.release()

        jobs = []

        for i in range(5):
            p = Process(target=fun)
            jobs.append(p)
            p.start()

        for i in jobs:
            i.join()
    ```

8.  注意：当在父进程中创建对象（文件对象，套接字对象，进程间通信对象），子进程从父进程中拷贝对象时父子进程对该对象的使用会有属性的相互影响。如果在父子进程中各自创建，则无影响。

    -   如果父进程中打开文件,创建进程通信对象或者创建套接字,子进程从父进程内存空间获取这些内容,那么父子进程对该对象的操作会有一定的属性关联。

* * *

#### 线程(Thread)

1.  什么是线程

    1.  线程被称为轻量级的进程
    2.  线程也是多任务编程方法，可以使用计算机多核资源
    3.  线程是系统分配内核的最小单元
    4.  线程可以理解为进程中的任务分支程序

2.  线程特征

    1.  一个进程可以包含多个线程
    2.  线程也是一个运行过程，消耗计算机资源
    3.  一个进程中的所有线程共享这个进程资源
    4.  多个线程运行互不影响，各自执行
    5.  线程的创建和消耗消耗资源远小于进程（消耗资源约进程消耗的二十分之一）
    6.  线程也有自己的特有属性特征命令集，id等

3.  threading模块创建线程

    1.  创建线程对象

        -   from threading import Thread

            -   t = Thread()
            -   功能: 创建线程对象
            -   参数：
                -   target 绑定的线程函数
                -   arges 元组  给线程函数位置传参
                -   kwargs 字典 给线程函数关键字传参

    2.  启动线程：t.start()

    3.  阻塞等待回收线程：t.join([timeout])

    ```py
        from threading import Thread
        from time import sleep

        def fun(sec, name):
            print("thread params")
            sleep(sec)
            print(name, "complete")

        jobs = []

        for i in range(5):
            t = Thread(target=fun, args=(2,), kwargs={"name": "T%d" % i})
            jobs.append(t)
            t.start()

        for t in jobs:
            t.join()
    ```

4.  线程对象属性

    -   t.name  线程名称
    -   t.setName()  设置线程名称
    -   t.getName()  获取线程名称

    -   t.is_alive()  查看线程是否在生命周期

    -   t.daemon  设置主线程和分支线程的退出关系
    -   t.setDaemon()  设置daemon属性值
    -   t.isDaemon()  查看daemon属性值
        -   设置daemon为True 此时主线程退出分支线程也会退出。设置在start前完成，不和join同用

5.  自定义线程类

    1.  创建步骤

        1.  继承Thread类
        2.  重写 init 方法添加自己的属性，使用 super 加载父类属性
        3.  重写run方法

    2.  使用
        1.  实例化对象
        2.  调用start自动执行run
        3.  join回收线程

    ```py
        from threading import Thread
        from time import sleep, ctime

        class MyThread(Thread):
            def __init__(self, target=None, args=(), kwargs={}, name="MyThread"):
                super().__init__()
                self.target = target
                self.args = args
                self.kwargs = kwargs
                self.name = name

            def run(self):
                self.target(*self.args, **self.kwargs)

        def player(sec, song):
            for i in range(2):
                print("Playing %s : %s" % (song, ctime()))
                sleep(sec)

        t = MyThread(target=player, args=(3,), kwargs={"song": "zf"}, name="answer")
        t.start()
        t.join()
    ```

##### 线程通信

1.  通信方法：线程间使用全局变量进行通信

2.  共享资源的争夺

    1.  共享资源：多个进程或者线程都可以操作的资源称为共享资源.对共享资源操作的代码段叫临界区

    2.  影响：对共享资源的无序操作可能会带来数据的混乱或者操作错误。此时需要同步互斥机制协调操作顺序。

    3.  同步互斥机制

        -   同步：同步是一种协作关系，为完成操作，多进程或者线程间形成一种协调，按照必要的步骤执行操作。

        ![Python-Net_Synchronize](http://localhost:9710/blog/Python/Python-Net_Synchronize.png)

        -   互斥：互斥是一种制约关系，当一个进程或者线程抢占到资源时进行加锁处理，其他进程线程就无法操作资源，直到解锁后才能操作

        ![Python-Net_Mutex](http://localhost:9710/blog/Python/Python-Net_Mutex.png)

    4.  线程互斥方法

        -   线程 Event

            -   from threading  import Event

            -   e = Event()  创建event对象

            -   e.wait([timeout]) 阻塞等待直到e被set

            -   e.set() 设置e ，使wait结束阻塞

            -   e.clear() 清除e的设置，wait会阻塞

            -   e.is_set()  判断e的状态

        ```py
            from threading import Event, Thread

            s = None
            e = Event()

            def fun1():
                print("this. is fun1")
                global s
                s = "-> fun1"
                e.set()

            t = Thread(target=fun1)

            t.start()
            e.wait()
            if s == "-> fun1":
                print("Yes")
            else:
                print("No")
            t.join()
        ```

        -   线程锁 Lock

            -   from  threading  import Lock

            -   lock = Lock()    创建锁
            -   lock.acquire()   上锁  如果lock已经上锁会阻塞
            -   lock.release()   解锁

            -   with  lock:   上锁
                -   ...
                    -   with 代码块结束自动解锁

            ```py
                from threading import Thread, Lock

                a = b = 0
                lock = Lock()

                def value():
                    while True:
                        lock.acquire()
                        if a != b:
                            print("a = %d, b = %d" % (a, b))
                        lock.release()

                t = Thread(target=value)
                t.start()
                while True:
                    with lock:
                        a += 1
                        b += 1
                t.join()
            ```

            -   死锁及其处理

                1.  定义

                    -   死锁是指两个或两个以上的线程在执行过程中,由于竞争资源或者由于彼此通信而造成的一种阻塞的现象,若无外力作用,它们都将无法推进下去。此时称系统处于死锁状态或系统产生了死锁。

                    ![Python-Net_DeadLock](http://localhost:9710/blog/Python/Python-Net_DeadLock.png)

                2.  死锁产生条件

                    -   死锁发生的必要条件

                        -   互斥条件:指线程对所分配到的资源进行排它性使用,即在一段时间内某资源只由一个进程占用。如果此时还有其它进程请求资源,则请求者只能等待,直至占有资源的进程用毕释放。
                        -   请求和保持条件:指线程已经保持至少—个资源,但又提出了新的资源请求,而该资源已被其它进程占有,此时请求线程阻塞,但又对自己已获得的其它资源保持不放。
                        -   不剥夺条件:指线程已获得的资源,在未使用完之前,不能被剥夺,只能在使用完时由自己释放通常CPU内存资源是可以被系统强行调配剥夺的。
                        -   环路等待条件:指在发生死锁时,必然存在一个线程—资源的环形链,即进程集合(T0,T1,T2,…,Tn中的TO正在等待一个T1占用的资源:T1正在等待T2占用的资源,…Tn正在等待已被T0占用的资源。

                    -   死锁的产生原因
                        -   简单来说造成死锁的原因可以概括成三句话:
                            -   当前线程拥有其他线程需要的资源
                            -   当前线程等待其他线程已拥有的资源
                            -   都不放弃自己拥有的资源

                3.  如何避免死锁

                    -   死锁是我们非常不愿意看到的一种现象,我们要尽可能避免死锁的情况发生。通过设置某些限制条件,去破坏产生死锁的四个必要条件中的一个或者几个,来预防发生死锁。预防死锁是种较易实现的方法。但是由于所施加的限制条件往往太严格,可能会导致系统资源利用率。
                    -   使用定时锁

                        ```py
                            import threading
                            import time

                            class Account:

                                def __init__(self, _id, balance, lock):
                                    self._id = _id
                                    self.balance = balance
                                    self.lock = lock

                                def withdraw(self, amount):
                                    self.balance -= amount

                                def deposit(self, amount):
                                    self.balance += amount

                                def get_balance(self):
                                    return self.balance

                            def transfer(from_, to, account):
                                if from_.lock.acquire():
                                    from_.withdraw(account)
                                    time.sleep(0.5) # 死锁产生
                                    if to.lock.acquire():
                                        to.deposit(account)
                                        to.lock.release()
                                    from_.lock.release()
                                print("transfer complete")

                            zf = Account("zf", 5000, threading.Lock())
                            cf = Account("cf", 8000, threading.Lock())

                            t1 = threading.Thread(target=transfer, args=(zf, cf, 1500))
                            t2 = threading.Thread(target=transfer, args=(cf, zf, 1000))
                            t1.start()
                            time.sleep(2) # 死锁解决
                            t2.start()
                            t1.join()
                            t2.join()

                            print("zf", zf.get_balance())
                            print("cf", cf.get_balance())
                        ```

                    -   使用重入锁 RLock() 用法同Lock。 RLock内部维护着一个 Lock和一个 counter变量, counter记录了 acquire的次数, 从而使得资源可以被多次 acquire。直到一个线程所有的 acquire 都被 Release,其他的线程才能获得资源。

                        ```py
                            from threading import Thread, RLock
                            import time

                            num = 0
                            lock = RLock()

                            class MyThread(Thread):
                                def fun1(self):
                                    global num
                                    with lock:
                                        num -= 1

                                def fun2(self):
                                    global num
                                    if lock.acquire():
                                        num += 1
                                        if num > 5:
                                            self.fun1()
                                        print("num = ", num)
                                        lock.release()

                                def run(self):
                                    time.sleep(2)
                                    self.fun2()

                            for i in range(10):
                                t = MyThread()
                                t.start()
                        ```

##### Python线程的GIL问题

1.  python GIL问题 （全局解释器锁）

    1.  什么是GIL ：由于python解释器设计中加入了全局解释器锁，导致python解释器同时只能解释一个线程，大大降低了python的执行效率。

    2.  导致后果： 因为遇到阻塞python线程会主动让出解释器，所以python线程在高延迟，多阻塞的情况下可以提高运行效率，其他情况并不适合。

    3.  GIL问题建议
        -   修改c解释器
        -   尽量使用进程完成无阻塞的并发行为
        -   不使用c作为解释器  （java  c#）

2.  结论:在无阻塞状态下,多线程程序和单线程程序执行效率几乎差不多,甚至还不如单线程效率。但是多进程运行相同内容却可以有明显的效率提升。

#### 进程线程的区别联系

1.  区别联系

    1.  两者都是多任务编程方式,都能使用计算机多核资源
    2.  进程的创建删除消耗的计算机资源比线程多
    3.  进程空间多里,数据互不干扰,有专门通信方法;线程使用去哪局变量通信
    4.  一个进程可以有多个分支线程,两者有包含关系
    5.  多个线程共享进程资源,在共享资源操作时往往需要同步互斥处理
    6.  进程线程在系统中都有自已的特有属性标志,如ID、代码段,命令集等

2.  使用场景

    1.  任务场景:如果是相对独立的任务模块,可能使用多进程,如果是多个分支共同形成一个整体任务可能用多线程
    2.  项目结构:多中编程语言实现不同任务模块,可能是多进程,或者前后端分离应该各自为一个进程
    3.  难以程度:通信难度,数理的复度来判断用进程间通信还是同步互际方法

#### 并发网络通信模型

1.  通信模型分类

    1.  循环网络模型：循环接收客户端请求，处理请求，同时只能处理一个客户端请求任务，处理完再进行下一个。

        -   优点：实现简单，占用资源少
        -   缺点：无法同时处理多个请求，效率不高

        -   适用情况： 客户端不会长期占有服务器，任务比较小，任务量不大。udp比tcp更加适合循环模型

    2.  IO并发网络模型：利用IO多路复用、异步IO等技术，同时处理多个客户端IO任务请求。

        -   优点：资源消耗少，能同时处理多个IO行为
        -   缺点：只能处理并发产生的IO事件，无法处理cpu计算
        -   适用情况：HTTP请求，网络传输等IO行为

    3.  多进程/线程网络并发模型:每当一个客户端连接服务器,就创建个新的进程/线程为该客户端服务,客户端退出时再销毁该进程/线程

        -   优点： 同时满足多个客户端长期占有服务器需求，可以处理各种请求
        -   缺点： 资源消耗较大

        -   适用情况：客户端同时连接量较少，客户端请求比较复杂，处理时间较长，配合较强的服务器部署技术（负载均衡，集群技术，分布式处理，缓存队列等）


        -   基于 fork的多进程网络并发

            -   创建监听套接字
            -   循环等待客户端连接
            -   有客户端连接创建新的进程处理客户端请求
            -   原进程继续等待其他客户端连接
            -   如果客户端退出，则销毁对应进程

        ```py
            from socket import *
            import os, sys
            import signal

            def handle(conn_fd):
                print("client: ", conn_fd.getpeername())
                while True:
                    data = conn_fd.recv(1024)
                    if not data:
                        break
                    print(data.decode())
                    conn_fd.send(b"OK")
                conn_fd.close()

            HOST = "0.0.0.0"
            PORT = 12016
            ADDR = (HOST, PORT)

            # 创建监听套接字
            sock_fd = socket()
            sock_fd.setsockopt(SOL_SOCKET, SO_REUSEADDR, 1)  # 设置端口的重用
            sock_fd.bind(ADDR)
            sock_fd.listen(3)

            # 处理僵尸进程
            signal.signal(signal.SIGCHLD, signal.SIG_IGN)
            print("listen 12016")

            while True:
                try:
                    conn_fd, addr = sock_fd.accept()
                except KeyboardInterrupt:
                    sys.exit("server exit")
                except Exception as e:
                    print(e)
                    continue

                pid = os.fork()

                if pid == 0:
                    sock_fd.close()
                    handle(conn_fd)
                    os._exit(0)
                else:
                    conn_fd.close()
        ```

        -   基于 threading的多线程网络并发

            -   创建监听套接字
            -   循环接收客户端连接请求
            -   当有新的客户端连接创建线程处理客户端请求
            -   主线程继续等待其他客户端连接
            -   当客户端退出,则对应分支线程退出

        ```py
            from threading import Thread
            import sys
            from socket import *

            def handle(conn_fd):
                print("client: ", conn_fd.getpeername())
                while True:
                    data = conn_fd.recv(1024)
                    if not data:
                        break
                    print(data.decode())
                    conn_fd.send(b"OK")
                conn_fd.close()

            HOST = "0.0.0.0"
            PORT = 12016
            ADDR = (HOST, PORT)

            sock_fd = socket()
            sock_fd.setsockopt(SOL_SOCKET, SO_REUSEADDR, 1)
            sock_fd.bind(ADDR)
            sock_fd.listen(3)

            while True:
                try:
                    conn, addr = sock_fd.accept()
                except KeyboardInterrupt:
                    sys.exit("server exit")
                except Exception as e:
                    print(e)
                    continue

                t = Thread(target=handle, args=(conn,))
                t.setDaemon(True)  # 主线程退出，分支线程随之退出
                t.start()
        ```

##### ftp 实例

```py
    ################### ftp_server ###################
    from socket import *
    from threading import Thread
    from time import sleep
    import os

    HOST = "0.0.0.0"
    PORT = 12016
    ADDR = (HOST, PORT)
    FTP = "./ftp/"

    class FtpServer:
        """
            服务端操作文件类
            conn_fd: 套接字
            FTP_PATH: 所操作目录地址
                查看目录列表标识符: "L"
                下载文件标识符: "G"
                上传文件标识符: "P"
                退出:"Q"
        """

        def __init__(self, conn_fd, FTP_PATH):
            self.conn_fd = conn_fd
            self.FTP_PATH = FTP_PATH

        # 查询目录文件
        def do_list(self):
            # 判断目录内是否存在文件
            files = os.listdir(self.FTP_PATH)
            if not files:
                self.conn_fd.send("this is empty!".encode())
                return
            else:
                self.conn_fd.send(b"OK")
                sleep(0.1)

            # 避免粘包，采用 \n 做边界，一次发送目录信息
            fs = ""
            for file in files:
                if file[0] != "." and os.path.isfile(self.FTP_PATH + file):
                    fs += file + "\n"
            self.conn_fd.send(fs.encode())

        # 下载文件
        def get_file(self, file_name):
            # 通过打开文件是否异常 判断文件是否存在
            try:
                fd = open(self.FTP_PATH + file_name, "rb")
            except Exception:
                self.conn_fd.send(b"File Not Exist")
                return
            else:
                self.conn_fd.send(b"OK")
                sleep(0.1)  # 连续发送,防止粘包

            # 下载文件核心步骤
            while True:
                read_data = fd.read(1024)
                if not read_data:
                    sleep(0.1)
                    self.conn_fd.send(b"##")
                    break
                self.conn_fd.send(read_data)

        # 上传文件
        def put_file(self, file_name):
            # 判断文件是否存在(不允许覆盖文件)
            if os.path.exists(self.FTP_PATH + file_name):
                self.conn_fd.send(b"File Exist")
                return
            self.conn_fd.send(b"OK")
            # 上传核心步骤
            fd = open(self.FTP_PATH + file_name, "wb")
            while True:
                file_data = self.conn_fd.recv(1024)
                if file_data == b"##":
                    break
                fd.write(file_data)
            fd.close()

    def do_require(conn_fd):
        cls = conn_fd.recv(1024).decode()
        FTP_PATH = FTP + cls + "/"
        ftp = FtpServer(conn_fd, FTP_PATH)
        while True:
            data = conn_fd.recv(1024).decode()
            if not data or data[0] == "Q":
                return
            elif data[0] == "L":
                ftp.do_list()
            elif data[0] == "G":
                file_name = data.split(" ")[-1]
                ftp.get_file(file_name)
            elif data[0] == "P":
                file_name = data.split(" ")[-1]
                ftp.put_file(file_name)

    def main():
        sock_fd = socket()
        sock_fd.setsockopt(SOL_SOCKET, SO_REUSEADDR, 1)
        sock_fd.bind(ADDR)
        sock_fd.listen(3)
        print("Listen the port 12016 ...")

        while True:
            try:
                conn_fd, addr = sock_fd.accept()
            except KeyboardInterrupt:
                print("server exit")
                return
            except Exception as e:
                print(e)
                continue
            print("Link client: ", addr)

            client = Thread(target=do_require, args=(conn_fd,))
            client.setDaemon(True)
            client.start()

    if __name__ == '__main__':
        main()

    ################### ftp_client ###################
    from socket import *
    from time import sleep
    import sys

    class FtpClient:
        """
            客户端操作文件类
            sock_fd: 套接字
                查看目录列表标识符: "L"
                下载文件标识符: "G"
                上传文件标识符: "P"
                退出:"Q"
        """

        def __init__(self, sock_fd):
            self.sock_fd = sock_fd

        # 查询目录列表
        def do_list(self):
            self.sock_fd.send(b"L")
            data = self.sock_fd.recv(128).decode()
            if data == "OK":
                request_msg = self.sock_fd.recv(4096)
                print(request_msg.decode())
            else:
                print(data)

        # 下载文件
        def get_file(self, filename):
            self.sock_fd.send(("G " + filename).encode())
            recv_signal = self.sock_fd.recv(128).decode()
            if recv_signal == "OK":
                # 下载文件核心步骤
                fd = open(filename, "wb")
                while True:
                    recv_data = self.sock_fd.recv(1024)
                    # 服务端不会发空，需要结束标志
                    if recv_data == b"##":
                        break
                    fd.write(recv_data)
                fd.close()
            else:
                print(recv_signal)

        # 上传文件
        def put_file(self, file_url):
            # 通过异常做文件判断
            try:
                fd = open(file_url, "rb")
            except Exception:
                print("file name exist, pl. input again!")
                return
            # 避免将路径上传到服务端
            file_name = file_url.split("/")[-1]

            self.sock_fd.send(("P " + file_name).encode())
            recv_signal = self.sock_fd.recv(128).decode()
            if recv_signal == "OK":
                # 上传文件核心步骤
                while True:
                    send_data = fd.read(1024)
                    if not send_data:
                        sleep(0.1)
                        self.sock_fd.send(b"##")
                        break
                    self.sock_fd.send(send_data)
                fd.close()
            else:
                print(recv_signal)

        def do_quit(self):
            self.sock_fd.send(b"Q")
            self.sock_fd.close()
            sys.exit("Thanks to use")

    def do_request(sock_fd):
        ftp = FtpClient(sock_fd)

        while True:
            print("\n======== Command  Options ========")
            print("************** list **************")
            print("************ get file ************")
            print("************ put file ************")
            print("************** quit **************")
            print("==================================")

            cmd = input("pl. input command : ")
            if cmd.strip() == "list":
                ftp.do_list()
            elif cmd.strip() == "quit":
                ftp.do_quit()
            elif cmd.strip()[:3] == "get":
                file_name = cmd.split(" ")[-1]
                ftp.get_file(file_name)
            elif cmd.strip()[:3] == "put":
                file_name = cmd.split(" ")[-1]
                ftp.put_file(file_name)

    def main():
        addr = ("127.0.0.1", 12016)
        sock_fd = socket()
        try:
            sock_fd.connect(addr)
        except Exception as e:
            print("Server Connect Error")
            return
        else:
            print("""
                ************************
                   Data  Image  File
                ************************
            """)
            cls = input("pl. input document type: ")
            if cls not in ["Data", "Image", "File"]:
                print("input Error!")
                return
            else:
                sock_fd.send(cls.encode())
                do_request(sock_fd)

    if __name__ == '__main__':
        main()
```

#### IO 并发

1.  IO 分类

    -   阻塞IO ，非阻塞IO，IO多路复用 ，异步IO

2.  阻塞IO

    -   定义： 在执行IO操作时由于不满足某些条件形成的阻塞形态。阻塞IO时IO的默认行为。
    -   效率：阻塞IO是一种效率很低的IO。逻辑简单

    -   阻塞情况

        -   因为某种条件没有达到造成的函数阻塞

            -   e.g.  accept   input   recv

        -   处理IO的时间较长产生的阻塞情况
            -   e.g. 网络传输，大文件的读写过程

3.  非阻塞IO

    -   定义 ： 通过修改IO的属性行为，使原本阻塞的IO变为非阻塞的状态(一般指的第一种阻塞情况)

    -   设置套接字为非阻塞IO

        -   sockfd.setblocking(bool)
        -   功能: 设置套接字为非阻塞IO
        -   参数：True表示套接字IO阻塞，False表示非阻塞

    -   设置超时检测 ： 设置一个最长阻塞时间,超过该时间后则不再阻塞等待。

        -   sockfd.settimeout(sec)
        -   功能: 设置套接字超时时间
        -   参数：超时时间

        ```py
            from socket import *
            from time import sleep, ctime

            fd = open("log.txt", "a")

            sock_fd = socket()
            sock_fd.bind(("0.0.0.0", 12016))
            sock_fd.listen(3)

            # sock_fd.setblocking(False)
            sock_fd.settimeout(3)

            while True:
                print("Waiting For Connect ...")
                try:
                    conn_fd, addr = sock_fd.accept()
                except (BlockingIOError, timeout) as e:
                    sleep(2)
                    fd.write("%s: %s\n" % (ctime(), e))
                    fd.flush()
                else:
                    data = conn_fd.recv(1024).decode()
                    print(data)
        ```

4.  IO多路复用

    -   定义：同时监控多个IO事件，当哪个IO事件准备就绪就执行哪个IO。以此形成可以同时处理多个IO的行为，避免一个IO阻塞造成的其他IO无法执行，提高IO执行效率。

    -   具体方案：

        -   select : windows  linux   unix
        -   poll : linux  unix
        -   epoll : linux

    -   python实现 ： select

        -   rs,ws,xs=select(rlist, wlist, xlist[, timeout])
        -   功能 ：监控多个IO事件，阻塞等待IO发生
        -   参数 ：
            -   rlist 列表  存放关注的等待发生的IO事件
            -   wlist 列表  存放要主动处理的IO事件
            -   xlist 列表  存入发生异常时要处理的IO
            -   timeout : 超时时间
        -   返回值 ：

            -   rs 列表  rlist 中准备就绪的IO
            -   ws 列表  wlist 中准备就绪的IO
            -   xs 列表  xlist 中准备就绪的IO

        -   步骤：

            1.  将关注的IO放入对应的监控类别列表
            2.  通过 select函数进行监控
            3.  遍历 select返回值列表,确定就绪IO事件
            4.  处理发生的IO事件

        -   注意：
            -   wlist中如果有IO事件，则select会立即返回给ws
            -   处理IO事件过程中不要出现死循环等长期占有服务端情况
            -   IO多路复用消耗资源较少，效率较高

        ```py
            from socket import *
            from select import select

            sock_fd = socket()
            sock_fd.setsockopt(SOL_SOCKET, SO_REUSEADDR, 1)
            sock_fd.bind(("0.0.0.0", 12016))
            sock_fd.listen(3)

            rlist = [sock_fd]
            wlist = []
            xlist = []

            while True:
                rs, ws, xs = select(rlist, wlist, xlist)
                for r in rs:
                    if r is sock_fd:
                        conn_fd, addr = sock_fd.accept()
                        print("Connect From", addr)

                        rlist.append(conn_fd)  # 加入新的关注IO
                    else:
                        data = r.recv(1024)
                        if not data:
                            rlist.remove(r)
                            r.close()
                            continue
                        print(data)
                        # r.send(b"OK")
                        wlist.append(r)

                for w in ws:
                    w.send(b"OK")
                    wlist.remove(w)
                for x in xs:
                    pass
        ```

    -   python实现 ： poll

        -   位运算：

            -   定义 ：将整数转换为二进制，按二进制位进行运算
            -   运算符号：低位 -> 高 两两进行运算  0假，1真
                -   `&`   按位与 （一假则假）
                    -   某属性是否有某特性（`xx & 固定值`）
                -   `|`   按位或 （一真则真）
                    -   增加属性（`xx | 固定值`）
                -   `^`   按位异或 （相同为0不同为1）
                -   `<<`  左移
                -   `>>`  右移
            -   e.g.

                -   14  -->  01110
                -   19  -->  10011

                -   `14 & 19` = 00010 = 2    一0则0
                -   `14 | 19` = 11111 = 31   一1则1
                -   `14 ^ 19` = 11101 = 29  相同为0不同为1
                -   `14 << 2` = 111000 = 56  向左移动右侧补0
                -   `19 >> 2` = 100 = 4   向右移动去掉低位

        -   p = select.poll()

            -   功能： 创建 poll 对象
            -   返回： poll 对象

        -   p.register(fd,event)
            -   功能：注册关注的IO事件（对象）
            -   参数：
                -   fd 要关注的IO
                -   event 要关注的IO事件类型
                    -   常用类型：
                        -   POLLIN 读IO事件（rlist）
                        -   POLLOUT 读IO事件（wlist）
                        -   POLLERR 异常IO （xlist）
                        -   POLLHUP 断开连接
                        -   e.g.: p.register(sock_fd,POLLIN|POLLERR)
        -   p.unregister(fd)
            -   功能：取消对IO的关注
            -   参数：IO对象或者IO对象的fileno（文件描述符）
        -   events = p.poll()
            -   功能：阻塞等待监控的IO事件发生
            -   返回值：返回发生的IO
                -   格式：[(fileno,event),()...]
                -   每个元组为一个就绪IO,元组第一项是该 IO 的fileno，第二项就绪IO的就绪事件。

        ```py
            from socket import *
            from select import *

            sock_fd = socket()
            sock_fd.bind(("0.0.0.0", 12016))
            sock_fd.listen(3)

            p = poll()

            fd_map = {
                sock_fd.fileno(): sock_fd
            }

            p.register(sock_fd, POLLIN | POLLERR)

            while True:
                events = p.poll()
                for fd, event in events:
                    if fd == sock_fd.fileno():
                        conn_fd, addr = fd_map[fd].accept()
                        print("connect from ", addr)
                        p.register(conn_fd, POLLIN | POLLHUP)
                        fd_map[conn_fd.fileno()] = conn_fd
                    # elif event & POLLHUP:  # 断开事件发生, POLLIN 也会就绪
                    #     print("client exit")
                    #     p.unregister(fd)
                    #     fd_map[fd].close()
                    #     del fd_map[fd]
                    elif event & POLLIN:
                        data = fd_map[fd].recv(1024)
                        if not data:
                            p.unregister(fd)
                            fd_map[fd].close()
                            del fd_map[fd]
                            continue
                        print(data.decode())
                        fd_map[fd].send(b"OK")
        ```

    -   python实现 ： epoll
        -   使用方法：与 poll 基本相同
            -   生成对象 改为 epoll()
            -   所有事件类型改为 EPOLL 类型
            -   p.poll()不需要改
        -   epoll特点
            -   epoll 效率比 select poll 要高
            -   epoll 监控 IO 数量 比 selec poll 要多
            -   epoll 的触发方式比 poll 要多（EPOLLET边缘触发）

#### 协程技术

> 在应用层 通过函数间的暂停跳转实现多任务“同时”操作,消耗较少的资源

1.  定义：纤程，微线程。是为非抢占式多任务产生子程序的计算机组件。协程允许不同入口点在不同位置开始或者暂停，简单来说，协程就是可以暂停执行的函数
2.  协程本质就是记录一个函数的上下文栈帧,协程调度切换时会将记录的上下文保存,在切换回来时进行调取,恢复原有的执行内容,以便从上一次执行位置继续执行
    -   yield 在python中时实现原生协程的关键字
3.  协程优缺点

    -   优点
        -   协程完成多任务占用计算资源很少
        -   由于协程的多任务切换在应用层完成,因此切换开销少
        -   协程为单线程程序,无需进行共享资源同步互斥处理
    -   缺点
        -   协程的本质是一个单线程,无法利用计算机多核资源

4.  标准库协程的实现

    -   python3.5以后,使用标准库 asyncio和 async/await语法来编写并发代码。 asyncio库通过对异步IO行为的支持完成 python的协成调用

    -   同步是指完成事务的逻辑,先执行第一个事务,如果阻塞了,会一直等待,直到这个事务完成,再执行第二个事务,顺序执行
    -   异步是和同步相对的,异步是指在处理调用这个事务的之后,不会等待这个事务的处理结果,直接处理第二个事务去了,通过状态、通知、回调来通知调用者处理结果。

    -   虽然官方说 asencio是未来的开发方向,但是由于其生态不够丰富,大量的客户端不支持 awaitable需要自己去封装,所以在使用上存在缺陷。更多时候只能使用已有的异步库( asyncio等),功能有限

    ```py
        import asyncio
        import time

        now = lambda: time.time()

        async def do_work(x):
            print("waiting", x)
            await asyncio.sleep(x)  # time.sleep() 不能自动阻塞调整，不够丰富
            return "Done after %s" % x

        start = now()

        cor1 = do_work(1)
        cor2 = do_work(2)
        cor3 = do_work(3)

        # 将协程对象生成一个可轮寻操作的对象列表
        tasks = [
            asyncio.ensure_future(cor1),
            asyncio.ensure_future(cor2),
            asyncio.ensure_future(cor3),
        ]
        # 得到 轮寻对象，调用 run 启动协程 执行
        loop = asyncio.get_event_loop()
        loop.run_until_complete(asyncio.wait(tasks))

        print("Time : ", now() - start)
    ```

5.  协程模块

    -   greenlet  （day9/greelet_test）

        -   sudo  pip3 install  greenlet
        -   函数
            -   greenlet.greenlet(func)
                -   功能: 创建协程对象
                -   参数：协程函数
            -   g.switch()
            -   功能: 选择执行的协程

        ```py
            from greenlet import greenlet

            def fun1():
                print("start fun1")
                gr2.switch()
                print("end fun1")
                gr2.switch()

            def fun2():
                print("start fun2")
                gr1.switch()
                print("end fun2")

            gr1 = greenlet(fun1)
            gr2 = greenlet(fun2)

            gr1.switch()
        ```

    -   gevent  （day9/gevent_test）

        1.  安装：  sudo  pip3 install gevent

        2.  函数

        -   gevent.spawn(func,argv,...)

            -   功能 ： 生成协程对象（将函数封装成协程）
            -   参数 ：
                -   func  协程函数
                -   argv  协程函数参数(不定参)
            -   返回值： 协程对象

        -   gevent.joinall(list,[timeout])

            -   功能: 阻塞等待协程执行完毕
            -   参数: list  协程对象列表

        -   gevent.sleep(sec)

            -   功能： gevent睡眠阻塞

            -   gevent协程只有遇到gevent标记的阻塞行为才会自动跳转

        ```py
            import gevent

            def foo(a, b):
                print("foo running ", a, "----", b)
                gevent.sleep(2)
                print("Foo again")

            def bar():
                print("bar running ")
                gevent.sleep(3)
                print("Bar again")

            f = gevent.spawn(foo, 1, "hello")
            g = gevent.spawn(bar)

            gevent.joinall([f, g])
        ```

        3.  monkey插件

        -   功能：在 gevent协程中,协程只有遇到 gevent指定类型的阻塞才能跳转到其他协程,因此,我们希望将普通的lO阻塞行为转换为可以触发 gevent协程跳转的阻塞,以提高执行效率。
        -   转换方法: gevent提供了一个脚本程序 monkey,可以修改底层解释O阻塞的行为,将很多普通阻塞转换为 gevent阻塞

        -   使用： from  gevent  import  monkey

        -   在模块导入前，运行响应的monkey插件函数
            -   e.g. monkey.patch_all() monkey.patch_socket()

        ```py
            # gevent 协程 TCP

            import gevent
            from gevent import monkey

            monkey.patch_all()
            from socket import *

            sock_fd = socket()
            sock_fd.setsockopt(SOL_SOCKET, SO_REUSEADDR, 1)
            sock_fd.bind(("0.0.0.0", 12016))
            sock_fd.listen(3)

            def handle(conn_fd):
                while True:
                    data = conn_fd.recv(1024)
                    if not data:
                        break
                    print(data.decode())
                    conn_fd.send(b"OK")
                conn_fd.close()

            while True:
                conn_fd, addr = sock_fd.accept()  # accept 阻塞才能触发协程
                print("connect ... ", addr)
                # handle(conn_fd)  # 循环方案
                gevent.spawn(handle, conn_fd)  # 协程方案

            sock_fd.close()
        ```

#### HTTP_Server

-   核心技术点：
    -   TCP 连接
    -   SELECT IO 多路复用
    -   类封装，让用户传递属性 + 重写功能达到 http 实现

```py
    from select import select
    from socket import *

    class HttpServer:
        def __init__(self, server_address, static_dir):
            self.server_address = server_address
            self.static_dir = static_dir
            self.rlist = self.wlist = self.xlist = []
            self.create_socket()
            self.bind()

        def create_socket(self):
            self.sock_fd = socket()
            self.sock_fd.setsockopt(SOL_SOCKET, SO_REUSEADDR, 1)

        def bind(self):
            self.sock_fd.bind(self.server_address)
            self.ip = self.server_address[0]
            self.port = self.server_address[1]

        def get_html(self, conn_fd, info):
            """
                网页处理
            :param conn_fd:
            :param info:
            :return:
            """
            if info == "/":
                file_name = self.static_dir + "/index.html"
            else:
                file_name = self.static_dir + info
            try:
                fd = open(file_name)
            except Exception:
                response_header = "HTTP/1.1 404 Not Found\r\n"
                response_header += "Content-Type: text/html\r\n"
                response_header += "\r\n"
                response_body = "<h1> Not Found </h1>"
            else:
                response_header = "HTTP/1.1 200 OK \r\n"
                response_header += "Content-Type: text/html\r\n"
                response_header += "\r\n"
                response_body = fd.read()
            finally:
                response_msg = response_header + response_body
                conn_fd.send(response_msg.encode())

        def get_data(self, conn_fd, info):
            response_header = "HTTP/1.1 200 OK\r\n"
            response_header += "Content-Type: text/html\r\n"
            response_header += "\r\n"
            response_body = "<h1> Waiting ... </h1>"
            response_msg = response_header + response_body
            conn_fd.send(response_msg.encode())

        def handle(self, conn_fd):
            """
                http 请求处理
            :param conn_fd: 连接套接字
            :return:
            """
            request = conn_fd.recv(4096)
            # 防止断开
            if not request:
                self.rlist.remove(conn_fd)
                conn_fd.close()
                return
            # 请求解析
            request_line = request.splitlines()[0]
            info = request_line.decode().split(" ")[1]
            if info == "/" or info[-5:] == ".html":
                self.get_html(conn_fd, info)
            else:
                self.get_data(conn_fd, info)

            self.rlist.remove(conn_fd)
            conn_fd.close()

        def server_forever(self):
            self.sock_fd.listen(5)
            print("listen port %d" % self.port)

            self.rlist.append(self.sock_fd)
            while True:
                rs, ws, xs = select(self.rlist, self.wlist, self.xlist)
                for r in rs:
                    if r == self.sock_fd:
                        conn_fd, addr = r.accept()
                        print("connect addr: ", addr)
                        self.rlist.append(conn_fd)
                    else:
                        self.handle(r)

                for w in ws:
                    pass

    if __name__ == '__main__':
        server_address = ("0.0.0.0", 12016)  # 服务器地址
        static_dir = "./static"  # 网页存放位置

        httpd = HttpServer(server_address, static_dir)  # 生成实例化对象
        httpd.server_forever()  # 启动服务
```

## 正则

### 动机

    1. 文本处理已经成为计算机常见工作之一
    2. 对文本内容的搜索,定位,提取是逻辑比较复杂的工作
    3. 为了快速方便的解决上述问题,产生了正则表达式技术

### 简介

-   定义：

    -   即文本的高级匹配模式,提供搜索,替换等功能,其本质是由一系列字符和特殊符号构成的字符串,这个字符串即正则表达式

-   原理：通过普通字符和有特定含义的字符,来组成字符串,用以描述一定的字符串规则，比如：重复，位置等,来表达某类特定的字符串,进而匹配。

### 元字符的使用

1.  普通字符

    -   匹配规则: 每个普通字符匹配其对应的字符

    -   In : re.findall("ab","abcda")
    -   Out: ['ab']

    -   注意：可以匹配utf-8字符

2.  或 --- 元字符： `|`

    -   匹配规则： 匹配 | 两侧任意正则表达式规则

    -   In ： re.findall("ab|cd","abcdefgh")
    -   Out： ['ab', 'cd']

3.  匹配单个字符 --- 元字符：  `.`

    -   匹配规则: 匹配除换行外任意一个字符

    -   In :  re.findall("张.丰","张三丰，张四丰，张五丰")
    -   Out:  ['张三丰', '张四丰', '张五丰']

4.  匹配字符集

    -   元字符 : `[字符集]`
    -   匹配规则 ： 匹配字符集中任意一个字符
    -   表达形式:

        -   `[abc#!好]` 匹配其中任意一个
        -   `[0-9],[a-z],[A-Z]` 匹配区间中任意一个
        -   `[_#?0-9a-z]` 混合书写，区间写在后边

    -   In : re.findall("[aeiou]","hello world")
    -   Out: ['e', 'o', 'o']

5.  匹配字符集取反

    -   元字符：`[^字符集]`
    -   匹配规则：匹配除了字符集以外的任意一个字符

    -   In : re.findall("[^0-9]","27017 port")
    -   Out: [' ', 'p', 'o', 'r', 't']

6.  匹配字符串开始位置

    -   元字符: `^`

    -   匹配规则: 匹配字符串的开始位置

    -   In : re.findall("^Jame","Jame,hello")
    -   Out: ['Jame']

7.  匹配字符串结束位置

    -   元字符: `$`
    -   匹配规则: 匹配目标字符串的结尾位置

    -   In : re.findall("Jame$","hi,Jame")
    -   Out: ['Jame']

    -   使用技巧: ^ 和 $表示开头结尾位置，这两个元字符一定在正则表达式开始和结尾处。中间的部分必须匹配目标字符串的全部内容

8.  匹配字符重复

    -   元字符 :  `*`

    -   匹配规则: 匹配前面的字符出现0次或多次

    -   In : `re.findall("wo*","wooooo~~w!")`
    -   Out: ['wooooo', 'w']


    -   元字符 ： `+`

    -   匹配规则： 匹配前面的字符出现1次或多次

    -   In : `re.findall("wo+","wooooo~~w!")`
    -   Out: ['wooooo']

    -   元字符：?

    -   匹配规则： 匹配前面的字符出现0次或1次

    -   In : `re.findall("-?[0-9]+","age:19,score:-60,00087")`
    -   Out: ['19', '-60', '00087']


    -   元字符 ： {n}
    -   匹配规则 ： 匹配前面的字符出现n次(指定重复次数)

    -   In : `re.findall("1[0-9]{10}","tel: 18866889928")`
    -   Out: ['18866889928']


    -   元字符： {m,n}
    -   匹配规则 ： 匹配前面的字符出现m-n次

    -   In : `re.findall("[1-9][0-9]{5,10}","qq:798119")`
    -   Out: ['798119']

9.  匹配任意（非）数字字符

    -   元字符 ： `\d`    `\D`

    -   匹配规则:

        -   `\d` 匹配任意数字字符  [0-9]
        -   `\D` 匹配任意非数字字符 [^0-9]

    -   In : `re.findall("\d{1,5}","mysql:3306,mongo:27017")`
    -   Out: ['3306', '27017']

10. 匹配任意（非）普通字符

    -   元字符: `\w`   `\W`

    -   匹配规则:

        -   `\w` 匹配普通字符
        -   `\W` 匹配非普通字符

    -   说明: 普通字符指，数字，字母，下划线，汉字(utf-8 字符)

    -   In : `re.findall("\w+","server_addr=('127.0.0.1',8888)")`
    -   Out: ['server_addr', '127', '0', '0', '1', '8888']

11. 匹配任意（非）空字符

    -   元字符： `\s`   `\S`

    -   匹配规则：

        -   `\s` 匹配任意空字符
        -   `\S` 匹配任意非空字符

    -   说明： 空字符指， 空格 `\r`(回车) `\n`(换行) `\t`(水平制表符) `\v`(垂直制表符) `\f`(换页符)

    -   In : `re.findall("\w+\s+\w+","hello     world")`
    -   Out: ['hello     world']

12. 匹配字符串开头结尾位置

    -   元字符 ： `\A`   `\Z`

    -   匹配规则：

        -   `\A` ==> `^`
        -   `\Z` ==> `$`

    -   In : `re.findall("\A\w+\Z","helloworld")`
    -   Out: ['helloworld']

    -   使用技巧:如果正则表达式中同时出现^ $,则两者之间的正则表达式需要将目标字符串内容全部匹配

13. 匹配单词（非）边界位置

    -   元字符：`\b`  `\B`

    -   匹配规则:

        -   `\b`  匹配单词边界
        -   `\B`  匹配非单词边界

    -   说明：单词边界指普通字符(`\w`代表的字符)与其他字符的交界位置

    -   In : `re.findall(r"\bis\b","This is a test")`
    -   Out: ['is']

* * *

**总结**

    -   匹配字符:  `.  [...] [^...]  \d  \D  \w  \W  \s \S`
    -   匹配重复： `*  +  ?   {n}  {m,n}`
    -   匹配位置:  `^   $   \A   \Z   \b   \B`
    -   其他:  `|  ()  \`

### 正则表达式的转义

1.  如果使用正则表达式匹配特殊字符，需要加 `\` 表示转义。

    -   特殊字符:  `.  *  + ? ^ $ [] {} () |  \`

    -   In : `re.findall(r"-?\d+\.?\d*","12,-45,12.34,-12.34")`
    -   Out: ['12', '-45', '12.34', '-12.34']

2.  raw （原生）字符串的使用

    -   raw字符串：在字符串前加r，表示该字符串为raw字符串，这样的字符串不会进行字符串转义处理。

    -   `"\\$\\d+"` --> `\$\d+`  --> `$100`
    -   `r"\$\d+"`

### 贪婪与非贪婪

-   贪婪模式： 默认情况下，匹配重复的元字符总是尽可能多的向后匹配更多内容。比如: `* + ? {m,n}`

-   非贪婪模式(懒惰) ：让重复元字符尽可能少的匹配内容

    -   贪婪 --> 非贪婪

    -   `*` --> `*？`
    -   `+` --> `+？`
    -   `？`--> `？？`
    -   `{m,n}` --> `{m,n}?`

-   In : `re.findall(r"\(.+?\)","(李-白),(杜-甫),(白 居 易)")`
-   Out: ['(李-白)', '(杜-甫)', '(白 居 易)']

### 正则表达式分组

1.  定义: 在正则表达式中，以()建立正则表达式内部分组，子组时正则表达式的一部分，可作为内部整体操作。

2.  作用：作为内部整体，改变元字符的操作对象

    -   In : `re.search(r"(ab)*","ababababababab").group()`
    -   Out: 'ababababababab'
    -   In : `re.search(r"(王|李)\w{1,3}","王力宏").group()`
    -   Out: '王力宏'

3.  分组

    -   可以通过编程语言某些接口获取匹配内容中,子组对应的内容部分

    -   In : `re.search("(ab)cd","abcdef").group(1)`
    -   Out: 'ab'

4.  捕获组

    -   给正则表达式子组起一个名字，表达一定的意义，该组就是捕获组。

    -   格式:  (?P<name>pattern)

    -   In : `re.search(r"(?P<zf>ab)cd","abcdef").group("zf")`
    -   Out: 'ab'

5.  注意事项

    -   一个正则表达式中可以有多个分组
    -   子组可以嵌套,不要重叠/结构过于复杂
    -   子组顺序一般由外到内，由左到右

### 正则表达式匹配原则

1.  正确性，能够正确的匹配出目标字符串
2.  排他性，除了目标字符串外尽可能不匹配其他内容
3.  全面性，尽可能考虑到目标字符串所有情况，不遗漏

### Python 的 re模块

![Python-Net_RE](http://localhost:9710/blog/Python/Python-Net_RE.png)

1.  regex = re.compile(pattern,flags = 0)

    -   功能：生成正则表达式对象
    -   参数：
        -   pattern 正则表达式
        -   flags 功能扩展标志位
    -   返回值: 正则表达式对象

2.  list = re.findall(pattern,string,flags=0)

    -   功能: 通过正则匹配目标字符串
    -   参数:
        -   pattern 正则
        -   string 目标字符串
    -   返回值： 返回匹配到的内容列表，如果正则中有子组，则列表中元素为子组对应内容。

3.  list = regex.findall(string,pos,endpos)

    -   功能: 通过正则匹配目标字符串
    -   参数:
        -   string 目标字符串
        -   pos 截取目标字符串开始位置
        -   endpos 截取目标字符串结束位置
    -   返回值： 返回匹配到的内容列表，如果正则中有子组，则列表中元素为子组对应内容。

4.  list = re.split(pattern,string,flags=0)

    -   功能: 通过正则匹配内容切割字符串
    -   参数:
        -   pattern 正则
        -   string 目标字符串
    -   返回值： 返回切割后的内容列表。
    -   In :re.split(r"[^\w]+","hello L-zf")

5.  s = re.sub(pattern,rel,string,count,flags=0)

    -   功能: 使用指定字符串替换匹配到的内容
    -   参数:
        -   pattern  正则
        -   rel 指定字符串
        -   string  目标字符串
        -   count  替换多少处，默认全部替换
    -   返回值: 返回替换后的字符串

6.  it = re.finditer(pattern,string,flags=0)
    -   功能: 通过正则匹配目标字符串
    -   参数:
        -   pattern 正则
        -   string 目标字符串
    -   返回值： 返回匹配内容的迭代对象(迭代器)


5.  re.fullmatch(pattern,string,flags=0)

    -   功能: 通过正则匹配目标字符串全部内容
    -   参数:
        -   pattern 正则
        -   string 目标字符串
    -   返回值： 返回匹配内容的match对象

6.  re.match(pattern,string,flags=0)

    -   功能: 通过正则匹配目标字符串起始内容
    -   参数:
        -   pattern 正则
        -   string 目标字符串
    -   返回值： 返回匹配内容的match对象

7.  re.search(pattern,string,flags=0)

    -   功能: 通过正则匹配目标字符串第一处内容
    -   参数:
        -   pattern 正则
        -   string 目标字符串
    -   返回值： 返回匹配内容的match对象

8.  regex对象属性：

    -   flags ：  flags标志位值
    -   pattern ： 正则表达式
    -   groups ：  子组数量
    -   groupindex: 捕获组与组序列号形成的字典

    -   In : regex = re.compile(r"(ab)cd(?P<pig>ef)",flags=re.I)

    -   In : regex.flags
    -   Out: 34

    -   In : regex.pattern
    -   Out: '(ab)cd(?P<pig>ef)'

    -   In : regex.groups
    -   Out: 2

    -   In : regex.groupindex
    -   Out: mappingproxy({'pig': 2})


7.  match对象属性方法

    1.  属性变量

        -   print(obj.pos)   #　匹配目标字符串开始位置
        -   print(obj.endpos) # 匹配目标字符串结束位置
        -   print(obj.re)   #　正则表达式
        -   print(obj.string)  #　目标字符串
        -   print(obj.lastgroup) # 最后一组的组名
        -   print(obj.lastindex) #　最后一组序号

    2.  属性方法

        -   start()   获取匹配内容的开始位置
        -   end()     获取匹配内容的结束位置
        -   span()    获取匹配内容的起止位置
        -   groupdict()  获取捕获组组名和对应内容的字典
        -   groups()   获取子组对应内容

        -   group()
        -   功能: 获取match对象对应内容
        -   参数:
            -   默认得到全部匹配内容
            -   组序号或者组名，则得到对应组内容

8.  flags 参数扩展

    1.  功能: 用于丰富正则表达式的匹配

    2.  使用函数: re模块直接调用的函数

        -   `re.compile(),re.findall,re.search`.....

    3.  常用flags

    -   A == `ASCII`  元字符只能匹配ascii码
    -   I == `IGNORECASE`  匹配时或略字母大小写
    -   S == `DOTALL`  使 . 可以匹配换行
    -   M == `MULTILINE`  使 ^  $ 可匹配每一行的开头结尾位置
    -   X == `VERBOSE`  为正则添加注释

    -   同时使用多个flag： `flags = re.I | re.A`

* * *

## 数据库

### 数据存储阶段

1.  人工管理阶段
    -   缺点:数据无法共享不能单独保持数据存储量有限
2.  文件管理阶段( .txt .doc .xls)
    -   优点:数据可以长期保存可以存储大量的数据,使用简单
    -   缺点:数据一致性差数据查找修改不方便数据冗余度可能比较大
3.  数据库管理阶段
    -   优点:数据组织结构化降低了冗余度提高了增刪改査的效率容易扩展方便程序调用,做自动化处理
    -   缺点:需要使用sql或者其他特定的语句,相对比较复杂

#### 数据库应用

> 融机构、游戏网站、购物网站、论坛网站

### 基础概念

> 数据:能够输入到计算机中并被识别处理的信息集合
>
> 数据结构:研究一个数据集合中数据之间关系的
>
> 数据库:按照数据结构,存储管理数据的仓库。数据库是在数据库管理系统管理和控制下,在一定介质上的数据集合。
>
> 数据库管理系统:管理数据库的软件,用于建立和维护数据库(图数据库系统)
>
> 数据库系统:由数据库和数据库管理系统,开发工具等组成的集合

#### 数据库

_存储数据的仓库、逻辑概念，非真实存在_

#### 数据库软件

_真是的软件用于实现数据库这个概念_

#### 数据仓库

_数据量更加庞大，更加侧重数据分析和数据挖掘，供企业决策分析之用，主句是数据查询，修改和删除很少_

#### 数据库分类和常见数据库

-   关系型数据库和非关系型数据库
    -   关系型:采用关系模型来组织数据结构的数据库(二维表)
    -   非关系型:不采用关系模型组织数据结构的数据库
-   开源数据库和非开源数据库
    -   开源: MySQL、 SQLite、 MongoDB
    -   非开源: Oracle、DB2、 SQL_Server
-   常见的关系型数据库
    -   MySQL、 Oracle、SQL_Server、DB2、SQLite

##### 关系型数据库 和 MySQL

1.  数据库结构(图库结构)

    -   数据元素->记录->数据表->数据库

2.  数据库概念解析

    -   数据表:存放数据的表格(图表结构)
    -   字段:每个列,用来表示该列数据的含义
    -   记录:每个行,表示一组完整的数据

3.  MySQL特点

    -   是开源数据库,使用C和C++编写
    -   能够工作在众多不同的平台上
    -   提供了用于C、C++、 Python、Java、Perl、PHP、Ruby众多语言的API
    -   存储结构优良,运行速度快
    -   功能全面丰富

4.  MySQL安装

    -   Ubuntu安装 MySQL服务

        -   安装服务端: sudo apt-get install mysql-server
        -   安装客户端: sudo apt-get install mysql-client

        -   配置文件: /etc/mysql
        -   命令集: /usr/bin
        -   数据库存储目录: /var/lib/mysql

    -   Windows安装 MySQL
        -   下载 MySQL安装包(windows)
        -   `https://dev.mysql.com/downloads/mysql/`
        -   `mysql-installer***5.7.***.msi`

5.启动和连接 MySQL服务

    -   服务端启动
        -   查看 MySQL状态 sudo /etc/init.d/mysql status
        -   启动服务:
            -   `sudo /etc/init.d/mysql start | stop | restart | status`
            -   `sudo service mysql start | stop | restart | status`

    -   客户端连接
        -   密码重置：
            -   sudo mysql -uroot -p
            -   用户密码
            -   use mysql;
            -   update mysql.user set authentication_string=PASSWORD('××××'),plugin='mysql_native_password' where user='root';
            -   flush privileges;
        -   命令格式
            -   进入 mysql
                -   mysql -h主机地址 -u用户名 -p密码
                -   mysql -hlocalhost -uroot -p123456
                -   本地连接可省略 -h 选项:mysql -uroot -p123456
            -   退出 mysql：Ctrl + D / quit / exit

### SQL语句

-   什么是SQL
    -   结构化查询语言( Structured Query Language),一种特殊目的的编程语言,是一种数据库查询和程序设计语言,用于存取数据以及查询、更新和管理关系数据库系统
-   SQL语句使用特点
    -   SQL语言基本上独立于数据库本身
    -   各种不同的数据库对SQL语言的支持与标准存在着细微的不同
    -   每条命令必须以;结尾
    -   SQL命令关键字不区分字母大小写

### MySQL 数据库操作

1.  查看已有数据库：`show databases;`
2.  创建库（指定字符集）：（默认 ASCII 码）

    -   `create database 库名 [charset=utf8];`

3.  查看数据库的 创建语句

    -   `show create database 库名;`

4.  查看当前使用的数据库：`select database();`
5.  切换数据库：`use 库名;`
6.  删除数据库：`drop database 库名;`

7.  库名的命名规则

    -   数字、字母、下划线,但不能使用纯数字
    -   库名区分字母大小写
    -   不能使用特殊字符和mysql关键字

#### 数据表的管理

1.  表结构设计初步

    1.  确定存储内容
    2.  确定字段构成
    3.  确定字段类型

2.  数据类型支持

    -   数字类型

        -   整数类型（精确值）- INTEGER, INT, SMALLINT, TINYINT, MEDIUMINT, BIGINT
        -   定点类型（精确值）- DECIMAL
        -   浮点类型（近似值）- FLOAT，DOUBLE
        -   比特值类型 - BIT

        ![Python-\_MySQL_Number](http://localhost:9710/blog/Python/Python-MySQL_Number.png)

        -   对于精度比较高的东西,比如 money,用 decimal类型提高精度减少误差。列的声明语法是 DECIMAL(M,D)。
            -   M是数字的最大位数(精度)。其范围为1~65,M的默认值是10
            -   D是小数点右侧数字的数目(标度)。其范围是0~30,但不得超过M。
            -   比如 DECIMALI(6,2)最多存6位数字,小数点后占2位,取值范围-9999.99到9999.99。
        -   比特值类型指0,1值表达2种情况,如真,假。

    -   字符串类型:

        -   CHAR 和 VARCHAR 类型
        -   BINARY 和 VARBINARY 类型
        -   BLOB 和 TEXT 类型
        -   ENUM 类型和 SET 类型

        ![Python-\_MySQL_String](http://localhost:9710/blog/Python/Python-MySQL_String.png)

        -   char 和 varchar

            -   char:定长,效率高,一般用于固定长度的表单提交数据存储,默认1字符

                -   eg. char(32) 不到32位，也开辟32位数据大小的空间，可省略括号内容，省略则是 1字符

            -   varchar:不定长,效率偏低

                -   eg. varchar(32) 不到32位，开辟实际存储数据大小的空间,必须写位数（括号内容）

        -   text 和 blob

            -   text 用来存储非二进制文本
            -   blob 用来存储二进制字节串

        -   enum 和 set

            -   enum 用来存储给出的一个值
                -   eg. enum("1","2","3") 值只能三选一
            -   set 用来存储给出的值中一个或多个值
                -   eg. enum("1","2","3") 值能三选多

    -   时间日期类型：

        -   DATE，DATETIME,TIMESTAMP类型
        -   TIME类型
        -   YEAR年份类型

        ![Python-\_MySQL_DATA_TIME](http://localhost:9710/blog/Python/Python-MySQL_DATA_TIME.png)

        -   注意：

            -   datatime：默认值 NULL
            -   timestamp：默认值 系统当前时间

        -   时间日期函数

            -   now() 返回服务器当前时间
            -   curdate() 返回当前日期
            -   curtime() 返回当前时间
            -   date(date) 返回指定时间的日期
            -   time(date) 返回指定时间的时间

            -   `insert into textbook (bookname,pubtime,buytime,droptime) values ("ccccccf",date("1944/1/22"),now(),2000);`

        -   时间操作

            -   可以做 比较 和 查找

            -   时间运算

                -   `select * from table_name where 字段 运算符 （时间-interval 时间间隔单位）`

                    -   时间间隔单位： 1day | 2hour | 1minute | 2year | 3 month

                    -   `select * from textbook where pubtime > (now()-interval 80 year);`
                    -   当前日期的 ... 内

##### 数据库 表的基本操作

1.  创建表（指定字符集）：

    ```bash
        $ create table "表名"(
            字段名 数据类型,
            字段名 数据类型,
            ...
            字段名 数据类型,
        );

        # eg.
        create table zf()
    ```

    -   数据类型后面可以添加修饰

        -   UNSIGNED 数字类型中的无符号，默认有符号
        -   如果不想让字段为NULL可以设置字段的属性为NOT NULL,在操作数据库时如果输入该字段的数据为NULL,就会报错。
        -   DEFAULT表示设置一个字段的默认值
        -   AUTO_INCREMENT定义列为自增的属性,一般用于主键,数值会自动加1。
        -   PRIMARY KEY关键字用于定义列为主键。主键的值不能重复。

    -   示例：

        -   `create table class1(id int primary key auto_increment, name varchar(32) not null,age int not null, sex enum("w","m"), score float default 0.0);`

        -   `create table interest(id int primary key auto_increment,name varchar(32) not null, hobby set("sing","dance","draw"), course char not null, price decimal(6,2),comment text);`

2.  查看数据表

    -   show tables;

3.  查看已有表的创建原型

    -   show create table 表名;

4.  查看表结构

    -   desc 表名;

5.  删除表

    -   drop table 表名;

#### 数据库 数据的基本操作

_where子句_

-   where子句在sql语句中扮演了重要角色,主要通过一定的运算条件进行数据的筛选(行/记录)

    -   MySQL主要有以下几种运算符:

        -   算术运算符

            -   加（`+`） 、 减（`-`） 、 乘（`*`） 、 除（`/` or DIV） 、 取余（`%` or MOD）

        -   比较运算符

            -   等于（`=`）、不等于（`<>`,`!=`）、大于（`>`）、小于（`<`）、大于等于（`>=`）、小于等于（`<=`）
            -   [NOT] BETWEEN：(不在)两值之间（ `>= min && <= max`）
            -   [NOT] IN：(不)在集合中
            -   `<=>` : 严格比较两个NULL值是否相等
                -   两个操作码均为NULL时，其所得值为1；而当一个操作码为NULL时，其所得值为0
            -   LIKE 模糊匹配
            -   REGEXP/RLIKE 正则匹配
            -   IS [NOT] NULL 不为空

        -   逻辑运算符

            -   NOT / ! 逻辑非
            -   AND 逻辑与
            -   OR 逻辑或
            -   XOR 逻辑异或 => A and not B

        -   位运算符

            -   `按位与（&）、按位或（|）、按位异或（^）、取反（!）、左移（<<）、右移（>>）`

        -   运算符优先级

            ![Python-MySQL_Operator_Level](http://localhost:9710/blog/Python/Python-MySQL_Operator_Level.png)

1.  插入（insert）

    -   `insert into 表名(字段1，...) values(值1),...;`
    -   `insert into 表名 values(值1),(值2),...;`

    -   `insert into class1 values (2,"cf",12,"w",39),(3,"zd",23,"m",79);`
    -   `insert into class1 (name,age) values ("answer", 11),("dd",33);`
    -   `insert into interest (name,hobby,course,price,comment) values ("Levi","sing,draw","A",5555.43,"这里是一个评价");`

2.  查询(select)

    -   `select 字段1，字段2 from 表名 [where 条件];`
    -   `select * from 表名 [where 条件];`

        -   `*` 代表所有字段

        -   `select * from class1 where age % 2 = 0;`
        -   `select * from class1 where age between 22 and 33;`
        -   `select * from class1 where age in (11,12,22);`
        -   `select * from class1 where sex = "m" and age > 12;`

3.  更新 (update)

    -   `update 表名 set 字段1=值1,... where 条件;`
        -   `update class1 set age = 14,score=88 where id = 1;`
        -   `update class1 set age = 12 where age = 77 limit 1;`

4.  删除 (delete)

    -   `delete from 表名 where 条件`
        -   delete 后 若不加 where，表示清空所有记录

#### 数据库 表字段操作

-   语法：`alter table 表名 执行动作;`

    -   添加字段(add)

        -   `alter table 表名 add 字段 数据类型;`
        -   `alter table 表名 add 字段 数据类型 first;`
        -   `alter table 表名 add 字段 数据类型 after;`
            -   `alter table interest add tel char(15) after price;`

    -   删除字段(drop)

        -   `alter table 表名 drop 字段;`

    -   修改数据类型(modify)

        -   `alter table 表名 modify 字段 数据类型;`
            -   `alter table interest modify name varchar(64) not null;`

    -   修改字段名(change)

        -   `alter table 表名 change 字段 新字段 数据类型;`
            -   `alter table class1 change sex gender enum("w","m");`

    -   表重命名(rename)

        -   `alter table 表名 rename 新表名;`

#### 数据库 高级查询

##### 模糊查询 正则查询

1.  LIKE用于在 where子句中进行模糊查询, SQL LIKE子句中使用百分号%字符来表示任
    意字符

    -   `SELECT field1, field2.. fieldN FROM table name WHERE field1 LIKE condition1`
    -   `select * from class_1 where name LIKE "z%";`

2.  mysql中对 正则表达式 的支持有限,只支持部分正则元字符

    -   `SELECT field1, field2... fieldN FROM table_name WHERE field1 REGEXP condition1`
    -   `select * from class_1 where name REGEXP "^z.*";`

##### 整体结构

```MySQL
    6 select ... 聚合函数 from table_name
    1 where ...
    2 group by ...
    3 having ...
    4 order by ...
    5 limit ...;
```

##### 聚合函数

-   AVG(feild_name)

    -   该字段的平均值

-   MAX(feild_name)

    -   该字段的最大值

-   MIN(feild_name)

    -   该字段的最小值

-   SUM(feild_name)

    -   该字段所有记录的和

-   COUNT(feild_name)

    -   统计该字段记录的个数
    -   字段为 NULL 不能统计

##### GROUP BY

-   给查询的结果进行分组
-   先分组再聚会
-   GROUP BY 的字段名必须出现在 SELECT 的后面
-   SELECT 后面的字段名没有在  GROUP BY 后面出现，必须做聚合处理

    ```mysql
        select country,avg(attack) from sanguo
        group by country;

        select country,count(id) as num from sanguo
        where gender="M"
        group by country
        order by num desc
        limit 2;
    ```

##### HAVING

-   对分组聚合后的结果进行进一步筛选
-   WHERE 只能操作表中实际存在的字段
-   HAVING 操作的是聚合函数生成的显示列

    ```mysql
        select country,avg(attack) as atk from sanguo
        group by country
        having atk > 105
        order by atk desc
        limit 2;
    ```

##### 排序

-   ORDER BY子句来设定你想按哪个字段哪种方式来进行排序,再返回搜索结果。

    -   使用 ORDER BY子句将查询数据排序后再返回数据

    -   `SELECT field1, field2... fieldN from table_name where condition ORDER BY field1 [ASC [DESC]]`

    -   默认情况ASC表示升序,DESC表示降序

    -   `select * from class_1 where gender = "m" order by age desc;`

##### 分页

-   LIMIT子句用于限制由 SELECT语句返回的数据数量或者 UPDATE DELETE语句的操作数量

-   带有LIMIT子句的 SELECT语句的基本语法如下

    -   `SELECT field1, field2... fieldN FROM table name WHERE condition LIMIT [num]`

##### DISTINCT

-   去重 SELECT DESTINCT field_name,... FROM table_name
-   DISTINCT 和 FROM 之间所有字段都相同才会去重
-   DISTINCT 不能对任何字段做聚合处理
-   eg. `select count(distinct country)from sanguo;`

##### 与运算符一起使用

-   `select field_name1,field_name2*2 from table_name;`
-   `update table_name set field_name=field_name*2;`
-   ...

##### 联合查询

-   UNION 操作符用于连接两个以上的 SELECT 语句的结果组合到一个结果集合中,多个SELECT语句会刪除重复的数据

-   UNION 操作符语法格式

    -   `SELECT field1, field2... fieldN FROM table [WHERE condition]`
    -   `UNION [ALL | DISTINCT]`
    -   `SELECT field1, field2... fieldN FROM table [WHERE condition]`

-   `select * from class_1 where age > 23 UNION ALL select * from class_1 where id < 3;`

##### 多表查询

-   多个数据表可以联合查询：

-   `select table_name1.field1,... from table_name1 ... where condition`

-   `select class_1.name,class_1.age,class_1.gender,interest.bobby from class_1,interest where class_1.name = interest.name;`

#### 索引(Btree)

-   定义

    -   对数据库表的一列或多列的值进行排序的一种结构(Btree方式) ✔

-   优点

    -   加快数据检索速度

-   缺点

    1.  占用物理存储空间
    2.  当对表中数据更新时,索引需要动态维护,降低数据维护速度

-   索引示例

    1.  开启运行时间检测 `set profiling=1;`
    2.  执行查询语句
    3.  查看执行时间
    4.  在name字段创建索引 `CREATE INDEX index_name ON table_name(field_name);`
    5.  再执行查询语句
    6.  查看执行时间 `show profiles;`

-   查询时使用 `*` 代替字段名，任何时候创建的索引都会失效

-   适合创建索引的字段
    1.  经常用来查询的字段
    2.  经常用来做 where 条件判断的字段
    3.  经常用来做 order by 排序的字段

##### 索引分类

###### 普通(MUL) and 唯一(UNI)

-   使用规则

    1.  可设置多个字段
    2.  普通索引:字段值无约束,KEY 标志为 MUL
    3.  唯一索引(unique):字段值不允许重复,但可为NULL ,KEY 标志为 UNI


-   创建普通索引 and 唯一索引

    ```MySQL
        # 在创建表时创建
        CREATE TABLE table_name(
        field_name DATATYPE,
        field_name DATATYPE,
        index(field_name),
        index(field_name),
        unique(field_name)
        )

        # 在已有表创建
        CREATE [UNIQUE] INDEX index_name ON table_name(field_name);
    ```

-   查询：

    ```MySQL
        DESC table_name; -> KEY
        SHOW INDEX FROM table_name\G;
            # Non_Unique:1 -> index
            # Non_Unique:0 -> unique
    ```

-   删除:`DROP INDEX index_name on table_name;`

###### 主键(PRI) and 自增长(AUTO_INCREMENT) ✔

-   使用规则

    1.  只能有一个主键字段
    2.  所带约束:不允许重复,且不能为NULL
    3.  KEY标志:PRI
    4.  通常设置记录编号字段id,能唯一锁定一条记录

-   创建普通索引 and 唯一索引

    ```MySQL
        # 在创建表时创建
        CREATE TABLE table_name(
        id INT PRIMARY KEY AUTO_INCREMENT,
        ...,
        # primary key(id)  第二种创建方式（不常用）
        )CHARSET=UTF8,AUTO_INCREMENT=1000;  # 自增长起始值

        # 在已有表创建
        ALTER TABLE table_name ADD PRIMARY KEY(id);
    ```

-   已有表操作自增长属性

    ```MySQL
        # 已有表添加自增长属性
        ALTER TABLE table_name MODIFY field_name INT AUTO_INCREMENT;

        # 已有表重新指定起始值
        ALTER TABLE table_name AUTO_INCREMENT=2000;
    ```

-   删除

    1.  删除自增长属性（modify）
    2.  删除主键索引：`ALTER TABLE table_name DROP PRIMARY KEY;`
        -   删除组件必须先删除自增长，在删除主键，否则删除失败

###### 外键（foreign key）✔

-   定义： 让当前表(从表)字段的值在另一个表(主表)的范围内选择(参照另一张表)

-   语法：

    ```mysql
    foreign key〔参考字段名)
    references 主表 (被参考字段名)
    on delete 级联动作
    on update 级联动作
    ```

-   使用规则

    1.  主表、从表字段数据类型要一致
    2.  主表的被参考字段必须是 KEY 的一种，一般为主键

    ```bash
        # 在从表中创建外键
        CREATE TABLE slave(
        stu_id INT,
        name VARCHAR(20),
        money DECIMAL(10,2),
        FOREIGN KEY (stu_id)
        REFERENCES master(id)
        ON DELETE CASCADE  # 同步删除
        ON UPDATE CASCADE  # 同步更新，只更新参考、被参考字段
        )CHARSET=UTF8;
    ```

-   级联动作

    1.  CASCADE
        -   数据级联删除、更新(参考字段)
    2.  RESTRICT(默认)
        -   从表有相关联记录,不允许主表操作
    3.  SET NULL
        -   主表删除、更新,从表相关联记录字段值为NULL

-   删除外键：`ALTER TABLE table_name DROP FOREIGN KEY foreign_name;`

-   添加外键：

    ```MySQL
        alter table slave add
        foreign key (stu_id)references master(id)
        on delete SET NULL
        on update SET NULL ;
    ```

#### 高级查询

##### 嵌套查询(子查询)

1.  定义：把内层的查询结果作为外层的查询条件。 ✔
2.  语法：SELECT...FROM table_name WHERE condition (SELECT...);

    ```MySQL
        select name,attack from sanguo
        where (country,attack) in (
        select country, max(attack) from sanguo
        group by country)
    ```

##### 多表查询（效率低）

-   笛卡尔积(表记录全匹配)✔

    -   `SELECT 字段名列表 FROM 表名列表;`

-   多表查询：`SELECT 字段名列表 FROM 表名列表 WHERE 条件;`

    ```MySQL
        select province.pname,city.cname,county.coname from province,city,county
        where province.pid=city.cp_id and county.copid=city.cid;
    ```

##### 连接查询✔

-   内连接

    ```MySQL
        select field_name from table_name1
        inner join table_name2 on condition
        inner join table_name3 on condition

        # eg.
        select province.pname,city.cname,county.coname from province
        inner join city on province.pid=city.cp_id
        inner join county on city.cid=county.copid;
    ```

-   外连接
    -   左连接：left
        -   以左边为主显示查询结果
    -   右连接：right
        -   以右边为主显示查询结果

#### 数据导入/导出

##### 数据导入

-   作用：把文件系统的内容导入到数据库中

1.  非 .sql 文件导入

    -   语法：

        ```MySQL
        LOAD DATA INFILE "文件名"
        INTO TABLE 表名
        FIELDS TERMINATED BY "分隔符"
        LINES TERMINATED BY "\n"
        ```

    -   执行步骤

        1.  将 表文件 放到数据库搜索路径中（Ubuntu系统独有）
            -   show variables like "secure%";
            -   `/var/lib/mysql-files/`
        2.  在数据库中创建对应的表
        3.  执行数据导入语句

        ```MySQL
            create table tbl(
            rank int,
            name varchar(20) not null,
            age int
            )charset=utf8;

            load data infile '/var/lib/mysql-files/mysql.csv'
            into table tbl
            fields terminated by ','
            lines terminated by '\n';

            alter table tbl add id int(3) zerofill primary key auto_increment first;
        ```

2.  .sql 文件导入

    -   进入MySQL 后，导入 `mysql> source sql文件绝对路径`

##### 数据导出

-   作用：将数据库中表的记录保存到系统文件里

-   语法：

    ```MySQL
        SELECT ... FROM table_name
        INTO OUTFILE "file_name"
        FIELDS TERMINATED BY "separator"
        LINES TERMINATED BY "separator";

        # 示例
        select name,attack,defense from sanguo
        into outfile '/var/lib/mysql-files/sanguo.csv'
        fields terminated by ','
        lines terminated by '\n';
    ```

-   注意

    1.  导出的内容由SQL查询语句决定
    2.  执行导出命令时路径必须指定在对应的数据库目录下

##### 复制表

-   语法：`CREATE TABLE 表名 SELECT 查询命令;`

    ```MySQL
        create table country.sanguo2 select * from country.sanguo;
    ```

-   注意

    -   复制表的时候不会把原有表的键属性复制过来

-   复制表结构

    -   `CREATE TABLE 表名 SELECT 查询命令 WHERE FALSE;`

##### 数据库备份

1.  备份命令

    -   `mysqldump -uroot -p DB_name > url`

        -   `DB_name` 可取代为

            -   `--all-databases` 备份所有库
            -   `-B DB_name1 DB_name2 DB_name3` 备份多个库
            -   `DB_name table_name1 table_name2 table_name3` 备份指定库的多张表

        -   `mysqldump -uroot -p stu > ../stu.sql`

2.  恢复命令

    -   `mysql -uroot -p [--one-database] DB_name < url`

        -   DB_name -> 目标库名

    -   指定恢复某一个数据库数据

#### 锁 ✔

-   目的：解决客户端并发访问的冲突问题（MySQL 自动加锁，释放锁，不需要手动操作）

-   锁类型分类

    -   读锁(共享锁): select 加读锁之后别人不能更改表记录，但可以进行查询
    -   写锁互(斥锁、排他锁):加写锁之后别人不能查、不能改

-   锁粒度分类

    -   表级锁: myisam
    -   行级锁：innodb

#### 存储引擎

-   定义：处理表的处理器

-   操作：

    1.  查看所有存储引擎：`show engines;`
    2.  查看已有表的存储引擎:`show create table table_name;`
    3.  创建表指定：`create table table_name(...)engine=MyISAM;`
    4.  已有表指定：`alter table table_name engine=innoDB`

-   常用存储引擎特点

    1.  InnoDB

        -   支持行级锁
        -   支持外键、事务、事务回滚
        -   表字段和索引同存储在一个文件中
            1.  表名.frm: 表结构及索引文件
            2.  表名.ibd: 表记录

    2.  MyISAM

        -   支持表级锁
        -   表字段和索引分开存储
            1.  表名.frm: 表结构
            2.  表名.myi: 索引文件
            3.  表名.myd: 表记录

    3.  Memory

        -   表记录存储在内存中,效率高
        -   服务或主机重启,表记录清除

-   如何选择存储引擎

    1.  执行查操作多的表用 MyISAM(使用InnoDB浪费资源)
    2.  执行写操作多的表用 InnoDB

#### MySQL 的用户账户管理

1.  开启MySQL 远程连接

    -   更改配置文件，重启服务

        1.  打开 配置文件 `/etc/mysql/mysql.conf.d/mysqld.cnf`
        2.  更改配置
            -   注释：`bind-address = 127.0.0.1`
            -   添加：`character_set_serve=UTF8`
        3.  重启：`/etc/init.d/mysql restart`

2.  添加授权用户

    1.  用 root 用户登录 mysql

    2.  授权

        -   `grant 授权列表 on 库.表 to "用户名"@"%" identified by "密码" with grand option;`
            -   授权列表：`all privileges、select、insert ...`
            -   库.表：`*.*` 所有库所有表
            -   % -> 任何地址
            -   with grand option -> 继续授权

    3.  刷新权限
        -   `flush privileges;`

    ```MySQL
        grant add privileges on *.*
        to 'zf'@'%' identified by '111111'
        with grant option;
    ```

* * *

#### 事务和事务回滚

-   事务定义: 一件事从开始发生到结束的过程
-   作用：确保数据的一致性、准确性、有效性
-   操作：
    1.  开启事务
        1.  mysql> begin; # 方法1
        2.  mysql> start transaction; # 方法2
    2.  开始执行事务中的1条或者n条SQL命令
    3.  终止事务
        1.  mysql> commit; # 事务中SQL命令都执行成功,提交到数据库执行，此事务结束。
        2.  mysql> rollback; # 事务中有SQL命令执行失败,回滚到初始状态，此事务结束。

##### 事务四大特性(ACID)

1.  原子性( atomicity)

    -   一个事务必须视为一个不可分割的最小工作单元,整个事务中的所有操作要么全部提交成功,要么全部失败回滚,对于一个事务来说,不可能只执行其中的部分操作

2.  一致性( consistency)

    -   数据库总是从一个一致性的状态转换到另一个一致性的状态

3.  隔离性( isolation)

    -   一个事务所做的修改在最终提交以前,对其他事务是不可见的

4.  持久性( durability)

    -   一旦事务提交,则其所做的修改就会永久保存到数据库中。此时即使系统崩溃,修改的数据也不会丢失

**注意**

    -   事务只针对于表记录操作(增删改)有效,对于库和表的操作无效
    -   事务一旦提交结束,对数据库中数据的更改是永久性的

#### ER模型( Entry- Relationship)

-   定义：

    1.  ER模型即实体关系数据模型,用于数据库设计
    2.  用简单的图(E-R图)反映了现实世界中存在的事物或数据以及他们之间的关系

-   实体、属性、关系

    1.  实体：现实世界中任何可以被认知、区分的事物

        -   矩形框（表）

    2.  属性：实体具有的特征

        -   椭圆框（字段）

    3.  关系：实体间的联系

        -   菱形框（字段）

        -   一对一关联（1：1）：（主外键关联 + 外键字段添加唯一索引）

            -   表A中的一条表记录，在表B中只能有一条表记录和他发生关联，反之也必须是（一一对应）

            ```MySQL
                create table t1(
                t1_id int primary key auto_increment
                );

                create table t2(
                t2_id int unique,
                foreign key (t2_id) references t1(t1_id)
                );
            ```

        -   一对多关联（1：n）：（主外键关联）

            -   表A中的一条表记录，在表B中有多条表记录和他发生关联
            -   表B中的一条表记录，在表A中只能有一条表记录和他发生关联

        -   多对多关联（m：n）：

            -   表A中的一条表记录，在表B中有多条表记录和他发生关联
            -   表B中的一条表记录，在表A中有多条表记录和他发生关联

            ```mysql
                create table teacher(
                id int primary key,
                tname varchar(20),
                level varchar(20)
                )charset=utf8;
                insert into teacher values (1,'zf','S'),(2,'cf','D');

                create table course(
                id int primary key,
                cname varchar(20),
                score int
                )charset=utf8;
                insert into course values (1,'HTML',5),(2,'Spider',5),(3,'MySQL',3);

                create table middel(
                id int primary key auto_increment,
                t_id int,
                c_id int,
                foreign key (t_id)
                references teacher(id),
                foreign key (c_id)
                references course(id)
                )charset=utf8;
                insert into middel values(1,1,1),(2,1,3),(3,2,1),(4,2,2),(5,2,3);

                select teacher.tname,course.cname from course
                inner join middel on course.id=middel.c_id
                inner join teacher on teacher.id=middel.t_id
                where teacher.tname='zf';
            ```

#### MySQL调优

##### 存储引擎优化

1.  读操作多：MyISAM
2.  写操作多：InnoDB

##### 索引优化

-   在 select、where、order by 常涉及到的字段建立索引

##### SQL 语句优化

1.  单条查询最后添加 `LIMIT 1`,停止全表扫描
2.  where 子句中不使用 `!=` ,否则放弃索引全表扫描
3.  尽量避免 NULL 值判断,否则放弃索引全表扫描
    -   select number from t1 where number is null;
        -   优化方案：在 number 列上设置默认值0,确保 number 列无 NULL 值
        -   select number from t1 where number=0;
4.  尽量避免 or 连接条件,否则放弃索引全表扫描

    -   select id from tl where id=10 or id=20;
        -   优化方案：`select id from t1 where id=10 union all select id from t1 where id=20;`

5.  模糊查询尽量避免使用前置 % ,否则全表扫描
6.  尽量避免使用in和 not in,否则全表扫描

    -   select id from t1 where id in (1,2,3,4);
        -   优化方案：`select id from t1 where id between 1 and 4;`

7.  尽量避免使用 `select * ...;` 用具体字段代替 `*` ，不要返回用不到的任何字段；

### Python 操作 MySQL数据库

#### pymysql的使用

1.  pymysql 安装 `sudo pip3 install pymysql`

2.  pymysql使用流程

    -   建立数据库连接：`db= pymysql. connect(参数列表)`

        -   参数列表：(host,port,user,password,database,charset)

        -   提交到数据库：`db.commit()`
        -   回滚：`db.rollback()`
        -   断开数据库连接：`db.close()`

    -   创建游标对象：`cur = db.cursor()`

        -   返回游标对象，用于执行SQL语句

        -   `cur.execute(sql,[参数])`

            -   一次 IO 执行一条表记录
            -   参数：
                -   sql:需要进行的sql语句字符串
                -   第二个列表类型的参数是字符串中的的变量
            -   写操作细节使用sql语句操作即可

            -   读操作（select）

                -   先使用游标对象调用 execute(),将数据存入游标对象中，再调用 fetch...() 方法取值
                -   游标对象承载数据，读取一次后，第二次读取，不包含第一次的数据
                -   获取查询结果集的第一条记录：`cur.fetchone()`
                -   获取n条记录：`cur.fetchmany(n)`
                -   获取所有记录：`cur.fetchall()`

        -   `cur.extcutemany(sql,[参数])`

            -   一次 IO 执行多条表记录，高效，节省资源
            -   列表参数：`[(v1,v2...),(v1,v2...),(v1,v2...)]`
                ```py
                    data_list = []
                    for i in range(1, 1000000):
                        name = 'Tom{}'.format(str(i))
                        data_list.append(name)
                    sql = "insert into text (name) values (%s);"
                    cur.executemany(sql, data_list)
                    db.commit()
                ```

        -   关闭游标对象：`cur.close()`

        ```py
            import pymysql

            db = pymysql.connect(host="localhost", port=3306, user="root", password="341309", database="stu", charset="utf8")

            cur = db.cursor()

            while True:
                name = input("name: ")
                # age = int(input("age: "))
                age = input("age: ")
                gender = input("gender: ")
                # score = float(input("score: "))
                score = input("score: ")
                # sql 必须是严格的 sql 语句
                # sql = "INSERT INTO class_1 (name,age,gender,score) VALUES ('%s',%d,'%s',%f);" % (name, age, gender, score)
                sql = "INSERT INTO class_1 (name,age,gender,score) VALUES (%s,%s,%s,%s);"
                try:
                    # cur.execute(sql)
                    cur.execute(sql, [name, age, gender, score])  # 列表中的元素全是字符串类型，执行语句，自动识别类型
                    db.commit()
                except Exception as e:
                    db.rollback()
                    print("Faild: ", e)

            cur.close()
            db.close()
        ```

#### 存储二进制文件

-   `create table Images (id int primary key auto_increment,filename varchar(16),data mediumblob);`

```py
    ################### Logged MySQL ###################

    import pymysql

    db = pymysql.connect(host="localhost", port=3306, user="root", password="341309", database="stu", charset="utf8")
    cur = db.cursor()

    with open("./MySQL.pdf", "rb") as fd:
        data = fd.read()
    try:
        sql = "INSERT INTO Images VALUES (1,'MySQL.pdf',%s);"
        cur.execute(sql,[data])
        db.commit()
    except Exception as e:
        db.rollback()
        print("Faile: ", e)

    cur.close()
    db.close()

    ################### READ MySQL ###################

    import pymysql

    db = pymysql.connect(host="localhost", port=3306, user="root", password="341309", database="stu", charset="utf8")
    cur = db.cursor()

    sql = "SELECT * FROM Images WHERE filename = 'MySQL.pdf';"
    cur.execute(sql)

    file_data = cur.fetchall()[0]

    with open(file_data[1], "wb") as fd:
        fd.write(file_data[2])

    cur.close()
    db.close()
```

* * *

## 模块

_用来从命令行获取参数，形成列表_

```py
    import sys
    args1 = sys.argv[1]
```

_隐藏输入内容_

```py
    import getpass
    getpass.getpass()  # 返回值：输入的字符串内容
```

_加密_

```py
    import hashlib
    # 生成加密对象，参数为“盐”
    hash = hashlib.md5(("zf"+"the-salt").encode())
    # 对密码进行算法加密
    hash.update(passwd.encode())
    # 获取加密后的字符串
    hash.hexdigest()
```

_Json_

```py
    import json
    # dict => json
    json.dumps( {...} / [...] / (...) )
    # json => dict
    json.loads( json_str )
```

_format 格式化字符串_

`hello {first} and {second}'.format(first='df', second='another df')`

_判断对象是否包含对应的属性_

`hasattr(object, name)`

_返回一个对象属性值_
`getattr(object, name[, default])`

_给 字典 设置 默认值，如果没有 当前key，设置为value；有则忽略_
`dict_.setdefault(key, value)`