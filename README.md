# Random stuff

## Functional calculation magnitude for an n-dimensional vector
`
from math import sqrt
from functools import reduce

mag = lambda *u: sqrt(reduce(lambda s, _s: (s**2) + _s, u)) # mag(2, 1) == 2.23606797749979
`
