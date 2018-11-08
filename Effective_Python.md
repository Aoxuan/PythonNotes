## 第2条：遵循PEP 8 风格指南
### 空白
Python中的空白会影响代码的含义

    . 使用space（空格）而非tab（制表符）来表示缩进
    . 和语法相关的每一层缩进都用4个空格来表示
    . 每行的字符数不应超过79
    . 对于占据多行的长表达式来说，除首行之外，其余行应再加上4个空格。
    . 文件中的函数与类之间应该用两个空行隔开
    . 在同一个类中，各方法之间应该用一个空行隔开
    . 在使用下表来获取列表元素、调用函数或给关键字参数赋值的时候，不要再两旁添加空格
    . 为变量赋值时，赋值符号左右两侧各有一个且只有一个空格。
  
### 命名
PEP 8提倡采用不同的明明风格来编写Python代码中的各个部分，以便在阅读代码时可以根据名称看出它们在Python语言中的角色。

    . 函数、变量及属性应该用小写字母来拼写，各单词之间以下划线相连，如 lowercase_underscore。
    . 受保护的实例属性，应该以单个下划线开头，例如， _leading_underscore
    . 私有的实例属性，应该以两个下划线开头，例如， __double_leading_underscore.
    . 类与异常，应该以每个单词首字母均大写的形式来命名，例如，CapitalizedWord
    . 模块级别的常量，应该全部采用大写字母来拼写，各单词之间以下划线相连，例如，ALL_CAPS
    . 类中的实例方法（instance methos），应该把收割参数命名为self，表示该对象自身。
    . 类方法（class method）的首个参数，应该命名为cls，以表示该类自身。
  
### 表达式和语句

    . 采用内联形式的否定词，而不要把否定词放在整个表达式的前面，例如，应该写 if a is not b 而不是 if not a is b
    . 不要通过检测长度的方法（如 if len(somelist)==0来判断somelist是否为[]或""等空值，而是应该采用 if not somelist
      这种写法来判断，它会假定：空值将自动评估为False
    . 检测 somelist 是否为[1]或"hi"等非空值时，也应如此，if somelist 语句默认会把非空的值判断为True.
    . 不要编写单行的if语句、for循环、while循环及except复合语句，而是应该把这些语句分成多行来书写，以示清晰。
    . import语句应该总是放在文件开头
    . 引入模块的时候，总是应该使用绝对名称，而不是应该根据当前模块的路径来使用相对名称。例如，引入bar包中的foo模块时，
      应该完整地写出， from bar import foo, 而不是简写为 import foo.
    . 如果一定要以相对名称来编写import 语句，那就采用明确的的写法： from . import foo
    . 文件中的那些import语句应该按顺序划分为三个部分，分别表示标准库模块、第三方模块以及自用模块。在每一个部分之中，
      各import语句应该按模块的字母顺序来排列。
    
## 第3条: 了解bytes、str与unicode的区别
python3有两种表示字符序列的类型：bytes和str。前者的实例包含原始的8位值，后者的实例包含Unicode字符。
python2也有两种表示字符序列的类型，分别为str和unicode.与python3不同的是，str的实例包含原始的8位值，而unicode的实例
则包含unicode。

把Unicode字符表示为二进制数据（也就是原始8位值）有许多种方法。最常见的编码方式是UTF-8。要想把Unicode
字符转换成二进制数据，就必须使用encode方法。要想把二进制数据转换成Unicode字符，则必须使用decode方法。

可以编写两个辅助（helper）函数，以便在二进制数据（UTF-8或其他编码形式）和Unicode数据之间转换：
    
    
    #--python3
    # 编写接受str或bytes，总是返回str的方法。（转换为Unicode字符）
    def to_str(bytes_or_str):
         if isinstance(bytes_or_str, bytes):
             value = bytes_or_str.decode('utf-8')
         else:
             value = bytes_or_str
         return value
    
    # 接受str 或 bytes，并总是返回bytes的方法。（转换为二进制编码形式）
    def to_bytes(bytes_or_str):
        if isinstance(bytes_or_str, str):
            value = bytes_or_str.encode('utf-8')
        else:
            value = bytes_or_str
        return value
    
