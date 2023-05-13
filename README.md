# Functional Programming, the Hard Way

This document is a _gentle_ introduction to the paradigm of functional programming for beginners. You are going to face some tough Python, fasten seat bells and stay tight.

As I just said, you are going to face tough code. Please don't do what a regular programmer would do when they see a code in a different, alien paradigm; scream and run. Regardless of how difficult the code looks like, stay here and try to understand. Get yourself a paper and a pencil. Try drawing the way the function works. Believe me, this is going to be a great exercise.

If everything goes right and you finish this document with a clean understanding of everything I mentioned, you will be ready for your Purely Functional Programming Languages journey, so Haskell!

Before starting, please make sure that you have a decent understanding of Python.

Well, people usually show some fibonacci or old-fashioned bullshit like that but let's try something else -- we are programming a simple calculator. Here's how it'll look like:

```
$ python program.py
Please enter numbers: 1 34 22
Please select the operation you want to perform [*, /, +, -]: +

Result: 57
```

Simple, right? Now why don't you try to code this in Python, without functional programming or anything? Simply do what you are going to do. Create something that works and come back here to see how to program it using **Functional Programming**. (You are not obligated to, feel free to continue if you want to.)

## Getting Started

Let's start with getting the input from user:

```python
def main():
    numbers   = input("Please enter numbers: ")
    operator  = input("Please select the operation you want to perform [*, /, +, -]: ")

    result = solution(numbers, operator)
    print(result)

def solution(numbers, operator):
    ...

# If you are not aware of this pattern, please check this out:
# https://realpython.com/if-name-main-python/
if __name__ == "__main__":
    main()
```

Simple. We've created a ``main`` function that runs the program. We also created a ``solution`` function that takes numbers and operator, and calculates the result. We simply would've written it like this:

```python
numbers   = input("Please enter numbers: ")
operator = input("Please select the operation you want to perform [*, /, +, -]: ")

# Calculating result...

print(result)
```

Here are some of the benefits of using separate functions:

- We used the ``__name__ == "__main__"`` pattern, so that importing the Python file won't cause our program to run. Instead, we will have access to ``main`` and ``solution`` but they won't run unless we execute them. You can test ``__name__ == "__main__"`` pattern if you'd like.
- Separating ``solution`` and ``main`` allows us to use ``solution`` function in another Python program. For example, if we create a user interface with tkinter and get the numbers and operator from GUI, we won't have to do any modifications on ``solution``. We can simply import and use it. We would get the input from UI and directly give it to ``solution``.

This is the base of our code. We will be working on ``solution`` from now on.

We will start with parsing the numbers. This is what we get from ``input``:

```
"1 34 22"
```

But this is not good, we need the numbers in this form:

```
[1, 34, 22]
```

So this is what we are going to do first. But before, we need to make sure you are aware of some fundamental concepts of functional programming. You can skip those if you already know very well.

## Preparation

### Hello, map!

