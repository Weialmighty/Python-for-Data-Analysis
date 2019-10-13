# **CHAPTER1 Preliminaries**
## **1.1 What Is This Book About?**
### **What Kinds of Data?**
*structured data*
## **1.2 Why Python for Data Analysis?**
### **Sloving the "Two-Language" Problem**
### **Why Not Python?**
## **1.3 Essential Python Libraries**
### **Numpy**
*short for Numerical Python, has long been a cornerstone of numerical computing in Python.* It provides the data structures, algorithms, and library glue needed for most scientific applications involving numerical data in Python. NumPy contains, among other thing.
### **pandas**
*pandas provides high-level data structures and functions designed to make working with structured or tabular data fast, easy, and expressive.* Since its emergence in 2010, it has helped enable Python to be a powerful and productive data analysis environment. The primary objects in pandas that will be used in this book are the DataFrame, a tabular, column-oriented data structure with both row and column labels, and the Series, a one-dimensional labeled array object.
### **matplotlib**
*matplotlib is the most popular Python library for producing plots and other two-dimensional data visualizations.* It was originally created by John D. Hunter and is now maintained by a large team of developers. It is designed for creating plots suitable for publication. While there are other visualization libraries available to Python programmers, matplotlib is the most widely used and as such has generally good integration with the rest of the ecosystem. I think it is a safe choice as a default visualization
tool.
### **SciPy**
*SciPy is a collection of packages addressing a number of different standard problem domains in scientific computing.*
### **scikit-learn**
*Since the project’s inception in 2010, scikit-learn has become the premier generalpurpose machine learning toolkit for Python programmers.* In just seven years, it has had over 1,500 contributors from around the world. It includes submodules for such models as:
- Classification: SVM, nearest neighbors, random forest, logistic regression, etc.
- Regression: Lasso, ridge regression（岭回归）, etc.
- Clustering: k-means, spectral clustering, etc.
- Dimensionality reduction: PCA(Principal Component Analysis), feature selection, matrix factorization（分解）, etc.
- Model selection: Grid search, cross-validation, metrics（度量）
- Preprocessing: Feature extraction, normalization
### **statsmodels**
*statsmodels is a statistical analysis package that was seeded by work from Stanford University statistics professor Jonathan Taylor, who implemented a number of regression analysis models popular in the R programming language.* This includes such submodules as:
- Regression models: Linear regression, generalized linear models, robust linear models, linear mixed effects models, etc.
- Analysis of variance (ANOVA)
- Time series analysis: AR, ARMA, ARIMA, VAR, and other models
- Nonparametric methods: Kernel density estimation, kernel regression
- Visualization of statistical model results
statsmodels is more focused on statistical inference, providing uncertainty estima
## **1.4 Import Conventions**
```python
import numpy as np
import matplotlib.pyplot as plt
import pandas as pd
import seaborn as sn
import statsmodel as sm
```
## **1.5 Jargon（行话、术语）**
- *Munge/munging/wrangling*  
Describes the overall process of manipulating unstructured and/or messy data into a structured or clean form. The word has snuck its way into the jargon of many modern-day data hackers. “Munge” rhymes with “grunge.”
- *Psedocode*
- *Syntactic sugar（语法糖）*  
Programming syntax that does not add new features, but makes something more convenient or easier to type.  
  
