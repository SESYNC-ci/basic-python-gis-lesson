---
---

## Data structures

The built-in structures for holding multiple values are:

- Tuple
- List
- Set
- Dictionary

===

## Tuple

The simplest kind of sequence, a tuple is declared with
comma-separated values, optionally inside `()`.


~~~python
T = 'x', 3, True
type(T)
~~~
{:.text-document title="{{ site.handouts[0] }}"}

~~~
Out[1]: tuple
~~~
{:.output}



Note that to declare a one-tuple without "(", a trailing "," is required.


~~~python
T = 'cat',
type(T)
~~~
{:.text-document title="{{ site.handouts[0] }}"}

~~~
Out[1]: tuple
~~~
{:.output}



===

## List

The more common kind of sequence in Python is the list, which is
declared with comma-separated values inside `[]`. Unlike a tuple, a
list is mutable.


~~~python
L = [3.14, 'xyz', T]
type(L)
~~~
{:.text-document title="{{ site.handouts[0] }}"}

~~~
Out[1]: list
~~~
{:.output}



===

## Subsetting Tuples and Lists

Subsetting elements from a tuple or list is performed with square
brackets in both cases, and selects elements using their integer
position starting from zero---their "index".


~~~python
L[0]
~~~
{:.input}
~~~
Out[1]: 3.14
~~~
{:.output}



===

Negative indices are allowed, and refer to the reverse ordering: -1 is
the last item in the list, -2 the second-to-last item, and so on.


~~~python
L[-1]
~~~
{:.input}
~~~
Out[1]: ('cat',)
~~~
{:.output}



===

The syntax `L[i:j]` selects a sub-list starting with the element at index
`i` and ending with the element at index `j - 1`.


~~~python
L[0:2]
~~~
{:.input}
~~~
Out[1]: [3.14, 'xyz']
~~~
{:.output}



A blank space before or after the ":" indicates the start or end of the list,
respectively. For example, the previous example could have been written 
`L[:2]`.

===

A potentially useful trick to remember the list subsetting rules in Python is
to picture the indices as "dividers" between list elements.

~~~
 0      1       2          3 
 | 3.14 | 'xyz' | ('cat',) |
-3     -2      -1
~~~
{:.input}

Positive indices are written at the top and negative indices at the bottom. 
`L[i]` returns the element to the right of `i` whereas `L[i:j]` returns
elements between `i` and `j`.


===

## An aside on Methods

Python is "object-oriented" in that everything beyond the basic data types (e.g. `int`, `str`, etc.) is called an object--an instance of an explicitly defined "class". It is equally if not more common to use functions that are "methods" of objects than to use functions that are independent of any object, although even "object-oriented" programming uses these regularly:


~~~python
'abc'.upper()
~~~
{:.text-document title="{{ site.handouts[0] }}"}

~~~
Out[1]: 'ABC'
~~~
{:.output}




~~~python
len(lst)
~~~
{:.text-document title="{{ site.handouts[0] }}"}

~~~
[0;31m---------------------------------------------------------------------------[0m
[0;31mNameError[0m                                 Traceback (most recent call last)
[0;32m<ipython-input-1-70b92d4ae0ca>[0m in [0;36m<module>[0;34m()[0m
[1;32m      1[0m [0;34m[0m[0m
[0;32m----> 2[0;31m [0mlen[0m[0;34m([0m[0mlst[0m[0;34m)[0m[0;34m[0m[0m
[0m
[0;31mNameError[0m: name 'lst' is not defined
~~~
{:.output}



===

Many objects in Python, including lists, are "mutable", meaning they can change in place without needing to be re-labled.


~~~python
L.pop()
~~~
{:.input}
~~~
Out[1]: ('cat',)
~~~
{:.output}



~~~python
L
~~~
{:.input}
~~~
Out[1]: [3.14, 'xyz']
~~~
{:.output}



A common mistake for those coming to Python from R, is to write `L = L.pop()`, which overwrites `L` with the output of `L.pop()`. This may be the intend when the "side-effect" on `L` is not used.

===

## Set

The third and last "sequence" data structure is a set, used mainly for quick access to set operations like "union" and "difference". Declare a set with comma-separated values inside `{}` or by casting another sequence with `set()`.


~~~python
S1 = set(L)
S2 = {3.14, 'z'}
S1.difference(S2)
~~~
{:.text-document title="{{ site.handouts[0] }}"}

~~~
Out[1]: {'xyz'}
~~~
{:.output}



Python is a rather principled language: a set is technically unordered, so its elements do not have an index. You cannot subset a set using `[]`.
{:.notes}

===

## Dictionary

Lists are useful when you need to access elements by their position in
a sequence. In contrast, a dictionary is needed to find values based
on arbitrary identifiers.

===

Construct a dictionary with comma-separated `key: value` pairs in `{}`.


~~~python
toons = {
  'Snowy': 'dog',
  'Garfield': 'cat',
  'Bugs': 'bunny',
}
type(toons)
~~~
{:.text-document title="{{ site.handouts[0] }}"}

~~~
Out[1]: dict
~~~
{:.output}



===

Individual values are accessed using square brackets, as for lists,
but the key must be used rather than an index.


~~~python
toons['Bugs']
~~~
{:.text-document title="{{ site.handouts[0] }}"}

~~~
Out[1]: 'bunny'
~~~
{:.output}



===

To add a single new element to the dictionary, define a new
`key:value` pair by assigning a value to a novel key in the
dictionary.


~~~python
toons['Goofy'] = 'dog'
~~~
{:.text-document title="{{ site.handouts[0] }}"}



~~~python
toons
~~~
{:.input}
~~~
Out[1]: {'Bugs': 'bunny', 'Garfield': 'cat', 'Goofy': 'dog', 'Snowy': 'dog'}
~~~
{:.output}



Dictionary keys are unique. Assigning a value to an existing key
overwrites its previous value.
