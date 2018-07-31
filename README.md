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
def paginate(iterable, size):
    length = len(iterable)
    return [iterable[i*size:i*size+size] for i in range(length // size + min([1, length % size]))]

# paginate([1, 2, 3, 4, 5], 3) == [[1, 2, 3], [4, 5]]
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
    return (x // n) * n if x % n == 0 else (x // n + 1) * n

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