# **CHAPTER2 Pyhton Language Basics**
## **2.1 The Python Interpreter**
```python
In[1]: 
import numpy as np
data = {i : np.random.randn() for i in range(7)}
data
Out[1]: 
{0: 0.26816922423129474,
 1: -0.751080185270001,
 2: -0.3923485657678745,
 3: 1.202910059919528,
 4: 0.9835632973769233,
 5: 1.7345376326386743,
 6: -0.9553103769648562}
```
`randn()` Return a sample (or samples) from the ["standard normal" distribution](https://en.wikipedia.org/wiki/Normal_distribution).
```python
In[2]: 
from numpy.random import randn
data = {i : randn() for i in range(7)}
data
Out[2]: 
{0: -0.0753720445222836,
 1: -0.6130818596072809,
 2: -1.3243342248501844,
 3: -0.9746658075656515,
 4: 0.7917487114501727,
 5: -0.5751811367566934,
 6: -1.0961269397945121}
```
### Introspection（自省）
Using a question mark `?` before or after a variable will display some general information about the object:
```python
In[3]: 
b = [1, 2, 3]
b?
Out[3]: 
Type:        list
String form: [1, 2, 3]
Length:      3
Docstring:  
Built-in mutable sequence.

If no argument is given, the constructor creates a new empty list.
The argument must be an iterable if specified.
```
```python
In[4]: 
print?
Out[4]: 
Docstring:
print(value, ..., sep=' ', end='\n', file=sys.stdout, flush=False)

Prints the values to a stream, or to sys.stdout by default.
Optional keyword arguments:
file:  a file-like object (stream); defaults to the current sys.stdout.
sep:   string inserted between values, default a space.
end:   string appended after the last value, default a newline.
flush: whether to forcibly flush the stream.
Type:      builtin_function_or_method
```
Then using `?` shows us the docstring:
```python
In[5]: 
def add_numbers(a, b):
    """
    Add Two numbers together
    
    Returns
    
    the sum : type of arguments
    """
    return a + b
add_numbers?
Out[5]: 
Signature: add_numbers(a, b)
Docstring:
Add Two numbers together

Returns

the sum : type of arguments
File:      ~/PycharmProjects/Data Science/<ipython-input-6-fb29343e0f5a>
Type:      function
```
Using `??` will also show the function's source code if possible
```python
In[6]: 
def add_numbers(a, b):
    """
    Add Two numbers together
    
    Returns
    
    the sum : type of arguments
    """
    return a + b
add_numbers??
Out[6]: 
Signature: add_numbers(a, b)
Source:   
def add_numbers(a, b):
    """
    Add Two numbers together
    
    Returns
    
    the sum : type of arguments
    """
    return a + b
File:      ~/PycharmProjects/Data Science/<ipython-input-6-fb29343e0f5a>
Type:      function
```
`?` has a final usage: A number of characters combined with the wildcard（通配符）(*) will show all names matching the wildcard expression. For example, we could get a list of all functions in the top-level NumPy namespace containing load:
```python
In[7]: 
np.*load*?
Out[7]: 
np.__loader__
np.load
np.loads
np.loadtxt
np.pkgload
```
### The %run Command
Suppose you had the following simple script stored in ipython_script_test.py:  
You can execute this by passing the filename to %run
In the Jupyter notebook, you may also use the related %load magic function, which imports a script into a code cell:
**Interrupting running code**  
Pressing Ctrl-C while any code is running, whether a script through %run or a longrunning command, will cause a KeyboardInterrupt to be raised. This will cause nearly all Python programs to stop immediately except in certain unusual cases.
### Excuting Code from Clipboard
If you are using the Jupyter notebook, you can copy and paste code into any code cell and execute it. It is also possible to run code from the clipboard in the IPython shell. Suppose you had the following code in some other application:
```
x = 5
y = 7
if x > 5:
    x += 1
    y = 8
```
The most foolproof methods are the %paste and %cpaste magic functions. %paste takes whatever text is in the clipboard and executes it as a single block in the shell:
%cpaste is similar, except that it gives you a special prompt for pasting code into:  
With the %cpaste block, you have the freedom to paste as much code as you like before executing it. You might decide to use %cpaste in order to look at the pasted code before executing it. If you accidentally paste the wrong code, you can break out of the %cpaste prompt by pressing Ctrl-C.
### About Magic Commands
*Some frequently used Ipython magic commands*  

Command | Description
------------ | -------------
%magic | Display detailed documentation for all of the available magic comma
%debug | Enter the interactive debugger at the bottom of the last exception traceback
%hist | Print command input (and optionally output) history
%pdb | Automatically enter debugger after any exception
%paste | Execute preformatted Python code from clipboard
%cpaste | Open a special prompt for manually pasting Python code to be executed
%reset | Delete all variables/names defined in interactive namespace
%page OBJECT | Pretty-print the object and display it through a pager
%run script.py | Run a Python script inside IPython
%prun statement | Execute statement with cProfile and report the profiler output
%time statement | Report the execution time of a single statement
%timeit statement | Run a statement multiple times to compute an ensemble average execution time; useful for timing code with very short execution time
%who, %who_ls, %whos | Display variables defined in interactive namespace, with varying levels of information/verbosity
%xdel variable | Delete a variable and attempt to clear any references to the object in the IPython internals
### Matplotlib Integration
In the IPython shell, running %matplotlib sets up the integration so you can create multiple plot windows without interfering with the console session, In Jupyter, the command is a little different:
```python
In[8]: 
%matplotlib inline
import numpy as np
import matplotlib.pyplot as plt
plt.plot(np.random.randn(50).cumsum())
Out[8]: 
[<matplotlib.lines.Line2D at 0x11a63b6a0>]
```
![](https://github.com/Weialmighty/Python-for-Data-Analysis/blob/master/images/image8.png)
## 2.3 Python Language Basis
*Binary operators*  

Operation | Description
------------ | -------------
a // b  | Floor-divide（整除） a by b, dropping any fractional remainder
a ** b | Raise a to the b power
a & b | True if both a and b are True; for integers, take the bitwise（按位、逐位） AND
a \| b | True if either a or b is True; for integers, take the bitwise OR
a ^ b| For booleans, True if a or b is True, but not both; for integers, take the bitwise EXCLUSIVE-OR
### Control Flow
#### if, elif, and else
#### for loops
#### while loops
A `while` loop specifies a condition and a block of code that is to be excuted until the condition evaluates to `False` or the loop is explicitly ended with break:
```python
In[9]: 
x = 256
total = 0
while x > 0:
    if total > 500:
        break
    total += x
    x = x // 2
total
Out[9]: 
504
```
#### pass
pass is the "no-op" statement in Python. It can be used in blocks where no action is to be taken (or as a placeholder for code not yet implemented); it is only required because Python uses whitespace to delimit（限定） blocks:
#### range
#### Ternary expression（三元表达式或三重表达式）
A *ternary expression* in Python allows you to combine an `if-else` block that produces a value into a single line or expression. The syntax for this in Python is:
```python
value = true-expr if condition else false-expr
```
While it may be tempting to always use ternary expressions to condense（压缩，浓缩） your code,realize that you may sacrifice readability if the condition as well as the true and false expressions are very complex.  
  
# CHARPTER3 Bulit-in Data Structures, Functions, and Files
## Data Structure and Sequences
### Tuple
A tuple is a fixed-length, immutable sequence of Python objects. The easiest way to create one is with a comma-separated sequence of values.
You can convert any sequence or iterator to a tuple by invoking tuple:
```python
In[10]: 
tuple([4, 0, 2])
Out[10]: 
(4, 0, 2)
In[11]: 
tup = tuple('string')
tup
Out[11]: 
('s', 't', 'r', 'i', 'n', 'g')
```
Elements can be accessed with square brackets `[]` as with most other sequence types. As in  C, C++, Java, and many other languages, sequences are 0-indexed in Python.
While the objects stored in a tuple may be mutable themselves, once the tuple is created it’s not possible to modify which object is stored in each slot.
If an object inside a tuple is mutable, such as a list, you can modify it in-place:
```python
In[12]: 
tup = tuple(['foo', [1, 2], True])
tup[1].append(3)
tup
Out[12]: 
('foo', [1, 2, 3], True)
```
You can concatenate tuples using the `+` operator to produce longer tuples:(**Notice ('bar',)**)
```python
In[13]: 
(4, None, 'foo') + (6, 0) + ('bar',)
Out[13]: 
(4, None, 'foo', 6, 0, 'bar')
```
Multiplying a tuple by an integer, as with lists, has the effect of concatenating（串联） together that many copies of the tuple:
```python
In[14]: 
('foo', 'bar') * 4
Out[14]: 
('foo', 'bar', 'foo', 'bar', 'foo', 'bar', 'foo', 'bar')
```
**Note that the objects themselves are not copied, only the references to them.**
#### Unpacking tuples
If you try to assign to a tuple-like expression of variables, Python will attempt to unpack the value on the righthand side of the equals sign:
```python
In[15]: 
tup = (4, 5, 6)
a, b, c = tup
b
Out[15]: 
5
```
Even sequences with nested tuples can be unpacked:
```python
In[16]: 
tup = 4, 5, (6, 7)
a, b, (c, d) = tup
d
Out[16]: 
7
```
In Python, the swap variable names can be done like this:
```python
In[17]: 
a, b = 1, 2
b, a = a, b
a
b
Out[17]: 
2
1
```
A common use of variable unpacking is iterating over（遍历） sequences of tuples or lists:
```python
In[18]: 
seq = [(1, 2, 3), (4, 5, 6), (7, 8, 9)]
for a, b, c in seq: 
    print('a={0}, b={1}, c={2}'.format(a, b, c))
Out[18]: 
a=1, b=2, c=3
a=4, b=5, c=6
a=7, b=8, c=9
```
The Python language recently acquired some more advanced tuple unpacking to help with situations where you may want to “pluck”（采摘） a few elements from the beginning of a tuple. This uses the special syntax `*rest`, which is also used in function signatures to capture an arbitrarily（任意地） long list of positional arguments:
```python
In[19]: 
values = 1, 2, 3, 4,5
a, b, *rest = values
a, b
rest
Out[19]: 
(1, 2)
[3, 4, 5]
```
This `rest` bit is sometimes something you want to discard; there is nothing special about the `rest` name. As a matter of convention, many Python programmers will use the underscore `_` for unwanted variables:
```python
In[20]: 
values = 1, 2, 3, 4,5
a, b, *_ = values
a, b
_
Out[20]: 
(1, 2)
[3, 4, 5]
```
##### Tuple methods
Since the size and contents of a tuple cannot be modified, it is very light on instance methods. A particularly useful one (also available on lists) is count, which counts the number of occurrences of a value:
```python
In[21]: 
a = (1, 2, 2, 2, 3, 4, 2)
a.count(2)
Out[21]: 
4
```
### List
In contrast with tuples, lists are variable-length and their contents can be modified in-place. You can define them using square brackets [] or using the list type function.
The `list` function is frequently used in data processing as a way to materialize（实现） an iterator（迭代） or generator expression:
```python
In[21]: 
gen = range(10)
gen
Out[21]: 
range(0, 10)
In[22]: 
list(gem)
Out[22]: 
[0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
```
#### Adding and removing elements
`append(element)`, `insert(index, element)`
`insert` is computationally expensive compared with `append`, because references to subsequent elements have to be shifted internally to make room for the new element. If you need to insert elements at both the beginning and end of a sequence, you may wish to explore `collections.deque`, a double-ended queue, for this purpose.
The inverse operation to `insert` is `pop`, which removes and returns an element at a particular index.
Elements can be removed by value with `remove`, which locates the first such value and removes it from the last.
#### Concatenating and combining lists
If you have a list already defined, you can append multiple elements to it using the extend method:
```python
In[23]: 
x = [4, None, 'foo']
x.extend([7, 8, (2, 3)])
x
Out[23]: 
[4, None, 'foo', 7, 8, (2, 3)]
```  
**Note that list concatenation by addition is a comparatively expensive operation since a new list must be created and the objects copied over. Using extend to append elements to an existing list, especially if you are building up a large list, is usually preferable.**
#### Sorting
You can sort a list in-place (without creating a new object) by calling its `sort` function:
```python
In[24]: 
a = [7, 2, 5, 1, 3]
a.sort()
a
Out[24]: 
[1, 2, 3, 5, 7]
``` 
sort has a few options that will occasionally come in handy（便利）. One is the ability to pass a secondary sort `key`—that is, a function that produces a value to use to sort the objects. For example, we could sort a collection of strings by their lengths:
```python
In[25]: 
b = ['saw', 'small', 'He', 'foxes', 'six']
b.sort(key=len)
b
Out[25]: 
['He', 'saw', 'six', 'small', 'foxes']
``` 
#### Binary search and maintaining a sorted list
The built-in `bisect` module implements binary search and insertion into a sorted list. `bisect.bisect` finds the location where an element should be inserted to keep it sorted, while `bisect.insort` actually inserts the element into that location:
```python
In[26]: 
import bisect
c = [1, 2, 2, 2, 3, 4, 7]
bisect.bisect(c, 2)
Out[26]: 
4
In[27]: 
bisect.insort(c, 6)
c
Out[27]: 
[1, 2, 2, 2, 3, 4, 6, 7]
``` 
**The `bisect` module functions do not check whether the list is sorted, as doing so would be computationally expensive. Thus, using them with an unsorted list will succeed without error but may lead to incorrect results.**
#### Slicing
Slices can also be assigned to with a sequence:
```python
In[28]: 
seq = [7, 2, 3, 7, 5, 6, 0, 1]
seq[3:4] = [6, 3]
seq
Out[28]: 
[7, 2, 3, 6, 3, 5, 6, 0, 1]
``` 
A `step` can also be used after a second colon to, say, take every other element. A clever use of this is to pass -1, which has the useful effect of reversing a list or tuple:
```python
In[29]: 
seq[::-1]
Out[29]: 
[1, 0, 6, 5, 3, 6, 3, 2, 7]
``` 
### Build-in Sequence Functions
#### enumerate
It’s common when iterating over a sequence to want to keep track of the index of the
current item. A do-it-yourself approach would look like:
```python
i = 0
for value in collection:
    # do something with value
    i += 1
``` 
Since this is so common, Python has a built-in function, `enumerat`, which returns a
sequence of `(i, value)` tuples:
```python
for i, value in enumerate(collection):
    # do something with value
``` 
When you are indexing data, a helpful pattern that uses `enumerate` is computing a
`dict` mapping the values of a sequence (which are assumed to be unique) to their
locations in the sequence:
```python
In[30]: 
some_list = ['foo', 'bar', 'baz']
mapping = {}
for i, v in enumerate(some_list):
    mapping[v] = i
mapping
Out[30]: 
{'foo': 0, 'bar': 1, 'baz': 2}
``` 
#### sorted
The `sorted` function returns a new sorted list from the elements of any sequence:
The `sorted` function accepts the same arguments as the sort method on lists.
```python
In[31]: 
sorted([7, 1, 2, 6, 0, 3, 2])
Out[31]: 
[0, 1, 2, 2, 3, 6, 7]
In[32]: 
sorted('horse race')
Out[32]: 
[' ', 'a', 'c', 'e', 'e', 'h', 'o', 'r', 'r', 's']
``` 
#### zip
`zip` “pairs” up the elements of a number of lists, tuples, or other sequences to create a list of tuples:
```python
In [33]: 
seq1 = ['foo', 'bar', 'baz']
seq2 = ['one', 'two', 'three']
zipped = zip(seq1, seq2)
list(zipped)
Out[33]: 
[('foo', 'one'), ('bar', 'two'), ('baz', 'three')]
``` 
`zip` can take an arbitrary number of sequences, and the number of elements it produces is determined by the shortest sequence:
```python
In [34]: 
seq3 = [False, True]
list(zip(seq1, seq2, seq3))
Out[34]: 
[('foo', 'one', False), ('bar', 'two', True)]
``` 
A very common use of `zip` is simultaneously iterating over multiple sequences, possibly also combined with `enumerate`:
```python
In [35]: 
for i, (a, b) in enumerate(zip(seq1, seq2)):
    print('{0}: {1}, {2}'.format(i, a, b))
Out[35]: 
0: foo, one
1: bar, two
2: baz, three
``` 
Given a “zipped” sequence, `zip` can be applied in a clever way to “unzip” the sequence. Another way to think about this is converting a list of rows into a list of columns. The syntax, which looks a bit magical, is:
```python
In [36]: 
pitchers = [('Nolan', 'Ryan'), ('Roger', 'Clemens'),('Schilling', 'Curt')]
first_names, last_names = zip(*pitchers)
first_names
last_names
Out[36]: 
('Nolan', 'Roger', 'Schilling')
('Ryan', 'Clemens', 'Curt')
``` 
#### reversed
`reversed` iterates over the elements of a sequence in reverse order:
```python
In [37]: 
list(reversed(range(10)))
Out[37]: 
[9, 8, 7, 6, 5, 4, 3, 2, 1, 0]
``` 
Keep in mind that `reversed` is a generator (to be discussed in some more detail later), so it does not create the reversed sequence until materialized (e.g., with `list` or a `for` loop).
### dict
You can access, insert, or set elements using the same syntax as for accessing elements of a list or tuple. You can delete values either using the `del` keyword or the `pop` method (which simultaneously returns the value and deletes the key):
```python
In [38]: 
d1 = {'a' : 'some value', 'b' : [1, 2, 3, 4]}
d1[5] = 'some value'
d1['dummy'] = 'another value'
ret = d1.pop('dummy')
ret
Out[38]: 
'another value'
``` 
The `keys` and `values` method give you iterators of the dict’s keys and values, respectively. While the key-value pairs are not in any particular order, these functions output the keys and values in the same order:
```python
In [39]: 
list(d1.keys())
Out[39]: 
['a', 'b', 5]
In [40]: 
list(d1.values())
Out[40]: 
['some value', [1, 2, 3, 4], 'some value']
``` 
You can merge one dict into another using the `update` method:
```python
In [41]: 
d1.update({'b' : 'foo', 'c' : 12})
Out[41]: 
{'a': 'some value', 'b': 'foo', 5: 'some value', 'c': 12}
``` 
The `update` method changes dicts in-place, so any existing keys in the data passed to
update will have their old values discarded.
#### Creating dicts from sequences
Since a dict is essentially a collection of 2-tuples, the `dict` function accepts a list of 2-tuples:
```python
In [42]: 
m = zip(range(5), reversed(range(5)))
list(m)
Out[42]: 
[(0, 4), (1, 3), (2, 2), (3, 1), (4, 0)]
``` 
```python
In [43]: 
mapping = dict(zip(range(5), reversed(range(5))))
mapping
Out[43]: 
{0: 4, 1: 3, 2: 2, 3: 1, 4: 0}
``` 
#### Default values
`get` by default will return `None` if the key is not present, while `pop` will raise an exception. With setting values, a common case is for the values in a dict to be other collections, like lists. For example, you could imagine categorizing a list of words by their first letters as a dict of lists:
```python
In [44]: 
words = ['apple', 'bat', 'bar', 'atom', 'book']
by_letter = {}
for word in words:
    letter = word[0]
    if letter not in by_letter:
        by_letter[letter] = [word]
    else:
        by_letter[letter].append(word)
by_letter
Out[44]: 
{'a': ['apple', 'atom'], 'b': ['bat', 'bar', 'book']}
``` 
The `setdefault` dict method is for precisely this purpose. The preceding `for` loop
can be rewritten as:
```python
for word in words:
    letter = word[0]
    by_letter.setdefault(letter, []).append(word)
``` 
The built-in collections module has a useful class, defaultdict, which makes this even easier. To create one, you pass a type or function for generating the default value for each slot in the dict:
```python
from collections import defaultdict
by_letter = defaultdict(list)
for word in words:
    by_letter[word[0]].append(word)
``` 
#### Valid dict key types
While the values of a dict can be any Python object, the keys generally have to be immutable objects like scalar types (int, float, string 标量) or tuples (all the objects in the tuple need to be immutable, too). The technical term here is hashability 散列性. You can check whether an object is hashable (can be used as a key in a dict() with the `hash` function:
```python
In [45]: 
hash('string')
Out[45]: 
-4749600703255240290
In [46]: 
hash((1, 2, (2, 3)))
Out[46]: 
1097636502276347782
In [47]: 
hash((1, 2, [2, 3]))
Out[47]: 
TypeError                                 Traceback (most recent call last)
<ipython-input-29-8ffc25aff872> in <module>
----> 1 hash((1, 2, [2, 3]))

TypeError: unhashable type: 'list'
``` 
To use a list as a key, one option is to convert it to a tuple, which can be hashed as long as its elements also can:
```python
In [48]: 
d = {}
d[tuple([1, 2, 3])] = 5
d
Out[48]: 
{(1, 2, 3): 5}
``` 
### set
A set is an unordered collection of unique elements. You can think of them like dicts, but keys only, no values. A set can be created in two ways: via the set function or via a set literal with curly braces花括号:
```python
In [49]: 
set([2, 2, 2, 1, 3, 3])
Out[49]: 
{1, 2, 3}
In [50]: 
{2, 2, 2, 1, 3, 3}
Out[50]: 
{1, 2, 3}
``` 
Sets support mathematical set operations like union, intersection, difference, and
symmetric difference. The union of these two sets is the set of distinct elements occurring in either set. This can be computed with either the `union` method or the | binary operator:
```python
In [51]: 
a = {1, 2, 3, 4, 5}
b = {3, 4, 5, 6, 7, 8}
a.union(b)
a | b
Out[51]: 
{1, 2, 3, 4, 5, 6, 7, 8}
``` 
The intersection contains the elements occurring in both sets. The & operator or the intersection method can be used:
```python
In [52]: 
a = {1, 2, 3, 4, 5}
b = {3, 4, 5, 6, 7, 8}
a.intersection(b)
a & b
Out[52]: 
{3, 4, 5}
``` 
*Commonly Used Set Methods*  

Function | Alternative syntax | Description
------------ | ------------- | -------------
a.union(b) | a \| b | All of the unique elements in a and b
a.update(b) | a |= b | Set the contents of a to be the union of the elements in a and b
a.intersection(b) | a & b | All of the elements in both a and b
a.intersection_update(b) | a &= b | Set the contents of a to be the intersection of the elements in a and b
a.difference(b) | a - b | The elements in a that are not in b
a.difference_update(b) | a -= b | Set a to the elements in a that are not in b
a.symmetric_difference(b) | a ^ b | All of the elements in either a or b but not both（交集的补集）
a.symmetric_difference_update(b) | a ^= b | Set a to contain the elements in either a or b but not both
a.issubset(b) | N/A | True if the elements of a are all contained in b
a.issuperset(b) | N/A | True if the elements of b are all contained in a
a.isdisjoint(b) | N/A | True if a and b have no elements in common  
All of the logical set operations have in-place counterparts, which enable you to replace the contents of the set on the left side of the operation with the result. For very large sets, this may be more efficient.
Sets are equal if and only if their contents are equal:
```python
In [53]: 
{1, 2, 3} == {3, 2, 1}
Out[53]: 
True
``` 
### List, Set, and Dict Comprehensions
*List comprehensions* are one of the most-loved Python language features. They allow you to concisely form a new list by filtering the elements of a collection, transforming the elements passing the filter in one concise expression. They take the basic form:
```[expr for val in collection if condition]```
This is equivalent to the following for loop:
```python
result = []
for val in collection:
    if condition:
        result.append(expr)
``` 
For example, given a list of strings, we could filter out strings with length 2 or less and also convert them to uppercase like this:
```python
In [54]: 
strings = ['a', 'as', 'bat', 'car', 'dove', 'python']
[x.upper() for x in strings if len(x) > 2]
Out[54]: 
['BAT', 'CAR', 'DOVE', 'PYTHON']
``` 
Set and dict comprehensions are a natural extension, producing sets and dicts in an  idiomatically similar way instead of lists. A dict comprehension looks like this:
```dict_comp = {key-expr : value-expr for value in collection if condition}```
A set comprehension looks like the equivalent list comprehension except with curly braces instead of square brackets:
```set_comp = {expr for value in collection if condition}```
Suppose we wanted a set containing just the lengths of the strings contained in the collection; we could easily compute this using a set comprehension:
```python
In [55]: 
unique_lengths = {len(x) for x in strings}
unique_lengths
Out[55]: 
{1, 2, 3, 4, 6}
``` 
We could also express this more functionally using the `map` function(*Make an iterator that computes the function using arguments from each of the iterables.  Stops when the shortest iterable is exhausted.*), introduced shortly:
```python
In [56]: 
set(map(len, strings))
Out[56]: 
{1, 2, 3, 4, 6}
``` 
As a simple dict comprehension example, we could create a lookup map of these strings to their locations in the list:
```python
In [57]: 
loc_mapping = {val : index for index, val in enumerate(strings)}
loc_mapping
Out[57]: 
{'a': 0, 'as': 1, 'bat': 2, 'car': 3, 'dove': 4, 'python': 5}
``` 
#### Nested list comprehensions
Suppose we have a list of lists containing some English and Spanish names:
Now, suppose we wanted to get a single list containing all names with two or more e’s in them. We could certainly do this with a simple `for` loop:
```python
names_of_interest = []
for names in all_data:
    enough_es = [name for name in names if name.count('e') >= 2]
    names_of_interest.extend(enough_es)
``` 
You can actually wrap this whole operation up in a single nested list comprehension, which will look like:
```python
In [58]: 
result = [name for names in all_data for name in names if name.count('e') >= 2]
result
Out[58]: 
['Steven']
``` 
At first, nested list comprehensions are a bit hard to wrap your head around. The for parts of the list comprehension are arranged according to the order of nesting, and any filter condition is put at the end as before. Here is another example where we “flatten” a list of tuples of integers into a simple list of integers:
```python
In [59]: 
some_tuples = [(1, 2, 3), (4, 5, 6), (7, 8, 9)]
flattened = [x for tup in some_tuples for x in tup]
flattened
Out[59]: 
[1, 2, 3, 4, 5, 6, 7, 8, 9]
``` 
You can have arbitrarily many levels of nesting, though if you have more than two or three levels of nesting you should probably start to question whether this makes sense from a code readability standpoint. It’s important to distinguish the syntax just shown from a list comprehension inside a list comprehension, which is also perfectly valid:
```python
In [60]: 
[[x for x in tup] for tup in some_tuples]
Out[60]: 
[1, 2, 3, 4, 5, 6, 7, 8, 9]
``` 
This produces a list of lists, rather than a flattened（扁平化）list of all of the inner elements.
## 3.2 Functions
**There is no issue with having multiple `return` statements. If Python reaches the end of a function without encountering a `return` statement, `None` is returned automatically.**
Each function can have positional arguments and keyword arguments. Keyword arguments are most commonly used to specify default values or optional arguments.
### Namespaces, Scope, and Local Functions
Functions can access variables in two different scopes: global and local. An alternative and more descriptive name describing a variable scope in Python is a namespace. Any variables that are assigned within a function by default are assigned to the local namespace. The local namespace is created when the function is called and immediately populated by（由...填充）the function’s arguments. After the function is finished, the local namespace is destroyed (with some exceptions that are outside the purview 范围 of this chapter). Consider the following function:
```python
def func():
    a = []
    for i in range(5):
        a.append(i)
``` 
When `func()` is called, the empty list a is created, five elements are appended, and then a is destroyed when the function exits. Suppose instead we had declared a as follows:
```python
a = []
def func():
    for i in range(5):
        a.append(i)
``` 
**Assigning variables outside of the function’s scope is possible, but those variables must be declared as global via the `global` keyword:**
```python
a = None
def bind_a_variable():
    global a
    a = []
bind_a_variable()
``` 
I generally discourage use of the `global` keyword. Typically global variables are used to store some kind of state in a system. If you find yourself using a lot of them, it may indicate a need for objectoriented programming (using classes).
### Returning Multiple Values
When I first programmed in Python after having programmed in Java and C++, one of my favorite features was the ability to return multiple values from a function with simple syntax. Here’s an example:
```python
def f():
    a = 5
    b = 6
    c = 7
    return a, b, c
a, b, c = f()
``` 
In data analysis and other scientific applications, you may find yourself doing this often. What’s happening here is that the function is actually just returning one object, namely a tuple, which is then being unpacked into the result variables. In the preceding example, we could have done this instead:
```python
return_value = f()
``` 
In this case, `return_value` would be a 3-tuple with the three returned variables. A potentially attractive alternative to returning multiple values like before might be to return a dict instead:
```python
def f():
    a = 5
    b = 6
    c = 7
    return {'a' : a, 'b' : b, 'c' : c}
``` 
This alternative technique can be useful depending on what you are trying to do.
### Functions Are Objects
Suppose we were doing some data cleaning and needed to apply a bunch of transformations to the following list of strings:
```python
states = [' Alabama ', 'Georgia!', 'Georgia', 'georgia', 'FlOrIda', 'south carolina##', 'West virginia?']
``` 
Anyone who has ever worked with user-submitted survey data has seen messy results like these. Lots of things need to happen to make this list of strings uniform and ready for analysis: stripping whitespace, removing punctuation symbols, and standardizing on proper capitalization. One way to do this is to use built-in string methods along with the `re` standard library module for regular expressions:
```python
import re
def clean_strings(strings):
    result = []
    for value in strings:
        value = value.strip() # Remove leading and trailing spaces
        value = re.sub('[!#?]', '', value) # Replace '!#?' with '' in value
        value = value.title()
        result.append(value)
    return result
In [61]: 
clean_strings(states)
Out[61]: 
['Alabama',
'Georgia',
'Georgia',
'Georgia',
'Florida',
'South Carolina',
'West Virginia']
``` 
An alternative approach that you may find useful is to make a list of the operations you want to apply to a particular set of strings:
```python
def remove_punctuation(value):
    return re.sub('[!#?]', '', value)
clean_ops = [str.strip, remove_punctuation, str.title]
def clean_strings(strings, ops):
    result = []
    for value in strings:
        for function in ops:
        value = function(value)
    result.append(value)
return result
``` 
A more functional pattern like this enables you to easily modify how the strings are transformed at a very high level. The `clean_strings` function is also now more reusable and generic(可重用的和通用的).
You can use functions as arguments to other functions like the built-in `map`(map() 会根据提供的函数对指定序列做映射。第一个参数 function 以参数序列中的每一个元素调用 function 函数，返回包含每次 function 函数返回值的新列表。) function, which applies a function to a sequence of some kind:
```python
In [62]:
for x in map(remove_punctuation, states):
    print(x)
Out[62]: 
Alabama
Georgia
Georgia
georgia
FlOrIda
south carolina
West virginia
``` 
### Anonymous(Lambada) Functions 无名函数
Python has support for so-called *anonymous or lambda functions*, which are a way of writing functions consisting of a single statement, the result of which is the return value. They are defined with the `lambda` keyword, which has no meaning other than “we are declaring an anonymous function”:
```python
def short_function(x):
    return x * 2
equiv_anon = lambda x: x * 2
``` 
For example, consider this silly example:
```python
def apply_to_list(some_list, f):
    return [f(x) for x in some_list]
ints = [4, 0, 1, 5, 6]
apply_to_list(ints, lambda x: x * 2)
```
You could also have written `[x * 2 for x in ints]`, but here we were able to succinctly(简洁地) pass a custom operator to the `apply_to_list` function.
As another example, suppose you wanted to sort a collection of strings by the number of distinct letters(不同的字母个数) in each string:
```python
In [63]:
strings = ['foo', 'card', 'bar', 'aaaa', 'abab']
strings.sort(key=lambda x: len(set(list(x)))) # set() 函数创建一个无序不重复元素集
Out[63]: 
['aaaa', 'foo', 'abab', 'bar', 'card']
```
### Currying: Partial Argument Application
*Currying* is computer science jargon (named after the mathematician Haskell Curry) that means deriving new functions from existing ones by partial argument application. For example, suppose we had a trivial function(不重要的函数) that adds two numbers together:
```python
def add_numbers(x, y):
    return x + y
```
Using this function, we could derive a new function of one variable `add_five`, that adds 5 to its argument:
```python
add_five = lambda y: add_numbers(5, y)
```
The second argument to `add_numbers` is said to be curried. There’s nothing very fancy here, as all we’ve really done is define a new function that calls an existing function. The built-in `functools` module can simplify this process using the `partial` function:
```python
from functools import partial
add_five = partial(add_numbers, 5)
```
### Generators
Having a consistent way to iterate(重复) over sequences, like objects in a list or lines in a file, is an important Python feature. This is accomplished by means of the iterator protocol, a generic way to make objects iterable. For example, iterating over a dict yields the dict keys:
```python
In [64]:
some_dict = {'a': 1, 'b': 2, 'c': 3}
for key in some_dict:
    print(key)
Out[64]: 
a
b
c
```
When you write `for key in some_dict`, the Python interpreter first attempts to create an iterator out of `some_dict`:
```python
In [65]:
dict_iterator = iter(some_dict)
dict_iterator
list(dict_iterator)
Out[65]: 
<dict_keyiterator at 0x107170e08>
['a', 'b', 'c']
```
A generator is a concise way to construct a new iterable object. Whereas normal functions execute and return a single result at a time, generators return a sequence of multiple results lazily, pausing after each one until the next one is requested. To create a generator, use the `yield` keyword instead of `return` in a function:
```python
In [66]:
def squares(n=10):
    print('Generating squares from 1 to {0}'.format(n ** 2))
    for i in range(1, n + 1):
        yield i ** 2
Out[66]: 
<dict_keyiterator at 0x107170e08>
['a', 'b', 'c']
```
When you actually call the generator, no code is immediately executed:
```python
In [67]:
gen = squares()
gen
Out[67]: 
<generator object squares at 0x105dd8f48>
```
It is not until you request elements from the generator that it begins executing its code:
```python
In [68]:
for x in gen:
    print(x, end = ' ')
Out[68]: 
Generating squares from 1 to 100
1 4 9 16 25 36 49 64 81 100
```
#### Generator expresssions
Another even more concise way to make a generator is by using a generator expression. This is a generator analogue to list, dict, and set comprehensions; to create one, enclose what would otherwise be a list comprehension within parentheses instead of brackets:
```python
In [69]:
gen = (x ** 2 for x in range(100))
gen
Out[69]: 
<generator object <genexpr> at 0x105dd8e58>
```
This is completely equivalent to the following more verbose（冗长）generator. Generator expressions can be used instead of list comprehensions as function arguments in many cases:
```python
In [70]:
sum(x ** 2 for x in range(100))
Out[70]: 
328350
In [71]:
dict((i, i **2) for i in range(5))
Out[71]: 
{0: 0, 1: 1, 2: 4, 3: 9, 4: 16}
```
#### intertools module
The standard library `itertools` module has a collection of generators for many common data algorithms. For example, `groupby` takes any sequence and a function, grouping consecutive elements in the sequence by return value of the function. Here’s an example:
```python
In [72]:
import itertools
first_letter = lambda x: x[0]
names = ['Alan', 'Adam', 'Wes', 'Will', 'Albert', 'Steven']
for letter, names in itertools.groupby(names, first_letter):
    print(letter, list(names)) # names is a generator
Out[72]: 
{0: 0, 1: 1, 2: 4, 3: 9, 4: 16}
```
*Some useful itertools functions*  

Function | Description
------------ | -------------
`combinations(iterable, k)` | Generates a sequence of all possible k-tuples of elements in the iterable, ignoring order and without replacement (see also the companion function `combinations_with_replacement`)
`permutations(iterable, k)` | Generates a sequence of all possible k-tuples of elements in the iterable, respecting order
`groupby(iterable[, keyfunc])` | Generates `(key, sub-iterator)` for each unique key
`product(\*iterables, repeat=1)` | Generates the Cartesian product of the input iterables as tuples, similar to a nested `for` loop

### Errors and Exception Handling
Handling Python errors or exceptions gracefully（优雅地） is an important part of building robust programs. In data analysis applications, many functions only work on certain kinds of input. As an example, Python’s `float` function is capable of casting a string to a floating-point number, but fails with ValueError on improper inputs:
```python
In [73]:
float('something')
Out[73]: 
---------------------------------------------------------------------------
ValueError                                Traceback (most recent call last)
<ipython-input-10-2649e4ade0e6> in <module>
----> 1 float('something')

ValueError: could not convert string to float: 'something'
```
Suppose we wanted a version of `float` that fails gracefully, returning the input argument. We can do this by writing a function that encloses the call to `float` in a `try/except` block:
```python
def attempt_float(x):
    try:
        return float(x)
    except:
        return x
```
You might want to only suppress ValueError, since a TypeError (the input was not a string or numeric value) might indicate a legitimate（合法） bug in your program. To do that, write the exception type after except:
```python
def attempt_float(x):
    try:
        return float(x)
    except ValueError:
        return x
In [74]:
attempt_float((1, 2))
Out[74]: 
---------------------------------------------------------------------------
TypeError                                 Traceback (most recent call last)
<ipython-input-12-8b0026e9e6b7> in <module>
----> 1 attempt_float((1, 2))

<ipython-input-11-6209ddecd2b5> in attempt_float(x)
      1 def attempt_float(x):
      2     try:
----> 3         return float(x)
      4     except ValueError:
      5         return x

TypeError: float() argument must be a string or a number, not 'tuple'
```
You can catch multiple exception types by writing a tuple of exception types instead (the parentheses（括号） are required):
```python
def attempt_float(x):
    try:
        return float(x)
    except(TypeError, ValueError):
        return x
```
In some cases, you may not want to suppress an exception, but you want some code to be executed regardless of whether the code in the `try` block succeeds or not. To do this, use `finally`:
```python
f = open(path, 'w')
try:
    write_to_file(f)
finally:
    f.close()
```
**AssertionError:**
Having additional context by itself is a big advantage over the standard Python interpreter (which does not provide any additional context). You can control the amount of context shown using the %xmode magic command, from Plain (same as the standard Python interpreter) to Verbose (which inlines function argument values and more). As you will see later in the chapter, you can step into the stack (using the %debug or %pdb magics) after an error has occurred for interactive post-mortem debugging.
## 3.3 Files and the Operating System