python3内置的open函数默认采用UTF-8编码格式来操作文件（encoding='utf-8'），Python2中则默认是二进制形式。如果要向文件中写入二进制数据，
必须用二进制写入模式（'wb'）来开启待操作的文件，而非（'w'），这样便可同时适配Python2和Python3。从文件中读取数据时也一样，用'rb'模式而不要
用'r'模式。
        
        
## 第4条：用辅助函数来取代复杂的表达式
    . Python语法精简，特别容易写出特别复杂且难以理解的单行表达式
    . 请把复杂的表达式移入辅助函数中，如果要反复使用相同的逻辑，那就更应该这么做。
    . 使用if/else表达式，要比用or或and这样的Boolean操作符写成的表达式更加清晰。

## 第5条：了解切割序列的办法
somelist[start:end]，start所指元素涵盖在切割后的范围内，而end所指元素则不包括在内（前闭后开）。在指定切片起止索引时，若要从列表尾
部向前算，则可使用负值来表示相关偏移量。

    a = ['a', 'b', 'c', 'd', 'e', 'f', 'g', 'h']
    a[:-1]  # ['a', 'b', 'c', 'd', 'e', 'f', 'g']
    
切割列表时，即便start或end索引越界也不会出问题。反之，访问列表中的单个元素时，下标不能越界，否则会导致异常。
    
    first_twenty_items = a[:20]
   
在赋值时对左侧列表使用切割操作，会把该列表中处在指定范围内的对象替换成新值。与元组（tuple）的赋值（如a,b=c[:2]）不同，此切片的长度无需新值的个数相等。
位于切片范伟之前及之后的那些值都保持不变，列表会根据新值的个数相应地扩张或收缩。

    print('Before', a)
    a[2:7] = [99, 22, 14]
    print('After', a)
    >>>
    Before ['a', 'b', 'c', 'd', 'e', 'f', 'g', 'h']
    After  ['a', 'b', 99, 22, 14, 'h']


## 第6条：在单词切片操作内，不要同时指定start、end和stride 
除了基本的切片操作之外，python还提供了somelist[start: end: stride]形式的写法，以实现步进式切割，也就是从start:end中每stride个元素里面取1个出来。
如:

    a = [1,2,3,4,5,6,7,8,9]
    lst1 = [::2]
    lst2 = [1::2]
    print(lst1)
    print(lst2)
    >>>
    [1,3,5,7,9]
    [2,4,6,8]
    
要点总结

    . 既有start和end, 又有stride的切割操作，可能会令人费解。
    . 尽量使用stride为正数，且不带start或end索引的切割操作。尽量避免用负数做stride
    . 在同一个切片操作内，不要同时使用start,end和stride。如果确实需要执行这种操作，那就考虑将其拆解为两条赋值语句，
其中一条做范围切割，另一条做步进切割，或考虑使用内置itertools模块中的islice.
  
  
## 第7条；用列表推导来取代map和filter
Python提供了一种精炼的写法，可以根据一份列表来制作另外一份，这种表达式称为列表推导，list comprehension。例如，要用列表中每个元素的平方值构建另一份列表：

    a = [1,2,3,4,5,6,7,8,9,10]
    
    # map 嵌套匿名函数lambda
    squares_map = map(lambda x: x**2, a)

    # 列表推导
    squares = [x**2 for x in a]
    # 使用列表推导可以直接过滤
    squares = [x**2 for x in a if x % 2 == 0]
    >>>
    [4,16,36,64,100]
    
字典（dict）与集（set）,也有喝列表类似的推导机制。编写算法时，可以通过这些推导机制来创建衍生的数据结构。

    chile_ranks = {'ghost':1, 'habanero':2, 'cayenne':3}
    rank_dict = {rank: name for name, rank in chile_ranks.items()}
    chile_len_set = {len(name) for name in rank_dict.values()}
    print(rank_dict)
    print(chile_len_set)
    >>>
    {1:'ghost', 2:'habanero',3:'cayenne'}
    {8,5,7}
    
