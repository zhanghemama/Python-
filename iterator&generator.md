极客时间学习笔记之迭代器生成器

#
list_1 = [i for i in range(100000000)]
这个是迭代器，这个是生成一个包含一亿元素的列表，每个元素生成后保存到内存，运行你会发现，它们占用了大量的内存，内存不够就会出现OOM错误；但是实际的应用场景，我们并不需要那么多元素同时存在，比如元素求和，我们只需要每个元素在相加那一刻存在就好了，用完就可以扔掉了；

list_2 = (i for i in range(100000000))
这个是生成器，生成器就是为了解决上面内存占用的问题，只有调用next函数的时候，才会产生下一个变量。生成器初始化的时候并不需要运行一次生成操作。所以一开始并不会像迭代器那样占用很多的内存，只有被使用的时候才会被调用。

#
这两个有什么区别，生成器是懒人版的迭代器，看个例子，

def generator(k):
        i = 1
        while True:
            yield i ** k
            i += 1

gen_1 = generator(1)
gen_3 = generator(3)
print(gen_1)
print(gen_3)

def get_sum(n):
        sum_1, sum_3 = 0, 0
        for i in range(n):
                next_1 = next(gen_1)
                next_3 = next(gen_3)
                print('next_1 = {}, next_3 = {}'.format(next_1, next_3))
                sum_1 += next_1
                sum_3 += next_3
        print(sum_1 * sum_1, sum_3)

get_sum(8)


函数generator返回一个生成器，yield可以理解为，函数运行到这一行的时候，程序就会从这里暂停，然后跳出，跳到哪里呢？跳到next函数，i**k就是next函数的返回值。每次next(gen)调用的时候，暂停的程序就又复活了，从yield这里向下继续执行，局部变量i并没有被清除，而是继续叠加。这个生成器可以生成一个无线集合，而迭代器是有限的。

# 求元素索引的例子

def index_generator(L, target):
        for i, num in enumerate(L):
                if num == target:
                    yield i

print(list(index_generator([1, 6, 2, 4, 5, 2, 8, 6, 3, 2], 2)))

index_generator返回一个生成器，用list将其转化为列表

# 判断子序列

def is_subsequence(a, b):
        b = iter(b)
        return all(i in b for i in a)

print(is_subsequence([1, 3, 5], [1, 2, 3, 4, 5]))
print(is_subsequence([1, 4, 3], [1, 2, 3, 4, 5]))

(i in b)相当于以下代码，

while True:
        val = next(b)
        if val == i:
            yield True
            
next函数运行的时候保存了当前的指针，所哟i = 1的时候返回true， i = 4， 返回true， i =3， 就返回false了

# 总结
所有的容器都是可迭代的，迭代器通过next函数可以得到下一个元素，从而支持遍历。生成器是懒人版的迭代器，使用迭代器可以降低内存使用，优化代码结构，提高程序运行速度。

                        

        







