---
title: Abstraction
date: "2020-04-14"
description: A concise look at abstraction concepts with corresponding code examples.
---

**Abstraction** is the art of hiding unnecessary details. It is a wonderful programming concept because it saves us a lot of time via the use of **parameters, scoping, and recursion**... 

While Bertrand Russell [praised idleness](https://harpers.org/archive/1932/10/in-praise-of-idleness/), idleness is second only to laziness as a desirable quality in a programmer, and dare I say, virtue! We don't do any unnecessary work but rather seek to make our programs more abstract as a result.

## Example: Fibonacci Numbers
```shell
fibs = [0, 1]
for i in range(8):
		fibs.append(fibs[-2] + fibs[-1])
```
While this gives the desired result...
```shell
>>> fibs
[0, 1, 1, 2, 3, 5, 8, 13, 21, 34]
```

...it doesn't scale! So we write a more flexible one that uses a dynamic range, with a length determined through user input:
```shell
fibs = [0, 1]
num = int(input("How many Fibonacci numbers do you want? "))
for i in range(num-2):
		fibs.append(fibs[-2] + fibs[-1])
print(fibs)
```

In an application where you compute the frequency of all the words used on several web pages, you would use recursion (which is discussed in depth below) rather than write the code again several times. So the previous code becomes:

```shell
num = input("How many numbers do you want? ")
print(fibs(num))
```

Reading the number and printing out the result is spelled out, as it is specific to this program, but the Fibonacci function itself is written abstractly and we call it only when we need its functionality, leading to much cleaner code.

---

## Functions

**Functions**: Blocks of statements you can call that perform an action and return a value.

- Functions are defined with the ```def``` statement
- Can receive values (parameters) and may return one or more values depending on the result of their computation

_Note:_ In Python, they do not always return a value. Functions that don't return anything are called _procedures_ in other languages, such as Pascal, but in Python, these are still referred to as functions. They simply do not have a return statement, or if they do, there is no value after the word "return" and it is used to end the function. When you don't tell a function what to return, it will return ```None```.

- You can tell whether a function is callable or not with the built-in function ```callable```

You can add comments in Python using the hash sign (#). If you put the string at the start of a function, it will be stored along with the function as a \_docstring_\.

```shell
def square(x):
		'Calculates the square of the number x.'
		return x * x
```

You can then access the docstring as such:
```shell
>>> square.__doc__
'Calculates the square of the number x.'
```

__doc__ is a function attribute, and the underscores in the attribute name signal that it is a special attribute.

---

## Parameters

**Parameters**: Variables that are set when a function is called

- **Formal Parameters**: Variables you write after your function name in def statements
- **Actual Parameters**: Values you supply when you _call_ the function. Also known as **arguments**

Parameters are kept in a **local scope**.
- Strings, numbers, and tuples are _immutable_, so you can't modify them. Rather, you can only replace them with new values.
- Mutable data structures such as a list can change parameters

The point of abstraction is to hide all the gory details, and one convenient way is to use functions to initialize a data structure:

```shell
def init(data):
		data['first'] = {}
		data['last'] = {}
		data['phone'] = {}
		data['addr'] = {}
```

Moving initialization statements inside a function makes the code more readable:
```shell
>>> customers = {}
>>> init(customers)
>>> customers
{'first': {}, 'last': {}, 'phone': {}, 'addr': {}}
```

The order may vary when using a dictionary as the keys don't have a specific order.

#### Two Kinds of Parameters:
1. **Positional parameters**
2. **Keyword parameters**

**Positional Parameters**: Parameters whose positions are important

### Example:

```shell
def hello1(greeting, name):
		print('{}, {}!'.format(greeting, name))

def hello2(name, greeting):
		print('{}, {}!'.format(greeting, name))
```

Here, the position of the variables matter. One trick is to supply the name of the parameter, and the order won't matter:

```shell
>>> hello1(greeting = 'Hello', name = 'world')
Hello, world!
```

**Keyword parameters**: Help clarify the role of the parameters
- In our example, "greeting" and "name"

---

## Scope

**Scope**: A type of "invisible dictionary". Also called "namespace"

#### Two Kinds of Scopes:
1. Local 
2. Global

- In addition to the **global scope**, each function call creates a new one.
- Functions can rebind a variable locally, but if you declare the same variable in the global namespace, this doesn't affect its value in the outer scope.
- There is no problem accessing a global variable inside a function so long as it is _read-only_ (not rebinding, as assigning a value to a variable inside a function automatically becomes local unless you specify otherwise)
- **Shadowing**: If a local variable or parameter has the same name as a global variable you are trying to access, the global variable will be shadowed by the local one
- You can use the function ```globals``` to gain access to the global variable (returns dictionary of global variables)

### Example:

```shell
>>> def combine(parameter):
... 		print(parameter + globals()['parameter'])
...
>>> parameter = 'berry'
>>> combine('Shrub')
Shrubberry
```
**Nested Scopes**: Putting one function inside another. Useful when using one function to "create" another
- The outer function returns the inner one
- This means the function itself is _returned,_ not called
- The returned function carries the environment and associated local variables with it
- Nested scopes are also known as **closures** (a function that stores its enclosing scopes)
- You cannot rebind outer scope variables, but rather use _nonlocal_ keyword to assign variables to outer, nonglobal scopes

### Example:

**Code:**
```shell
def multiplier(factor):
		def multiplyByFactor(number):
				return number * factor
		return multiplyByFactor
```
**Output:**
```shell
>>> double = multiplier(2)
>>> double(5)
10

>> multiplier(5)(4)
20
```
Now, onto recursion...

---

## Recursion

**Recursion**: Functions calling themselves

***Infinite loop***: a loop beginning with ```while True``` and containing no ```break``` or ```return statements```

### Example:
```shell
def recursion():
		return recursion()
```
While this should theoretically run forever, every time the function is called, it uses up a bit of memory, so after a while there is no more room and the program ends with ```maximum recursion depth exceeded```.

What about actually useful recursive functions?
They consist of 2 parts:
1. ***Base case***: solving the smallest possible problem and returning a value directly
2. ***Recursive case***: one or more recursive calls on smaller parts of the problem

### Example: Factorial

A factorial is the product of an integer and all the integers below it. For example, 5! (read: "5 factorial") is equivalent to 5 x 4 x 3 x 2 x 1 = 120

**Non-recursive way of writing a factorial function (using a loop)**:
```shell
def factorial(n):
		result = n
		for i in range(1, n):
				result *= i
		return result
```

We set the result to n, and then multiply it by each number from 1 to n-1.

**Recursive way of writing a factorial function**:
```shell
def factorial(n):
		if n == 1:
				return 1
		else:
				return n * factorial(n - 1)
```
- **Base case**: factorial of 1 is 1
- **Recursive case**: the factorial of a number n greater than 1 is the product of n and the factorial of n-1.

factorial(n - 1) is a different entity than factorial(n)

### Example: Powers

Calculating powers like the built-in function ```pow``` (or ** operator)

**Non-recursive way of writing a power function (using a loop)**:
```shell
def power(x, n):
		result = 1
		for i in range(n):
				result *= x
		return result
```
Raising x to the power of n is equivalent to multiplying a number times itself n-1 times. So power(2, 3) is 2 multiplied with itself twice (2 x 2 x 2 = 8)

**Recursive way of writing a power function (using a loop)**:
```shell
def power(x, n):
		if n == 0:
				return 1
		else:
				return x * power(x, n - 1)
```
- **Base case**: power(x, 0) = 1 for all numbers x
- **Recursive case**: power(x, n) for n > 0 is the product of x and power(x, n-1)

**Trade-off**: Loops can be more efficient, but recursion can be more readable.
