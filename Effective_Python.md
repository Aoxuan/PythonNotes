# 第1章  用Pythonic方式来思考
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

在赋值时对左侧列表使用切割操作，会把该列表中处在指定范围内的对象替换成新值。与元组（tuple）的赋值（如a,b=c[:2]）不同，此切片的长度无需新值的个数相等。位于切片范伟之前及之后的那些值都保持不变，列表会根据新值的个数相应地扩张或收缩。

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
    . 在同一个切片操作内，不要同时使用start,end和stride。如果确实需要执行这种操作，那就考虑将其拆解为两条赋值语句，其中一条做范围切割，另一条做步进切割，或考虑使用内置itertools模块中的islice.
  
  
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
    
## 第8条：不要使用含有两个以上表达式的列表推导
列表推导也支持多重循环，每一级循环的for表达式后面都可以指定条件，但包含复杂式子的列表推导会使其他人很难理解这段代码。
如，要从原矩阵中把那些本身能被3整除且其所在行的各元素之和大于10的单元格挑出来。我们可以用很剪短的代码来实现此功能，但是这样的代码却非常难懂。

    matrix = [[1,2,3], [4,5,6], [7,8,9]]
    filtered = [[x for x in row if x%3 ==0]
                for row in matrix if sum(row) >= 10]
    print(filtered)
    >>>
    [[6],[9]]
    
 要点总结：
 
    . 列表推导支持多级循环，每一级循环也支持多项条件
    . 超过两个表达式的列表推导是很难理解的，应该尽量避免
    
## 第9条：用生成器表达式来改写数据量较大的列表推导
  列表推导的缺点是：在推导过程中，对于输入序列中的每个值来说，可能都要创建仅含一项元素的全新列表，当输入数据较少时，不会出现问题，但如果输入的数据非常多，那么可能会小号大量内存，并导致程序崩溃。
  例如，要读取一份文件并返回每行的字符数，若采用列表推导来做，则需要把文件每一行的长度都保存在内存中。如果这个文件特别大，或是通过无休止的network socket来读取，那么这种列表推导就会出问题。下面的这段列表推导代码，只适合处理少量的输入值：
  
    value = [len(x) for x in open('/tmp/my_file.txt')]
    print(value)
    >>>
    [100, 57, 15, 1, 12, 75, 5 ,86, 89, 11]
  为了解决此问题，python提供了生成器表达式（generator expression）,它是对列表推导和生成器的一种泛化（generalization）。生成器表达式在运行的时候，并不会把整个输出序列都呈现出来，而是会估值为迭代器（iterator）,这个迭代器每次可以根据生成器表达式产生一项数据。
  把实现列表推导所用的那种写法放在一对圆括号中，就构成了生成器表达式。下面给出的生成器表达式与刚才的代码等效，二者的区别在于，对生成其表达式求值的时候，它会立刻返回一个迭代器，而不会深入处理文件中的内容。
  
      it = (len(x) for x in open('tmp/my_file.txt'))
      print(it)
      >>>
      <generator object <genexpr> at 0x101b8148>
   以刚才返回的那个迭代器为参数，逐次调用内置的next函数，即可使其按照声称其表达式来输出下一个值，可以根据自己的需要，多次命令迭代器根据生成器表达式来生成新值，而不用担心内存用量激增。
   
     print(next(it))
     print(next(it))
     >>>
     100
     57
   使用生成器表达式还有个好处，就是可以互相组合。下面这个代码会把刚才的那个生成器表达式所返回的迭代器用作另一个生成器表达式的输入值》
     
     roots = ((x, x**0.5) for x in it)
   外围的迭代器每次前进时，都会推动内部那个迭代器，这就产生了连锁效应，使得执行循环、评估条件表达式、对接输入和输出等逻辑都组合在了一起。
     
     print(next(roots))
     >>>
     (15, 3.872983346207417)
   上面这种连锁生成器表达式，可以迅速在python中执行。如果要把多种手法组合起来，以操作大批量的输入数据，那最好使用生成器表达式来实现。只是要注意：由生成器表达式所返回的那个迭代器是有状态的，用过一轮之后，就不要反复使用了。
   要点总结：
   
     . 当输入的数据量较大时，列表推导可能会因为占用太多内存而出问题。
     . 由生成器表达式所返回的迭代器，可以逐次产生输出值，从而避免了内存用量问题。
     . 把某个生成器表达式所返回的迭代器，放在另一个生成器表达式的for子表达式中，即可将二者结合起来
     . 串在一起的生成器表达式执行速度很快。
     

## 第10条：尽量用enumerate取代range
  python提供了内置的enumerate把各种迭代器包装为生成器，以便稍后产生输出值。生成器每次产生一堆输出值，其中，前者表示循环下标，后者表示从迭代器中获取的下一个序列元素。
  
    flavor_list = ['vanilla', 'chocolate', 'pecan']
    for i, flavor in enumerate(flavor_list):
        print('%d: %s' %(i+1, flavor))
    >>>
    1: vanilla
    2: chocolate
    3: pecan
    
    # 还可以直接指定enumerate函数开始计数时所用的值
    for i, flavor in enumerate(flavor_list, 1):
        print('%d: %s' % (i, flavor))
        
