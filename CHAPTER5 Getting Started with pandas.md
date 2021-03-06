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
Another common form of data is a nested dict of dicts:
```python3
pop = {'Nevada': {2001: 2.4, 2002: 2.9},
       'Ohio': {2000: 1.5, 2001: 1.7, 2002: 3.6}}
```
If the nested dict is passed to the DataFrame, pandas will interpret the outer dict keys as the columns and the inner keys as the row indices:
```python3
In[171]:
frame3 = pd.DataFrame(pop)
frame3
Out[171]:
	Nevada	Ohio
2000	NaN	1.5
2001	2.4	1.7
2002	2.9	3.6
```
You can transpose the DataFrame (swap rows and columns) with similar syntax to a
NumPy array:
```python3
In[172]:
frame3.T
Out[172]:
	2000	2001	2002
Nevada	NaN	2.4	2.9
Ohio	1.5	1.7	3.6
```
The keys in the inner dicts are combined and sorted to form the index in the result.
This isn’t true if an explicit index is specified:
```python3
In[173]:
pd.DataFrame(pop, index=[2001, 2002, 2003])
Out[173]:
	Nevada	Ohio
2001	2.4	1.7
2002	2.9	3.6
2003	NaN	NaN
```
Dicts of Series are treated in much the same way:
```python3
In[174]:
pdata = {'Ohio': frame3['Ohio'][:-1],
	 'Nevada': frame3['Nevada'][:2]}
pd.DataFrame(pdata)
Out[174]:
	Nevada	Ohio
2001	2.4	1.7
2002	2.9	3.6
2003	NaN	NaN
```
If a DataFrame’s `index` and `columns` have their `name` attributes set, these will also be
displayed:
```python3
In[175]:
frame3.index.name = 'year'; frame3.columns.name = 'state'
frame3
Out[175]:
state	Nevada	Ohio
year		
2000	NaN	1.5
2001	2.4	1.7
2002	2.9	3.6
```
As with Series, the values attribute returns the data contained in the DataFrame as a
two-dimensional ndarray:
```python3
In[176]:
frame3.values
Out[176]:
array([[nan, 1.5],
       [2.4, 1.7],
       [2.9, 3.6]])
```
If the DataFrame’s columns are different dtypes, the dtype of the values array will be
chosen to accommodate all of the columns:
```python3
In[177]:
frame2.values
Out[177]:
array([[2000, 'Ohio', 1.5, nan],
       [2001, 'Ohio', 1.7, -1.2],
       [2002, 'Ohio', 3.6, nan],
       [2001, 'Nevada', 2.4, -1.5],
       [2002, 'Nevada', 2.9, -1.7],
       [2003, 'Nevada', 3.2, nan]], dtype=object)
```
*Table 5-1. Possible data inputs to DataFrame construtor*  

Type | Notes
------------ | -------------
2D ndarray | A matrix of data, passing optional row and column labels
dict of arrays, lists, or tuples | Each sequence becomes a column in the DataFrame; all sequences must be the same length
NumPy structured/record array | Treated as the “dict of arrays” case
dict of Series | Each value becomes a column; indexes from each Series are unioned together to form the result’s row index if no explicit index is passed
dict of dicts | Each inner dict becomes a column; keys are unioned to form the row index as in the “dict of Series” case
List of dicts or Series | Each item becomes a row in the DataFrame; union of dict keys or Series indexes become the DataFrame’s column labels
List of lists or tuples | Treated as the “2D ndarray” case
Another DataFrame | The DataFrame’s indexes are used unless different ones are passed
NumPy MaskedArray | Like the “2D ndarray” case except masked values become NA/missing in the DataFrame result

### Index Objects
pandas’s Index objects are responsible for holding the axis labels and other metadata
(like the axis name or names). Any array or other sequence of labels you use when
constructing a Series or DataFrame is internally converted to an Index:
```python3
In[178]:
obj = pd.Series(range(3), index=['a', 'b', 'c'])
index = obj.index
index
Out[178]:
Index(['a', 'b', 'c'], dtype='object')
```
**Index objects are immutable and thus can’t be modified by the user:**
`TypeError: Index does not support mutable operations`
Immutability makes it safer to share Index objects among data structures:
```python3
In[179]:
labels = pd.Index(np.arange(3))
labels
Out[179]:
Int64Index([0, 1, 2], dtype='int64')
In[180]:
obj2 = pd.Series([1.5, -2.5, 0], index=labels)
obj2
Out[180]:
0    1.5
1   -2.5
2    0.0
dtype: float64
```
In addition to being array-like, an Index also behaves like a fixed-size set:
Unlike Python sets, a pandas Index can contain duplicate labels:
```python3
In[181]:
dup_labels = pd.Index(['foo', 'foo', 'bar', 'bar'])
dup_labels
Out[181]:
Index(['foo', 'foo', 'bar', 'bar'], dtype='object')
```
*Table 5-2. Some Index methods and properties*  

