# Python高级编程和异步IO并发编程

##  第1章  课程简介

## 第2 章 Python中一切皆对象

### 2.1 Python中的一切皆对象

- 函数和类也是对象，属于Python一等公民：
  - 赋值给一个变量
  - 可以添加到集合对象中
  - 可以作为参数传递给函数
  - 可以当做函数的返回值

### 2.2  type 、object、class的关系

​	type-->int-->1

​	type-->class-->obj

​	<img src="img/type_class_object.png">



###  2.3 python 里面常见的内置类型

- 对象的三个特征：
  - 身份   id()
  - 类型  type()
  - 值
- None (全局只有一个)
- 数值
  - int
  - float
  - complex
  - bool
- 迭代类型
- 序列类型
  - list
  - bytes/bytearray/memoryview
  - range
  - tuple
  - str
  - array
- 映射类型
  - dict
- 集合
  - set
  - frozenset
- 上下文管理
  - with语句
- 其他
  - 模块类型
  - class和实例
  - 函数类型
  - 方法类型
  - 代码类型
  - object对象
  - type类型
  - ellipsis类型
  - notimplemented类型对象

##  第3章 魔法函数

### 3.1 什么是魔法函数

- 所谓魔法函数就是双下划线方法，而且是Python提供的，我们不能自己定义

```python
class Company:
    def __init__(self, employ_list):
        self.employee = employ_list

    def __getitem__(self, item):
        return self.employee[item]


if __name__ == "__main__":
    company = Company(["dgf", "ss", "ssnj"])
    # 这里面没有指定employee 是__getitem__()优化了这个方法
    # 使用了这个方法之后增强了类型，将其序列化
    for item in company:
        print(item)   
```

### 3.2 Python的数据模型以及数据模型对Python的影响

###  3.3 魔法函数一栏

- 非数学计算类
  - 字符串表示
    - `__repr__`
    - `__str__`



- 数学计算类

## 第4章 深入类和对象

### 4.1 鸭子类型和多态

- 鸭子类型：当看到一只鸭子走、游泳和叫都像鸭子，那么这只鸟就被称为鸭子

### 4.2 抽象基类（abc模块）

- 动态语言缺陷：无法做类型检查

```python
import abc


class ChaheBase(metaclass=abc.ABCMeta):
    @abc.abstractmethod
    def get(self, key):
        pass

    @abc.abstractmethod
    def set(self, key, value):
        pass


class RedisCache(ChaheBase):
    """这里面的东西需要自己去实现"""
    pass


test = RedisCache()

```





### 4.3 isinstance 和type

- 推荐使用isinstance

  ```python
  class A:
      pass
  
  
  class B(A):
      pass
  
  
  b = B()  # 实例化B
  
  print(isinstance(b, B))
  print(type(b) is B)
  print(type(b) is A)  # 开辟了两个不同的空间
  print(isinstance(b, A))
  ```

  

### 4.4 类变量和对象变量

- 类变量需要通过类来访问
- 变量查找方式：实例变量--->类变量

```python
class A:
    aa = 1

    def __init__(self, x, y):
        self.x = x
        self.y = y


a = A(2, 4)
A.aa = 11
a.aa = 100
print(a.x, a.y, A.aa)
```

### 4.5 类属性和实例属性查找顺序

- 复习C3算法

### 4.6 静态方法、类方法、实例方法

```python

class Date(object):
    def __init__(self, year, month, day):
        self.year = year
        self.month = month
        self.day = day

    def __str__(self):
        return "{}/{}/{}".format(self.year, self.month, self.day)

    def tomorrow(self):
        self.day += 1

    """
    @staticmethod
    def func(date_str):
        date_tuple = tuple(date_str.split("-"))
        # 这个位置是调用类名
        # 但是进一步地这个类名是有可能变化的
        # 到时候后面的调用也要跟着修改 所以这个位置改成类方法
        day = Date(int(date_tuple[0]), int(date_tuple[1]), int(date_tuple[2]))
        return day
    """

    @classmethod
    def func(cls, date):
        date_tuple = tuple(date.split("-"))
        date = cls(int(date_tuple[0]), int(date_tuple[1]), int(date_tuple[2]))
        return date


date_str = "1985-5-8"
# date_tuple = tuple(date_str.split("-"))
# day = Date(int(date_tuple[0]), int(date_tuple[1]), int(date_tuple[2]))
day = Date.func(date_str)
print(day)
# print(day.tomorrow)
# day.tomorrow()
# print(day)

```



### 4.7数据封装和私有属性

- 私有属性的定义以及访问

  - 这个是为了编码的规范，并不是为了安全

    ```python
    class Person:
        def __init__(self):
            self.__birthday = "1999-04-15"
    
    
    sb = Person()
    # 访问私有属性
    print(sb._Person__birthday)
    ```

### 4.8 Python对象的自省机制

- 自省：通过的一定的机制查询到对象的内部结构

  `dir`

  `__dict__`

  

