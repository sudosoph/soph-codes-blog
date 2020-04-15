---
title: Abstraction
date: "2020-04-14"
description: "A simple look at abstraction concepts through the Fibonacci example"
---

Abstraction is a wonderful programming concept because it saves us a lot of time via the use of **parameters, scoping, and recursion**... In other words, it is the perfect programming principle for the lazy programmer!

While Bertrand Russell [praised idleness](https://harpers.org/archive/1932/10/in-praise-of-idleness/), it is second only to laziness as a desirable quality in a programmer, and dare I say, virtue! They don't do any unnecessary work and instead seek to make their programs more abstract as a result.

Let's use the example of Fibonacci numbers:
```shell
fibs = [0, 1]
for i in range(8):
		fibs.append(fibs[-2] + fibs[-1])
```
While this gives the desired result:
```shell
>>> fibs
[0, 1, 1, 2, 3, 5, 8, 13, 21, 34]
```

It doesn't scale. So we write a more flexible one that uses a dynamic range, with a length determined through user input:
```shell
fibs = [0, 1]
num = int(input("How many Fibonacci numbers do you want? "))
for i in range(num-2):
		fibs.append(fibs[-2] + fibs[-1])
print(fibs)
```

In an application where you compute the frequency of all the words used on several web pages, you would use recursion rather than write the code again several times. So the previous code becomes:

```shell
num = input("How many numbers do you want? ")
print(fibs(num))
```