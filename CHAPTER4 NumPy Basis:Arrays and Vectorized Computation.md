# **CHAPTER4 NumPy Basis:Arrays and Vectorized Computation**
NumPy, short for Numerical Python, is one of the most important foundational packages for numerical computing in Python. Most computational packages providing scientific functionality use NumPy’s array objects as the *lingua franca（通用语）* for data exchange.
While NumPy provides a computational foundation for general numerical data processing, many readers will want to use pandas as the basis for most kinds of statistics or analytics, especially on tabular data（表格数据）. pandas also provides some more domainspecific functionality like time series manipulation, which is not present in NumPy.
**NumPy-based algorithms are generally 10 to 100 times faster (or more) than their pure Python counterparts and use significantly less memory.**
## 4.1 The NumPy ndarray: A Multidimensional Array Object
One of the key features of NumPy is its N-dimensional array object, or ndarray, which is a fast, flexible container for large datasets in Python. Arrays enable you to perform mathematical operations on whole blocks of data using similar syntax to the equivalent operations between scalar（标量） elements.
```python
In [86]:
import numpy as np
data = np.random.randn(2, 3)
data
Out[86]: 
array([[ 0.22982124, -0.53056639,  1.09106596],
       [-0.9078659 , -1.67770926, -0.12138464]])
In [87]:
data * 10
Out[87]: 
array([[  2.29821239,  -5.30566387,  10.91065958],
       [ -9.07865899, -16.77709265,  -1.21384635]])
In [88]:
data + data
Out[88]: 
array([[ 0.45964248, -1.06113277,  2.18213192],
       [-1.8157318 , -3.35541853, -0.24276927]])
```
An ndarray is a generic multidimensional container for homogeneous data; that is, all of the elements must be the same type. Every array has a `shape`, a tuple indicating the size of each dimension, and a `dtype`, an object describing the *data type* of the array:
```python
In [89]:
data.shape
Out[89]: 
(2, 3)
In [90]:
data.dtype
Out[90]: 
dtype('float64')
```
This chapter will introduce you to the basics of using NumPy arrays, and should be sufficient for following along with the rest of the book. While it’s not necessary to have a deep understanding of NumPy for many data analytical applications, becoming proficient in array-oriented programming and thinking is a key step along the way to becoming a scientific Python guru（大师）.
### 4.1.1 Creating ndarrays
The easiest way to create an array is to use the `array` function. This accepts any sequence-like object (including other arrays) and produces a new NumPy array containing the passed data. For example, a list is a good candidate for conversion:
```python
In [91]:
data1 = [6, 7.5, 8, 0, 1]
arr1 = np.array(data1)
arr1
Out[91]: 
array([ 6. , 7.5, 8. , 0. , 1. ])
In [92]:
data2 = [[1, 2, 3, 4], [5, 6, 7, 8]]
arr2 = np.array(data2)
arr2
Out[92]: 
array([[1, 2, 3, 4],
[5, 6, 7, 8]])
```
Since `data2` was a list of lists, the NumPy array `arr2` has two dimensions with shape inferred from the data. We can confirm this by inspecting the `ndim` and shape attributes:
```python
In [93]:
arr2.ndim
Out[93]: 
2
In [94]:
arr2.shape
Out[94]: 
(2, 4)
```
In addition to `np.array`, there are a number of other functions for creating new arrays. As examples, `zeros` and `ones` create arrays of 0s or 1s, respectively, with a given length or shape. empty creates an array without initializing its values to any particular value. To create a higher dimensional array with these methods, pass a tuple for the shape:
```python
In [95]:
np.zeros(10)
Out[95]: 
array([0., 0., 0., 0., 0., 0., 0., 0., 0., 0.])
In [96]:
np.zeros((3, 6))
Out[96]: 
array([[0., 0., 0., 0., 0., 0.],
       [0., 0., 0., 0., 0., 0.],
       [0., 0., 0., 0., 0., 0.]])
In [97]:
np.empty((2, 3, 2))
Out[97]: 
array([[[6.91691904e-323, 0.00000000e+000],
        [0.00000000e+000, 5.02034658e+175],
        [1.11661862e-046, 4.00886206e+174]],

       [[3.54390428e-061, 1.11690050e+165],
        [5.01163106e+217, 5.49419094e-143],
        [1.50008929e+248, 1.06396443e+224]]])
```

