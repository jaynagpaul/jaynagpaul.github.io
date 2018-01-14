---
layout: post
title: Automatic Type Annotations For Python
description: How to setup automatic type annotations for Python, to maintain type safety with less hassle.
image: assets/images/python.png
tags: python tutorial
comments: true
---
## Setup
This guide assumes a working installation and basic knowledge of Python 3.6.x. First we'll install the library used to create automatic type annotations, [MonkeyType by Instagram](https://github.com/Instagram/MonkeyType).
```sh
>>> pip install MonkeyType
>>> pip install pytest
```
Then we'll create our example project to test out MonkeyType. Let's try it out with the fibonacci sequence.
```sh
>>> mkdir fibonacci
>>> cd fibonacci
```

```py
# fibonacci.py
def fibonacci(n):
    if n == 0: 
        return 0
    elif n == 1:
        return 1
    else: 
        return fibonacci(n-1)+fibonacci(n-2)
```

Let's try it out

```py
>>> fibonacci(10)
55
>>> fibonacci('10')
Traceback (most recent call last):
TypeError: unsupported operand type(s) for -: 'str' and 'int'
```

As you can see, this function is expecting for you to pass in an integer for it otherwise it will raise a type error. We can manually add the type annotation to help future consumers of this code, but being humans **and** programmers we often forget.

## Automating It
Great, we now have a file named fibonacci.py in our folder named fibonacci. Let's write a simple test for us to use.
```py
# test.py
from fibonacci import fibonacci as F

def test_fibonacci():
    assert F(10) == 55
    assert F(0) == 0
    assert F(1) == 1
```
We can test this out by running the command `pytest`.

Now it's time for us to collect the call traces. Run the command `monkeytype run test.py`. You should see a new file appear "monkeytype.sqlite3", this is a sqlite database filled with each function call, and the types returned and passed in. We can apply type annotations with the command `monkeytype apply fibonacci`.
Now check out the file fibonacci.py:

```py
def fibonacci(n: int) -> int:
    if n == 0:
        return 0
    elif n == 1:
        return 1
    else:
        return fibonacci(n - 1) + fibonacci(n - 2)
```
**Success!** We have successfully added type annotations to our module with barely any work!