Method | Description
------------ | -------------
`append` | Concatenate with additional Index objects, producing a new Index
`difference` | Compute set difference as an Index
`intersection` | Compute set intersection
`union` | Compute set union
`isin` | Compute boolean array indicating whether each value is contained in the passed collection
`drop` | Compute new Index by deleting passed values
`insert` | Compute new Index by inserting element at index i
`is_monotonic（单调的）` | Returns True if each element is greater than or equal to the previous element
`is_unique` | Returns True if the Index has no duplicate values
`unique` | Compute the array of unique values in the Index

## 5.2 Essential Functionality
In the chapters to come, we will delve more
deeply into data analysis and manipulation topics using pandas. This book is not
intended to serve as exhaustive(详尽的) documentation for the pandas library; instead, we’ll
focus on the most important features, leaving the less common (i.e., more esoteric 深奥)
things for you to explore on your own.
### Reindexing
An important method on pandas objects is `reindex`, which means to create a new
object with the data *conformed* to a new index. Consider an example:
```python3
In[182]:
obj = pd.Series([4.5, 7.2, -5.3, 3.6], index=['d', 'b', 'a', 'c'])
obj
Out[182]:
d    4.5
b    7.2
a   -5.3
c    3.6
dtype: float64
```
Calling `reindex` on this Series rearranges the data according to the new index, introducing
missing values if any index values were not already present:
```python3
In[183]:
obj2 = obj.reindex(['a', 'b', 'c', 'd', 'e'])
obj2
Out[183]:
a   -5.3
b    7.2
c    3.6
d    4.5
e    NaN
dtype: float64
```
For ordered data like time series, it may be desirable to do some interpolation（插值） or filling
of values when reindexing. The `method` option allows us to do this, using a
method such as `ffill`, which forward-fills the values:
```python3
In[184]:
obj3 = pd.Series(['blue', 'purple', 'yellow'], index=[0, 2, 4])
obj3
Out[184]:
0      blue
2    purple
4    yellow
dtype: object
```
```python3
In[185]:
obj3.reindex(range(6), method='ffill')
Out[185]:
0      blue
1      blue
2    purple
3    purple
4    yellow
5    yellow
dtype: object
```
With DataFrame, `reindex` can alter either the (row) index, columns, or both. When
passed only a sequence, it reindexes the rows in the result:
```python3
In[186]:
frame = pd.DataFrame(np.arange(9).reshape((3, 3)), index=['a', 'c', 'd'], columns=['Ohio', 'Texas', 'California'])
frame
Out[186]:
        Ohio	Texas	California
a	0	1	2
c	3	4	5
d	6	7	8
In[187]:
frame2 = frame.reindex(['a', 'b', 'c', 'd'])
frame2
Out[187]:
        Ohio	Texas	California
a	0.0	1.0	2.0
b	NaN	NaN	NaN
c	3.0	4.0	5.0
d	6.0	7.0	8.0
```
The columns can be reindexed with the `columns` keyword:
```python3
In[188]:
states = ['Texas', 'Utah', 'California']
frame.reindex(columns=states)
Out[188]:
        Texas	Utah	California
a	1	NaN	2
c	4	NaN	5
d	7	NaN	8
```
See Table 5-3 for more about the arguments to `reindex`.  
As we’ll explore in more detail, you can reindex more succinctly by label-indexing
with loc, and many users prefer to use it exclusively:
```python3
In[189]:
frame.loc[['a', 'b', 'c', 'd'], states]
Out[189]:
        Texas	Utah	California
a	1.0	NaN	2.0
b	NaN	NaN	NaN
c	4.0	NaN	5.0
d	7.0	NaN	8.0
```
*Table 5-3. Reindex function arguments*  

Method | Description
------------ | -------------
`index` | New sequence to use as index. Can be Index instance or any other sequence-like Python data structure. An Index will be used exactly as is without any copying.
`method` | Interpolation (fill) method; 'ffill' fills forward, while 'bfill' fills backward.
`fill_value` | Substitute value to use when introducing missing data by reindexing.
`limit` | When forward- or backfilling, maximum size gap (in number of elements) to fill.
`tolerance` | When forward- or backfilling, maximum size gap (in absolute numeric distance) to fill for inexact matches.
`level` | Match simple Index on level of MultiIndex; otherwise select subset of.
`copy` | If True, always copy underlying data even if new index is equivalent to old index; if False, do not copy the data when the indexes are equivalent.  
### Dropping Entries from an Axis
Dropping one or more entries（词条） from an axis is easy if you already have an index array
or list without those entries. As that can require a bit of munging（数组转换） and set logic, the
 `drop` method will return a new object with the indicated value or values deleted from
