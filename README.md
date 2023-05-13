# Functional Programming, the Hard Way

This document is a _gentle_ introduction to the paradigm of functional programming for beginners. You are going to face some tough Python, fasten seat bells and stay tight.

As I just said, you are going to face tough code. Please don't do what a regular programmer would do when they see a code in a different, alien paradigm; scream and run. Regardless of how difficult the code looks like, stay here and try to understand. Get yourself a paper and a pencil. Try drawing the way the function works. Believe me, this is going to be a great exercise.

Before starting, please make sure that you have a decent understanding of Python.

Well, people usually show some fibonacci or old-fashioned bullshit like that but let's try something else -- we are programming a simple calculator. Here's how it'll look like:

```
$ python program.py
Please enter numbers: 1 34 22
Please select the operation you want to perform [*, /, +, -]: +

Result: 57
```

Simple right? Now why don't you try to code this in Python, without functional programming or anything? Simply do what you are going to do. Create something that works and come back here to see how to program it using **Functional Programming**. (You are not obligated to, feel free to continue if you want to.)

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
- Separating ``solution`` and ``main`` allows us to use ``solution`` function in another Python program. For example, if we create a user interface with tkinter and get the numbers and operator from GUI, we won't have to do any modifications on ``solution``. We can simply import and use it. We get the input from UI and directly give it to ``solution``.

This is the base of our code. We will be working on ``solution`` from now on.

We will start with parsing the numbers. This is what we get from ``input``:

```
"1 34 22"
```

But this is not good, we need the numbers in this form:

```
[1, 34, 22]
```

So this is what we are going to do first. But before, we need to make sure you are aware of some fundamental concepts of functional programming. You can skip those that you already know very well.

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