要点总结：
    
    . enumerate函数提供了一种精简的写法，可以在遍历迭代器时获知每个元素的索引
    . 尽量用enumerate来改写那种将range与下标访问相结合的序列遍历代码
    . 可以给enumerate提供第二个参数，以指定开始计数时所用的值（默认为0）
    
## 第11条： 用zip函数同时遍历两个迭代器
  Python3中的zip函数，可以把两个或两个以上的迭代器封装为生成器，以便稍后求值。这种zip生成器，会从每个迭代器中获取该迭代器的下一个值，然后把这些值汇聚成元组（tuple）。
     
    name = ['Cecilia', 'Lise', 'Marie']
    letters = [len(n) for n in names]
    
    longest_name = None
    max_letters = 0
    for name, count in zip(names, letters):
        if count > max_letters:
            longest_name = name
            max_letters = count
    
要点：
    
    . 内置的zip函数可以平行地遍历多个迭代器
    . python3中的zip相当于生成器，会在遍历过程中逐次产生元组，而Python2中的zip则是直接把这些元组完成生成好，并一次性地返回整份列表。
    . 如果提供的迭代器长度不等，那么zip就会自动提前终止（取决于最短的迭代器）
    . itertools内置模块中的zip_longest函数可以平行地遍历多个迭代器而不用在乎它们的长度是否相等。
   
## 第12条：不要在for和while循环后面写else块
  python提供了一种很多变成语言都不支持的功能，那就是可以再循环内部的语句块后面直接编写else块,这种else模块会在整个循环执行完之后立刻运行。而如果循环没有正常执行完，在循环里用break语句提前挑出，会导致程序部执行else块。
    
    for i in range(3):
        print('Loop %d' % i)
    else:
        print('Else block!')
    >>>
    Loop 0
    Loop 1
    Loop 2
    Else block!
    
    for i in range(3):
        print('Loop %d' % i)
        if i == 1:
            break
    else:
         print('Else block!')
    >>>
    Loop 0 
    Loop 1
    
  但对于不熟悉 for/else的人来说，else块显得有些难以理解。像循环这种简单的语言结构，在python程序中应当写的非常直白才对。
  要点：
  
    . python有种特殊语法，可在for及while循环的内部语句块之后紧跟一个else块
    . 只有当整个循环主体都没遇到break语句时，循环后面的else块才会执行。
    . 不要在循环后面使用else块，因为这种写法既不直观，又容易引人误解。
    

## 第13条： 合理利用try/except/else/finally结构中的每个代码块
  python程序的异常处理可能要考虑四种不同的时机。这些时机可以用try、except、else和finally块来表述。
  ### finally块
  如果既要将异常向上传播，又要在异常发生时执行清理工作，那就可以使用 **try/finally** 结构。这种结构有一项常见的用途，就是确保程序能够可靠地关闭文件句柄：
    
    handle = open('tmp/random_data.txt') # may raise IOError
    try:
        data = handle.read() # may raise UnicodeDecodeError
    finally:
        handle.close() # always runs after try:
  在上面这段代码中，read方法所抛出的异常会向上传播给调用方，而finally块中的handle.close方法则一定能够执行。open方法必须放在try块外面，因为如果打开文件时发生异常（例如，由于找不到该文件而抛出IOError），那么程序应该跳过finally块。
  
  ### else块
  try/except/else结构可以清晰地描述出哪些异常会由自己的代码来处理、哪些异常会传播到上一级。
  如果try块没有发生异常，那么久执行else块。有了这种else块，我们可以尽量缩减try块内的代码量，使其更加易读。例如，要从字符串中加载JSON字典数据，然后返回字典里某个键所对应的值。
  
    def load_json_key(data, key):
        try:
            result_dict = json.loads(data) # may raise ValueError
        except ValueError as e:
            raise KeyError from e
        else:
            return result_dict[key] # may raise KeyError
  如果数据不是有效的JSON格式，那么用json.loads解码时，会产生ValueError。这个异常会由except块来捕获并处理，如果能够解码，那么else块里的查找语句就会执行，它会根据键来查出相关的值。查询时若有异常，则该异常会向上传播，因为查询语句不在刚才那个try块的范围内。这种else子句，会把try/except后面的内容和except块本身区分开，使异常的传播行为变得更加清晰。
  
   ### 混合使用
   如果要在复合语句中把上面几种机制都用到，那就编写完整的try/except/else/finally结构。
     
     def divide_json(path):
         handle = open(path, 'r+') # May raise IOError
         try:
             data = handle.read()  # May raise UnicodeDecodeError
             op = json.loads(data)  # May raise ValueError
             value = (
                      op['numerator']/
                      op['denominator']) # May raise ZeroDivisionError
          except ZeroDivisionError as e:
             return UNDEFINED
          else:
              op['result'] = value
              result = json.dumps(op)
              handle.seek(0)
              handle.write(result)  # May raise IOError
              return value
          finally:
              handle.close()  # Always runs
              
   ### 要点
   
     。 无论try块是否发生异常，都可以利用try/finally复合语句中的finally来执行清理工作
     。 else块可以用来缩减try块中的代码量，并把没有发生异常时索要执行的语句与try/except代码块隔开
     。 顺利运行try块后，若想要使某些操作能在finally块的清理代码之前执行，则可将这些操作写到else块中。
  
  
  
  
  
  
  
  
  
  
