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
If you specify a sequence of columns, the DataFrame’s columns will be arranged in
that order:
```python3
In[163]:
pd.DataFrame(data, columns=['year', 'state', 'pop'])
Out[163]:
        year	state	pop
0	2000	Ohio	1.5
1	2001	Ohio	1.7
2	2002	Ohio	3.6
3	2001	Nevada	2.4
4	2002	Nevada	2.9
5	2003	Nevada	3.2
```
If you pass a column that isn’t contained in the dict, it will appear with missing values
in the result:
```python3
In[164]:
frame2 = pd.DataFrame(data, columns=['year', 'state', 'pop', 'debt'], index=['one', 'two', 'three', 'four', 'five', 'six'])
frame2
Out[164]:
        year	state	pop	debt
one	2000	Ohio	1.5	NaN
two	2001	Ohio	1.7	NaN
three	2002	Ohio	3.6	NaN
four	2001	Nevada	2.4	NaN
five	2002	Nevada	2.9	NaN
six	2003	Nevada	3.2	NaN
In[165]:
frame2.columns
Out[165]:
Index(['year', 'state', 'pop', 'debt'], dtype='object')
```
A column in a DataFrame can be retrieved as a Series either by dict-like notation or
by attribute:
```python3
In[165]:
frame2['state']
frame2.state
Out[165]:
one        Ohio
two        Ohio
three      Ohio
four     Nevada
five     Nevada
six      Nevada
Name: state, dtype: object
```
Note that the returned Series have the same index as the DataFrame, and their `name`
attribute has been appropriately set.
Rows can also be retrieved by position or name with the special `loc` attribute (much
more on this later):
```python3
In[165]:
frame2.loc['three']
Out[165]:
year     2002
state    Ohio
pop       3.6
debt      NaN
Name: three, dtype: object
```
Columns can be modified by assignment. For example, the empty `'debt'` column
could be assigned a scalar value or an array of values:
```python3
In[166]:
frame2['debt'] = 16.5
frame2
Out[166]:
        year	state	pop	debt
one	2000	Ohio	1.5	16.5
two	2001	Ohio	1.7	16.5
three	2002	Ohio	3.6	16.5
four	2001	Nevada	2.4	16.5
five	2002	Nevada	2.9	16.5
six	2003	Nevada	3.2	16.5
In[167]:
frame2['debt'] = np.arange(6.)
frame2
Out[167]:
	year	state	pop	debt
one	2000	Ohio	1.5	0.0
two	2001	Ohio	1.7	1.0
three	2002	Ohio	3.6	2.0
four	2001	Nevada	2.4	3.0
five	2002	Nevada	2.9	4.0
six	2003	Nevada	3.2	5.0
```
When you are assigning lists or arrays to a column, the value’s length must match the
length of the DataFrame. If you assign a Series, its labels will be realigned exactly to
the DataFrame’s index, inserting missing values in any holes:
```python3
In[168]:
val = pd.Series([-1.2, -1.5, -1.7], index=['two', 'four', 'five'])
frame2['debt'] = val
frame2
Out[168]:
	year	state	pop	debt
one	2000	Ohio	1.5	NaN
two	2001	Ohio	1.7	-1.2
three	2002	Ohio	3.6	NaN
four	2001	Nevada	2.4	-1.5
five	2002	Nevada	2.9	-1.7
six	2003	Nevada	3.2	NaN
```
Assigning a column that doesn’t exist will create a new column. The `del` keyword will
delete columns as with a dict.
As an example of del, I first add a new column of boolean values where the `state`
column equals 'Ohio':
```python3
In[169]:
frame2['eastern'] = frame2.state == 'Ohio'
frame2
Out[169]:
	year	state	pop	debt	eastern
one	2000	Ohio	1.5	NaN	True
two	2001	Ohio	1.7	-1.2	True
three	2002	Ohio	3.6	NaN	True
four	2001	Nevada	2.4	-1.5	False
five	2002	Nevada	2.9	-1.7	False
six	2003	Nevada	3.2	NaN	False
```
The `del` method can then be used to remove this column:
```python3
In[170]:
del frame2['eastern']
frame2.columns
Out[170]:
Index(['year', 'state', 'pop', 'debt'], dtype='object')
```
