> **核心策略**：你不是从零开始学编程，而是在迁移思维模型。重点在于"去掉C++的习惯"和"拥抱Python的哲学"，而不是翻译语法。

---

## 目录

- [[#第0阶段：思维准备（第1天）]]
- [[#第1阶段：语法速通（第1—3天）]]
	- [[#1. 变量与类型]]
	- [[#2. 字符串]]
	- [[#3. 条件与循环]]
	- [[#4. 函数]]
	- [[#5. 列表推导]]
- [[#第2阶段：核心数据结构（第4—7天）]]
	- [[#6. List]]
	- [[#7. Dict]]
	- [[#8. Set 与 Tuple]] 
	- [[#9. 生成器]]
- [[#第3阶段：Python式编程（第2—3周）]]
	- [[#10. 装饰器]]
	- [[#11. 上下文管理器]]
	- [[#12. 类与面向对象]]
- [[#第4阶段：标准库精讲（第3—4周）]]
	- [[#13. 文件与路径]]
	- [[#14. 异常处理]]
	- [[#15. 正则表达式]]
	- [[#16. JSON 与序列化]]
	- [[#17. subprocess 调用系统命令]]
	- [[#18. 并发编程]]
	- [[#19. 单元测试（pytest）]]
- [[#第5阶段：进阶掌握（第2—3月）]]
	- [[#20. 类型注解]]
	- [[#21. 性能分析与优化]]
	- [[#22. 与 C++ 互操作（pybind11）]]

---

## 学习时间线

| 阶段 | 内容 | 时间 |
|------|------|------|
| 第0阶段 | 思维准备 | 第1天（1小时） |
| 第1阶段 | 语法速通 | 第1—3天 |
| 第2阶段 | 核心数据结构 | 第4—7天 |
| 第3阶段 | Python式编程 | 第2—3周 |
| 第4阶段 | 标准库精讲 | 第3—4周 |
| 第5阶段 | 进阶掌握 | 第2—3月 |

---

## C++ 经验对照表

### ✅ C++ 优势可加速的地方

- 算法与数据结构
- 多线程概念
- 系统编程思维
- 内存与性能意识

### ⚠️ C++ 经验可能带来的思维陷阱

- 过度使用 class 封装（Python 里函数是一等公民）
- 执念于静态类型（类型注解是可选的）
- 手动优化微小性能（先写清晰代码，再用工具找瓶颈）
- 到处写 `for` 循环（应该用推导式）

---

## 第0阶段：思维准备（第1天）

在写任何代码之前，先做心理重置。

### 主动放弃的 C++ 习惯

- [ ] 无需 `new`/`delete`，GC 会处理一切
- [ ] 不用写类型，除非你想写（type hints 是可选的）
- [ ] 不需要编译，直接跑 `python file.py`
- [ ] 不要把所有东西都包进 class，Python 里函数是一等公民
- [ ] 没有头文件，没有 `.h` 和 `.cpp` 分离

### 环境准备

```bash
# 安装 Python 3.12+
python3 --version

# 创建虚拟环境（每个项目都应该用）
python3 -m venv .venv
source .venv/bin/activate       # Linux/Mac
.venv\Scripts\activate          # Windows

# 安装常用工具
pip install ruff mypy pytest

# 进入交互式解释器（边学边验证）
python3
```

---

## 第1阶段：语法速通（第1—3天）

### 1. 变量与类型

> C++ 需要显式声明类型，Python 完全不需要。但类型是真实存在的，只是在运行时确定。

```python
# C++: int x = 10; double y = 3.14; bool flag = true;
x = 10
y = 3.14
flag = True        # 注意：True/False 首字母大写
name = "Alice"
nothing = None     # 等同于 C++ 的 nullptr / null

# 查看类型
print(type(x))     # <class 'int'>
print(type(y))     # <class 'float'>

# Python 的整数没有溢出！
big = 2 ** 100     # 完全合法，自动大整数
print(big)         # 1267650600228229401496703205376

# 多变量赋值（C++ 没有这个）
a, b, c = 1, 2, 3
a, b = b, a        # 交换！不需要 temp 变量
```

**练习**：打开终端，输入 `python3`，逐行敲上面的代码，观察输出。

---

### 2. 字符串

> 远比 C++ string 强大。

```python
s = "Hello, World"

# 基本操作
print(len(s))           # 12
print(s.upper())        # HELLO, WORLD
print(s.lower())        # hello, world
print(s.replace("World", "Python"))  # Hello, Python

# 切片（重要！）
print(s[0:5])           # Hello
print(s[-5:])           # World（负数从末尾数）
print(s[::-1])          # dlroW ,olleH（反转）

# f-string（告别 printf）
name = "Alice"
age = 30
score = 98.567
print(f"Name: {name}, Age: {age}")      # Name: Alice, Age: 30
print(f"Score: {score:.2f}")            # Score: 98.57
print(f"Next year: {age + 1}")          # 表达式可以直接写进去

# 字符串方法
text = "  hello world  "
print(text.strip())                     # "hello world"
print(text.strip().split(" "))          # ['hello', 'world']
print(",".join(["a", "b", "c"]))        # a,b,c

# 多行字符串
sql = """
SELECT *
FROM users
WHERE age > 18
"""

# 字符串检查
print("hello".startswith("he"))   # True
print("hello".endswith("lo"))     # True
print("ell" in "hello")           # True（in 操作符，很常用）
```

---

### 3. 条件与循环

```python
# if/elif/else（用缩进代替花括号，没有括号要求）
x = 15
if x > 10:
    print("big")
elif x > 5:
    print("medium")
else:
    print("small")

# 单行条件表达式（三元运算符）
# C++: result = (x > 10) ? "big" : "small";
result = "big" if x > 10 else "small"

# for 循环（Python 的 for 是 for-each）
for i in range(5):          # 0, 1, 2, 3, 4
    print(i)

for i in range(2, 10, 2):   # start=2, stop=10, step=2 → 2,4,6,8
    print(i)

# 遍历列表
fruits = ["apple", "banana", "cherry"]
for fruit in fruits:
    print(fruit)

# 同时拿索引和值（告别 fruits[i]）
for i, fruit in enumerate(fruits):
    print(f"{i}: {fruit}")

# 同时遍历两个列表
names = ["Alice", "Bob", "Charlie"]
scores = [90, 85, 92]
for name, score in zip(names, scores):
    print(f"{name}: {score}")

# while 循环
n = 10
while n > 0:
    print(n, end=" ")   # end=" " 不换行，用空格分隔
    n -= 1

# break / continue 和 C++ 一样
for i in range(10):
    if i == 3:
        continue    # 跳过3
    if i == 7:
        break       # 到7停止
    print(i, end=" ")
# 输出：0 1 2 4 5 6
```

---

### 4. 函数

```python
# 基础函数
def add(a, b):
    return a + b

print(add(3, 4))   # 7

# 默认参数
def greet(name, greeting="Hello"):
    return f"{greeting}, {name}!"

print(greet("Alice"))           # Hello, Alice!
print(greet("Bob", "Hi"))       # Hi, Bob!

# 关键字参数（调用时指定参数名，顺序无所谓）
def create_user(name, age, role="user"):
    return {"name": name, "age": age, "role": role}

user = create_user(age=25, name="Alice", role="admin")

# 多返回值（C++ 要用 tuple 或 out 参数）
def min_max(lst):
    return min(lst), max(lst)

lo, hi = min_max([3, 1, 4, 1, 5, 9])
print(f"min={lo}, max={hi}")    # min=1, max=9

# 可变数量参数
def sum_all(*args):             # args 是一个 tuple
    return sum(args)

print(sum_all(1, 2, 3, 4, 5))  # 15

def print_info(**kwargs):       # kwargs 是一个 dict
    for key, value in kwargs.items():
        print(f"  {key} = {value}")

print_info(name="Alice", age=30, city="Beijing")

# Lambda（匿名函数）
square = lambda x: x * x
print(square(5))    # 25

# 常配合 sorted/map/filter 使用
data = [(1, "banana"), (3, "apple"), (2, "cherry")]
sorted_data = sorted(data, key=lambda x: x[1])  # 按字符串排序

# 函数是一等公民（可以作为参数传递）
def apply(func, value):
    return func(value)

print(apply(square, 6))         # 36
```

---

### 5. 列表推导

> Python 最重要的特性，没有之一。

```python
# 基础格式：[表达式 for 变量 in 可迭代对象]
squares = [x*x for x in range(10)]
print(squares)   # [0, 1, 4, 9, 16, 25, 36, 49, 64, 81]

# 带条件过滤
evens = [x for x in range(20) if x % 2 == 0]

# 嵌套
matrix = [[i*j for j in range(1, 4)] for i in range(1, 4)]
# [[1,2,3], [2,4,6], [3,6,9]]

# 展平二维列表
flat = [x for row in matrix for x in row]

# 字典推导
word_len = {w: len(w) for w in ["apple", "banana", "cherry"]}
print(word_len)   # {'apple': 5, 'banana': 6, 'cherry': 6}

# 集合推导
unique_lengths = {len(w) for w in ["apple", "banana", "cherry", "kiwi"]}

# 生成器表达式（圆括号，懒计算，省内存）
total = sum(x*x for x in range(1000000))   # 不会创建百万元素的列表
```

> **规则**：凡是能用推导式写的，就不要用 `for` + `append`。

---

## 第2阶段：核心数据结构（第4—7天）

### 6. List

```python
# 创建
lst = [1, 2, 3, 4, 5]
empty = []
mixed = [1, "hello", 3.14, True]   # 可以混合类型

# 增删改查
lst.append(6)               # 末尾添加
lst.insert(2, 99)           # 索引2处插入
lst.pop()                   # 移除并返回最后一个
lst.pop(2)                  # 移除索引2处的元素
lst.remove(3)               # 移除第一个值为3的元素
lst[0] = 100                # 修改

# 查找
print(3 in lst)             # True/False（O(n)）
print(lst.index(4))         # 返回4的索引，不存在会抛异常
print(lst.count(3))         # 出现次数

# 排序
numbers = [3, 1, 4, 1, 5, 9, 2, 6]
numbers.sort()              # 原地排序
numbers.sort(reverse=True)  # 降序
sorted_copy = sorted(numbers)  # 返回新列表，不修改原列表

# 按自定义规则排序
people = [{"name": "Bob", "age": 30}, {"name": "Alice", "age": 25}]
people.sort(key=lambda p: p["age"])

# 切片（非常重要）
lst = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
print(lst[2:5])     # [2, 3, 4]
print(lst[:3])      # [0, 1, 2]
print(lst[7:])      # [7, 8, 9]
print(lst[::2])     # [0,2,4,6,8]（每隔一个）
print(lst[::-1])    # [9,8,...,0]（反转）
print(lst[1:8:2])   # [1, 3, 5, 7]

# 常用操作
print(len(lst))           # 长度
print(sum(lst))           # 求和
print(min(lst), max(lst)) # 最值
lst2 = lst + [10, 11]     # 拼接（返回新列表）
lst3 = lst * 3            # 重复
```

---

### 7. Dict

> Python 最重要的数据结构。

```python
# 创建
d = {"name": "Alice", "age": 30, "city": "Beijing"}
empty = {}
from_keys = dict.fromkeys(["a", "b", "c"], 0)  # {'a':0,'b':0,'c':0}

# 访问
print(d["name"])                # Alice（key不存在会抛 KeyError）
print(d.get("age"))             # 30
print(d.get("salary", 0))       # 0（key不存在返回默认值，不报错）

# 修改与添加
d["age"] = 31
d["email"] = "alice@example.com"

# 删除
del d["city"]
popped = d.pop("email", None)   # 不存在时返回None

# 遍历
for key, value in d.items():
    print(f"{key}: {value}")

# 检查key是否存在
print("name" in d)              # True

# 合并字典（Python 3.9+）
d1 = {"a": 1, "b": 2}
d2 = {"b": 99, "c": 3}
merged = d1 | d2                # {'a':1,'b':99,'c':3}

# defaultdict（key不存在时自动创建默认值）
from collections import defaultdict

# 统计词频
text = "apple banana apple cherry banana apple"
word_count = defaultdict(int)
for word in text.split():
    word_count[word] += 1

# 分组
names = ["Alice", "Bob", "Anna", "Charlie", "Ben"]
by_letter = defaultdict(list)
for name in names:
    by_letter[name[0]].append(name)
# {'A': ['Alice', 'Anna'], 'B': ['Bob', 'Ben'], 'C': ['Charlie']}

# Counter（专门用于计数）
from collections import Counter
c = Counter(text.split())
print(c.most_common(2))   # [('apple', 3), ('banana', 2)]
```

---

### 8. Set 与 Tuple

```python
# Set（无序，不重复）
s = {1, 2, 3, 4, 5}
empty_set = set()           # 注意：{} 是空dict，不是空set

s.add(6)
s.remove(3)                 # 不存在会报错
s.discard(99)               # 不存在不报错

# 集合运算
a = {1, 2, 3, 4}
b = {3, 4, 5, 6}
print(a | b)                # 并集：{1,2,3,4,5,6}
print(a & b)                # 交集：{3,4}
print(a - b)                # 差集：{1,2}
print(a ^ b)                # 对称差：{1,2,5,6}

# 最常用法1：去重
data = [1, 2, 2, 3, 3, 3, 4]
unique = list(set(data))

# 最常用法2：快速成员检查（O(1)）
valid_ids = {101, 102, 103, 104}
print(105 in valid_ids)     # False

# Tuple（不可变列表）
t = (1, 2, 3)
single = (42,)              # 单元素tuple，注意逗号

# 解包（非常常用）
x, y, z = t
first, *rest = [1, 2, 3, 4, 5]   # first=1, rest=[2,3,4,5]
*init, last = [1, 2, 3, 4, 5]    # init=[1,2,3,4], last=5
a, _, c = (1, 2, 3)              # _ 表示不关心的变量

# namedtuple（给tuple字段起名字）
from collections import namedtuple
Point = namedtuple('Point', ['x', 'y'])
p = Point(3, 4)
print(p.x, p.y)             # 3 4
```

---

### 9. 生成器

> 重要的性能利器：懒计算，按需产生数据，不占内存。

```python
# 生成器函数：用 yield 代替 return
def count_up(n):
    i = 0
    while i < n:
        yield i       # 暂停，把 i 返回给调用者
        i += 1        # 下次 next() 时从这里继续

gen = count_up(5)
print(next(gen))      # 0
print(next(gen))      # 1
for x in gen:         # 继续消费剩余的值
    print(x)          # 2, 3, 4

# 实用例子：读大文件（不会把整个文件加载进内存）
def read_lines(filepath):
    with open(filepath) as f:
        for line in f:
            yield line.strip()

for line in read_lines("/var/log/huge.log"):
    if "ERROR" in line:
        print(line)

# 无限序列
def fibonacci():
    a, b = 0, 1
    while True:
        yield a
        a, b = b, a + b

fib = fibonacci()
first_10 = [next(fib) for _ in range(10)]
print(first_10)   # [0, 1, 1, 2, 3, 5, 8, 13, 21, 34]

# itertools：生成器工具箱
import itertools

# 无限计数
counter = itertools.count(start=10, step=2)
print(list(itertools.islice(counter, 5)))      # [10,12,14,16,18]

# 笛卡尔积（代替嵌套循环）
colors = ["red", "blue"]
sizes = ["S", "M", "L"]
for color, size in itertools.product(colors, sizes):
    print(f"{color}-{size}", end="  ")

# 分组（要先排序）
data = [("A", 1), ("B", 2), ("A", 3), ("B", 4)]
data.sort(key=lambda x: x[0])
for key, group in itertools.groupby(data, key=lambda x: x[0]):
    print(key, list(group))

# 链式迭代
for x in itertools.chain([1,2,3], [4,5,6]):
    print(x, end=" ")   # 1 2 3 4 5 6
```

---

## 第3阶段：Python式编程（第2—3周）

### 10. 装饰器

> 本质是：接收函数，返回函数的函数。

```python
import functools
import time

# 基础装饰器
def my_decorator(func):
    @functools.wraps(func)   # 保留原函数的名字和文档
    def wrapper(*args, **kwargs):
        print(f"Before calling {func.__name__}")
        result = func(*args, **kwargs)
        print(f"After calling {func.__name__}")
        return result
    return wrapper

@my_decorator              # 等价于：greet = my_decorator(greet)
def greet(name):
    print(f"Hello, {name}!")

greet("Alice")

# 实用装饰器1：计时
def timer(func):
    @functools.wraps(func)
    def wrapper(*args, **kwargs):
        start = time.perf_counter()
        result = func(*args, **kwargs)
        elapsed = time.perf_counter() - start
        print(f"{func.__name__} took {elapsed*1000:.2f}ms")
        return result
    return wrapper

@timer
def slow_sum(n):
    return sum(range(n))

slow_sum(10_000_000)

# 实用装饰器2：重试
def retry(max_attempts=3, delay=1.0):
    def decorator(func):
        @functools.wraps(func)
        def wrapper(*args, **kwargs):
            for attempt in range(max_attempts):
                try:
                    return func(*args, **kwargs)
                except Exception as e:
                    if attempt == max_attempts - 1:
                        raise
                    print(f"Attempt {attempt+1} failed: {e}, retrying...")
                    time.sleep(delay)
        return wrapper
    return decorator

@retry(max_attempts=3, delay=0.5)
def fetch_data(url):
    import random
    if random.random() < 0.7:
        raise ConnectionError("Network error")
    return "data"

# 实用装饰器3：缓存（内置！）
from functools import lru_cache

@lru_cache(maxsize=128)
def fibonacci(n):
    if n < 2:
        return n
    return fibonacci(n-1) + fibonacci(n-2)

print(fibonacci(50))
print(fibonacci.cache_info())   # 缓存命中情况
```

---

### 11. 上下文管理器

> C++ RAII 的 Python 版本，保证资源一定被释放。

```python
import time
from contextlib import contextmanager

# 文件（最常见）
with open("test.txt", "w") as f:
    f.write("Hello\n")
# 这里文件已自动关闭

# 自定义（方法1：contextmanager 装饰器）
@contextmanager
def timer_context(label):
    start = time.perf_counter()
    try:
        yield              # with块在这里执行
    finally:               # 无论是否异常，都会执行
        elapsed = time.perf_counter() - start
        print(f"[{label}] {elapsed*1000:.2f}ms")

with timer_context("matrix ops"):
    result = sum(i*i for i in range(1_000_000))

# 自定义（方法2：实现 __enter__ 和 __exit__）
class ManagedConnection:
    def __init__(self, host):
        self.host = host
    
    def __enter__(self):
        print(f"Connecting to {self.host}")
        return f"<connection to {self.host}>"
    
    def __exit__(self, exc_type, exc_val, exc_tb):
        print(f"Closing connection to {self.host}")
        return False    # 返回 True 会吞掉异常

with ManagedConnection("db.example.com") as conn:
    print(f"Using {conn}")

# 多个上下文管理器
with open("input.txt", "r") as fin, open("output.txt", "w") as fout:
    for line in fin:
        fout.write(line.upper())
```

---

### 12. 类与面向对象

```python
# 基础类
class Animal:
    kingdom = "Animalia"    # 类变量（所有实例共享）
    
    def __init__(self, name, sound):
        self.name = name
        self.sound = sound
        self._energy = 100  # 约定：单下划线表示"内部使用"
    
    def speak(self):
        return f"{self.name} says {self.sound}"
    
    def __repr__(self):     # 等同于 C++ 的 operator<<
        return f"Animal(name={self.name!r})"
    
    def __str__(self):      # print() 时调用
        return self.name
    
    @property
    def energy(self):
        return self._energy
    
    @energy.setter
    def energy(self, value):
        self._energy = max(0, min(100, value))


# 继承
class Dog(Animal):
    def __init__(self, name, breed):
        super().__init__(name, "Woof")
        self.breed = breed
    
    def speak(self):        # 重写
        return f"{super().speak()}! (I'm a {self.breed})"

dog = Dog("Rex", "Labrador")
print(dog.speak())
print(isinstance(dog, Animal))   # True

# dataclass（推荐的现代写法，代替简单的 C++ 结构体）
from dataclasses import dataclass, field
from typing import List

@dataclass
class Config:
    host: str = "localhost"
    port: int = 8080
    debug: bool = False
    tags: List[str] = field(default_factory=list)
    
    def endpoint(self):
        return f"http://{self.host}:{self.port}"
    
    def __post_init__(self):    # 构造后自动调用，用于验证
        if not 0 < self.port < 65536:
            raise ValueError(f"Invalid port: {self.port}")

cfg = Config(port=9000, tags=["prod", "api"])
print(cfg.endpoint())
print(cfg)

# 魔术方法（让自定义类行为像内建类型）
class Vector:
    def __init__(self, x, y):
        self.x = x
        self.y = y
    
    def __add__(self, other):       # v1 + v2
        return Vector(self.x + other.x, self.y + other.y)
    
    def __mul__(self, scalar):      # v * 3
        return Vector(self.x * scalar, self.y * scalar)
    
    def __rmul__(self, scalar):     # 3 * v
        return self.__mul__(scalar)
    
    def __abs__(self):              # abs(v)
        return (self.x**2 + self.y**2) ** 0.5
    
    def __eq__(self, other):        # v1 == v2
        return self.x == other.x and self.y == other.y
    
    def __repr__(self):
        return f"Vector({self.x}, {self.y})"

v1 = Vector(1, 2)
v2 = Vector(3, 4)
print(v1 + v2)      # Vector(4, 6)
print(abs(v2))      # 5.0
```

---

## 第4阶段：标准库精讲（第3—4周）

### 13. 文件与路径

```python
from pathlib import Path   # 现代方式，忘掉 os.path

# 创建路径对象
p = Path.home() / "documents" / "report.txt"

# 基本操作
print(p.name)           # report.txt
print(p.stem)           # report
print(p.suffix)         # .txt
print(p.parent)         # /home/user/documents
print(p.exists())       # True/False

# 遍历目录
cwd = Path(".")
for f in cwd.iterdir():         # 遍历当前目录
    print(f)

for f in cwd.rglob("*.py"):     # 递归查找所有 .py 文件
    print(f)

# 读写文件
content = p.read_text(encoding="utf-8")
p.write_text("Hello, World!", encoding="utf-8")

# 大文件逐行读
with p.open("r", encoding="utf-8") as f:
    for line in f:
        print(line.strip())

# 创建目录
(Path.home() / "new_dir" / "subdir").mkdir(parents=True, exist_ok=True)

# 文件信息
import datetime
stat = p.stat()
print(stat.st_size)     # 文件大小（字节）
mtime = datetime.datetime.fromtimestamp(stat.st_mtime)

# 重命名、复制、删除
p.rename(p.with_suffix(".bak"))
import shutil
shutil.copy(src_path, dst_path)
shutil.copytree(src_dir, dst_dir)
p.unlink()
shutil.rmtree(some_dir)
```

---

### 14. 异常处理

```python
# 基本结构
try:
    result = 10 / 0
except ZeroDivisionError:
    print("Division by zero!")
except (TypeError, ValueError) as e:
    print(f"Type or value error: {e}")
except Exception as e:
    print(f"Unexpected error: {e}")
    raise               # 重新抛出
else:
    print("No exception occurred")  # 没有异常时执行
finally:
    print("Always executed")

# 常见内建异常
# FileNotFoundError, PermissionError  -> 文件操作
# KeyError, IndexError                -> 容器访问
# ValueError, TypeError               -> 参数错误
# AttributeError                      -> 不存在的属性
# ImportError, ModuleNotFoundError    -> 模块导入失败

# 自定义异常
class InsufficientFundsError(Exception):
    def __init__(self, amount, balance):
        self.amount = amount
        self.balance = balance
        super().__init__(f"Cannot withdraw {amount}, balance is {balance}")

class BankAccount:
    def __init__(self, balance):
        self.balance = balance
    
    def withdraw(self, amount):
        if amount > self.balance:
            raise InsufficientFundsError(amount, self.balance)
        self.balance -= amount

account = BankAccount(100)
try:
    account.withdraw(200)
except InsufficientFundsError as e:
    print(e)
    print(f"Tried: {e.amount}, Had: {e.balance}")

# 忽略某类异常
from contextlib import suppress
with suppress(FileNotFoundError):
    Path("nonexistent.txt").unlink()
```

---

### 15. 正则表达式

```python
import re

text = "Alice (age 30) called at 2024-01-15 and Bob (age 25) replied."

# 找第一个匹配
match = re.search(r'\d+', text)
if match:
    print(match.group())                # 30

# 找所有匹配
all_nums = re.findall(r'\d+', text)
print(all_nums)                         # ['30', '2024', '01', '15', '25']

# 分组（重要）
pattern = r'(\w+) \(age (\d+)\)'
for match in re.finditer(pattern, text):
    name, age = match.groups()
    print(f"{name} is {age} years old")

# 日期提取
date_pattern = r'(\d{4})-(\d{2})-(\d{2})'
m = re.search(date_pattern, text)
if m:
    year, month, day = m.groups()

# 替换
cleaned = re.sub(r'\s+', ' ', "too   many    spaces")

# 编译正则（重复使用时提高性能）
email_re = re.compile(r'[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}')
emails = email_re.findall("Contact alice@example.com or bob@test.org")

# split
parts = re.split(r'[,;:\s]+', "one,two;three:four five")

# 命名分组（更清晰）
log = "2024-01-15 ERROR: Connection timeout"
m = re.match(r'(?P<date>\d{4}-\d{2}-\d{2}) (?P<level>\w+): (?P<msg>.*)', log)
if m:
    print(m.group('date'))    # 2024-01-15
    print(m.group('level'))   # ERROR
    print(m.group('msg'))     # Connection timeout
```

---

### 16. JSON 与序列化

```python
import json

data = {
    "name": "Alice",
    "age": 30,
    "scores": [95, 87, 92],
    "address": {"city": "Beijing", "zip": "100000"},
    "active": True,
    "nickname": None        # None -> null
}

# Python 对象 -> JSON 字符串
json_str = json.dumps(data, indent=2, ensure_ascii=False)

# JSON 字符串 -> Python 对象
parsed = json.loads(json_str)

# 文件读写
with open("data.json", "w", encoding="utf-8") as f:
    json.dump(data, f, indent=2, ensure_ascii=False)

with open("data.json", "r", encoding="utf-8") as f:
    loaded = json.load(f)

# 自定义序列化（处理 datetime 等不支持的类型）
from datetime import datetime

class DateTimeEncoder(json.JSONEncoder):
    def default(self, obj):
        if isinstance(obj, datetime):
            return obj.isoformat()
        return super().default(obj)

event = {"name": "Meeting", "time": datetime.now()}
print(json.dumps(event, cls=DateTimeEncoder))
```

---

### 17. subprocess 调用系统命令

```python
import subprocess

# 执行命令，等待完成（推荐方式）
result = subprocess.run(
    ["ls", "-la"],              # 参数列表，防止注入
    capture_output=True,        # 捕获 stdout 和 stderr
    text=True,                  # 以字符串形式返回
    check=True                  # 非0退出码抛 CalledProcessError
)
print(result.stdout)
print(result.returncode)

# 传递输入
result = subprocess.run(
    ["grep", "error"],
    input="line1\nerror: something\nline3\n",
    capture_output=True, text=True
)

# 实时读取输出
process = subprocess.Popen(
    ["ping", "-c", "4", "8.8.8.8"],
    stdout=subprocess.PIPE, text=True
)
for line in process.stdout:
    print(line, end="")
process.wait()

# 错误处理
try:
    subprocess.run(["git", "push"], check=True, capture_output=True, text=True)
except subprocess.CalledProcessError as e:
    print(f"Command failed with code {e.returncode}")
    print(f"stderr: {e.stderr}")
except FileNotFoundError:
    print("git not found in PATH")
```

---

### 18. 并发编程

```python
import time
import threading
from concurrent.futures import ThreadPoolExecutor, ProcessPoolExecutor
import asyncio

# === threading：I/O 密集型 ===
def download(url, results, index):
    time.sleep(1)
    results[index] = f"data from {url}"

urls = ["http://a.com", "http://b.com", "http://c.com"]
results = [None] * len(urls)
threads = [threading.Thread(target=download, args=(url, results, i))
           for i, url in enumerate(urls)]
for t in threads:
    t.start()
for t in threads:
    t.join()

# === concurrent.futures：推荐方式 ===
def fetch(url):
    time.sleep(0.5)
    return f"data from {url}"

# 线程池（I/O密集型）
with ThreadPoolExecutor(max_workers=4) as executor:
    results = list(executor.map(fetch, urls))

# 进程池（CPU密集型，绕过 GIL）
def cpu_heavy(n):
    return sum(i*i for i in range(n))

with ProcessPoolExecutor(max_workers=4) as executor:
    results = list(executor.map(cpu_heavy, [10**6]*4))

# === asyncio：协程，高并发 I/O ===
async def fetch_async(url):
    await asyncio.sleep(0.5)    # 非阻塞等待
    return f"data from {url}"

async def main():
    urls = [f"http://api.com/{i}" for i in range(10)]
    results = await asyncio.gather(*[fetch_async(url) for url in urls])
    return results

results = asyncio.run(main())   # 10个请求并发，总时间约0.5s

# asyncio + 超时
async def with_timeout():
    try:
        result = await asyncio.wait_for(fetch_async("http://slow.com"), timeout=2.0)
    except asyncio.TimeoutError:
        print("Request timed out")
```

> **选择原则**：I/O 密集型用 `ThreadPoolExecutor` 或 `asyncio`；CPU 密集型用 `ProcessPoolExecutor`。

---

### 19. 单元测试（pytest）

```python
# 文件名：test_mycode.py
# 运行：pytest test_mycode.py -v

import pytest

# 被测代码
def divide(a, b):
    if b == 0:
        raise ZeroDivisionError("Cannot divide by zero")
    return a / b

def fibonacci(n):
    if n < 0:
        raise ValueError("n must be non-negative")
    a, b = 0, 1
    for _ in range(n):
        a, b = b, a + b
    return a

# 基础测试
def test_divide_basic():
    assert divide(10, 2) == 5.0
    assert divide(7, 2) == 3.5

def test_divide_by_zero():
    with pytest.raises(ZeroDivisionError):
        divide(10, 0)

# 参数化测试（避免重复代码）
@pytest.mark.parametrize("n, expected", [
    (0, 0), (1, 1), (2, 1), (3, 2), (10, 55), (20, 6765)
])
def test_fibonacci(n, expected):
    assert fibonacci(n) == expected

# fixture（提供测试数据或环境）
@pytest.fixture
def sample_data():
    return [3, 1, 4, 1, 5, 9, 2, 6]

def test_sum(sample_data):
    assert sum(sample_data) == 31

def test_max(sample_data):
    assert max(sample_data) == 9

# fixture with cleanup（tmp_path 是 pytest 内置 fixture）
@pytest.fixture
def temp_file(tmp_path):
    f = tmp_path / "test.txt"
    f.write_text("hello")
    yield f
    # tmp_path 会自动清理

def test_file_content(temp_file):
    assert temp_file.read_text() == "hello"
```

```bash
# 运行测试
pytest                          # 运行所有测试
pytest -v                       # 详细输出
pytest -k "fibonacci"           # 只运行名字包含 fibonacci 的测试
pytest --tb=short               # 简短的错误信息
pytest --cov=mymodule           # 代码覆盖率（需安装 pytest-cov）
```

---

## 第5阶段：进阶掌握（第2—3月）

### 20. 类型注解

```python
from typing import Optional, Union, List, Dict, Callable, TypeVar, Generic, Any

# 基础注解
def add(a: int, b: int) -> int:
    return a + b

# Optional：可能是 None
def find_user(user_id: int) -> Optional[dict]:
    return None

# Union：多种类型（Python 3.10+ 可用 |）
def process(data: str | bytes) -> str:
    if isinstance(data, bytes):
        return data.decode()
    return data

# 复合类型
def get_stats(numbers: List[float]) -> Dict[str, float]:
    return {"mean": sum(numbers)/len(numbers), "max": max(numbers)}

# Callable（函数类型）
def apply_twice(func: Callable[[int], int], x: int) -> int:
    return func(func(x))

# 泛型
T = TypeVar('T')

def first(lst: List[T]) -> Optional[T]:
    return lst[0] if lst else None

# 泛型类
class Stack(Generic[T]):
    def __init__(self) -> None:
        self._items: List[T] = []
    
    def push(self, item: T) -> None:
        self._items.append(item)
    
    def pop(self) -> T:
        return self._items.pop()
    
    def peek(self) -> Optional[T]:
        return self._items[-1] if self._items else None

stack: Stack[int] = Stack()
stack.push(1)
stack.push(2)

# dataclass with types
from dataclasses import dataclass
from datetime import datetime

@dataclass
class Event:
    name: str
    timestamp: datetime
    tags: List[str]
    metadata: Dict[str, Any]
    priority: int = 0
    
    def is_recent(self, hours: int = 24) -> bool:
        delta = datetime.now() - self.timestamp
        return delta.total_seconds() < hours * 3600
```

```bash
# 静态类型检查
mypy src/ --strict
```

---

### 21. 性能分析与优化

```python
import cProfile
import pstats
import timeit
import numpy as np

# === 1. 找瓶颈：cProfile ===
profiler = cProfile.Profile()
profiler.enable()

# ... 你的代码 ...

profiler.disable()
stats = pstats.Stats(profiler)
stats.sort_stats('cumulative')
stats.print_stats(10)   # 显示前10个耗时函数

# 命令行方式
# python -m cProfile -s cumulative script.py

# === 2. 微基准测试：timeit ===
def slow():
    result = []
    for i in range(10000):
        result.append(i * i)
    return result

def fast():
    return [i * i for i in range(10000)]

print(timeit.timeit(slow, number=1000))
print(timeit.timeit(fast, number=1000))

# === 3. 常见优化手段 ===

# 用 join 代替字符串拼接
words = ["hello", "world"] * 1000
# 慢：O(n²)，每次创建新字符串
slow_str = ""
for w in words:
    slow_str += w + " "
# 快：O(n)
fast_str = " ".join(words)

# 用 set 做成员检查（O(1) vs O(n)）
valid_ids = set(range(100000))
print(99999 in valid_ids)

# __slots__ 减少内存（大量对象时有效）
class Point:
    __slots__ = ('x', 'y')
    def __init__(self, x, y):
        self.x = x
        self.y = y

# === 4. NumPy 数值计算（比纯Python快100x以上）===
arr = np.arange(1_000_000)
result = np.sum(arr * arr)   # 向量化，极快

# === 5. 调用 C 库 ===
from cffi import FFI
ffi = FFI()
ffi.cdef("double sqrt(double x);")
lib = ffi.dlopen(None)
print(lib.sqrt(2.0))    # 1.4142135623730951
```

---

### 22. 与 C++ 互操作（pybind11）

**C++ 代码（my_lib.cpp）：**

```cpp
#include <pybind11/pybind11.h>
#include <pybind11/stl.h>
#include <vector>
#include <numeric>

namespace py = pybind11;

double fast_sum(const std::vector<double>& data) {
    return std::accumulate(data.begin(), data.end(), 0.0);
}

class Matrix {
public:
    Matrix(int rows, int cols) : rows_(rows), cols_(cols), data_(rows*cols, 0.0) {}
    double get(int r, int c) const { return data_[r*cols_ + c]; }
    void set(int r, int c, double v) { data_[r*cols_ + c] = v; }
private:
    int rows_, cols_;
    std::vector<double> data_;
};

PYBIND11_MODULE(my_lib, m) {
    m.doc() = "My C++ library";
    m.def("fast_sum", &fast_sum, "Sum a list of doubles");
    py::class_<Matrix>(m, "Matrix")
        .def(py::init<int, int>())
        .def("get", &Matrix::get)
        .def("set", &Matrix::set);
}
```

**编译并在 Python 中使用：**

```bash
# 安装 pybind11
pip install pybind11

# 编译（CMake 或 setup.py）
c++ -O2 -shared -fPIC $(python3-config --includes) \
    $(python3 -m pybind11 --includes) \
    my_lib.cpp -o my_lib$(python3-config --extension-suffix)
```

```python
import my_lib

result = my_lib.fast_sum([1.0, 2.0, 3.0, 4.0])
print(result)   # 10.0

m = my_lib.Matrix(3, 3)
m.set(0, 0, 1.0)
print(m.get(0, 0))   # 1.0
```

---

## 学习进度追踪

### 第1周（语法速通 + 数据结构）

- [ ] 变量与类型
- [ ] 字符串与 f-string
- [ ] 条件与循环
- [ ] 函数（默认参数、关键字参数、多返回值）
- [ ] 列表推导
- [ ] List 操作
- [ ] Dict 操作
- [ ] Set 与 Tuple
- [ ] 生成器基础

### 第2—3周（Python式编程）

- [ ] 装饰器（timer / retry / lru_cache）
- [ ] 上下文管理器
- [ ] 类与魔术方法
- [ ] dataclass

### 第3—4周（标准库）

- [ ] pathlib 文件操作
- [ ] 异常处理
- [ ] 正则表达式
- [ ] JSON 序列化
- [ ] subprocess
- [ ] 并发（ThreadPoolExecutor / asyncio）
- [ ] pytest 单元测试

### 第2—3月（进阶）

- [ ] 类型注解 + mypy
- [ ] cProfile 性能分析
- [ ] NumPy 基础
- [ ] pybind11 C++ 互操作

---

## 验收标准

当你满足以下条件时，说明已真正掌握：

1. **不用 for 循环写集合操作**（用推导式或 `map`/`filter`）
2. **不手动管理文件/连接**（用 `with` 语句）
3. **函数默认接受迭代器而不是具体容器类型**
4. **代码能跑通 `ruff check` 和 `mypy --strict`**
5. **写的代码让 10 年 Python 程序员看了不皱眉**

```bash
# 验收命令
pip install ruff mypy
ruff check .
mypy src/ --strict
pytest --cov=src
```

---

*生成于 Claude · 针对 C++ 10年经验工程师定制*
