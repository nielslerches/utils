# Random stuff

## Functional calculation of magnitude for an n-dimensional vector
```python
from math import sqrt
from functools import reduce

mag = lambda *u: sqrt(reduce(lambda s, _s: (s**2) + _s, u)) # mag(2, 1) == 2.23606797749979
```

## Pagination
```python
paginate = lambda s, x: [s[i:i+x] for i in range(len(s)) if i % x == 0]
# paginate([1, 2, 3, 4, 5], 3) == [[1, 2, 3], [4, 5]]
```

and for those who don't like to read:

```python
paginate = lambda s, x: [ s[i*x:i*x+x] for i in range((len(s) // x) + 1 if len(s) % x != 0 else len(s) // x) ]
# paginate([1, 2, 3, 4, 5], 3) == [[1, 2, 3], [4, 5]]
```
