# How To Check If A Variable Is An Integer Or Not?

**Summary:** Use the **isinstance\(var, int\)** method to check if a variable is an Integer or not. The method checks if a specified object is of a specified type or not. Another method to check if the variable is an integer or not is the **is\_integer\(\)** function which checks if a given float variable holds an integer value or a decimal value.

**Problem**: Given a variable in a program; how to check if it is an integer or not?

**Example:**

```text
def checInt(val):
  #Some function to check if val is an Integer

  #if YES:
  return True

  #else
  return False

a = 25
b = 'FINXTER' 
print(checInt(a))
print(checInt(b))
```

**Output:**

```text
True
False
```

Thus, in the above example we need to define a procedure such that when we check if `a` is an integer then it returns `True` while for `b` it should return `False`.

🐍 Before proceeding further, let me give you a **teaser of the solutions** that are lined up next. Please have a look at the code given below.

```text
def checInt(val):
  # Method 1
  if (isinstance(val,(int,float)) == True):
    print ("Method 1 Successful!")
  # Method 2
  if (val.is_integer):
    print ("Method 2 Successful!")
  # Method 3
  if (val == round(val)):
    print ("Method 3 Successful!")
  # Method 4
  if (val % 1 == 0):
    print ("Method 4 Successful!")

a = 25.0
checInt(a)
```

**Output:**

```text
Method 1 Successful!
Method 2 Successful!
Method 3 Successful!
Method 4 Successful!
```

Now, without further delay let us discuss the various methods to get our desired output.

## Method 1: Using **isinstance\(var, int\)**

◉ `isinstance` is a built-in method in Python which returns `True` when the specified object is an instance of the specified type, otherwise it returns `False`.

**Syntax:**

**Example:**

```text
# Example 1
class Finxter:
  name = "Python"

obj = Finxter()

test = isinstance(obj, Finxter)

print(test)

# Example 2
x = isinstance("Hello Finxter!", (str,float,int))
print(x)
```

**Output:**

```text
True
True
```

Now that we know about the `isinstance` method, let us check how we can use it to check if the variable is [integer](https://blog.finxter.com/how-to-make-an-integer-larger-than-any-other-integer-in-python/) or not. Please follow the program given below:

```text
class Finxter(int): pass
x = Finxter(0)

print(isinstance(x, int))
```

**Output:**

```text
True
```

### Why We Should Not use _**“type”**_?

The usage of type to solve our problem should be avoided because if you subclass `int` as in the above example, **`type`** won’t register the value of the variable as int. Let us have a look at the following piece of code to understand, why using `type` is not a good option.

```text
class Test(int): pass
x = Test(0)
print(type(x) == int) # False
print(type(x),"is not same as int")
```

**Output**:

```text
False
 is not same as int
```

✍ A probable workaround for the above issue could be the usage of `subclass()` which is a built-in function in Python that checks if a given class/object is a subclass of another specified class. Let us have a look at this in a program given below:

```text
class Test(int): pass
x = Test(0)
print(issubclass(type(x), int))
```

**Output:**

```text
True
```

## Method 2: Using **var.is\_integer\(\)**

An integer represents only an integer value \(no decimal\), while float represents numbers that can be integers like 25.0 as well as decimal numbers like 25.75. We can thus call the `is_integer()` method to check if the float represents an integer.

Let us have a look at the following to get a clear picture.

```text
a = 25.0
b = 25.75
print(a.is_integer())
print(b.is_integer())
```

**Output:**

```text
True
False
```

## Method 3: Using **round\(\)**

A simple approach to check if the given variable is an integer or not can be to check if it is equal to the value when it is rounded. For this purpose, we can use the in-built [`round()`](https://blog.finxter.com/python-math-module/#Python_Math_Round) method in Python which returns the nearest integer when no values are passed on to the optional `digit` argument.

**Syntax:**

```text
round(number, digits)
```

* `number` represents the number that has to be rounded.
* `digits` represent the number of decimals to be used while rounding the number.

Let’s have a look at the following program to check if a variable is an integer or not:

```text
def is_int(value):
  if value == round(value):
    print ('True')
  else:
    print ('False')


a = 25.0
b = 25.75
is_int(a)
is_int(b)
```

**Output:**

```text
True
False
```

## Method 4: A Quick Hack

Here’s a quick hack to check if the given variable is an integer or not. All we need is a simple modulo operator as shown below.

```text
def is_int(value):
  if value%1 == 0 :
    print ('True')
  else:
    print ('False')

a = 25.0
b = 25.75
is_int(a)
is_int(b)
```

**Output:**

```text
True
False
```

_**Caution:**_ This just a quick workaround and should not be considered as a first choice while checking if a variable is an integer or not.

## Method 5: Using **try** And **except** Block

Another approach to our problem is to use the `try` and `except` block. Instead of directly checking if the value is an integer or not, we consider it to be an integer initially and catch the exception if it isn’t. If this sounds confusing, please have a look at the program below which will make things easy for you.

```text
def is_int(value):
  try:
    return int(value)==value
  except ValueError:
    return False

a = 25.0
b = 25.75
c = "FINXTER"

print(is_int(a))
print(is_int(b))
print(is_int(c))
```

**Output:**

```text
True
False
False
```

## Method 6: Using **str.isdigit\(\)** Method

If you want to check if a variable has an integer string value or not, then the `str.`[`isdigit()`](https://blog.finxter.com/how-to-extract-numbers-from-a-string-in-python/#Method_3_Using_isdigit%28%29_Function_In_A_List_Comprehension) method can be very useful. As the name suggests, `str.isdigit()` returns `True` if all the characters present in the string are digits. Otherwise, it returns `False`.

☠ **Alert:**

* It only works if the str is string. Integers do not have an `isdigit` method.
* `isdigit()` returns `False` for negative integers

Now, let us have a look at a working example of the `isdigit()` method to check if a variable is an integer string or not.

```text
def is_int(value):
  return str.isdigit(value)

a = "25"
b = "FINXTER"

print(is_int(a))
print(is_int(b))
```

**Output:**

```text
True
False
```

## Conclusion

The key points that we learned in this article were:

* Using the `isinstance()` method.
* Why `type` should be avoided in solving our problem.
* Using the `issubclass()` method.
* Using the `is_integer()` method.
* Using the round\(\) function.
* Using `try` and `except` blocks to solve our problem.
* Using the `isdigit()` method.

