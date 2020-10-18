# How To Sort A Dictionary By Value in Python?

**Summary:** Use one of the following methods to sort a dictionary by value:

1. Using The **sorted\(dict1, key=dict1.get\)** Method.
2. Using **Dictionary Comprehension** And **Lambda** With **sorted\(\)** Method.
3. Using **OrderedDict** \(For Older Versions Of Python\).
4. Using **itemgetter\(\)** with **sorted\(\)** Method.
5. Using **Counter** subclass.

**Problem:** Given a [dictionary](https://blog.finxter.com/python-dictionary/); how to sort it by values?

**Example:** The following example shows a dictionary by the name `rank` that stores the names of individuals as the keys while their corresponding ranks represent the values. We shall be using this example as a reference while discussing the solutions.

```text
rank = {
  'Bob': 2,
  'Alice': 4,
  'Sharon': 5,
  'Dwyane': 1,
  'John': 3
}

# Some Procedure to Sort the Dictionary by its Values
```

**Output:**

```text
{'Dwyane': 1, 'Bob': 2, 'John': 3, 'Alice': 4, 'Sharon': 5}
```

Before we dive into the solutions here are a few **Points To Remember** about dictionaries:

* From Python 3.7 onwards, Python dictionaries are ordered \(Insertion is also ordered\). This means the ordered in which the keys are inserted into dictionaries are preserved.
* In normal scenario sorting dictionaries, results in a sorted dictionary with respect to its keys.

To get a better insight into Python dictionaries, please follow our Blog Tutorial on dictionaries [**here**](https://blog.finxter.com/python-dictionary/). However, the purpose of this article is purely to guide you through the numerous methods to sort a dictionary based on the values instead of the keys. So without further delay let us dive into the solutions.

➤ Here’s a quick overview of all the methods used in this article. Please follow the slide show give below:

## Method 1: Using The **sorted\(dict1, key=dict1.get\)** Method

The `sorted()` method is an in-built method in Python which is used to sort the elements of an iterable in a specific order \(**ascending** or **descending**\). After sorting the elements it returns the sorted sequence, in the form of a sorted list.

**Syntax:**

In order to sort the dictionary using the values, we can leverage the power of the `sorted()` function. To sort the dictionary based on the value, we can use the `get()` method and pass it to the `key` argument of the `sorted()` function.

Let us have a look at the following code to understand the usage of `sorted` function in order to solve our problem:

```text
rank = {
  'Bob': 2,
  'Alice': 4,
  'Sharon': 5,
  'Dwyane': 1,
  'John': 3
}

for w in sorted(rank, key=rank.get):
    print(w, rank[w])
```

**Output:**

```text
Dwyane 1
Bob 2
John 3
Alice 4
Sharon 5
```

In order to sort the dictionary using its values in reverse order, we need to specify the `reverse` argument as `true`, i.e.,

```text
rank = {
  'Bob': 2,
  'Alice': 4,
  'Sharon': 5,
  'Dwyane': 1,
  'John': 3
}

for w in sorted(rank, key=rank.get, reverse=True):
    print(w, rank[w])
```

**Output:**

```text
Sharon 5
Alice 4
John 3
Bob 2
Dwyane 1
```

## Method 2: Using **Dictionary Comprehension** And **Lambda** With **sorted\(\)** Method

If you are using [Python 3.6 and above](https://blog.finxter.com/how-to-check-your-python-version/) then our problem can be solved in a single line using a dictionary comprehension and a lambda function within the `sorted` method. This is an efficient and concise solution to sort dictionaries based on their values.

⦿ [**Dictionary Comprehension**](https://blog.finxter.com/python-dictionary-comprehension/) is a concise and memory-efficient way to **create and initialize dictionaries** in one line of Python code. It consists of two parts: expression and context. The **expression** defines how to map keys to values. The **context** loops over an iterable using a single-line for loop and defines which \(key, value\) pairs to include in the new dictionary. To learn more about dictionary comprehensions in python have a look at our [blog tutorial here](https://blog.finxter.com/python-dictionary-comprehension/).

Now, let us have a look at the following code given below that explains the usage of dictionary comprehensions to solve our problem in a single lie of code.

```text
rank = {
  'Bob': 2,
  'Alice': 4,
  'Sharon': 5,
  'Dwyane': 1,
  'John': 3
}

print({k: v for k, v in sorted(rank.items(), key=lambda item: item[1])})
```

**Output:**

```text
{'Dwyane': 1, 'Bob': 2, 'John': 3, 'Alice': 4, 'Sharon': 5}
```

## Method 3: Using **OrderedDict** \(For Older Versions Of Python\)

Dictionaries are generally unordered for versions prior to Python 3.7, so it is not possible to sort a dictionary directly. Therefore, to overcome this constraint we need to use the `OrderedDict` subclass.

⦿ An [**`OrderedDict`**](https://blog.finxter.com/python-dictionary/#OrderedDict) is a dictionary subclass that preserves the order in which key-values are inserted in a dictionary. It is included in the `collections` module in Python.

Let us have a look at how we can use [**`OrderedDict`**](https://blog.finxter.com/python-dictionary/#OrderedDict) to order dictionaries in earlier versions of Python and sort them.

```text
from collections import OrderedDict
rank = {
  'Bob': 2,
  'Alice': 4,
  'Sharon': 5,
  'Dwyane': 1,
  'John': 3
}
a = OrderedDict(sorted(rank.items(), key=lambda x: x[1]))
for key,value in a.items():
  print(key, value)
```

**Output:**

```text
('Dwyane', 1)
('Bob', 2)
('John', 3)
('Alice', 4)
('Sharon', 5)
```

## Method 4: Using **itemgetter\(\)** With The **sorted\(\) Method**

⦿ `itemgetter()` is a built-in function of the `operator` module that constructs a callable that accepts an iterable like a list, tuple, set, etc as input and fetches the nth element out of it.

**Example:**

```text
from operator import itemgetter

rank = {
  'Bob': 2,
  'Alice': 4,
  'Sharon': 5,
  'Dwyane': 1,
  'John': 3
}

a = sorted(rank.items(), key=itemgetter(1))
print(dict(a))
```

**Output:**

```text
{'Dwyane': 1, 'Bob': 2, 'John': 3, 'Alice': 4, 'Sharon': 5}
```

In the above example `a` stores a list of tuples. Therefore we converted it to a `dictionary` explicitly, while printing.

## Method 5: Using **Counter**

[_**Counter**_](https://docs.python.org/3/library/collections.html#collections.Counter) is a dictionary subclass that is used for counting hashable objects. Since the values we are using are integers, we can use the `Counter` class to sort them. The `Counter` class has to be imported from the collections module.

**Disclaimer:** This is a work-around for the problem at hand and might not fit in every situation. Since, values of the dictionary in our case are integers, the **Counter** subclass fits as a solution to our problem. Consider it as a bonus trick!😉

```text
from collections import Counter

rank = {
  'Bob': 2,
  'Alice': 4,
  'Sharon': 5,
  'Dwyane': 1,
  'John': 3
}

count = dict(Counter(rank).most_common())
print(count)
```

**Output:**

```text
{'Dwyane': 1, 'Bob': 2, 'John': 3, 'Alice': 4, 'Sharon': 5}
```

⦿ In the above program the method `most_common()` has been used to return all elements in the Counter.

## Conclusion

In this we learned about the following methods to sort a dictionary by values in Python:

* Using The **sorted\(dict1, key=dict1.get\)** Method.
* Using **Dictionary Comprehension** And **Lambda** With **sorted\(\)** Method.
* Using **OrderedDict** \(For Older Versions Of Python\).
* Using **itemgetter\(\)** with **sorted\(\)** Method.
* Using **Counter** subclass.

I hope after reading this article you can sort dictionaries by their values with ease. Please subscribe and stay tuned for more interesting articles in the future.

## Where to Go From Here?

Enough theory, let’s get some practice!

To become successful in coding, you need to get out there and solve real problems for real people. That’s how you can become a six-figure earner easily. And that’s how you polish the skills you really need in practice. After all, what’s the use of learning theory that nobody ever needs?

**Practice projects is how you sharpen your saw in coding!**

Do you want to become a code master by focusing on practical code projects that actually earn you money and solve problems for people?

Then become a Python freelance developer! It’s the best way of approaching the task of improving your Python skills—even if you are a complete beginner.

Join my free webinar [“How to Build Your High-Income Skill Python”](https://blog.finxter.com/webinar-freelancer/) and watch how I grew my coding business online and how you can, too—from the comfort of your own home.

[Join the free webinar now!](https://blog.finxter.com/webinar-freelancer/)

