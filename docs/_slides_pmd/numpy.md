---
---

## NumPy

Python lists contain elements of any data type, so they are not
useful as numeric vectors or arrays. In particular, the `+` and `*` operations do not
perform numerical calculations when applied to lists.

```python
[1, 2] + [3, 4]
```

```python
[5, 6] * 2
```

===

The [numpy](){:.pylib} package, with its "array" and "ndarray" types, provide a ubiquitous representation of vectors, matrices, and higher-dimensional arrays.

```{python, title = "{{ site.handouts }}"}
import numpy as np
vect = np.array([5, 20, 12])
```
```python
vect
```

The first line of this code, `import numpy as np`, gives Python access to functions
from the `numpy` package, using the `package.function` syntax. To save time typing 
package names, Python programmers often define short aliases for them, such as `np` 
here. This allows us to write `np.array` instead of `numpy.array` on the following 
line.
{:.notes}

===

The definition of the array itself looks much like a Python list, and array
subsetting follows the same conventions as list subsetting. The main difference is
for multidimensional arrays, where the indices in each dimensions can be 
separated by commas within one set of brackets. As an example, we create a 2 x 3
matrix and selected the first two columns.

```{python, title = "{{ site.handouts }}"}
mat = np.array([[1, 2, 3], [4, 5, 6]])
```
```python
mat[:, 0:2]
```

The initial ":" (with no indices) is interpreted as "select all rows".
{:.notes}

===

Arithmetic operators and basic mathematical functions (e.g. exp, sqrt) are
applied element-wise to arrays.

```python
vect + np.array([1, 2, 3])
```

```{python}
vect * 2
```

```{python}
mat * vect
```

===

In the last example, `vect` was multipled element-wise with each row of `mat`. To 
multiply a matrix and a vector (or two matrices, or two vectors in a dot-product), 
use the `dot` function.

```{python}
np.dot(mat, vect)
```
