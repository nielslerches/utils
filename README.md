# Random stuff

## Functional calculation of magnitude of an n-dimensional vector
```python
from math import sqrt
from functools import reduce

mag = lambda u: sqrt(reduce(lambda s, _s: (s**2) + _s, u)) # mag([2, 1]) == 2.23606797749979
```

## Pagination
```python
paginate = lambda s, x: [s[i:i+x] for i in range(len(s)) if i % x == 0]
# paginate([1, 2, 3, 4, 5], 3) == [[1, 2, 3], [4, 5]]
```

and a version using less cycles:

```python
def paginate(s, x):
    if len(s) % x != 0:
        return [s[i*x:i*x+x] for i in range(len(s) // x + 1]
    else:
        return [s[i*x:i*x+x] for i in range(len(s) // x)]

# paginate([1, 2, 3, 4, 5], 3) == [[1, 2, 3], [4, 5]]
```

## Experiments
```python
def round_up(x, n):
    """round_up(x: int, n: int) -> int
    
    Rounds x _up_ to the nearest multiple of n.
    """
    return (x // n) * n if x % n == 0 else (x // n + 1) * n

# round_up(5, 8) == 8
# round_up(14, 8) == 16
```
