# Python Metaclasses \| Finxter

**Summary: Metaclasses are a class of a class**. In other words, **a class is an instance of a metaclass**. Metaclass can be created using two methods: \(1\) **type** Class which is the built-in metaclass in Python. \(2\) Creating **custom metaclass** using the `metaclass` keyword.

**Problem:** What are metaclasses and why do we use them in Python?

Before we dive into metaclasses, it is highly recommended that you go through the concepts of classes and objects. We have a [tutorial here](https://blog.finxter.com/a-simple-example-for-python-objects-and-classes-video/) and an entire course that helps you to understand the object-oriented programming concepts in python. You might want to have a look at the course at [this link.](https://blog.finxter.com/course-object-oriented-programming/) Having said that, let us have a quick revision of the python object-orientation.

Please swipe right to go through the concepts in our Instagram post:

>

Honestly speaking, metaclasses are an advanced concept in Python and can be quite complex to grasp. However, in this article I will try and make it simple for you.

## What is a **Metaclass**?

The term **meta** means something that is self-referential. You probably would have heard about metadata which means data about data. \(Please don’t worry if you haven’t!\). Therefore, Meta-X would typically mean the X about the X. I hope you get the point. That finally brings us the question: What is a “metaclass”?

As we know an object is an instance of a class, similarly a class is an instance of a metaclass. Therefore it is safe to say that metaclass is a class of class. Simply put, just like classes that determine the behavior of objects, _metaclasses_ determine the behavior of a class. Hence, metaclasses provide code for the creation and execution of classes. To simplify this more, let us have a look at the analogy given below :

_**Instances : Classes :: Classes : Instances**_

A real world example for you which should self explanatory :_fig:- The above figure shows that the coffee object is an instance of the coffee vending machine class which is an instance of the metaclass factory._

Before we proceed further with our discussion, here’s a rule of thumb in Python that I want to remind you:

✍ **Everything in Python is an Object.**

## The _**type**_ Metaclass

To get hold of the concept of metaclasses it is extremely important to understand the usage of _**type**_ in Python.

In simple terms, `type` is a metaclass that produces other classes.

Now there are 3 different ways in which you can use `type` in Python:

1. `type` can be used to find the **type of an** **Object.**
2. `type` can be used to find the **type of a** **Class.**
3. `type` can be used to **Create New Classes**.

Now let’s dive into each use case of the _type_ metaclass with help of examples.

### ❖ **Using** _**type**_ **to find the type of an Object**

As stated above in the thumb rule, everything in python is an object. That means, it does not matter whether you are using a string variable or a dictionary or a tuple, all of them are treated as objects in Python.

 Let us have a look at the following program that returns the type of a string variable and an object of a class. \(Please follow the comments in the code given below for a better grip on the concept.\)

```text
#creating a class
class Finxter(object):
  pass

#instatiating the class with an object
obj = Finxter()
# finding the type of the object
print(type(obj)) 

# creating a string variable
name = 'Harry'
# finding the type of the variable name
print(type(name))
```

**Output**:

### ❖ **Using** _**type**_ **To Find The Type Of** **Class**

Again going back to our basic thumb rule where we studied that everything in Python is a class. This means class is also an object in Python and must have a type like any other object. Let us find out what is the type of a class in python in the following program:

```text
#creating a class
class Finxter(object):
  pass

#instatiating the class with an object
obj = Finxter()
# finding the type of class Finxter
print(type(Finxter)) 
# finding type of class string 
print(type(str))
```

**Output:**

```text

```

The above output clearly demonstrates that `type` is the metaclass of all classes in Python.

_**TRIVIA**_**:** `type` is its own metaclass. To understand what this means, let us have a look at the following line of code given below:

```text
print(type(type))
```

**Output:**

```text

```

### ❖ **Using** _**type**_ **To Create New Class**

When the `type` class is called with only 1 argument it returns the type of object but when called using **3 parameters**, it creates a class.

The following arguments given below are passed to the `type` class:

1. Class Name.
2. Tuple containing base classes inherited by the class.
3. A Class Dictionary which serves as a local namespace and contains class methods and variables.

★ The syntax to create a class using the `type` class has been given below:

**Example 1:** A simple example which has no inherited class and an empty class dictionary.

```text
Finxter = type('Finxter', (), {})
obj = Finxter()
print(obj)
```

**Output:**

```text
<__main__.Finxter object at 0x7f8cf58025e0>
```

**Example 2:** Now let us have a look at a program which has the class name, a base class and an attribute inside the dictionary.

```text
def coffee(self): 
	print("Factory Class - Coffee Class Method!") 

# base class 
class machine: 
	def vendingMachine(self): 
		print("Base Class - Machine Class Method!") 

# creating factory class 
factory = type('factory', (machine, ), dict(greet="Welcome Finxter!", foo=coffee)) 
# Creating instance of factory class 
obj = factory() 
# calling inherited method 
obj.vendingMachine() 
# calling factory class method 
obj.foo()
# printing variable 
print(obj.greet) 
```

**Output:**

```text
Base Class - Machine Class Method!
Factory Class - Coffee Class Method!
Welcome Finxter!
```

**Example 3:** A complex example which has an external function that is assigned to the attribute of the namespace dictionary using the function name.

```text
# Defining external function
def foo(object):
  print("value = ",object.val)

# Creating class using type with attributes val and code that access the external function 
Finxter = type('Finxter', (), {'val':'PYTHON','code':foo})

# Using Object of Finxter class to access the attributes
obj = Finxter()
print(obj.val)
obj.code()
```

**Output:**

```text
PYTHON
value =  PYTHON
```

Now, let us have a look at the simplified version of creating the above `Finxter` class without using the `type()` function.

```text
def foo(object):
  print("value = ",object.val)
 
class Finxter:
   val = "PYTHON"
   code = foo

obj = Finxter()
print(obj.val)
obj.code()
```

★ `type` is the default data metaclass, however it contains special methods / “magic methods” that can be used create custom metaclasses. These methods are:

* `__new__()`
* `__init__()`
* `__prepare__()`
* `__call__()`

## Creating A **Custom Metaclass**

What happens when we create a class like the one given below?

```text
class Finxter:
  pass

obj = Finxter()
```

As soon as the object for the class `Finxter` is created the `__call__()` method of the type metaclass \(which happens to be the parent class of `Finxter`\) is invoked. The `__call__()` method then invokes the `__new__()` and `__init__()` methods. Since these methods are not defined within the class `Finxter` , these methods are automatically inherited. However, we can override these methods in a custom metaclass, thereby providing a customized behavior while instantiating the class `Finxter`.

**Example:** Let us consider that we have a testing framework, and we want to keep track of the order in which classes are defined. We can do this using a metaclass. Let’s have a look at how that can be done in the following code:

```text
class MyMeta(type):

    counter = 0

    def __init__(cls, name, bases, d):
        type.__init__(cls, name, bases, d)
        cls._order = MyMeta.counter
        MyMeta.counter += 1
        print(cls._order," ",name)

class MyClass(metaclass=MyMeta):    
    pass

class MyClass_A(MyClass):
    pass

class MyClass_B(MyClass):    
    pass
    
class MyClass_C(MyClass):
    pass
```

**Output:**

```text
0   MyClass
1   MyClass_A
2   MyClass_B
3   MyClass_C
```

All the subclass of `MyClass` gets a class attribute `_order` that records the order in which the classes were defined.

## Should We Use Metclasses?

The simple answer to this is, if the problem can be solved in a simpler method then you should not use metaclasses as they are quite complicated and difficult to grasp. Most of the class alterations can be done using other techniques, like using class decorators. The main use case of metaclass is creating an API, for e.g. Django ORM.

Python Guru Tim Peters \(author of the Zen Of Python\) said,

> “Metaclasses are deeper magic that 99% of users should never worry about it. If you wonder whether you need them, you don’t \(the people who actually need them to know with certainty that they need them and don’t need an explanation about why\).”

 Let us have a look at the following program which further justifies why we should avoid the use of metaclasses unless absolutely necessary. We will use the same scenario as we did in the previous case while creating a custom metaclass. However instead of using metaclass we will use decorators which are simpler than custom metaclass creation. Let’s have a look.

```text
counter = 0

def decorator(cls):
  class Main(cls):
    global counter
    cls._order = counter
    print(cls._order," ",cls.__name__)
    counter += 1
  return Main

@decorator
class MyClass():    
    pass

@decorator
class MyClass_A(MyClass):
    pass

@decorator
class MyClass_B(MyClass):    
    pass

@decorator    
class MyClass_C(MyClass):
    pass
```

**Output:**

```text
0   MyClass
1   MyClass_A
2   MyClass_B
3   MyClass_C
```

If you want to learn more about decorators, please have a look at our [blog tutorial here](https://blog.finxter.com/closures-and-decorators-in-python/).

## Conclusion

We learned the following in this tutorial:

* A quick recap of Object-Oriented Concepts in Python.
* What is a metaclass?
* The type metaclass.
  * Using type to find the type of an object.
  * Using type to find the type of class.
  * Using type to create a new class.
* Creating a custom metaclass.
* Why we should avoid using metaclasses?

