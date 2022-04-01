# Useful and less useful utils

## SkipChain
```python
class SkipChain:
    def __init__(self, *sliceables):
        self.sliceables = sliceables

    def __getitem__(self, slice: slice):
        start = slice.start
        stop = slice.stop
        items = []
        for sliceable in self.sliceables:
            sliceable_len = len(sliceable)
            if start > sliceable_len:
                start -= sliceable_len
                stop -= sliceable_len
                continue
            items_len = len(items)
            items.extend(
                sliceable[start -items_len:stop - items_len]
            )
            if len(items) >= (stop - start):
                break
        return items

    def __len__(self):
        return sum(len(sliceable) for sliceable in self.sliceables)


print(SkipChain([0, 1, 2, 3], [4, 5, 6, 7, 8, 9])[0:10])
print(SkipChain([0, 1, 2, 3], [4, 5, 6, 7, 8, 9])[0:9])
print(SkipChain([0, 1, 2, 3], [4, 5, 6, 7, 8, 9])[0:8])
print(SkipChain([0, 1, 2, 3], [4, 5, 6, 7, 8, 9])[0:5])
print(SkipChain([0, 1, 2, 3], [4, 5, 6, 7, 8, 9])[0:4])
print(SkipChain([0, 1, 2, 3], [4, 5, 6, 7, 8, 9])[5:9])

```

## Incremental mean value

```python
from dataclasses import dataclass
from typing import Optional


@dataclass
class Mean:
    count: int = 0
    value: Optional[int | float] = None

    def add(self, value: int | float):
        self.count += 1
        if self.value is None:
            self.value = value
        else:
            self.value = ((self.value * (self.count - 1)) + value) / self.count
        return self.value


m = Mean()
m.add(10)
m.add(-20)
m.add(40)
```

## Functional calculation of magnitude of an n-dimensional vector
```python
from math import sqrt
from functools import reduce
from itertools import islice

mag = lambda u: sqrt(reduce(lambda s, _s: (s**2) + _s, u))

# mag([2, 1]) == 2.23606797749979
```

## Pagination
```python
paginate = lambda s, x: [s[i:i+x] for i in range(len(s)) if i % x == 0]
# paginate([1, 2, 3, 4, 5], 3) == [[1, 2, 3], [4, 5]]
```

and a version using less cycles:

```python
from itertools import islice

def paginate(iterable, size):
    length = len(iterable)
    return (islice(iterable, i * size, i * size + size) for i in range(length // size + min(1, length % size)))

# paginate([1, 2, 3, 4, 5], 3) == [[1, 2, 3], [4, 5]]
```

## Nth order number of a set unsorted numbers (basically insort)
```python
def nth_order(n, iterable):
    iterable = list(iterable).copy()
    buffer = []
    while iterable:
        for i, buf in enumerate(buffer):
            if iterable[0] < buf:
                buffer.insert(i, iterable.pop(0))
                break
        else:
            buffer.append(iterable.pop(0))
    return buffer[n]

# nth_order(1, [9, 4, 3, 5, 2]) == 3
```

## Omission of keys in dictionary (from [stackoverflow](https://stackoverflow.com/a/41010331))
```python
def omit(dictionary, *keys):
    return {key: value for key, value in dictionary.items() if key not in keys}
```

## Experiments
```python
def round_up(x, n):
    """round_up(x: int, n: int) -> int
    
    Rounds x _up_ to the nearest multiple of n.
    """
    return (x // n + int(x % n != 0)) * n

# round_up(5, 8) == 8
# round_up(14, 8) == 16
```

### Recursive normalization & denormalization of dictionaries
```python
def normalize(dictionary_list, key):
    normalized_dictionary = {}
    for dictionary in dictionary_list:
        new_dictionary = {}
        new_dictionary_keys = omit(dictionary, key).keys()
        for dictionary_key in new_dictionary_keys:
            if isinstance(dictionary[dictionary_key], list) and dictionary_key in dictionary.keys():
                new_dictionary[dictionary_key] = normalize(dictionary[dictionary_key], key)
            else:
                new_dictionary[dictionary_key] = dictionary[dictionary_key]
        normalized_dictionary[dictionary[key]] = new_dictionary
    return normalized_dictionary
```