Type | Type code | Description
------------ | ------------- | -------------
int8, uint8 | i1, u1 | Signed and unsigned 8-bit (1 byte) integer types
int16, uint16 | i2, u2 | Signed and unsigned 16-bit integer types
int32, uint32 | i4, u4 | Signed and unsigned 32-bit integer types
int64, uint64 | i8, u8 | Signed and unsigned 64-bit integer types
float16 | f2 | Half-precision floating point
float32 | f4 or f | Standard single-precision floating point; compatible with C float
float64 | f8 or d | Standard double-precision floating point; compatible with C double and Python float object
float128 | f16 or g | Extended-precision floating point
complex64 | c8, c16 | Complex numbers represented by two 32, 64, or 128 floats, respectively
complex128 | c32 | 
complex256 | | 

If you have an array of strings representing numbers, you can use astype to convert them to numeric form:
```python
In [98]:
numeric_strings = np.array(['1.25', '-9.6', '42'], dtype=np.string_)
numeric_strings.astype(float)
Out[98]: 
array([ 1.25, -9.6 , 42.  ])
```
### 4.1.2 Arithmetic（算术） with NumPy Arrays
Arrays are important because they enable you to express batch operations on data without writing any `for` loops. NumPy users call this vectorization（向量化）. Any arithmetic operations between equal-size arrays applies the operation element-wise:
them to numeric form:
```python
In [99]:
arr = np.array([[1, 2, 3], [4, 5, 6]])
arr * arr
Out[99]: 
array([[ 1,  4,  9],
       [16, 25, 36]])
```
Arithmetic operations with scalars propagate the scalar argument to each element in the array:
```python
In [100]:
arr = np.array([[1, 2, 3], [4, 5, 6]])
arr ** 0.5
Out[100]: 
array([[1.        , 1.41421356, 1.73205081],
       [2.        , 2.23606798, 2.44948974]])
```
### 4.1.3 Basic Indexing and Slicing
NumPy array indexing is a rich topic, as there are many ways you may want to select a subset of your data or individual elements. One-dimensional arrays are simple; on the surface they act similarly to Python lists:
An important first distinction
from Python’s built-in lists is that array slices are views on the original array.
This means that the data is not copied, and any modifications to the view will be
reflected in the source array.
To give an example of this, I first create a slice of arr:
```python
In [101]:
arr = np.arange(10)
arr[5:8] = 12
arr_slice = arr[5:8]
arr_slice[1] = 12345
arr
Out[101]: 
array([    0,     1,     2,     3,     4,    12, 12345,    12,     8,
           9])
```
The “bare” slice [:] will assign to all values in an array:
```python
In [102]:
arr_slice[:] = 64
arr
Out[102]: 
array([ 0,  1,  2,  3,  4, 64, 64, 64,  8,  9])
```
**If you are new to NumPy, you might be surprised by this, especially if you have used other array programming languages that copy data more eagerly. As NumPy has been
designed to be able to work with very large arrays, you could imagine performance
and memory problems if NumPy insisted on always copying data.**  
If you want a copy of a slice of an ndarray instead of a view, youwill need to explicitly copy the array—for example,``arr[5:8].copy()``.  
With higher dimensional arrays, you have many more options. In a two-dimensional
array, the elements at each index are no longer scalars but rather one-dimensional
arrays:
```python
In [103]:
arr2d = np.array([[1, 2, 3], [4, 5, 6], [7, 8, 9]])
arr2d[2]
Out[103]: 
array([7, 8, 9])
```
Thus, individual elements can be accessed recursively（递归地）. But that is a bit too much work, so you can pass a comma-separated list of indices to select individual elements.So these are equivalent:
```python
In [104]:
arr2d[0][2]
arr2d[0, 2]
Out[104]: 
3
```
#### Indexing with slices
![Two-dimensional array slicing](https://raw.githubusercontent.com/Weialmighty/Python-for-Data-Analysis/master/images/Two-dimensional%20array%20slicing.png)
#### Boolean Indexing
Let’s consider an example where we have some data in an array and an array of names with duplicates.
```python
In [105]:
names = np.array(['Bob', 'Joe', 'Will', 'Bob', 'Will', 'Joe', 'Joe'])
data = np.random.randn(7, 4)
names
data
Out[105]: 
array(['Bob', 'Joe', 'Will', 'Bob', 'Will', 'Joe', 'Joe'], dtype='<U4')
array([[ 0.05259706,  0.63026513,  0.43002745,  0.21723217],
       [ 0.09837067,  1.03194437, -1.61616254,  0.51231335],
       [-0.42059041, -0.12141926,  0.92325247,  0.53682676],
       [-0.4736811 , -1.7549849 , -0.69585262,  0.13715611],
       [ 1.25152543,  0.19938113,  0.24788077,  0.5999967 ],
       [ 0.24395716, -0.12508072,  0.33555566, -0.60460334],
       [-1.20109676,  0.05939765, -0.06060381, -0.2713454 ]])
In [106]:
names == 'Bob'
data[names == 'Bob']
Out[106]: 
array([ True, False, False,  True, False, False, False])
array([[ 0.05259706,  0.63026513,  0.43002745,  0.21723217],
       [-0.4736811 , -1.7549849 , -0.69585262,  0.13715611]])
In [107]:
data[names != 'Joe'] = 7
data
Out[107]: 
array([[ 7.        ,  7.        ,  7.        ,  7.        ],
       [ 0.09837067,  1.03194437, -1.61616254,  0.51231335],
       [ 7.        ,  7.        ,  7.        ,  7.        ],
       [ 7.        ,  7.        ,  7.        ,  7.        ],
       [ 7.        ,  7.        ,  7.        ,  7.        ],
       [ 0.24395716, -0.12508072,  0.33555566, -0.60460334],
       [-1.20109676,  0.05939765, -0.06060381, -0.2713454 ]])
```
#### Fancy Indexing
```python
In [108]:
arr = np.empty((8, 4))
for i in range(8):
    arr[i] = i
Out[108]: 
array([[0., 0., 0., 0.],
       [1., 1., 1., 1.],
       [2., 2., 2., 2.],
       [3., 3., 3., 3.],
       [4., 4., 4., 4.],
       [5., 5., 5., 5.],
       [6., 6., 6., 6.],
       [7., 7., 7., 7.]])
```
To select out a subset of the rows in a particular order, you can simply pass a list or ndarray of integers specifying the desired order:
```python
In [109]:
arr[[4, 3, 0, 6]]
Out[109]: 
array([[4., 4., 4., 4.],
       [3., 3., 3., 3.],
       [0., 0., 0., 0.],
       [6., 6., 6., 6.]])
```
Using negative indices selects rows from the end:
```python
In [110]:
arr[[-3, -5, -7]]
Out[110]: 
array([[5., 5., 5., 5.],
       [3., 3., 3., 3.],
       [1., 1., 1., 1.]])
```
Passing multiple index arrays does something slightly different; it selects a one-dimensional array of elements corresponding to each tuple of indices:
```python
In [111]:
arr = np.arange(32).reshape((8, 4))
arr
Out[111]: 
array([[ 0,  1,  2,  3],
       [ 4,  5,  6,  7],
       [ 8,  9, 10, 11],
       [12, 13, 14, 15],
       [16, 17, 18, 19],
       [20, 21, 22, 23],
       [24, 25, 26, 27],
       [28, 29, 30, 31]])
In [112]:
arr[[1, 5, 7, 2], [0, 3, 1, 2]]
Out[112]: 
array([[ 0,  1,  2,  3],
       [ 4,  5,  6,  7],
       [ 8,  9, 10, 11],
       [12, 13, 14, 15],
       [16, 17, 18, 19],
       [20, 21, 22, 23],
       [24, 25, 26, 27],
       [28, 29, 30, 31]])
```
Here the elements `(1, 0)`, `(5, 3)`, `(7, 1)`, and `(2, 2)` were selected. Regardless of how many dimensions the array has (here, only 2), the result of fancy indexing is always one-dimensional.
**The behavior of fancy indexing in this case is a bit different from what some users might have expected (myself included), which is the rectangular region formed by selecting a subset of the matrix’s rows and columns. Here is one way to get that:**
```python
In [113]:
#选择第2、6、8、3行，并按[0,3,1,2]的顺序输出
arr[[1, 5, 7, 2]][:, [0, 3, 1, 2]]
Out[113]: 
array([[ 4,  7,  5,  6],
       [20, 23, 21, 22],
       [28, 31, 29, 30],
       [ 8, 11,  9, 10]])
```
**Keep in mind that fancy indexing, unlike slicing, always copies the data into a new array.**
### Transposing(转置) Arrays and Swapping Axes
Transposing is a special form of reshaping that similarly returns a view on the underlying data without copying anything. Arrays have the `transpose` method and also the special T attribute:
```python
In [114]:
arr = np.arange(15).reshape((3, 5))
arr.T
Out[114]: 
array([[ 0,  5, 10],
       [ 1,  6, 11],
       [ 2,  7, 12],
       [ 3,  8, 13],
       [ 4,  9, 14]])
```
When doing matrix computations, you may do this very often—for example, when computing the inner matrix product using `np.dot`:
```python
In [115]:
arr = np.random.randn(6, 3)
arr
np.dot(arr.T, arr)
Out[115]: 
array([[ 2.10776823,  2.10363774,  0.32784765],
       [ 0.10630046,  1.15645314,  0.67524184],
       [ 0.58085279,  1.99302591,  0.15709767],
       [-0.14919957, -1.02491907,  0.23112372],
       [-0.67886396, -1.78571283, -0.60596028],
       [-0.1954028 , -0.75675842, -1.20430483]])
array([[ 5.31267569,  7.22761324,  1.46626157],
       [ 7.22761324, 14.54674063,  3.54021372],
       [ 1.46626157,  3.54021372,  2.45907145]])
```
For higher dimensional arrays, `transpose` will accept a tuple of axis numbers to permute（置换） the axes (for extra mind bending):
```python
In [116]:
#多维数组就是数组的数组
arr = np.arange(16).reshape((2, 2, 4))
arr
arr.transpose((1, 0, 2))
Out[116]: 
array([[[ 0,  1,  2,  3],
        [ 4,  5,  6,  7]],

       [[ 8,  9, 10, 11],
        [12, 13, 14, 15]]])
In [117]:
#多维数组就是数组的数组
arr = np.arange(16).reshape((2, 2, 4))
arr
arr.transpose((1, 0, 2))
Out[117]: 
array([[[ 0,  1,  2,  3],
        [ 4,  5,  6,  7]],

       [[ 8,  9, 10, 11],
        [12, 13, 14, 15]]])

array([[[ 0,  1,  2,  3],
        [ 8,  9, 10, 11]],

       [[ 4,  5,  6,  7],
        [12, 13, 14, 15]]])
```
![](https://github.com/Weialmighty/Python-for-Data-Analysis/blob/master/images/three-dimension%20array.jpg)
Simple transposing with .T is a special case of swapping axes. ndarray has the method `swapaxes`, which takes a pair of axis numbers and switches the indicated axes to rearrange the data:
```python
In [118]:
arr.swapaxes(1, 2)
Out[118]: 
array([[[ 0,  1,  2,  3],
        [ 4,  5,  6,  7]],

       [[ 8,  9, 10, 11],
        [12, 13, 14, 15]]])
```
`swapaxes` similarly returns a view on the data **without making a copy.**
## 4.2 Universal Functions: Fast Element-Wise Array Functions
Many ufuncs are simple element-wise（按元素） transformations, like `sqrt` or `exp`:
```python
In [119]:
arr = np.arange(10)
arr
np.sqrt(arr)
np.exp(arr)
Out[119]: 
array([0, 1, 2, 3, 4, 5, 6, 7, 8, 9])

array([0.        , 1.        , 1.41421356, 1.73205081, 2.        ,
       2.23606798, 2.44948974, 2.64575131, 2.82842712, 3.        ])
       
array([1.00000000e+00, 2.71828183e+00, 7.38905610e+00, 2.00855369e+01,
       5.45981500e+01, 1.48413159e+02, 4.03428793e+02, 1.09663316e+03,
       2.98095799e+03, 8.10308393e+03])
```
These are referred to as *unary* ufuncs. Others, such as `add` or `maximum`, take two arrays (thus, binary ufuncs) and return a single array as the result:
```python
In [120]:
x = np.random.randn(8)
y = np.random.randn(8)
x
y
np.maximum(x, y)
Out[120]: 
array([ 0.05708168, -0.64941674,  0.98896889, -0.53771057,  0.94243047,
       -1.00778455, -0.37653225, -0.97950608])
       
array([-1.44891236, -0.050785  ,  0.18511402, -1.30783113,  0.54861108,
       -0.37563996, -0.75057934,  0.76797747])
       
array([ 0.05708168, -0.050785  ,  0.98896889, -0.53771057,  0.94243047,
       -0.37563996, -0.37653225,  0.76797747])
```
While not common, a ufunc can return multiple arrays. `modf` is one example, a vectorized version of the built-in Python `divmod`; it returns the fractional and integral parts of a floating-point array:
```python
In [121]:
arr = np.random.randn(7) * 5
arr
remainder, whole_part = np.modf(arr)
remainder
whole_part
np.maximum(x, y)
Out[121]: 
array([ 6.70341005,  5.3828393 , -3.91961551, 11.00272227,  7.36444749,
       -2.43786265,  4.04770636])
       
array([ 0.70341005,  0.3828393 , -0.91961551,  0.00272227,  0.36444749,
       -0.43786265,  0.04770636])
       
array([ 6.,  5., -3., 11.,  7., -2.,  4.])
```
Ufuncs accept an optional `out` argument that allows them to operate in-place on arrays:
```python
In [122]:
arr

#不替代arr
np.sqrt(arr)
arr

#替代arr
np.sqrt(arr, arr)
arr
Out[122]: 
array([ 6.70341005,  5.3828393 , -3.91961551, 11.00272227,  7.36444749,
       -2.43786265,  4.04770636])
       
array([2.58909445, 2.32009468,        nan, 3.31703516, 2.71375155,
              nan, 2.01189124])
array([ 6.70341005,  5.3828393 , -3.91961551, 11.00272227,  7.36444749,
       -2.43786265,  4.04770636])              

array([2.58909445, 2.32009468,        nan, 3.31703516, 2.71375155,
              nan, 2.01189124])
              
array([2.58909445, 2.32009468,        nan, 3.31703516, 2.71375155,
              nan, 2.01189124])              
```
*Table 4-3. Unary（一元的） ufuncs*  

Function | Description
------------ | -------------
`abs, fabs` | Compute the absolute value element-wise for integer, floating-point, or complex values
`sqrt` | Compute the square root of each element (equivalent to `arr ** 0.5`)
`square` | Compute the square of each element (equivalent to `arr ** 2`)
`exp` | Compute the exponent ex of each element
`log, log10, log2, log1p` | Natural logarithm (base e), log base 10, log base 2, and log(1 + x), respectively
`sign` | Compute the sign of each element: 1 (positive), 0 (zero), or –1 (negative)
`ceil` | Compute the ceiling of each element (i.e., the smallest integer greater than or equal to that number)
`floor` | Compute the floor of each element (i.e., the largest integer less than or equal to each element)
`rint` | Round elements to the nearest integer, preserving the `dtype`
`modf` | Return fractional and integral parts of array as a separate array
`isnan` | Return boolean array indicating whether each value is `NaN` (Not a Number)
`isfinite, isinf` | Return boolean array indicating whether each element is finite (non-inf, non-NaN) or infinite, respectively
`cos, cosh, sin, sinh, tan, tanh, arccos, arccosh, arcsin, arcsinh, arctan, arctanh` | Regular and hyperbolic（双曲） trigonometric functions and Inverse trigonometric functions
`logical_not` | Compute truth value of not x element-wise (equivalent to ~arr)  

*Table 4-4. Binary（一元的） universal functions*  

Function | Description
------------ | -------------
`add` | Add corresponding elements in arrays
`subtract` | Subtract elements in second array from first array
`multiply` | Multiply array elements
`divide, floor_divide` | Divide or floor divide (truncating the remainder)
`power` | Raise elements in first array to powers indicated in second array
`maximum, fmax` | Element-wise maximum; `fmax` ignores `NaN`
`minimum, fmin` | Element-wise minimum; `fmin` ignores `NaN`
`mod` | Element-wise modulus (remainder of division)
`copysign` | Copy sign of values in second argument to values in first argument
`greater, greater_equal, less, less_equal, equal, not_equal` | Perform element-wise comparison, yielding boolean array (equivalent to infix operators >, >=, <, <=, ==, !=)
`logical_and, logical_or, logical_xor` | Compute element-wise truth value of logical operation (equivalent to infix operators & |, ^)  

## Array-Oriented Programmming with Arrays
As a simple example, suppose we wished to evaluate the function `sqrt(x^2 + y^2)` across a regular grid of values. The `np.meshgrid` function takes two 1D arrays and produces two 2D matrices corresponding to all pairs of `(x, y)` in the two arrays:
```python
In [123]:
points = np.arange(-5, 5, 0.01) # 1000 equally spaced points
xs, ys = np.meshgrid(points, points)
xs
ys
Out[123]: 
array([[-5.  , -4.99, -4.98, ...,  4.97,  4.98,  4.99],
       [-5.  , -4.99, -4.98, ...,  4.97,  4.98,  4.99],
       [-5.  , -4.99, -4.98, ...,  4.97,  4.98,  4.99],
       ...,
       [-5.  , -4.99, -4.98, ...,  4.97,  4.98,  4.99],
       [-5.  , -4.99, -4.98, ...,  4.97,  4.98,  4.99],
       [-5.  , -4.99, -4.98, ...,  4.97,  4.98,  4.99]])
array([[-5.  , -5.  , -5.  , ..., -5.  , -5.  , -5.  ],
       [-4.99, -4.99, -4.99, ..., -4.99, -4.99, -4.99],
       [-4.98, -4.98, -4.98, ..., -4.98, -4.98, -4.98],
       ...,
       [ 4.97,  4.97,  4.97, ...,  4.97,  4.97,  4.97],
       [ 4.98,  4.98,  4.98, ...,  4.98,  4.98,  4.98],
       [ 4.99,  4.99,  4.99, ...,  4.99,  4.99,  4.99]])        
```
Now, evaluating the function is a matter of writing the same expression you would write with two points:
```python
In [124]:
z = np.sqrt(xs ** 2 + ys ** 2)
z
Out[124]: 
array([[7.07106781, 7.06400028, 7.05693985, ..., 7.04988652, 7.05693985,
        7.06400028],
       [7.06400028, 7.05692568, 7.04985815, ..., 7.04279774, 7.04985815,
        7.05692568],
       [7.05693985, 7.04985815, 7.04278354, ..., 7.03571603, 7.04278354,
        7.04985815],
       ...,
       [7.04988652, 7.04279774, 7.03571603, ..., 7.0286414 , 7.03571603,
        7.04279774],
       [7.05693985, 7.04985815, 7.04278354, ..., 7.03571603, 7.04278354,
        7.04985815],
       [7.06400028, 7.05692568, 7.04985815, ..., 7.04279774, 7.04985815,
        7.05692568]])
```
Use matplotlib to create visualizations of this twodimensional array:
```python
In [125]:
import matplotlib.pyplot as plt
plt.imshow(z, cmap=plt.cm.gray); plt.colorbar()
plt.title("Image plot of $\sqrt{x^2 + y^2}$ for a grid of values")
Out[125]: 
array([[7.07106781, 7.06400028, 7.05693985, ..., 7.04988652, 7.05693985,
        7.06400028],
       [7.06400028, 7.05692568, 7.04985815, ..., 7.04279774, 7.04985815,
        7.05692568],
       [7.05693985, 7.04985815, 7.04278354, ..., 7.03571603, 7.04278354,
        7.04985815],
       ...,
       [7.04988652, 7.04279774, 7.03571603, ..., 7.0286414 , 7.03571603,
        7.04279774],
       [7.05693985, 7.04985815, 7.04278354, ..., 7.03571603, 7.04278354,
        7.04985815],
       [7.06400028, 7.05692568, 7.04985815, ..., 7.04279774, 7.04985815,
        7.05692568]])
```
![](https://github.com/Weialmighty/Python-for-Data-Analysis/blob/master/images/Figure%204-3.png)
### Expressing Conditional logic as Array Operations
The `numpy.where` function is a vectorized version of the ternary expression x `if condition else y`. Suppose we had a boolean array and two arrays of values, Suppose we wanted to take a value from xarr whenever the corresponding value in cond is True, and otherwise take the value from yarr. A list comprehension doing this might look like::
```python
In [126]:
xarr = np.array([1.1, 1.2, 1.3, 1.4, 1.5])
yarr = np.array([2.1, 2.2, 2.3, 2.4, 2.5])
cond = np.array([True, False, True, True, False])
result = np.where(cond, xarr, yarr)
result
Out[126]: 
array([1.1, 2.2, 1.3, 1.4, 2.5])
```
The second and third arguments to np.where don’t need to be arrays; one or both of them can be scalars. A typical use of where in data analysis is to produce a new array of values based on another array. Suppose you had a matrix of randomly generated data and you wanted to replace all positive values with 2 and all negative values with –2. This is very easy to do with np.where:
```python
In [127]:
arr = np.random.randn(4, 4)
arr
arr > 0
Out[127]: 
array([[-1.44700794,  0.83268887, -0.67602992, -0.16833106],
       [ 1.64530508, -0.75205006, -0.10626227,  0.06787526],
       [ 0.90069935, -1.07647617, -0.19616563, -1.05209306],
       [ 0.28371622, -1.24829144,  0.55126666, -0.28608807]])
array([[False,  True, False, False],
       [ True, False, False,  True],
       [ True, False, False, False],
       [ True, False,  True, False]])
In [128]:
np.where(arr > 0, 2, -2)
Out[128]: 
array([[-2,  2, -2, -2],
       [ 2, -2, -2,  2],
       [ 2, -2, -2, -2],
       [ 2, -2,  2, -2]])
```
### Mathematical and Statistical Methods
A set of mathematical functions that compute statistics about an entire array or about the data along an axis are accessible as methods of the array class. You can use aggregations (often called reductions) like `sum`, `mean`, and `std` (standard deviation) either by calling the array instance method or using the top-level NumPy function.
Here I generate some normally distributed random data and compute some aggregate statistics:
```python
In [129]:
arr = np.random.randn(5, 4)
arr
Out[129]: 
array([[-1.44700794,  0.83268887, -0.67602992, -0.16833106],
       [ 1.64530508, -0.75205006, -0.10626227,  0.06787526],
       [ 0.90069935, -1.07647617, -0.19616563, -1.05209306],
       [ 0.28371622, -1.24829144,  0.55126666, -0.28608807]])
array([[False,  True, False, False],
       [ True, False, False,  True],
       [ True, False, False, False],
       [ True, False,  True, False]])
In [130]:
np.where(arr > 0, 2, -2)
Out[130]: 
array([[-2,  2, -2, -2],
       [ 2, -2, -2,  2],
       [ 2, -2, -2, -2],
       [ 2, -2,  2, -2]])
In [131]:
arr.mean()
np.mean(arr)
Out[131]: 
-0.10516923223446992
```
**Functions like `mean` and `sum` take an optional axis argument that computes the statistic over the given axis, resulting in an array with one fewer dimension:**
```python
In [132]:
arr.mean(axis=1)#压缩列，对各行求均值，返回m*1矩阵
Out[132]: 
array([-0.52548653, -0.72405978,  0.23357447,  0.44441339,  0.0457123 ])
In [133]:
arr.sum(axis=0)#压缩行，对各列求和，返回1*n矩阵
Out[133]: 
array([-1.44625899,  0.26889354,  2.14307966, -3.06909885])
```
Here, `arr.mean(1)` means “compute mean across the columns” where `arr.sum(0)` means “compute sum down the rows.”
