---
---

## Iteration

The data structures just discussed have multiple values. Subsetting is
one way to get at them individually. Stepping through all values is
called iterating.

Python formally declares a thing "iterable" if it can be used in an
expression `for x in y`. where `y` is the iterable thing and `x` will
label each element in turn.

===

## In-line Iteration

Packing the `for x in y` expression inside a sequence constructor is
one way to build a sequence.


~~~python
letters = [x for x in 'abcde']
letters
~~~
{:.text-document title="{{ site.handouts[0] }}"}

~~~
Out[1]: ['a', 'b', 'c', 'd', 'e']
~~~
{:.output}



This way of building a list with `for` and `in` is called a "list comprehension" in Python. A set comprehension works just the same inside "{}" rather than "[]"

===

## Dictionary Comprehension

Create a dictionary using an iterable by declaring a `key: value` pair.


~~~python
caps = {x: x.upper() for x in 'abcde'}
caps
~~~
{:.input}
~~~
Out[1]: {'a': 'A', 'b': 'B', 'c': 'C', 'd': 'D', 'e': 'E'}
~~~
{:.output}


