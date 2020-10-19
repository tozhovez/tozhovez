# Python Re \* – The Asterisk Quantifier for Regular Expressions

Every computer scientist knows the asterisk quantifier of regular expressions. But many non-techies know it, too. Each time you search for a text file `*.txt` on your computer, you use the asterisk operator. But how does it work?

This article is all about the **asterisk `*` quantifier in Python’s** [**re library**](https://docs.python.org/3/library/re.html)**.** Study it carefully and master this important piece of knowledge once and for all!

Alternatively, you can also watch the video where I lead you through the whole article:

**Related article:** [Python Regex Superpower – The Ultimate Guide](https://blog.finxter.com/python-regex/)

_**Do you want to master the regex superpower?**_ Check out my new book [_**The Smartest Way to Learn Regular Expressions in Python**_](https://blog.finxter.com/ebook-the-smartest-way-to-learn-python-regex/) with the innovative 3-step approach for active learning: \(1\) study a book chapter, \(2\) solve a code puzzle, and \(3\) watch an educational chapter video.

## What’s the Python Re \* Quantifier?

When applied to regular expression **`A`**, Python’s **`A*`** quantifier matches zero or more occurrences of **`A`**. The star `*` symbol is called _asterisk_ or _wildcard operator_ and it applies to the preceding regular expression. For example, the regular expression **`'yes*'`** matches strings **`'ye'`**, **`'yes'`**, and **`'yesssssss'`** but not the empty string **`''`**.

Let’s study two basic examples to help you gain a deeper understanding. Do you get all of them?

```text
>>> import re
>>> text = 'finxter for fast and fun python learning'
>>> re.findall('f.* ', text)
['finxter for fast and fun python ']
>>> re.findall('f.*? ', text)
['finxter ', 'for ', 'fast ', 'fun ']
>>> re.findall('f[a-z]*', text)
['finxter', 'for', 'fast', 'fun']
>>> 
```

Don’t worry if you had problems understanding those examples. You’ll learn about them next. Here’s the first example:

### Greedy Asterisk Example

```text
>>> re.findall('f.* ', text)
['finxter for fast and fun python ']
```

You use the `re.findall()` method. In case you don’t know it, here’s the definition from the [Finxter blog article](https://blog.finxter.com/python-re-findall/):

**The `re.findall(pattern, string)` method finds all occurrences of the `pattern` in the `string` and returns a list of all matching substrings.**

[Please consult the blog article to learn everything you need to know about this fundamental Python method.](https://blog.finxter.com/python-re-findall/)

The first argument is the regular expression pattern **`'f.* '`**. The second argument is the string to be searched for the pattern. In plain English, you want to find all patterns in the string that start with the character `'f'`, followed by an arbitrary number of optional characters, followed by an empty space.

The `findall()` method returns only one matching substring: **`'finxter for fast and fun python '`**. The asterisk quantifier `*` is greedy. This means that it tries to match as many occurrences of the preceding regex as possible. So in our case, it wants to match as many arbitrary characters as possible so that the pattern is still matched. Therefore, the regex engine “consumes” the whole sentence.

### Non-Greedy Asterisk Example

But what if you want to find all words starting with an `'f'`? In other words: how to match the text with a non-greedy asterisk operator?

The second example is the following:

```text
>>> re.findall('f.*? ', text)
['finxter ', 'for ', 'fast ', 'fun ']
```

In this example, you’re looking at a similar pattern with only one difference: you use the non-greedy asterisk operator **`*?`**. You want to find all occurrences of character `'f'` followed by an arbitrary number of characters \(but as few as possible\), followed by an empty space.

Therefore, the regex engine finds four matches: the strings `'finxter '`, `'for '`, `'fast '`, and `'fun '`.

### Asterisk + Character Class Example

The third example is the following:

```text
>>> re.findall('f[a-z]*', text)
['finxter', 'for', 'fast', 'fun']
```

This regex achieves almost the same thing: finding all words starting with `f`. But you use the asterisk quantifier in combination with a character class that defines specifically which characters are valid matches.

Within the character class, you can define character ranges. For example, the character range `[a-z]` matches one lowercase character in the alphabet while the character range `[A-Z]` matches one uppercase character in the alphabet.

But note that the empty space is not part of the character class, so it won’t be matched if it appears in the text. Thus, the result is the same list of words that start with character `'f'`: `'finxter '`, `'for '`, `'fast '`, and `'fun '`.

## What If You Want to Match the Asterisk Character Itself?

You know that the asterisk quantifier matches an arbitrary number of the preceding regular expression. But what if you search for the asterisk \(or star\) character itself? How can you search for it in a string?

The answer is simple: escape the asterisk character in your regular expression using the backslash. In particular, use `'\*'` instead of `'*'`. Here’s an example:

```text
>>> import re
>>> text = 'Python is ***great***'
>>> re.findall('\*', text)
['*', '*', '*', '*', '*', '*']
>>> re.findall('\**', text)
['', '', '', '', '', '', '', '', '', '', '***', '', '', '', '', '', '***', '']
>>> re.findall('\*+', text)
['***', '***']
```

You find all occurrences of the star symbol in the text by using the regex `'\*'`. Consequently, if you use the regex `'\**'`, you search for an arbitrary number of occurrences of the asterisk symbol \(including zero occurrences\). And if you would like to search for all maximal number of occurrences of subsequent asterisk symbols in a text, you’d use the regex `'\*+'`.

## \[Collection\] What Are The Different Python Re Quantifiers?

The asterisk quantifier—Python re `*`—is only one of many regex operators. If you want to use \(and understand\) regular expressions in practice, you’ll need to know all of them by heart!

So let’s dive into the other operators:

A regular expression is a decades-old concept in computer science. Invented in the 1950s by famous mathematician Stephen Cole Kleene, the decades of evolution brought a huge variety of operations. Collecting all operations and writing up a comprehensive list would result in a very thick and unreadable book by itself.

Fortunately, you don’t have to learn all regular expressions before you can start using them in your practical code projects. Next, you’ll get a quick and dirty overview of the most important regex operations and how to use them in Python. In follow-up chapters, you’ll then study them in detail — with many practical applications and code puzzles.

Here are the most important regex quantifiers:

| **Quantifier** | **Description** | **Example** |
| :--- | :--- | :--- |
| `.` | The **wild-card** \(‘dot’\) matches any character in a string except the newline character `'\n'`. | Regex `'...'` matches all words with three characters such as `'abc'`, `'cat'`, and `'dog'`. |
| `*` | The **zero-or-more** asterisk matches an arbitrary number of occurrences \(including zero occurrences\) of the immediately preceding regex. | Regex `'cat*'` matches the strings `'ca'`, `'cat'`, `'catt'`, `'cattt'`, and `'catttttttt'`. — |
| `?` | The **zero-or-one** matches \(as the name suggests\) either zero or one occurrences of the immediately preceding regex. | Regex ‘cat?’ matches both strings `'ca'` and `'cat'` — but not `'catt'`, `'cattt'`, and `'catttttttt'`. |
| `+` | The **at-least-one** matches one or more occurrences of the immediately preceding regex. | Regex `'cat+'` does not match the string `'ca'` but matches all strings with at least one trailing character `'t'` such as `'cat'`, `'catt'`, and `'cattt'`. |
| `^` | The **start-of-string** matches the beginning of a string. | Regex `'^p'` matches the strings `'python'` and `'programming'` but not `'lisp'` and `'spying'` where the character `'p'` does not occur at the start of the string. |
| `$` | The **end-of-string** matches the end of a string. | Regex `'py$'` would match the strings `'`main.py`'` and `'`pypy`'` but not the strings `'python'` and `'pypi'`. |
| `A|B` | The **OR** matches either the regex A or the regex B. Note that the intuition is quite different from the standard interpretation of the or operator that can also satisfy both conditions. | Regex `'`\(hello\)\|\(hi\)`'` matches strings `'hello world'` and `'hi python'`. It wouldn’t make sense to try to match both of them at the same time. |
| `AB` |  The **AND** matches first the regex A and second the regex B, in this sequence. | We’ve already seen it trivially in the regex `'ca'` that matches first regex `'c'` and second regex `'a'`. |

Note that I gave the above operators some more meaningful names \(in bold\) so that you can immediately grasp the purpose of each regex. For example, the `^` operator is usually denoted as the ‘caret’ operator. Those names are not descriptive so I came up with more kindergarten-like words such as the “start-of-string” operator.

We’ve already seen many examples but let’s dive into even more!

```text
import re

text = '''
    Ha! let me see her: out, alas! he's cold:
    Her blood is settled, and her joints are stiff;
    Life and these lips have long been separated:
    Death lies on her like an untimely frost
    Upon the sweetest flower of all the field.
'''

print(re.findall('.a!', text))
'''
Finds all occurrences of an arbitrary character that is
followed by the character sequence 'a!'.
['Ha!']
'''

print(re.findall('is.*and', text))
'''
Finds all occurrences of the word 'is',
followed by an arbitrary number of characters
and the word 'and'.
['is settled, and']
'''

print(re.findall('her:?', text))
'''
Finds all occurrences of the word 'her',
followed by zero or one occurrences of the colon ':'.
['her:', 'her', 'her']
'''

print(re.findall('her:+', text))
'''
Finds all occurrences of the word 'her',
followed by one or more occurrences of the colon ':'.
['her:']
'''


print(re.findall('^Ha.*', text))
'''
Finds all occurrences where the string starts with
the character sequence 'Ha', followed by an arbitrary
number of characters except for the new-line character. 
Can you figure out why Python doesn't find any?
[]
'''

print(re.findall('n$', text))
'''
Finds all occurrences where the new-line character 'n'
occurs at the end of the string.
['n']
'''

print(re.findall('(Life|Death)', text))
'''
Finds all occurrences of either the word 'Life' or the
word 'Death'.
['Life', 'Death']
'''
```

In these examples, you’ve already seen the special symbol `'\n'` which denotes the new-line character in Python \(and most other languages\). There are many special characters, specifically designed for regular expressions. Next, we’ll discover the most important special symbols.

## What’s the Difference Between Python Re \* and ? Quantifiers?

You can read the Python Re `A?` quantifier as **zero-or-one regex**: the preceding regex `A` is matched either zero times or exactly once. But it’s not matched more often.

Analogously, you can read the Python Re `A*` operator as the **zero-or-more regex** \(I know it sounds a bit clunky\): the preceding regex `A` is matched an arbitrary number of times.

Here’s an example that shows the difference:

```text
>>> import re
>>> re.findall('ab?', 'abbbbbbb')
['ab']
>>> re.findall('ab*', 'abbbbbbb')
['abbbbbbb']
```

The regex `'ab?'` matches the character `'a'` in the string, followed by character `'b'` if it exists \(which it does in the code\).

The regex `'ab*'` matches the character `'a'` in the string, followed by as many characters `'b'` as possible.

## What’s the Difference Between Python Re \* and + Quantifiers?

You can read the Python Re `A*` quantifier as **zero-or-more regex**: the preceding regex `A` is matched an arbitrary number of times.

Analogously, you can read the Python Re `A+` operator as the **at-least-once regex**: the preceding regex `A` is matched an arbitrary number of times too—but at least once.

Here’s an example that shows the difference:

```text
>>> import re
>>> re.findall('ab*', 'aaaaaaaa')
['a', 'a', 'a', 'a', 'a', 'a', 'a', 'a']
>>> re.findall('ab+', 'aaaaaaaa')
[]
```

The regex `'ab*'` matches the character `'a'` in the string, followed by an arbitary number of occurrences of character `'b'`. The substring `'a'` perfectly matches this formulation. Therefore, you find that the regex matches eight times in the string.

The regex `'ab+'` matches the character `'a'`, followed by as many characters `'b'` as possible—but at least one. However, the character `'b'` does not exist so there’s no match.

## What are Python Re `*?`, `+?`, `??` Quantifiers?

You’ve learned about the three quantifiers:

* The quantifier `A*` matches an arbitrary number of patterns `A`.
* The quantifier `A+` matches at least one pattern `A`.
* The quantifier `A?` matches zero-or-one pattern `A`.

Those three are all **greedy**: they match as many occurrences of the pattern as possible. Here’s an example that shows their greediness:

```text
>>> import re
>>> re.findall('a*', 'aaaaaaa')
['aaaaaaa', '']
>>> re.findall('a+', 'aaaaaaa')
['aaaaaaa']
>>> re.findall('a?', 'aaaaaaa')
['a', 'a', 'a', 'a', 'a', 'a', 'a', '']
```

The code shows that all three quantifiers `*`, `+`, and `?` match as many `'a'` characters as possible.

So, the logical question is: how to match as few as possible? We call this **non-greedy** matching. You can append the question mark after the respective quantifiers to tell the regex engine that you intend to match as few patterns as possible: `*?`, `+?`, and `??`.

Here’s the same example but with the non-greedy quantifiers:

```text
>>> import re
>>> re.findall('a*?', 'aaaaaaa')
['', 'a', '', 'a', '', 'a', '', 'a', '', 'a', '', 'a', '', 'a', '']
>>> re.findall('a+?', 'aaaaaaa')
['a', 'a', 'a', 'a', 'a', 'a', 'a']
>>> re.findall('a??', 'aaaaaaa')
['', 'a', '', 'a', '', 'a', '', 'a', '', 'a', '', 'a', '', 'a', '']
```

In this case, the code shows that all three quantifiers `*?`, `+?`, and `??` match as few `'a'` characters as possible.

## Related Re Methods

There are five important regular expression methods which you should master:

* The **`re.findall(pattern, string)`** method returns a list of string matches. Read more in [our blog tutorial](https://blog.finxter.com/python-re-findall/).
* The **`re.search(pattern, string)`** method returns a match object of the first match. Read more in [our blog tutorial](https://blog.finxter.com/python-regex-search/).
* The **`re.match(pattern, string)`** method returns a match object if the regex matches at the beginning of the string. Read more in [our blog tutorial](https://blog.finxter.com/python-regex-match/).
* The **`re.fullmatch(pattern, string)`** method returns a match object if the regex matches the whole string. Read more in [our blog tutorial](https://blog.finxter.com/python-regex-fullmatch/).
* The **`re.compile(pattern)`** method prepares the regular expression pattern—and returns a regex object which you can use multiple times in your code. Read more in [our blog tutorial](https://blog.finxter.com/python-regex-compile/).
* The **`re.split(pattern, string)`** method returns a list of strings by matching all occurrences of the pattern in the string and dividing the string along those. Read more in [our blog tutorial](https://blog.finxter.com/python-regex-split/).
* The **`re.sub(pattern, repl, string, count=0, flags=0)`** method returns a new string where all occurrences of the pattern in the old string are replaced by `repl`. Read more in [our blog tutorial](https://blog.finxter.com/python-regex-sub/).

These seven methods are 80% of what you need to know to get started with Python’s regular expression functionality.

## Where to Go From Here?

You’ve learned everything you need to know about the asterisk quantifier `*` in this regex tutorial.

_**Summary**: When applied to regular expression **A**, Python’s **A\*** quantifier matches zero or more occurrences of **A**. The \* symbol is called asterisk operator_ and it applies to the preceding regular expression. For example, the regular expression **‘yes\*’** matches strings **‘ye’**, **‘yes’**, and **‘yesssssss’** but not the empty string **”**.

**Want to earn money while you learn Python?** Average Python programmers earn more than $50 per hour. You can certainly become average, can’t you?

Join the free webinar that shows you how to become a thriving coding business owner online!

[\[Webinar\] Become a Six-Figure Freelance Developer with Python](https://blog.finxter.com/webinar-freelancer/)

Join us. It’s fun! 🙂

