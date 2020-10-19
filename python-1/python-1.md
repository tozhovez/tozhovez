# Yield Keyword in Python – A Simple Illustrated Guide

## Introduction To **yield** In Python

While using a function, we generally use the `return` [keyword](https://blog.finxter.com/python-cheat-sheet/) to return a value computed by the function. Similarly, the `yield` keyword also returns a value from a function, but it also maintains the state of the local variables inside the function and when the function is reused in the program, the execution of the function begins from the state of the `yield` statement that was executed in the previous function call.

**Example:**

```text
def counter():
    x = 1
    while x <= 5:
        yield x
        x += 1

for y in counter():
    print(y)
```

**Output:**

```text
1
2
3
4
5
```

To understand the usage of yield keyword, you have to understand what are:

* Iterables
* Generators

So let us discuss generators and Iterables before diving into the `yield` keyword.

## Iterables

An **iterable** is an object in Python from which we can get an iterator. For example, when a list is created, all its items can be iterated one by one. Thus, reading the items of the list one by one is known as iteration while the list is iterable. In Python, string, [lists](https://blog.finxter.com/python-lists/), [sets](https://blog.finxter.com/sets-in-python/), [tuples](https://blog.finxter.com/the-ultimate-guide-to-python-tuples/), and [dictionaries](https://blog.finxter.com/python-dictionary/) are iterable containers from which we can get an iterator.

**Example:**

```text
name = "FINXTER"
li = [1,2,3]
tup = (4,5,6)
s = {"A","B","C"}
d = {"a":100,"b":200,"c":300}

print("\nIterating over String:")
for x in name:
  print(x, end=", ")
print("\nIterating over list:")
for x in li:
  print(x, end=" ")
print("\nIterating over tuple:")
for x in tup:
  print(x, end=" ")
print("\nIterating over set:")
for x in s:
  print(x, end=" ")
print("\nIterating over dictionary:")
for x in d:
  print(d[x], end=" ")
```

**Output:**

```text
Iterating over String:
F, I, N, X, T, E, R, 
Iterating over list:
1 2 3 
Iterating over tuple:
4 5 6 
Iterating over set:
A C B 
Iterating over dictionary:
100 200 300
```

So we know, what is an iterable object. But what is an iterator?

### ❖ **Iterator**

Simply put, an iterator is any object that can be iterated upon. Iterators are implemented using loops.

**Iterators implement the following methods which are known as iterator protocols:**

* \_\_iter\_\_\(\) : returns the iterator object.
* \_\_next\_\_\(\) : allows us to perform operations and returns the next item in the sequence.

Let us have a look at the following program how we can iterate through an iterator in Python using the iterator protocol.

**Example:** Returning an iterator from a list\(iterable\) and printing each value one by one:

```text
li = [1,2,3,4,5]
it = iter(li)

print(next(it))
print(next(it))
print(next(it))
print(next(it))
print(next(it))
```

**Output:**

```text
1
2
3
4
5
```

Now that brings us to the question, what’s the difference between an iterator and iterable?

Here’s a one-liner to answer that:

> Every **Iterator** is an **iterable**, but every **iterable** is not an **iterator**.

For example, a list is an iterable but it is not an iterator. We can create an iterator from an iterable object using the iterable object as shown above.

### ❖ **Creating Iterator Objects**

As mentioned earlier, the `__iter__()` and `__next__()` methods have to be implemented in an object/class to make it an iterator.

**Example:** The following program demonstrates the creation of an iterator that returns a sequence of numbers starting from 100 and each iteration will increase the value by 100.

```text
class IterObj:
  def __iter__(self):
    self.value = 100
    return self

  def __next__(self):
    x = self.value
    self.value += 100
    return x

obj = IterObj()
it = iter(obj)

print(next(it))
print(next(it))
print(next(it))
```

**Output:**

```text
100
200
300
```

The above program will continue to print forever if you keep using the `next()` statements. There must be a way to stop the iteration to go on forever. This is where the `StopIteration` statement comes into use.

### ❖ **StopIteration**

Once the iteration is done for a specific number of times, we can define a terminating condition that raises an error once the desired number of iterations are over. This terminating condition is given by the **StopIteration** statement.

**Example:**

```text
class IterObj:
  def __iter__(self):
    self.value = 100
    return self

  def __next__(self):
    if self.value <= 500:
      x = self.value
      self.value += 100
      return x
    else:
      raise StopIteration

obj = IterObj()
it = iter(obj)

for a in it:
  print(a)
```

**Output:**

```text
100
200
300
400
500
```

## Generators

While using iterators, we learned that we need to implement `__iter__()` and `__next__()` methods along and raise `StopIteration` to keep track of the number of the iterations. This can be quite lengthy and this is where generators come to our rescue. All the procedures that need to be followed while using iterators are automatically handled by generators.

**Generators** are simple functions used to create iterators and return an iterable set of items, one value at a time.

➡ You can iterate over generators only once. Let us have a look at this in a program.

**Example 1:** Using an iterator to iterate over the values twice.

```text
it = [x for x in range(6)]
print("Iterating over generator")
for i in it:
  print(i, end=", ")
print("\nIterating again!")
for j in it:
  print(j, end=", ")
```

**Output:**

```text
Iterating over generator
0, 1, 2, 3, 4, 5, 
Iterating again!
0, 1, 2, 3, 4, 5,
```

**Example 2:** Using generator to iterate over values. \( The generator can be used only once, as shown in the output.\)

```text
gen = (x for x in range(6))
print("Iterating over generator")
for i in gen:
  print(i, end=", ")
print("\nTrying to Iterate over the generator again!")
for j in gen:
  print(j, end=", ")
```

**Output:**

```text
Iterating over generator
0, 1, 2, 3, 4, 5, 
Trying to Iterate over the generator again!
```

➡ Generators do not store all the values in the memory, instead, they generate the values on the fly. In the above example 2, the generator calculates and prints the value 0 and forgets it and then calculates and prints 1 and so on.

Now this brings us to our discussion on the `yield` keyword.

## The **yield** Keyword

As mentioned earlier, `yield` is a keyword similar to the `return` keyword, but in case of `yield` the function returns a generator.

**Example:** The following uses a generator function that yields 7 random integers between 1 and 99.

```text
from random import randint

def game():
    # returns 6 numbers between 1 and 50
    for i in range(6):
        yield randint(1, 50)

    # returns a 7th number between 51 and 99
    yield randint(51,99)

for random_no in game():
       print("Lucky Number : ", (random_no))
```

**Output:**

```text
 Lucky Number :  12
 Lucky Number :  12
 Lucky Number :  47
 Lucky Number :  36
 Lucky Number :  28
 Lucky Number :  25
 Lucky Number :  55
```

In the above program the generator function `game()` generates 6 random integers between 1 and 50 by executing the `yield` statement one at a time and finally generates the 7th random number between 51 and 99 by executing the yield outside the loop.

➡ **Note:** When the function is called, the code within the function body does not run. Instead, the function body simply returns the generator object, and then the code will continue from where it left off each time the `for` loop uses the generator. Tricky!!! Isn’t it? 😉

Let us discuss the workflow to make things a little simple:

1. When the `for loop` is used for the first time, it calls the generator object created from the function. It runs the code in the function from the beginning until it hits `yield`.
2. Then it returns the first value in the loop.
3. Then each subsequent function call runs another iteration of the loop inside the function and returns the next value.
4. This continues until the generator is empty, that is when the function runs without an `yield` statement. This happens when the loop is exhausted or the `if-else` condition is no longer met.

**Things To Remember:**

* Since yield stores the state of local variables, overhead of memory allocation is controlled.
* This also ensures that the program control flow doesn’t start from the beginning all over again, thereby saving time.
* However, time and memory optimization can make the code complex to grasp.

### **Comparing Time And Memory Optimization For Iterator Functions Vs Generators**

**Example 1:** The program given below computes the time and memory usage while using a function with an iterator.

```text
import time
import random
import os
import psutil


mobile_name = ["iPhone 11", "iPhone XR", "iPhone 11 Pro Max"]
colors = ["red","black","grey"]
def mobile_list(ph):
    phones = []
    for i in range(ph):
      phone = {
        'name': random.choice(mobile_name),
        'color': random.choice(colors)
      }
      colors.append(phone)
    return phones



# Calculate time of processing
t1 = time.time()
cars = mobile_list(1000000)
t2 = time.time()
print('Took {} seconds'.format(t2-t1))

# Calculate Memory used
process = psutil.Process(os.getpid())
print('Memory used: ' + str(process.memory_info().rss/1000000))
```

**output:**

```text
Took 14.238950252532959 seconds
Memory used: 267.157504
```

**Example 2:** The following program uses a generator with the yield statement instead of a function and then we calculate the memory and time used in this case.

```text
import time
import random
import os
import psutil


mobile_name = ["iPhone 11", "iPhone XR", "iPhone 11 Pro Max"]
colors = ["red","black","grey"]
def mobile_list(ph):
    for i in range(ph):
      phone = {
        'name': random.choice(mobile_name),
        'color': random.choice(colors)
      }
      yield phone


# Calculate time of processing
t1 = time.time()
for car in mobile_list(1000000):
    pass
t2 = time.time()
print('Took {} seconds'.format(t2-t1))

# Calculate Memory used
process = psutil.Process(os.getpid())
print('Memory used: ' + str(process.memory_info().rss/1000000))
```

**Output:**

```text
Took 7.272227048873901 seconds
Memory used: 15.663104
```

The above examples clearly depict the superiority of generators and `yield` keyword over normal functions with `return` keyword.

**Disclaimer:** You have to `pip install psutil` so that the code works in your machine. Further, the time and memory usage values returned will vary based on the specifications of the machine in use.

## Exercise

Now let us have some practice. Run the code given below to find out a real-time example of generators and the yield keyword in Python.

**Hint:** In mathematics, the Fibonacci numbers, commonly denoted Fₙ, form a sequence, called the Fibonacci sequence, such that each number is the sum of the two preceding ones, starting from 0 and 1. That is, and for n &gt; 1. \(Source: [Wikipedia](https://en.wikipedia.org/wiki/Fibonacci_number)\)

```text
def fibo(a=0, b=1):
    while True:
        yield a
        a, b = b, a + b

f = fibo()
print(', '.join(str(next(f)) for _ in range(10)))
```

## **return** Keyword vs **yield** Keyword

Before we conclude our discussion, let us finish what we started and discuss the difference between the `yield` and `return` statements in Python.

## Conclusion

In this article we learned:

* What are Iterables?
* What are Iterators?
* The difference between Iterables and Iterators.
* Creating Iterator Objects.
* The `StopIteration` statement.
* What are Generators in Python?
* The Yield Keyword.
* Comparing Time And Memory Optimization For Iterator Functions Vs Generators.
* The difference between `return` and `yield` keywords.

Here’s a small recap of the concepts that we learned in this article; please follow the slide show give below:

Please [subscribe](https://blog.finxter.com/subscribe/) and [stay tuned](https://blog.finxter.com/) for more interesting articles!

## Where to Go From Here?

Enough theory, let’s get some practice!

To become successful in coding, you need to get out there and solve real problems for real people. That’s how you can become a six-figure earner easily. And that’s how you polish the skills you really need in practice. After all, what’s the use of learning theory that nobody ever needs?

**Practice projects is how you sharpen your saw in coding!**

Do you want to become a code master by focusing on practical code projects that actually earn you money and solve problems for people?

Then become a Python freelance developer! It’s the best way of approaching the task of improving your Python skills—even if you are a complete beginner.

Join my free webinar [“How to Build Your High-Income Skill Python”](https://blog.finxter.com/webinar-freelancer/) and watch how I grew my coding business online and how you can, too—from the comfort of your own home.

[Join the free webinar now!](https://blog.finxter.com/webinar-freelancer/)

