-  `pip` is actually a module in `python`, accessible via `python -m pip`, `conda` also have another `pip` could be found in for example `/home/user/miniconda3/envs/ml/bin/python`. It's just a small script calling `python -m pip` for sake of simplicity.

- You can access the script name and arguments via `sys.argv` as a list of strings.

- We can change the size of a `list` even with slicing:
```python
a = [1, 2, 3, 4 , 5, 6]
a[1:3] = [7, 8, 9, 10, 11, 12, 13, 14, 15]
print(a)

# output
[1, 7, 8, 9, 10, 11, 12, 13, 14, 15, 4, 5, 6]
```

- `for` might have an `else` clause:
```python
for x in a:
    print(x)
else:
    print("done")
```
It would work well with `try`
```python
try:
    result = risky_operation()   # may raise
except SomeError:
    handle_error()
else:
    process_result(result)       # safe code
```


- Important: the default values of a function are only evaluated once:
```python
def f(a, L=[]): 
	L.append(a) 
	return L  

print(f(1)) 
print(f(2)) 
print(f(3))


# output
[1] 
[1, 2] 
[1, 2, 3]
```


- `docstring` conventions:
1. The first line is the succinct, precise summary of function's functionality. This line should begin with capital letter and end with a period.
2. If there are additional lines, the second line should be left blank for separation.

- `PEP` is short for Python Enhancement Proposal.

```python
# Trước đây

data = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11]
n = len(data)
if n > 10:
    print(f"Too long ({n})")

  

# Với walrus operator
if (n := len(data)) > 10:
    print(f"Too long ({n})")
```


- `list` is not optimized for queues (as it does for stack), thus use `collections.deque` instead:
```python
from collections import deque
```

==Immutable==:
- `string`