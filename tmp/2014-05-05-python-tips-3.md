Title: Python-Tips(三）
Date:2014-05-05
Author:李鹏
Slug:python 
Tags:Python-Tips
category:编程语言-Python

**Tips15**

* python中所有正则表达式相关功能都包含在re模块中

* ^匹配字符串开始　$匹配字符串结尾

* re模块最基本的方法是search()函数

* pattern = '^M?M?M?$'  # ^表示从字符串开头匹配　M?重复３次，匹配0-3次Ｍ字符，$　匹配字符传结束　？表示匹配是可选的

* pattern = '^M?M?M?(CM|CD|D?C?C?C?)$' # "|"表示是可选的，一旦CM匹配成功，后面的均被忽略

* {N,M} {1,4} 匹配１到４个前面的模式　
  pattern = '^M{0,3}$' 和前面的 '^M?^M?^m?$'等同

* python允许使用松散正则表达式。特点：空白符被忽略。空格、制表符和回车不会被匹配，匹配时需要加转义字符“\"
  注释信息被忽略，＃开头的注释被忽略
　例如：
　pattern = '''
　    ^ 　　　                           # 字符开始
　    Ｍ{0,3}                           # 0-3个Ｍ
     (CM|CD|D?C{0,3})                  # CM 或CD 或D?0-3个C(可选）
      $                                # 字符结束

* \d匹配所有0-9的数字 \D匹配除了数字外的所有字符

* \d{3}  # 匹配３个数字

* phonePattern = re.compile(r'^(\d{3})\D+(\d{3})\D+(\d{4})\D+(\D+(\d+)$')

* (\d{3})  3个数字的分组　\D+　\D匹配数字以外的字符，　+一个或多个

* phonePattern = re.compile(r'^(\d{3}\D*(\d{3})\D*(\d{4})\D*(\d*)$') +换成了*

* phonePattern = re.compile(r'^\D*(\d{3})\D*(\d{3})\D*(\d{4})\D*(\d*)$)

如果不明确字符串开始的地方：

    phonePattern = re.compile(r'(\d{3})\D*(\d{3})\D*(\d{4})\D*(\d*)$')

松散正则表达式会清晰一点：

    phonePattern = re.compile(r'''
        (\d{3})    # 先匹配３个数字（如８００）
        \D*        # 这个区域是任意非数字的字符如（"-")
        (\d{3})     # 匹配三个数字如（555)
        \D*        #任意非数字的字符
        (\d{4})    # 匹配４个数字如（1212)
        \D*        # 任意非数字的字符
        (\d*)      # 任何数字字符
        $          # 字符结束
        ''', re.VERBOSE)

    例如　phone.Pattern.search('work 1-(800) 555.1212 #1234').groups()
    ('800','555','1212','1234')

**Tips16**
 
* \b 匹配一个单词边界

* x? 匹配可选的x字符（匹配０个或１个x字符）

* x* 匹配０个或多个x字符

* x+ 匹配１个或多个x   x{n,m} 匹配n-m个x  

* (a|b|c) 匹配人一个a或b或c

* (x)　这是一个组，会记忆匹配到的字符串

**Tips17**

* [sxz] “s、x或z"其中一个

* ^ 非，　[^abc] 除了a、b、c以外的字符

* re.sub()函数执行基于正则表达式的字符串替换，　如re.sub('y$', 'ies', noun)


**Tips18**

* 在动态函数中使用外部参数值的技术称为闭合 [closures]

* yield命令不是一个普通的函数。它是一次生成一个值的特殊类型函数。可以将其视为可恢复函数。调用该函数将返回一个可用于生成连续x值的生成器【Generator】
yield 的作用就是把一个函数变成一个 generator，带有 yield 的函数不再是一个普通函数，Python 解释器会将其视为一个 generator，调用 fab(5) 不会执行 fab 函数，而是返回一个 iterable 对象！在 for 循环执行时，每次循环都会执行 fab 函数内部的代码，执行到 yield b 时，fab 函数就返回一个迭代值，下次迭代时，代码从 yield b 的下一条语句继续执行，而函数的本地变量看起来和上次中断执行前是完全一样的，于是函数继续执行，直到再次遇到 yield。