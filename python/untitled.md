# The Mutable Default Argument In Python

**Summary:** Passing mutable objects as default arguments leads to unexpected outputs because Python initializes the default mutable object only once, not \(as you may have expected\) each time the function is called. To fix this, initialize the mutable default argument with the **`None`** keyword in the argument list and then initialize it within the function. This causes the mutable default argument to be initialized freshly in each function execution.

## Overview

Very interesting research on the most commonly asked questions on the internet prompted me to have a go at this article. Python is normally regarded as a very consistent and easy to grasp programming language. However, if you are a newbie then you might come across a few scenarios which can confuse you. This might be surprising initially, but as you dig deep into the code, it becomes absolutely sensible and justifiable for you. So that’s the intention of this article where we discuss the reason behind such unexpected behaviors in Python and the right approach to assess such situations.

Before we discuss anything else let us get rid of the all important universal truth of Python :

This essentially means that unlike some of the other programming languages, functions in Python are regarded as first-class objects and not just a block of code. Please keep this in mind, as this is the base of our discussion in this article. Now let us have a look at one scenario where you might be confused with a certain code and its output.

## The Confusion 🤔

Python novices often come across a common confusion while using a default **mutable** data type as an argument in a function. Given below is a small example of the confusion/problem statement that you might face in your early days with Python.

**Example:** Consider the given snippet given below.

```text
def confused(a, e=[]):
    e.append(a)
    return e

# Using function the first time
print(confused(10))
# Using function the second time
print(confused(20))
```

**Output \(Expectation Vs Reality**\):

You can try it yourself in the interactive Python shell:

So there’s a striking difference between the expected output and the output that we actually get. Now, that brings us to some of the most important concepts that we need to know, to understand why this happens.

Following concepts need to be kept in mind while dealing with functions and mutable data types in Python:

1. Mutable Objects Vs Immutable Objects.
2. [Pass By Object Reference in Python.](https://blog.finxter.com/how-to-pass-a-variable-by-reference-in-python/#Fundamentals_Of_Call_By_Object_Reference)

We already have an article which discusses the concept of pass by object reference and I highly recommend you have a look at by following [this link.](https://blog.finxter.com/how-to-pass-a-variable-by-reference-in-python/#Fundamentals_Of_Call_By_Object_Reference)

Let us discuss the difference between a **mutable object** and an **immutable object** in Python.

## Mutable Vs Immutable [Objects](https://blog.finxter.com/course-object-oriented-programming/)

Since everything is treated as an object in python, every variable has a corresponding object instance. Therefore, whenever a variable of a certain type is created, it is assigned a unique object id. The type of the variable \(which is an object in Python\) is defined in the runtime and cannot be changed; however, **the state of the variable can be changed if it is mutable**. But if the variable is an **immutable object, we cannot change its state.**

The table given lists the mutable and immutable objects available in Python.

 Now, this makes our life easy and the reason we get an unexpected output becomes self-explanatory! Here’s the _reason why the variation happened in the output:-_

## The Reason

When the function is defined, a new list is created. Thereafter, every time you call the same function, the same list is used because the list is a mutable object and if you try to modify/mutate a mutable object in a specific function call then the function will return the mutated list in each successive call. To simplify it further, I have created a dry-run of the above program which shows the exact mechanism behind the function call. Please have a look at it below:The table clearly depicts, with every successive function call an element gets added to the pre-existing list, i.e., a new list is not created in every function call, rather the same list it reused every time since list is a mutable object.

You can check the state of a default argument using the [`__defaults__`](https://docs.python.org/3/reference/datamodel.html) tuple as shown in the program below.

```text
def confused(a, e=[]):
    e.append(a)
    print("State of e[] = {0} for function call no. {1}".format(confused.__defaults__,len(e)))
    return (e)

# Using function the first time
print("Output Function_Call 1: ",confused(10))
# Using function the second time
print("Output Function_Call 2: ",confused(20))
```

**Output:**

```text
State of e[] = ([10],) for function call no. 1
Output Function_Call 1:  [10]
State of e[] = ([10, 20],) for function call no. 2
Output Function_Call 2:  [10, 20]
```

## The Solution

Thankfully, the solution is quite simple. We can use `None` in place of the mutable default argument/object and then assign a value to the mutable object within the local scope of the function. So, now you can check the values for `None` instead of directly assigning them to the mutable object which is a list in our case.

Let us have a look at the following program to understand how we can resolve our issue:

```text
def confused(a, e=None):
    if e is None:
      e = []
    e.append(a)
    return e

# Using function the first time
print(confused(10))
# Using function the second time
print(confused(20))
```

**Output:**

```text
[10]
[20]
```

❖ **`None`** is a [keyword](https://blog.finxter.com/python-cheat-sheet/) in Python that denotes a null value. You can consider `None` same as 0, False or an Empty string. The type of `None` is `None` itself.

## Confusion With Closures And Late Binding

[Lambda functions](https://blog.finxter.com/a-simple-introduction-of-the-lambda-function-in-python/) can lead to similar confusion when you are dealing with closures. A closure is something that occurs when a function tries to access a variable outside its scope. Given below is an example of a closure:

```text
def func(msg):
    def foo():
        print(msg)
    foo()
func("Finxter")
```

In the above code, it is evident the function `foo()` depends on the variable `msg` outside its scope. Hence, this is an example of a closure.

Things become a little complex and confusing when it comes to the late binding of closures. The [python-guide](https://docs.python-guide.org/writing/gotchas/#late-binding-closures) states that:

> _**Python’s closures are late binding. This means that the values of variables used in closures are looked up at the time the inner function is called.**_

Here’s an example:

```text
def table():
    return [lambda x : i*x for i in range(1,6)]

print([a(2) for a in table()])
```

**Desired Output Vs Expected Output:**

### **The Reason:**

The variance in the output is because the lambda function does not receive the value of `i` until the `for loop` has finished execution. Thus, when the value of `i` is passed on to the lambda function, it is 4 every time. Hence, the result is `[2*5, 2*5, 2*5, 2*5, 2*5]`.

### **The Solution:**

The solution is to bind the closure to the arguments immediately by creating a default argument as shown below:

```text
def multiply():
    return [lambda x,arg=i : arg*x for i in range(1,6)]

print([a(2) for a in multiply()])
```

```text
[2, 4, 6, 8, 10]
```

## Conclusion

Key takeaways from this article:

1. The difference between mutable and immutable objects.
2. The confusion with mutable default arguments in Python.
3. Using `none` to resolve unwanted outputs while dealing with mutable arguments.
4. The confusion with closures in lambdas.
5. Binding the closure to a default argument to resolve our issue.

I hope you found this article helpful and it helped you get a better view of functions and mutable arguments. Please [subscribe](https://blog.finxter.com/subscribe/) and [stay tuned](http://blog.finxter.com/) for interesting articles.

