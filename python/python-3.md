# Python Double Asterisk \(\*\*\) \| Finxter

**Summary:** The double asterisk operator has the following uses:

* a\*\*b – **Exponentiation**.
* def f\(\*\*kwargs\) – **Unpacking:** Defining an arbitrary number of keyword arguments.
* f\(\*\*d\) – [**Dictionary**](https://blog.finxter.com/category/python-dictionary/) **Unpacking**.
* f\(\*\*d1,\*\*d2\) – [**Merging Dictionaries**](https://blog.finxter.com/how-to-merge-two-python-dictionaries-in-a-single-expression-in-python/).

While using a function in your program, you might be uncertain about the number of named arguments that have to be passed into the function. The purpose of this article is to guide and help you to deal with such situations. Let us dive into the discussion on double asterisk operator in Python right away.

## Using Double Asterisk \(\*\*\) In Python

The \*\* can have different meanings in different scenarios. This brings us to the question, **when to** **use the \*\* operator in Python?**

There are different answers to this question. Let us discuss them one by one.

### ❖ **Used As Exponentiation Operator** **\*\***

While using numeric data types to perform mathematical operations, double asterisk \(\*\*\) is used as an **exponentiation** operator.

 **Example:**

```text
a = 5
b = 2
print("a**b = ",a**b)
print("pow(a,b) = ",pow(a,b))
```

**Output:**

```text
a**b =  25
pow(a,b) =  25
```

The above example uses double asterisks to calculate **“a to the power b”** in numerical expressions in Python.

### ❖ **Used To Accept Arbitrary Keyword Arguments \*\*kwargs**

If you are uncertain about how many keyword arguments have to be passed to a function in the program, then you can use an argument with the double asterisks as a prefix to the argument which allows us to pass _an arbitrary number of keyword arguments_ into our function. This allows the function to receive a dictionary of arguments and it can then access the items accordingly.

**Example:**

```text
def foo(**kwargs):
  print("Learn freelancing with {0} using {1}!".format(kwargs["org"],kwargs["tech"]))

foo(org = "Finxter", tech = "Python")
```

**Output:**

```text
Learn freelancing with Finxter using Python!
```

➡ **Important** **Notes**

◆ **Types of Arguments**

* **Positional Arguments** – The arguments that can be called by their position in the function definition. When the function is, called the first positional argument must be provided first, the second positional argument must be provided second, and so on.
* **Keyword Arguments** – The arguments that can be called using their name. It is generally followed by an equal sign and an expression to provide it a default value.

Let us have a closer look at these arguments in a program given below.

```text
 # a,b required; c optional
def foo(a, b, c = 1): 
    return a * b + c

print ("positional and default: ",foo(1, 2))   # positional and default
print ("positional: ",foo(1, 2, 3)) # positional
print ("keyword: ",foo(c = 5, b = 2, a = 2)) # keyword
print ("keyword and default: ",foo(b = 2, a = 2)) # keyword and default.
print ("positional and keyword: ",foo(5, c = 2, b = 1)) # positional and keyword.
print ("positional, named and default: ",foo(8, b = 0)) #positional, named and default.
```

**Output:**

```text
positional and default:  3
positional:  5
keyword:  9
keyword and default:  5
positional and keyword:  7
positional, named and default:  1
```

◆ You can use any other name in place of `kwargs` with the **unpacking operator** \(\*\*\) as a prefix to the argument. For example, you can use `**var` instead of using `**kwargs` in your program. Thus, `kwargs` is just a convention and is often defined as a shortened form of _Arbitrary Keyword Arguments_ in Python documentations.

**Example:**

```text
def foo(**var):
    print(var)

foo(a=1,b=2,c=3)
```

**Output**:

```text
{'a': 1, 'b': 2, 'c': 3}
```

◆ The subtle difference between **\*args** and **\*\*kwargs**

* In function definitions a single asterisk `(*`\) takes an iterator like a list or tuple and expands it into a sequence of arguments whereas double asterisk \(\*\*\) takes a dictionary only and expands it.
* \*args is used to allow an arbitrary number of non-keyword arguments while \*\*kwargs allows an arbitrary number of keyword arguments.
* Just like non-default arguments should be placed before default arguments, similarly \*\*kwargs should always be placed after \*args. Otherwise, Python throws an error. The correct order of arguments is:
  1. Standard arguments
  2. \*args arguments
  3. \*\*kwrgs arguments

**Example**

```text
def foo(a, b,*args,**kwargs):
   print(a+b)
   print(args)
   print(kwargs)

d = {'name':"FINXTER",'tech':"Python"}
l = [4,5,6,7]
foo(2,3,*l,**d)
```

**Output**:

```text
5
(4, 5, 6, 7)
{'name': 'FINXTER', 'tech': 'Python'}
```

You can read more about the single asterisk operator in our [**blog tutorial here**](https://blog.finxter.com/what-is-asterisk-in-python/).

### ❖ **Used For Dictionary Unpacking**

What does **UNPACKING** mean?

It is a feature in Python which allows us to assign/pack all values/arguments of a sequence into a single variable.

**Example**

```text
def fruits(*args):
    for key in args:
        print(key)


f = ["Apple", "Mango", "Orange"]
fruits(*f)
```

**Output**:

```text
Apple
Mango
Orange
```

In the above example, we unpacked an arbitrary number of elements into a single variable using the \* operator. Now if you unpack a dictionary using a single asterisk operator, you only get the unpacked form of the dictionary’s keys. To unpack the key as well as the values together, we use the double-asterisk operator! The example given below explains this.

**Example:**

```text
def Result(**kwargs):
    for key in kwargs:
        if kwargs[key] > 90:
            print(str(key) + " " + str(kwargs[key]))


marks = {"Ben" : 96,
        "Emma" : 98,
        "Ron" : 89}

Result(**marks)
```

**Output:**

```text
Ben 96
Emma 98
```

#### ❒ **Merging Dictionaries Using Dictionary Unpacking**

The double asterisk operator can be used to merge two dictionaries in Python. The **dictionary unpacking feature** `z = {**dict1, **dict2}` creates a new dictionary and unpacks all \(key-value\) pairs into the new dictionary. Duplicate keys are automatically resolved by this method.

**Example:**

```text
d1 = {'value1': 10, 'value': 20}
d2 = {'value': 30, 'value2': 40}
# Merging d1 and d2
z = {**d1, **d2} 
print("MERGED DICTIONARY: ", z)
```

**Output**

```text
MERGED DICTIONARY:  {'value1': 10, 'value': 30, 'value2': 40}
```

## Conclusion

The key take-aways from this article were –

* Using double asterisk as an exponentiation operator in Python.
* Using a double asterisk to accept an arbitrary number of keyword arguments.
* What are the keyword and positional arguments?
* What is dictionary unpacking?
* Merging Dictionaries using the asterisk operator.

Now that brings us to the end of this article and I hope this article helped you to master the concept of double asterisk **\*\*** in Python. Please [subscribe](https://blog.finxter.com/subscribe/) and [stay tuned](https://blog.finxter.com/) for more interesting articles!

