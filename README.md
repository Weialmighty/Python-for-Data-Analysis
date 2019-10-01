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
