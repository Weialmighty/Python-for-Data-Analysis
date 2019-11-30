# CHAPTER5 Getting Started with pandas
pandas will be a major tool of interest throughout much of the rest of the book. It
contains data structures and data manipulation tools designed to make data cleaning
and analysis fast and easy in Python. pandas is often used in tandem with numerical
computing tools like NumPy and SciPy, analytical libraries like statsmodels and
scikit-learn, and data visualization libraries like matplotlib. pandas adopts significant
parts of NumPy’s idiomatic style of array-based computing, especially array-based
functions and a preference for data processing without `for` loops.  

While pandas adopts many coding idioms from NumPy, the biggest difference is that
pandas is designed for working with tabular or heterogeneous data. NumPy, by contrast,
is best suited for working with homogeneous numerical array data.  

Since becoming an open source project in 2010, pandas has matured into a quite
large library that’s applicable in a broad set of real-world use cases. The developer
community has grown to over 800 distinct contributors, who’ve been helping build
the project as they’ve used it to solve their day-to-day data problems.  

Throughout the rest of the book, I use the following import convention for pandas:
```python3
In[149]:
import pandas as pd
```
Thus, whenever you see `pd.` in code, it’s referring to pandas. You may also find it easier
to import Series and DataFrame into the local namespace since they are so frequently
used:
```python3
In[150]:
from pandas import Series, DataFrame
```
## 5.1 Introduction to pandas Data Structures
To get started with pandas, you will need to get comfortable with its two workhorse
data structures: *Series* and *DataFrame*. While they are not a universal solution for
every problem, they provide a solid, easy-to-use basis for most applications.
### Series
A Series is a one-dimensional array-like object containing a sequence of values (of
similar types to NumPy types) and an associated array of data labels, called its index.
The simplest Series is formed from only an array of data:
```python3
In[151]:
obj = pd.Series([4, 7, -5, 3])
obj
Out[151]:
0    4
1    7
2   -5
3    3
dtype: int64
```
The string representation of a Series displayed interactively shows the index on the
left and the values on the right. Since we did not specify an index for the data, a
default one consisting of the integers 0 through N - 1 (where N is the length of the
data) is created. You can get the array representation and index object of the Series via
its values and index attributes, respectively:
```python3
In[152]:
obj.values
obj.index
Out[152]:
array([ 4,  7, -5,  3])
RangeIndex(start=0, stop=4, step=1)
```
Often it will be desirable to create a Series with an index identifying each data point
with a label:
```python3
In[153]:
obj2 = pd.Series([4, 7, -5, 3], index=['d', 'b', 'a', 'c'])
obj2
Out[153]:
d    4
b    7
a   -5
c    3
dtype: int64
In[154]:
obj2.index
Out[154]:
Index(['d', 'b', 'a', 'c'], dtype='object')
```
Should you have data contained in a Python dict, you can create a Series from it by
passing the dict:
```python3
In[155]:
sdata = {'Ohio': 35000, 'Texas': 71000, 'Oregon': 16000, 'Utah': 5000}
obj3 = pd.Series(sdata)
obj3
Out[155]:
Ohio      35000
Texas     71000
Oregon    16000
Utah       5000
dtype: int64
```
When you are only passing a dict, the index in the resulting Series will have the dict’s
keys in sorted order. You can override this by passing the dict keys in the order you
want them to appear in the resulting Series:
```python3
In[156]:
states = ['California', 'Ohio', 'Oregon', 'Texas']
obj4 = pd.Series(sdata, index=states)
obj4
Out[156]:
California        NaN
Ohio          35000.0
Oregon        16000.0
Texas         71000.0
dtype: float64
```
I will use the terms “missing” or “NA” interchangeably to refer to missing data. The
`isnull` and `notnull` functions in pandas should be used to detect missing data:
```python3
In[157]:
pd.isnull(obj4)
Out[157]:
California     True
Ohio          False
Oregon        False
Texas         False
dtype: bool
```
A useful Series feature for many applications is that it automatically aligns by index
label in arithmetic operations:
```python3
In[158]:
obj3 + obj4
Out[158]:
California         NaN
Ohio           70000.0
Oregon         32000.0
Texas         142000.0
Utah               NaN
dtype: float64
```
Data alignment（对齐） features will be addressed in more detail later. If you have experience
with databases, you can think about this as being similar to a join operation.
Both the Series object itself and its index have a `name` attribute, which integrates with
other key areas of pandas functionality:
```python3
In[159]:
obj4.name = 'population'
obj4.index.name = 'state'
obj4
Out[159]:
California         NaN
Ohio           70000.0
Oregon         32000.0
Texas         142000.0
Utah               NaN
dtype: float64
```
A Series’s index can be altered in-place by assignment:
```python3
In[160]:
obj
obj.index = ['Bob', 'Steve', 'Jeff', 'Ryan']
obj
Out[160]:
0 4
1 7
2 -5
3 3
dtype: int64
Bob 4
Steve 7
Jeff -5
Ryan 3
dtype: int64
```
### DataFrame
A DataFrame represents a rectangular table of data and contains an ordered collection
of columns, each of which can be a different value type (numeric, string,
boolean, etc.). The DataFrame has both a row and column index; it can be thought of
as a dict of Series all sharing the same index. Under the hood, the data is stored as one
or more two-dimensional blocks rather than a list, dict, or some other collection of
one-dimensional arrays. The exact details of DataFrame’s internals are outside the
scope of this book.
There are many ways to construct a DataFrame, though one of the most common is
from a dict of equal-length lists or NumPy arrays:
```python3
data = {'state': ['Ohio', 'Ohio', 'Ohio', 'Nevada', 'Nevada', 'Nevada'],
'year': [2000, 2001, 2002, 2001, 2002, 2003],
'pop': [1.5, 1.7, 3.6, 2.4, 2.9, 3.2]}
frame = pd.DataFrame(data)
```
The resulting DataFrame will have its index assigned automatically as with Series, and
the columns are placed in sorted order:
```python3
In[161]:
frame
Out[161]:
	state	year	pop
0	Ohio	2000	1.5
1	Ohio	2001	1.7
2	Ohio	2002	3.6
3	Nevada	2001	2.4
4	Nevada	2002	2.9
5	Nevada	2003	3.2
```
For large DataFrames, the `head` method selects only the first five rows:
```python3
In[162]:
frame.head()
Out[162]:
	state	year	pop
0	Ohio	2000	1.5
1	Ohio	2001	1.7
2	Ohio	2002	3.6
3	Nevada	2001	2.4
4	Nevada	2002	2.9
```
