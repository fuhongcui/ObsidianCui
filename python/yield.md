
```python
def yieldWait() :
	val = 0
	while True :
		r = yield print('wait ...')
		val += r * 2
		print(f'receive => {r}, val => {val}')

waitHandler = yieldWait()
waitHandler.send(None)
waitHandler.send(2)
waitHandler.send(4)
```

输出：
```
wait ...
receive => 2, val => 4
wait ...
receive => 4, val => 12
wait ... 
```