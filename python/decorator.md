**装饰器 (Decorator)** 是 Python 中最华丽、也最实用的语法糖之一。它的本质是一个**高阶函数**：**接收一个函数作为参数，并返回一个替换它的新函数。**
简单来说，装饰器让你可以在不修改原函数代码的情况下，给函数**“平补”**额外的功能（比如打日志、计时、权限校验）。

```python
def decoratorFunc(func) :
	def wraper(*args, **kwargs) :
		print('wraper begin')
		func(*args, **kwargs)
		print('wraper end')
	return wraper

# 被装饰的函数
@decoratorFunc
def wrapFunc(x, y, z, w) :
print(f'wrapFunc => {x}, {y}, {z}, {w}')

# 调用被装饰函数
wrapFunc(1, 2, 3, 4)
```

输出：
```
wraper begin
wrapFunc => 1, 2, 3, 4
wraper end
```