极客时间学习笔记之匿名函数

匿名函数的格式：
lambda argument1, argument2,... argumentN : expression

举个例子如下，
square = lambda x: x**2
square(3)

这个相当于，
def square(x):
return x**2
square(3)

#
1.lamda 是一个表达式，并不是语句，所以它可以用在一些常规函数不能用的地方，比如用在
列表内部，
[(lambda x: x*x)(x) for x in range(10)]
# 输出
[0, 1, 4, 9, 16, 25, 36, 49, 64, 81]

还可以作为某些函数的参数，而常规函数不能
l = [(1, 20), (3, 0), (9, 10), (2, -1)]
l.sort(key=lambda x: x[1]) # 按列表中元祖的第二个元素排序
print(l)
# 输出
[(2, -1), (3, 0), (9, 10), (1, 20)]

常规函数必须通过函数名被调用，因此它首先需要被定义，但是作为一个表达式的lamda则不需要名字

2.lamada的主体只是一行简单的表达式，不能扩展成多行的代码块，这个是出于设计的考虑，python
之所以发明lamda，就是为了让它和常规函数各司其职，lamda专职与简单的任务，而常规函数负责更
复杂的多行逻辑。lamda函数通常的使用场景是：程序中需要使用一个函数完成一个简单的功能，并且该函数只调用一次。

3.例子
squared = map(lambda x: x**2, [1, 2, 3, 4, 5])
函数map(function, iterable)第一个参数是函数对象，第二个参数是一个可以遍历的集合，它表示对集合中的每一个元素都
运用fuction这个函数

4.python函数式编程
函数式编程指的是代码中的每一块儿都是不可变得，都由纯函数的形式组成，这里的纯函数，指的是函数本身相互独立，互不影响，对于相同的输入总是有相同的输出，没有任何副作用
举个例子，
def multiply_2(l):
for index in range(0, len(l)):
l[index] *= 2
return l

这个函数改变了输入参数的值，有副作用，多次调用每次返回的结果都不一样，如果把它变成一个纯函数的形式，如下，
def multiply_2(l):
for index in range(0, len(l)):
l[index] *= 2
return l

函数编程的优点是其纯函数和不可变的特性使得程序更加健壮，易于调试和测试，缺点是限制多，难写。python不同于scala，
它不是一门函数式编程语言，但是它提供了一些函数式编程的特性，像map，filter，reduce；
map函数的性能跟for循环和list comprehension相比还是很好的，数据如下，
python3 -mtimeit -s'xs=range(1000000)' 'map(lambda x: x*2, xs)'
2000000 loops, best of 5: 171 nsec per loop

python3 -mtimeit -s'xs=range(1000000)' '[x * 2 for x in xs]'
5 loops, best of 5: 62.9 msec per loop

python3 -mtimeit -s'xs=range(1000000)' 'l = []' 'for i in xs: l.append(i * 2)'
5 loops, best of 5: 92.7 msec per loop

map函数是最快的，因为map函数是直接由C语言写的运行时不需要通过python解释器间接调用，并且做了诸多优化，所以运行速度最快。

filter函数表示对集合中每个元素都用functuon判断，并且返回True或者False
l = [1, 2, 3, 4, 5]
new_list = filter(lambda x: x % 2 == 0, l) # [2, 4]

reduce是对一个集合做一些累积操作
l = [1, 2, 3, 4, 5]
product = reduce(lambda x, y: x * y, l) # 1*2*3*4*5 = 120

#
如何选择，在数据量大的时候，比如机器学习，那我们一般更倾向于用函数式编程，因为效率高，在数据量不大的时候，list comprehension可读性更高，如果对于集合中的元素，做一些比较复杂的操作，那么for循环更合适；