an axis:
```python3
In[190]:
obj = pd.Series(np.arange(5.), index=['a', 'b', 'c', 'd', 'e'])
obj
Out[190]:
a    0.0
b    1.0
c    2.0
d    3.0
e    4.0
dtype: float64
```
```python3
In[191]:
new_obj = obj.drop('c')
new_obj
Out[191]:
a    0.0
b    1.0
d    3.0
e    4.0
dtype: float64
```
```python3
In[192]:
obj.drop(['d', 'c'])
Out[192]:
a    0.0
b    1.0
e    4.0
dtype: float64
```
With DataFrame, index values can be deleted from either axis. To illustrate this, we
first create an example DataFrame:
```python3
In[193]:
data = pd.DataFrame(np.arange(16).reshape((4, 4)), index=['Ohio', 'Colorado', 'Utah', 'New York'], columns=['one', 'two', 'three', 'four'])
data
Out[193]:
	       one	two	three	four
Ohio	       0	1	2	3
Colorado       4	5	6	7
Utah	       8	9	10	11
New York       12	13	14	15
```
Calling `drop` with a sequence of labels will drop values from the row labels (axis 0):
```python3
In[194]:
data.drop(['Colorado', 'Ohio'])
Out[194]:
               one	two	three	four
Utah	       8	9	10	11
New York       12	13	14	15
```
You can drop values from the columns by passing `axis=1` or `axis='columns'`:
```python3
In[195]:
data.drop('two', axis=1)
Out[195]:
 	       one	three	four
Ohio	       0	2	3
Colorado       4	6	7
Utah           8	10	11
New York       12	14	15
```
```python3
In[196]:
data.drop(['two', 'four'], axis='columns')
Out[196]:
 	       one	three
Ohio	       0	2
Colorado       4	6
Utah	       8	10
New York       12	14
```
Many functions, like `drop`, which modify the size or shape of a Series or DataFrame,
can manipulate an object `inplace` without returning a new object:
```python3
In[197]:
obj.drop('c', inplace=True)
obj
Out[197]:
a    0.0
b    1.0
d    3.0
e    4.0
dtype: float64
```
*Be careful with the `inplace`, as it destroys any data that is dropped.*  
### Indexing, Selection, and Filtering
Series indexing (obj\[...\]) works analogously（类似的，近似的） to NumPy array indexing, except you
can use the Series’s index values instead of only integers. Here are some examples of
this:
```python3
In[198]:
obj = pd.Series(np.arange(4.), index=['a', 'b', 'c', 'd'])
obj
Out[198]:
a    0.0
b    1.0
c    2.0
d    3.0
dtype: float64
```
Slicing with labels behaves differently than normal Python slicing in that the endpoint
is inclusive:
```python3
In[199]:
obj['b':'c']
Out[199]:
b    1.0
c    2.0
dtype: float64
```
Setting using these methods modifies the corresponding section of the Series:
```python3
In[200]:
obj['b':'c'] = 5
obj
Out[200]:
a    0.0
b    5.0
c    5.0
d    3.0
dtype: float64
```
Indexing into a DataFrame is for retrieving（检索） one or more columns either with a single
value or sequence:
```python3
In[201]:
data = pd.DataFrame(np.arange(16).reshape((4, 4)), index=['Ohio', 'Colorado', 'Utah', 'New York'], columns=['one', 'two', 'three', 'four'])
data
Out[201]:
	        one	two	three	four
Ohio	        0	1	2	3
Colorado	4	5	6	7
Utah	        8	9	10	11
New York	12	13	14	15
In[202]:
data['two']
Out[202]:
Ohio         1
Colorado     5
Utah         9
New York    13
Name: two, dtype: int64
In[203]:
data[['three', 'one']]
Out[203]:
                three	one
Ohio	        2	0
Colorado	6	4
Utah	        10	8
New York	14	12
```
Indexing like this has a few special cases. First, slicing or selecting data with a boolean
array:
```python3
In[204]:
data[:2]
Out[204]:
	        one	two	three	four
Ohio	        0	1	2	3
Colorado	4	5	6	7
In[205]:
data[data['three'] > 5]
Out[205]:
                one	two	three	four
Colorado	4	5	6	7
Utah	        8	9	10	11
New York	12	13	14	15
```
