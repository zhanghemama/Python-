极客时间学习笔记之类

# 面向对象4要素
1.类是一群有着相似性的事物的集合，对应关键字class；
2.对象是对类的实例化，是集合中的一个事物，由class关键字生成某一个object；
3.属性，对象的某个静态特征；
4.函数，对象的某个动态能力；

综上得出，类是一组有相同属性和函数的对象的集合。

# 例子
看下面代码，定义了一个类
class Document():

WELCOME_STR = 'Welcome! The context for this book is {}.'

def __init__(self, title, author, context):
print('init function called')
self.title = title
self.author = author
self.__context = context

# 类函数
@classmethod
def create_empty_book(cls, title, author):
return cls(title=title, author=author, context='nothing')

# 成员函数
def get_context_length(self):
return len(self.__context)

# 静态函数
@staticmethod
def get_welcome(context):
return Document.WELCOME_STR.format(context)


empty_book = Document.create_empty_book('What Every Man Thinks About Apart from Sex', 'Professor Sheridan Simove')


print(empty_book.get_context_length())
print(empty_book.get_welcome('indeed nothing'))

########## 输出 ##########

init function called
7
Welcome! The context for this book is indeed nothing.


1.WELCOME_STR是类常量，在类内部可以通过self.WELCOME_STR来访问，在外部可以通过class.WELCOME_STR来访问。
2.静态函数与类没有什么关联，所以它的第一个参数无特别；静态函数可以用来做一些简单的任务，既方便测试，也能优化代码结构，前面加@staticmethod修饰符。
3.类函数，第一个参数必须是cls，传进一个类来，类函数最常用的功能是实现不同的init构造函数，如上面create_empty_book，创建一个context为nil的Document。类函数需要@staticmethod来声明；

# 继承之抽象类
from abc import ABCMeta, abstractmethod

class Entity(metaclass=ABCMeta):
@abstractmethod
def get_title(self):
pass

@abstractmethod
def set_title(self, title):
pass

class Document(Entity):
def get_title(self):
return self.title

def set_title(self, title):
self.title = title

document = Document()
document.set_title('Harry Potter')
print(document.get_title())

entity = Entity()

########## 输出 ##########

Harry Potter

---------------------------------------------------------------------------
TypeError                                 Traceback (most recent call last)
<ipython-input-7-266b2aa47bad> in <module>()
21 print(document.get_title())
22 
---> 23 entity = Entity()
24 entity.set_title('Test')

TypeError: Can't instantiate abstract class Entity with abstract methods get_title, set_title

抽象类生来就是做父类用，一旦对象化就会报错，抽象函数前面加@abstractmethod修饰。


