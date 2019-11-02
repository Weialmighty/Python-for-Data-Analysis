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
*Since the project’s inception in 2010, scikit-learn has become the premier generalpurpose machine learning toolkit for Python programmers.* In just seven years, it has had over 1,500 contributors from around the world. It includes submodules for such models as:# **CHAPTER1 Preliminaries**
## **1.1 What Is This Book About?**
### **What Kinds of Data?**
*structured data*
## **1.2 Why Python for Data Analysis?**
### **Solving the "Two-Language" Problem**
### **Why Not Python?**
## **1.3 Essential Python Libraries**
### **Numpy**
*short for Numerical Python, has long been a cornerstone of numerical computing in Python.* It provides the data structures, algorithms, and library glue needed for most scientific applications involving numerical data in Python. NumPy contains, among other things.
### **pandas**
*pandas provides high-level data structures and functions designed to make working with structured or tabular data fast, easy, and expressive.* Since its emergence in 2010, it has helped enable Python to be a powerful and productive data analysis environment. The primary objects in pandas that will be used in this book are the DataFrame, a tabular, column-oriented data structure with both row and column labels, and the Series, a one-dimensional labeled array object.
### **matplotlib**
*matplotlib is the most popular Python library for producing plots and other two-dimensional data visualizations.* It was originally created by John D. Hunter and is now maintained by a large team of developers. It is designed for creating plots suitable for publication. While there are other visualization libraries available to Python programmers, matplotlib is the most widely used and as such has generally good integration with the rest of the ecosystem. I think it is a safe choice as a default visualization
tool.
### **SciPy**
*SciPy is a collection of packages addressing a number of different standard problem domains in scientific computing.*
### **scikit-learn**
*Since the project’s inception in 2010, scikit-learn has become the premier general purpose machine learning toolkit for Python programmers.* In just seven years, it has had over 1,500 contributors from around the world. It includes submodules for such models as:
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
statsmodels is more focused on statistical inference, providing uncertainty estimates and p-values for parameters. scikit-learn, by contrast, is more prediction-focused.
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
