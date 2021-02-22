---
title: "Python Decorators"
date: 2020-06-28T14:18:03+08:00
draft: false
categories: ["blog"]
tags: ["blog", "python", "decorators"]
---

{{< image src="https://upload.wikimedia.org/wikipedia/commons/f/f8/Python_logo_and_wordmark.svg" alt="Python Logo" position="center">}}

## Objective

In this short blog, I would like to introduce **Python decorators** to everyone. Some of you might have heard and used it daily.
And to those who have never heard of it, let go through some demonstration to show how powerful it can be.

## What a Python Decorator is

*TL;DR*: It is a Python object that is used to modify function or class regarding its behaviour.

***

### Examples

#### 1. Without Decorators

Let's start with vanilla functions to do some basic arithmetic operations.

```python
def addition(a: int, b: int) -> int:
    return a+b


def substraction(a: int, b: int) -> int:
    return a-b


def multiplication(a: int, b: int) -> int:
    return a*b
```

Clearly, the results from using those functions are pretty simple and straight forward.

```python
addition(1, 2) # 3
substraction(1, 2) # -1
multiplication(1, 2) # 2
```

So far so good. What's the problem with those functions? What if we parse other data types, besides *integer*, to one of the functions. What will happen? Of course, it will display an error since we have not handled type checking at all.

```python
addition('a', 2)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "main.py", line 8, in addition
    return a+b
TypeError: can only concatenate str (not "int") to str
```

Let's fix it by adding type checking to each function. There are a good number of approaches to check data type. For the sake of simplicity, we will use this following condition.

```python
isinstance(num, int) # one parameter

all([isinstance(num, int) for num in [1.....n]]) # multiple parameters
```

Apply the condition to all existing functions

```python
def addition(a: int, b: int) -> int:
    if all([isinstance(num, int) for num in [a, b]]):
        return a+b


def substraction(a: int, b: int) -> int:
    if all([isinstance(num, int) for num in [a, b]]):
        return a-b


def multiplication(a: int, b: int) -> int:
    if all([isinstance(num, int) for num in [a, b]]):
        return a*b
```

As you can see, the functions will now only return data when the arguments parsed are all *integer*

```python
addition(1, -5) # -4
addition(1, 'sd')
```

The problem of using this approach is that we need to apply the condition to every function that requires type checking which can be tedious.

***

#### 2. With Decorators

We can simplify all of that by using **python decorators**. First, we define an outer function to receive a decorated function. Then we create a wrapper function (inner) to deal with all logics we need.

```python
from functools import wraps

def type_check(func):
    wraps(func)
    def wrapper(*args, **kwargs):
        res = None
        if all([isinstance(num, int) for num in [*args]]):
            print('Running function {}'.format(func.__name__))
            res = func(*args, **kwargs)
        return res
    return wrapper
```

Apply the decorators to all existing functions with *@decoration_function_name*. As a result, we have a clear and clean code which works as intended.

```python
@type_check
def addition(a: int, b: int) -> int:
    return a+b


@type_check
def substraction(a: int, b: int) -> int:
    return a-b


@type_check
def multiplication(a: int, b: int) -> int:
    return a*b
```

```python
multiplication(1, 434)
# Running function multiplication
# 434

multiplication(1, []) # None
```

***

## Wrap Up

A Python decorator is a higher-order function that allows a user to add new functionality to an existing object without modifying its structure. The applications of a decorator can be..

* Authorization (JWT, etc)
* Indicate running functions
* Measure time executed
* etc.