Technically speaking, ``map`` is a [higher-order function](https://en.wikipedia.org/wiki/Higher-order_function). It's pretty simple. It applies a given function to each element of a collection (_e.g._ list, string).

Here's what it does:

```python
mynumbers = [1, 2, 3]

def multiply_with_2(number):
    return number * 2

print(
    list(map(multiply_with_2, mynumbers))
)
# 2, 4, 6
```

This is basically it. ``map`` takes a function and a "collection", applies the function to each element and returns the applied collection.

Well, we had to convert what ``map`` returned to a list, because in Python ``map`` returns a ``map`` object. This allows us to get the collection in any form, str, tuple or list.

Before moving on, please keep in mind that ``map`` is a common concept. It exists in most of the programming languages.

### Anonymous Function (i.e. lambda)

Here is an example:

```python
def multiply_with_2(number):
    return number * 2
```

This is too long, 2 lines. It's such a simple thing, why not write it in just one line:

```python
multiply_with_2 = lambda number: number * 2
```

``lambda`` expression can work anonymously, let's combine it with the code above.

```python
mynumbers = [1, 2, 3]

print(
    list(map(lambda number: number * 2, mynumbers))
)
# 2, 4, 6
```

We don't need a variable for numbers array neither:


```python
print(
    list(map(lambda number: number * 2, [1, 2, 3]))
)
```

The definition is pretty close to mathematical functions:

```
lambda n: n + 2

f(x) = x + 2
```

## Part 1: Parsing Numbers

So, what was I saying?

> We will start with parsing the numbers. This is what we get from ``input``:
> ```
> "1 34 22"
> ```
> But this is not good, we need the numbers in this form:
> ```
> [1, 34, 22]
> ```

Yeah. We need to parse numbers. Let's go!

First of all, we need numbers in this form: ``["1", "34", "22"]``. So that we can apply map to convert these strings to integers. There's a perfect method for it in Python:

```python
numbers.split()
# ["1", "34", "22"]

# It also takes an argument to choose where to split at:
numbers2 = "1$34$22"
numbers2.split("$")
```

Great, now let's map!

```python
list(map(lambda n: int(n), numbers.split()))
# [1, 34, 22]
```

Nice, but we miss one thing. ``int`` takes a str (or anything that can convert to int) and returns an int. Isn't this already what we need? Do we need ``lambda n: int(n)``? No, of course we don't!

```python
list(map(int, numbers.split()))
# [1, 34, 22]
```

Clean!

## Part 2: Hard Part

> As I just said, you are going to face tough code. Please don't do what a regular programmer would do when they see a code in a different, alien paradigm; scream and run. Regardless of how difficult the code looks like, stay here and try to understand. Get yourself a paper and a pencil. Try drawing the way the function works. Believe me, this is going to be a great exercise.

Alright, here's the part I mentioned before. Some of the code in this part will be hard to understand if you are freshly new in recursion and functional programming. Take your time, take breaks, draw the way it works. Don't move on without having a clear understanding of the code.

We are going to create ``fold`` on our own. ``fold`` is one of the standard higher-order functions. You can find diagrams explaining the way it works on the internet in case you struggle.

This part focuses on creating a function that takes a list of numbers and a function that takes two numbers, so that we can calculate a result:

```python
from collections.abc import Callable, Iterable
# For those that are not aware of typing in Python:
# https://docs.python.org/3/library/typing.html
# For those of you that are too lazy, simply ignore the part after (:) in variables. (fold(func, l, initial=0))
# They do nothing but inform the reader about the types of arguments and returns.

def fold(func: Callable[[int, int], int], l: Iterable[int], initial: int=0) -> Iterable[int]:
    ...
```

This is how we expect it to function:

```python
fold(lambda n1, n2: n1 + n2, [1, 2, 3]) # 1 + 2 + 3
# 6

fold(lambda n1, n2: n1*n2, [1,2,3,4], initial=1) # 1 * 2 * 3 * 4
# 24
```

You can try to code it with a (regular) imperative approach on your own, we are doing functional programming here. Remember, this is "Functional Programming, the Hard Way".

```python
def fold(func: Callable[[int, int], int], l: Iterable[int], initial: int=0) -> Iterable[int]:
    # Base Case
    if l == []: 
        return initial
    
    # Recursive Case
    return fold(func, l[1:], func(initial, l[0]))
```

### Anatomy of a recursive function

Recursive functions are simply functions that call themselves. They have **Base Cases**, which is an edge where the loop of self-calls should stop. Base cases prevent infinite loops.

They also have **Recursive Cases** which is the part where they call themselves with little modifications in arguments. For example, in the code above, recursion calls change the initial value of the fold by calling our target function on first element of the list and the initial value (``func(initial, l[0]``), and serve the list without its first element (since it's already processed).

Our _Base Case_ here returns the initial value instead of calling itself again if the given list is empty. You can try to draw a diagram to understand how is this useful.

## Part 3: Easier Part

Alright, let's combine our ``fold`` with our previous code and add a few more modifications. I'm not going to deep dive into details. Just make sure you don't miss anything.

```python
from collections.abc import Callable, Iterable
import operator

OPERATORS = {
    # "operator": (function, initial)
    "+": (operator.add, 0),
    "-": (operator.sub, 0),
    "*": (operator.mul, 1),
    "/": (operator.truediv, 1),
}

def main():
    numbers  = input("Please enter numbers: ")
    operator = input("Please select the operation you want to perform [*, /, +, -]: ")

    result = solution(numbers, operator)
    print(result)

def fold(func: Callable[[int, int], int], l: Iterable[int], initial: int=0) -> Iterable[int]:
    if l == []:
        return initial

    return func(l[0], fold(func, l[1:], initial))

class UnknownOperatorError(Exception): pass

def solution(numbers, operator):
    try:
        operator, initial = OPERATORS[operator]
    except KeyError:
        raise UnknownOperatorError(f"{operator} operator is unknown.")

    return fold(
        operator,
        list(map(int, numbers.split())),
        initial,
    )

if __name__ == "__main__":
    main()
```

> Exercise: I changed the fold code, its now a "foldr" instead of a "foldl", which means it starts applying from right. Read and interpret the code.

