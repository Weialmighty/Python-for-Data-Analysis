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
