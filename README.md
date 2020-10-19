# Powerful Python One-Liners - Python Wiki

This page is devoted to short programs that can perform powerful operations called _Python One-Liners_.

## Free Python One-Liners Learning Resources <a id="Free_Python_One-Liners_Learning_Resources"></a>

* [Free ''Python One-Liners'' videos & book resources](https://pythononeliners.com/)
* [Collection of ''One-Liners'' with interactive shell](https://blog.finxter.com/10-python-one-liners/)
* [Book ''Python One-Liners''](https://www.amazon.com/gp/product/B07ZY7XMX8)
* [Interesting Quora Thread ''Python One-Liner''](https://www.quora.com/What-are-some-of-the-most-elegant-greatest-Python-one-liners)
* [Python One-Line X](https://blog.finxter.com/python-one-line-x/) - How to accomplish different tasks in a single line
* [Subreddit '''Python One-Liners'''](https://www.reddit.com/r/PythonOneLiners/)
* [Github '''Python One-Liners'''](https://github.com/finxter/PythonOneLiners/) - Share your own one-liners with the community

## Overview: 10 one-liners that fit into a tweet <a id="Overview:_10_one-liners_that_fit_into_a_tweet"></a>

I visited this page oftentimes and I loved studying the one-liners presented above. Thanks for creating this awesome resource, JAM, and RJW!

Because I learned a lot from studying the one-liners, I thought why not revive the page \(after almost ten years since the last change happened\)?

After putting a lot of effort into searching the web for inspiration, I created the following ten one-liners. Some of them are more algorithmic \(e.g. Quicksort\). Some day, I will add a detailed explanation here - but for now, you can [read this blog article](https://blog.finxter.com/10-python-one-liners/) to find explanations.

```text
   1 
   2 phrase.find(phrase[::-1])
   3 
   4 
   5 a, b = b, a
   6 
   7 
   8 sum(stock_prices[::2])
   9 
  10 
  11 [line.strip() for line in open(filename)]
  12 
  13 
  14 reduce(lambda x, y: x * y, range(1, n+1))
  15 
  16 
  17 python -m cProfile foo.py
  18 
  19 
  20 lambda l: reduce(lambda z, x: z + [y + [x] for y in z], l, [[]])
  21 
  22 
  23 lambda x: x if x<=1 else fib(x-1) + fib(x-2)
  24 
  25 
  26 lambda L: [] if L==[] else qsort([x for x in L[1:] if x< L[0]]) + L[0:1] + qsort([x for x in L[1:] if x>=L[0]])
  27 
  28 
  29 reduce( (lambda r,x: r-set(range(x**2,n,x)) if (x in r) else r), range(2,int(n**0.5)), set(range(2,n)))
```

## Find All Indices of an Element in a List <a id="Find_All_Indices_of_an_Element_in_a_List"></a>

Say, you want to do the same as the list.index\(element\) method but return all indices of the element in the list rather than only a single one.

In this one-liner, you’re looking for element 'Alice' in the list lst = \[1, 2, 3, 'Alice', 'Alice'\] so it even works if the element is not in the list \(unlike the list.index\(\) method\).

```text
   1 
   2 lst = [1, 2, 3, 'Alice', 'Alice']
   3 
   4 
   5 indices = [i for i in range(len(lst)) if lst[i]=='Alice']
   6 
   7 
   8 print(indices)
   9
```

## echo unicode character: <a id="echo_unicode_character:"></a>

```text
python -c "print unichr(234)"
```

This script echos "ê"

### Reimplementing cut <a id="Reimplementing_cut"></a>

Print every line from an input file but remove the first two fields.

```text
python -c "import sys;[sys.stdout.write(' '.join(line.split(' ')[2:])) for line in sys.stdin]" < input.txt
```

### Decode a base64 encoded file <a id="Decode_a_base64_encoded_file"></a>

```text
import base64, sys; base64.decode(open(sys.argv[1], "rb"), open(sys.argv[2], "wb"))
```

### Editing a list of files in place <a id="Editing_a_list_of_files_in_place"></a>

I came up with this one-liner in response to an [article](http://linuxgazette.net/issue96/orr.html) that said it couldn't be done as a one-liner in Python.

What this does is replace the substring "at" by "op" on all lines of all files \(in place\) under the path specified \(here, the current path\).

* _**Caution:**_ Don't run this on your home directory or you're going to get **all your text files edited**.

```text
import sys,os,re,fileinput;a=[i[2] for i in os.walk('.') if i[2]] [0];[sys.stdout.write(re.sub('at','op',j)) for j in fileinput.input(a,inplace=1)]
```

Clearer is: import os.path; a=\[f for f in os.listdir\('.'\) if not os.path.isdir\(f\)\]

### Set of all subsets <a id="Set_of_all_subsets"></a>

* Function that returns the set of all subsets of its argument

```text
f = lambda x: [[y for j, y in enumerate(set(x)) if (i >> j) & 1] for i in range(2**len(set(x)))]
```

```text
>>>f([10,9,1,10,9,1,1,1,10,9,7])
[[], [9], [10], [9, 10], [7], [9, 7], [10, 7], [9, 10, 7], [1], [9, 1], [10, 1], [9, 10, 1], [7, 1], [9, 7, 1], [10, 7, 1], [9, 10, 7, 1]]
```

-RJW

Alternative \(shorter, more functional version\):

```text
f = lambda l: reduce(lambda z, x: z + [y + [x] for y in z], l, [[]])
```

### Terabyte to Bytes <a id="Terabyte_to_Bytes"></a>

Want to know many bytes a terabyte is? If you know further abbreviations, you can extend the list.

```text
import pprint;pprint.pprint(zip(('Byte', 'KByte', 'MByte', 'GByte', 'TByte'), (1 << 10*i for i in range(5))))
```

### Largest 8-Bytes Number <a id="Largest_8-Bytes_Number"></a>

And what's the largest number that can be represented by 8 Bytes?

```text
print '\n'.join("%i Byte = %i Bit = largest number: %i" % (j, j*8, 256**j-1) for j in (1 << i for i in range(8)))
```

Cute, isn't it?

## Display List of all users on Unix-like systems <a id="Display_List_of_all_users_on_Unix-like_systems"></a>

```text
print '\n'.join(line.split(":",1)[0] for line in open("/etc/passwd"))
```

## CSV file to json <a id="CSV_file_to_json"></a>

```text
python -c "import csv,json;print json.dumps(list(csv.reader(open('csv_file.csv'))))"
```

## Compress CSS file <a id="Compress_CSS_file"></a>

```text
python -c 'import re,sys;print re.sub("\s*([{};,:])\s*", "\\1", re.sub("/\*.*?\*/", "", re.sub("\s+", " ", sys.stdin.read())))'
```

## Decode string written in Hex <a id="Decode_string_written_in_Hex"></a>

```text
python -c "print ''.join(chr(int(''.join(i), 16)) for i in zip(*[iter('474e552773204e6f7420556e6978')]*2))"
```

## Retrieve content text from HTTP data <a id="Retrieve_content_text_from_HTTP_data"></a>

```text
python -c "import sys; print sys.stdin.read().replace('\r','').split('\n\n',2)[1]";
```

## Prints file extension <a id="Prints_file_extension"></a>

```text
print '~/python/one-liners.py'.split('.')[-1]
```

## Escapes content from stdin <a id="Escapes_content_from_stdin"></a>

This can be used to convert a string into a "url safe" string

```text
python -c "import urllib, sys ; print urllib.quote_plus(sys.stdin.read())";
```

## Reverse lines in stdin <a id="Reverse_lines_in_stdin"></a>

```text
python -c "import sys; print '\n'.join(reversed(sys.stdin.read().split('\n')))"
```

## Print top 10 lines of stdin <a id="Print_top_10_lines_of_stdin"></a>

```text
python -c "import sys; sys.stdout.write(''.join(sys.stdin.readlines()[:10]))" < /path/to/your/file
```

## Apply regular expression to lines from stdin <a id="Apply_regular_expression_to_lines_from_stdin"></a>

```text
[another command] | python -c "import sys,re;[sys.stdout.write(re.sub('PATTERN', 'SUBSTITUTION', line)) for line in sys.stdin]"
```

## Modify lines from stdin using map <a id="Modify_lines_from_stdin_using_map"></a>

```text
python -c "import sys; tmp = lambda x: sys.stdout.write(x.split()[0]+'\t'+str(int(x.split()[1])+1)+'\n'); map(tmp, sys.stdin);"
```

### Cramming Python into Makefiles <a id="Cramming_Python_into_Makefiles"></a>

A related issue is embedding Python into a Makefile. I had a really long script that I was trying to cram into a makefile so I automated the process:

```text
   1 import sys,re
   2 
   3 def main():
   4     fh = open(sys.argv[1],'r')
   5     lines = fh.readlines()
   6     print '\tpython2.2 -c "`printf \\"if 1:\\n\\'
   7     for line in lines:
   8         line = re.sub('[\\\'\"()]','\\\g<0>',line)
   9         
  10         
  11         wh_spc_len = len(re.match('\s*',line).group())
  12 
  13         sys.stdout.write('\t')
  14         sys.stdout.write(wh_spc_len/4*'\\t'+line.rstrip().lstrip())
  15         sys.stdout.write('\\n\\\n')
  16     print '\t\\"`"'
  17 
  18 if __name__=='__main__':
  19     main()
```

This script generates a "one-liner" from make's point of view.

They call it "The Pyed Piper" or pyp. It's pretty similar to the -c way of executing python, but it imports common modules and has its own preset variable that help with splitting/joining, line counter, etc. You use pipes to pass information forward instead of nested parentheses, and then use your normal python string and list methods. Here is an example from the homepage:

Here, we take a Linux long listing, capture every other of the 5th through the 10th lines, keep the username and filename fields, replace "hello" with "goodbye", capitalize the first letter of every word, and then add the text "is splendid" to the end:

```text
ls -l | pyp "pp[5:11:2] | whitespace[2], w[-1] | p.replace('hello','goodbye') | p.title(),'is splendid'"
```

and the explanation:

This uses pyp's built-in string and list variables \(p and pp\), as well as the variable whitespace and its shortcut w, which both represent a list based on splitting each line on whitespace \(whitespace = w = p.split\(\)\). The other functions and selection techniques are all standard Python. Notice the pipes \("\|"\) are inside the pyp command.

[http://code.google.com/p/pyp/](http://code.google.com/p/pyp/) [http://opensource.imageworks.com/?p=pyp](http://opensource.imageworks.com/?p=pyp)

