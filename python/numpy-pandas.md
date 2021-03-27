
## NumPy
### Use NumPy to create arrays with built-in functions inlcuding `np.array()`, `np.arange()`, `np.linspace()` and `np.full()`, `np.zeros()`, `np.ones()`

#### Arrays
- n-dimentional data structures
- can contain all the basic dtypes (best with numeric)
- homogenous
- shape (row, column)
- attributes
  - `np.ndim`
  - `np.shape`
  - `np.size`

#### Creating Arrays
- `np.array()`: from existing data. ex) `np.array([1, 2, 3])`
- `np.arange(start, end-exclusive, stepsize)`: `np.arange(1,5)` -> `array([1, 2, 3, 4])`
- `np.linspace(start, end, n-of-points)`: `np.linspace(0, 10, 5)` -> `array([0., 2.5, 5., 7.5, 10. ])`
- `np.ones((row, col))`: an array of ones with size row x col
- `np.zeros((row, col))`: an array of zeros with size row x col
- `np.full((row, col), num)`: an array of the number `num` with size row x col
  - ex) `np.full((3, 3), 3.14)`
  ```
  array([[3.14, 3.14, 3.14],
       [3.14, 3.14, 3.14],
       [3.14, 3.14, 3.14]])
  ```
- `np.random.rand(5, 2)`: random numbers uniformly distributed from 0 to 1 with size 5 x 2
- else) `np.transpose()`, `np.mean()`, `np.astype(int)`


### Be able to access values from a NumPy array by numeric indexing and slicing and boolean indexing
#### Numeric indexing, slicing
- For 1D array: same as lists.
- For 2D array: x[row, col] (start from **0**!)
```
array([[9, 5, 4, 0, 6, 2],
       [8, 7, 3, 4, 7, 3],
       [4, 4, 6, 3, 1, 0],
       [4, 2, 2, 1, 1, 9]])
```
  - x[3, 4]: 1
  - x[3]: [4, 2, 2, 1, 1, 9]
  - `x.shape`: 4, 6 (row x col)
  - x[2:, :3]:
  ```
    array([[4, 4, 6], # from row 2
          [4, 2, 2]]) # to col 2 (3 exclusive)
  ```
  - x.T (same as x.transpose())

#### Chanching values with indexing
- x[1, 1] = 55555
```
array([[     9,      5,      4,      0,      6,      2],
       [     8, 555555,      3,      4,      7,      3],
       [     4,      4,      6,      3,      1,      0],
       [     4,      2,      2,      1,      1,      9]])
```
- z[0] = 5
```array([5., 0., 0., 0., 0.]) # z = array([0., 0., 0., 0., 0.])```

#### Boolean indexing
```
x = np.random.rand(10)
x

array([0.17699813, 0.92610928, 0.34761085, 0.43033649, 0.57393065,
       0.41272028, 0.65097474, 0.9755686 , 0.10781718, 0.41944245])
```
```
x_thresh = x > 0.5
x_thresh

array([False,  True, False, False,  True, False,  True,  True, False,
       False])
```
```
x[x_thresh] = 0.5  # set all elements  > 0.5 to be equal to 0.5
x

array([0.17699813, 0.5       , 0.34761085, 0.43033649, 0.5       ,
       0.41272028, 0.5       , 0.5       , 0.10781718, 0.41944245])
```


### Perform mathematical operations on and with arrays.
```
x = np.random.rand(10)
x

array([0.17699813, 0.92610928, 0.34761085, 0.43033649, 0.57393065,
       0.41272028, 0.65097474, 0.9755686 , 0.10781718, 0.41944245])
```
```
x + 1

array([1.17699813, 1.92610928, 1.34761085, 1.43033649, 1.57393065,
       1.41272028, 1.65097474, 1.9755686 , 1.10781718, 1.41944245])
```
- If the arrays' sizes are the same, then numpy does operations **elementwise**


### Explain what broadcasting is and how to use it.
#### Boradcasting
- The term broadcasting describes *how numpy treats arrays with different shapes* during arithmetic operations. Subject to certain constraints, the smaller array is “broadcast” across the larger array so that **they have compatible shapes**. Broadcasting provides a means of vectorizing array operations so that looping occurs in C instead of Python. (source: Numpy.org)

#### How to use it
- Numpy does `np.repeat().reshape()` under the hood if the arrays are compatible.
![img](https://pages.github.ubc.ca/MDS-2020-21/DSCI_511_py-prog_students/_images/pies_broadcast.png)
```
cost = np.repeat(cost, 3).reshape((3, 3))
cost

array([[20, 20, 20],
       [15, 15, 15],
       [25, 25, 25]])
```
#### Comatible arrays
- Numpy compares arrays **element-wise**. It starts with the **trailing** dimensions, and works its way forward.
- Dimensions are compatible if:
  - they are equal, or
  - one of them is 1.
- Testing array compatibility
```  
a = np.ones((3, 2))
b = np.ones((3, 2, 1))
print(f"The shape of a is: {a.shape}")
print(f"The shape of b is: {b.shape}")
print("")
try:
    print(f"The shape of a + b is: {(a + b).shape}")
except:
    print(f"ERROR: arrays are NOT broadcast compatible!")
```

- `np.arange(3). reshape((3, 1))+np.arange(3)`
![](https://pages.github.ubc.ca/MDS-2020-21/DSCI_511_py-prog_students/_images/broadcasting.png)


### Reshape arrays by adding/removing/reshaping axes with `.reshape()`, `np.newaxis()`, `.ravel()`, `.flatten()`
#### `.reshape()`
```
x = np.full((4, 3), 3.14)
x

array([[3.14, 3.14, 3.14],
       [3.14, 3.14, 3.14],
       [3.14, 3.14, 3.14],
       [3.14, 3.14, 3.14]])
```

```
x.reshape(6, 2)

array([[3.14, 3.14],
       [3.14, 3.14],
       [3.14, 3.14],
       [3.14, 3.14],
       [3.14, 3.14],
       [3.14, 3.14]])
```

```
x.reshape(2, -1)  # using -1 will calculate the dimension for you (if possible)

array([[3.14, 3.14, 3.14, 3.14, 3.14, 3.14],
       [3.14, 3.14, 3.14, 3.14, 3.14, 3.14]])
```

#### `np.newaxis`
- Adding dimensions to an array for broadcasting purposes
- `None` = `np.newaxis`
```
a = np.ones(3) # 1 dimension, (3, ) shape, size 3

a
[1. 1. 1.]

a[:, np.newaxis] # = a[:, None] - 2 dimentions, (3, 1) shape, size 3
[[1.]
 [1.]
 [1.]]
```

#### `.ravel()/.flatten()`
- Flatten arrays to a single dimension
```
x
array([[3.14, 3.14, 3.14],
       [3.14, 3.14, 3.14],
       [3.14, 3.14, 3.14],
       [3.14, 3.14, 3.14]])
       

x.flatten()
Dimensions: 1
     Shape: (12,)
      Size: 12

[3.14 3.14 3.14 3.14 3.14 3.14 3.14 3.14 3.14 3.14 3.14 3.14]
```


### Understand how to use built-in NumPy functions like `np.sum()`, `np.mean()`, `np.log()` as stand alone functions or as methods of numpy arrays (when available)
#### Numpy methods
- `np.sum()`
- `np.mean()`
- `np.log()`
- `np.sqrt()`
- `np.power()`

#### Vectorized operation
```
sides = np.array([3, 4])
(sides ** 2).sum() ** 0.5
> 5.0
```
- “vectorization” in NumPy refers to the use of optmized C code to perform an operation
- Because numpy arrays are homogenous (contain the same dtype), we don’t need to check that we can perform an operation on elements of a sequence before we do the operation which results in a huge speed-up


## Pandas
### Create Pandas series with `pd.Series()` and Pandas dataframe with `pd.DataFrame()`

#### Series
- A Series is like a Numpy array but with labels (indexes)
- Strickly 1-D
- Can contain any data type, including a mix of them (cf. arrays)
- Series labels may be integers strings
- Can be created from a scalar, a list, ndarray or dictionary using `pd.Series()`

```examples
pd.Series(data = [-5, 1.3, 21, 6, 3],
          index = ['a', 'b', 'c', 'd', 'e'])
          
pd.Series(data = {'a': 10, 'b': 20, 'c': 30})

pd.Series(data = np.random.randn(3)) # from an ndarray

pd.Series(3.141)

pd.Series(data=3.141, index=['a', 'b', 'c'])
```
- `.index`: access the index labels
- `.to_numpy()`: access the underlying data array
```
s.to_numpy()
array([ 0.19123343,  0.45933683, -1.59095764,  0.65270509, -0.38267213])
```

#### DataFrames
- What are DataFrames?
  - A 2-dimensional labeled data structure with columns of potentially different types
  - Like a spreadsheet or SQL table, or a dict of Series objects.
- Accepts many different kinds of input:
  - Dict of 1D ndarrays, lists, dicts, or Series
  - 2-D numpy.ndarray
  - Structured or record ndarray
  - A Series
  - Another DataFrame

- Creating DataFrames
```
pd.DataFrame([[1, 2, 3],
              [4, 5, 6],
              [7, 8, 9]],
             index = ["R1", "R2", "R3"],
             columns = ["C1", "C2", "C3"])
```
```
pd.DataFrame({"C1": [1, 2, 3],
              "C2": ['A', 'B', 'C']},
             index=["R1", "R2", "R3"])
```
```
pd.DataFrame(np.random.randn(5, 5),
             index=[f"row_{_}" for _ in range(1, 6)], # list comprehension
             columns=[f"col_{_}" for _ in range(1, 6)])
```
```
pd.DataFrame(zip(['Tom', 'Mike', 'Tiffany'], [7, 15, 3])) # list of tuples 
```

### Be able to access values from a Series/DataFrame by indexing, slicing and boolean indexing using notation such as `df[]`, `df.loc[]`, `df.iloc[]`, `df.query[]`

#### Indexing and Slicing Series
- similar to ndarrays
```
s = pd.Series([1, [1, 2, 3], 'a'])

s
0            1
1    [1, 2, 3]
2            a
dtype: object
```
```
s[0] >> 1  ## return int
s[[0]].    ## return Series
0    1
dtype: object
```
- s["A":"C"], s[["B", "D", "C"]] (dictionary style)
- allows for non-unique indexing, but won't return unique values
- Boolean indexing
```
x = pd.Series({'A':1, 'B':1, 'C':2, 'D':3, 'E':4})
x
A    1
B    1
C    2
D    3
E    4
dtype: int64
```
```
x[x > x.mean()] # x.mean() = 2.2
D    3
E    4
dtype: int64
```

#### Indexing and Slicing DataFrames
##### `[]`
- Difference between `[]` and `[[]]`
  - `[]` returns a series
  - `[[]]` returns a dataframe
- You can index rows by using slices, not single values but not recommended.

##### `.loc[]`
- labels are references to rows/columns
- df.loc[:, 'Name'] : returns a series
- df.loc[:, 'Name':'Language'] : returns a df
- df.loc[df.index[0], 'Courses'] : reference the first row and the column names 'Courses'
- df.loc[2, df.columns[1]]  : reference row "2" and the second column (df.loc[2, 1] returns an error..)

##### `.iloc[]`
- integers as references to rows/columns
- df.iloc[0] : returns a series
- df.iloc[0:2] : returns a dataframe
- df.iloc[2, 1] : returns the indexed object
```
df.iloc[2, 1]
'R'
```
- df.iloc[[0, 1], [1, 2]] : returns a dataframe

##### Boolean indexing
- df[df['Courses'] > 5]
- df[df['Name'] == "Tom"]

##### `.query()`
- accepts a string expression to evaluate and it knows the names of the cols in your df
```
df.query("Courses > 4 & Language == 'Python'")
```
- To reference variable in the current workspace using the `@` symbol
```
course_threshold = 4
df.query("Courses > @course_threshold")
```

#### Indexing cheatsheet
| Method                               | Syntax                                 | Output                                                                      |
|--------------------------------------|----------------------------------------|-----------------------------------------------------------------------------|
| Select column                        | df[col_label]                          | Series                                                                      |
| Select row slice                     | df[row_1_int:row_2_int]                | DataFrame                                                                   |
| Select row/column by label           | df.loc[row_label(s), col_label(s)]     | Object for single selection, Series for one row/column, otherwise DataFrame |
| Select row/column by integer         | df.iloc[row_int(s), col_int(s)]        | Object for single selection, Series for one row/column, otherwise DataFrame |
| Select by row integer & column label | df.loc[df.index[row_int], col_label]   | Object for single selection, Series for one row/column, otherwise DataFrame |
| Select by row label & column integer | df.loc[row_label, df.columns[col_int]] | Object for single selection, Series for one row/column, otherwise DataFrame |
| Select by boolean                    | df[bool_vec]                           | Object for single selection, Series for one row/column, otherwise DataFrame |
| Select by boolean expression         | df.query("expression")                 | Object for single selection, Series for one row/column, otherwise DataFrame |


### Perform basic arithmetic operations between two series and anticipate the result.

#### Series Operations
- Unlike ndarrays operations, between Series (+, -, /, \*) align values based on their **labels** (not the position - like ndarrays, element-wise)
![](https://pages.github.ubc.ca/MDS-2020-21/DSCI_511_py-prog_students/_images/series_addition.png)

- x ** 2
- np.exp(x)
- x.mean(), x.sum(), x.astyle(float)
- Chaining: x.add(1).pow(2).mean().astyle(int)

### Describe how Pandas assigns dtypes to Series and what the object dtype is
#### dtype "object"
- Series of strings or mixed data
- Higher memory requirements; stores information about its indiviual dtype
- Try to use uniform dtypes as much as possible

#### NaN
- missing value
- is treated as a float: a series of integers and one missing value --> dtype: float64


### Read a standard `.csv` file from a local path or url using Pandas `pd.read_csv()`. 
#### `pd.read_csv()`
```
path = 'data/cycling_data.csv'
df = pd.read_csv(path, index_col=0, parse_dates=True)
df
```
- index_col = {int} : set the {int} column to index
- delimiter = 'delim' : specify the separator
- accepts url

#### Common DF operations
- `df.min()`
- `df.idxmin()`
- `df.sum()`
- `df.mean()`
- `df.sort_values(by = 'col', ascending= True/False)`
  - `df.sort_index(ascending=True/False)`

### Explain the relationship and differences between np.ndarray, pd.Series and pd.DataFrame objects in Python.
- Numpy is typically faster/uses less memory than pandas
- Not all Python packages are compatible with numpy & pandas
- For pandas, the ability to add labels to data can be useful (e.g., for time series)
- Numpy and pandas have different built-in functions available
- How to go from: ndarray (np.array()) -> series (pd.series()) (with labels!) -> dataframe (pd.DataFrame()) (many series!)
- Remember that we can also go the other way: dataframe/series -> ndarray using df.to_numpy()


### Inspect a dataframe with `df.head()`, `df.tail()`, `df.info()`, `df.describe()`.
#### cheatsheet
| method          | description                                                                          | example                      |
|-----------------|--------------------------------------------------------------------------------------|------------------------------|
| `df.head()`     | allows you to view the top n (default 5) rows of a df                                | `df.head(10)`                |
| `df.tail()`     | allows you to view the bottom n (default 5) rows of a df                             | `df.tail(6)`                 |
| `df.info()`     | prints information about the df itself (dtypes, memory usages, non-null values, etc) | `df.info()`                  |
| `df.describe()` | provides summary statistics(count, mean, std, ..) of the values within a df          | `df.describd(include='all'`) |

### Obtain dataframe summaries with `df.info()` and `df.describe()`.
#### `df.info()`
```python
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 33 entries, 0 to 32
Data columns (total 6 columns):
 #   Column    Non-Null Count  Dtype  
---  ------    --------------  -----  
 0   Date      33 non-null     object 
 1   Name      33 non-null     object 
 2   Type      33 non-null     object 
 3   Time      33 non-null     int64  
 4   Distance  31 non-null     float64
 5   Comments  33 non-null     object 
dtypes: float64(1), int64(1), object(4)
memory usage: 1.7+ KB
```

#### `df.dscribe()`
```python
	          Time     Distance
count	33.000000	    31.000000
mean	3512.787879	  12.667419
std	  8003.309233	  0.428618	
min	  1712.000000	  11.790000
25%	  1863.000000	  12.480000
50%	  2118.000000	  12.620000
75%	  2285.000000	  12.750000
max	  48062.000000	14.570000
```

- Default : numeric features
- `include = 'all'` : force it to give summaries on all features


### Manipulate how a dataframe displays in Jupyter by modifying Pandas configuration options such as `pd.set_option("display.max_rows", n)`.

- If a dataframe has more than 60 rows, Pandas will only display the first 5 and last 5 rows.

#### `pd.set_option()`
```python
pd.set_option("display.max_rows", 20)
df
```
- Anything with more than 20 rows will be summarised by the first and last 5 rows as before

#### Style your tables
```python
test = pd.DataFrame(np.random.randn(5, 5),
                    index = [f"row_{_}" for _ in range(5)],
                    columns = [f"feature_{_}" for _ in range(5)])
test.style.background_gradient(cmap='plasma')
```

### Rename columns of a dataframe using the `df.rename()` function or by accessing the df.columns attribute.

#### `df.rename()`
```python
df.rename(columns={"Date": "Datetime",
                   "Comments": "Notes",
                   inplace=True})         # using inplace argument

df = df.rename(columns={"Date": "Datetime",
                        "Comments": "Notes"})   # using reassignment
```

#### `.columns` attribute
- If you wish to change all of the columns of a dataframe
```python
df.columns = [f"Column {_}" for _ in range(1, 7)]
```

#### Adding/Removing Columns
- use `[]` to add columns
```python
df['Rider'] = 'Tom Beuzen'
df['Avg Speed'] = df['Distance'] * 1000 / df['Time']  # avg. speed in m/s
df
```
- use `df.drop(columns=[])` to drop columns
```python
df = df.drop(columns=['Rider', 'Avg Speed'])
df
```

#### Adding/Removing Rows
- use `.append()` to add rows
```python
another_row = pd.DataFrame([["12 Oct 2019, 00:10:57", "Morning Ride", "Ride",
                             2331, 12.67, "Washed and oiled bike last night"]],
                           columns = df.columns,
                           index = [33])
df = df.append(another_row)
df
```

- use `df.drop(index=range(start,end))` to drop rows
```python
df.drop(index=range(30, 34))
```

### Modify the index name and index values of a dataframe using `.set_index()`, `.reset_index()` , `df.index.name`, `.index.`
#### `.set_index()` and `df.index.name`
```python
df = df.set_index("Column 1")
df.index.name = "New Index"
df
```
- Set column 1 as the index, and change the name to "New Index"

#### `.reset_index()`
```python
df = df.reset_index()
df
```
- Send the index back to a column and have a defaut integer index

#### `.index`
- Like with column names, we can also modify the index directly.
```python
df.index = range(100, 133, 1)
df
```

### Use `df.melt()` and `df.pivot()` to reshape dataframes, specifically to make tidy dataframes.
#### Tidy data
1. Each variable forms a column
2. Each observation forms a row
3. Each type of observational unit forms a table

#### Melt and Pivot
![](https://pages.github.ubc.ca/MDS-2020-21/DSCI_511_py-prog_students/_images/melt_pivot.gif)

##### `.melt()`
- `pivot_longer` in R
```python
df_melt = df.melt(id_vars="Name",         # identifier
                  var_name="Year",        # variables -> new column "Year"
                  value_name="Courses")   # values -> new column "Courses"
df_melt
```
- You can specify `value_vars` argument to select which specific variables we want to melt
```python
df.melt(id_vars="Name",
        value_vars=["2019", "2020"],
        var_name="Year",
        value_name="Courses")
```        

##### `.pivot()`
- `pivot_wider` in R
```python
df_pivot = df_melt.pivot(index="Name",        # set "Name" column as index
                         columns="Year",      # set values in "Year" as columns
                         values="Courses")    # set values in "Courses" as values for each column
df_pivot
```

### Combine dataframes using `df.merge()` and `pd.concat()` and know when to use these different methods.

#### `pd.concat()`
```python
pd.concat((df1, df2), axis=0, ignore_index=True)    ## axis=0 : a vertical stick
pd.concat((df1, df2), axis=1, ignore_index=True)    ## axis=1 : a horizontal stick
pd.concat(concat_list, axis=1, join="inner")        ## list as a data argument, specify the join method
```
- you can concatenate as many as you want

#### `pd.merge()`
##### inner
```python
pd.merge(df1, df2, how="inner", on="publisher")
```
![](https://pages.github.ubc.ca/MDS-2020-21/DSCI_511_py-prog_students/_images/inner_join.png)

##### outer
```python
pd.merge(df1, df2, how="outer", on="publisher")
```
![](https://pages.github.ubc.ca/MDS-2020-21/DSCI_511_py-prog_students/_images/outer_join.png)

##### left
```python
pd.merge(df1, df2, how="left", on="publisher")
```
![](https://pages.github.ubc.ca/MDS-2020-21/DSCI_511_py-prog_students/_images/left_join.png)

##### indicator
```python
pd.merge(df1, df2, how="outer", on="publisher", indicator=True)
```
- the `indicator` argument will add a column to the result telling you where matches were found in the dataframes

### Apply functions to a dataframe `df.apply()` and `df.applymap()`
| method                        | description                                                                                                         |
|-------------------------------|---------------------------------------------------------------------------------------------------------------------|
| `df.apply()`                  | applies a function column-wise or row-wise across a dataframe (the function must be able to accept/return an array) |
| `df.applymap()`               | applies a function element-wise (for functions that accept/return single values at a time)                          |
| `series.apply()/series.map()` | same as above but for Pandas series                                                                                 |

#### `df.apply()`
- You can use functions that you define, even that require additional arguments.
```python
def convert_seconds(x, to="hours"):
    if to == "hours":
        return x / 3600
    elif to == "minutes":
        return x / 60

df[['Time']].apply(convert_seconds, to="minutes")
```

#### `df.applymap()`
- for functions only accept/return a scalar
```python
df[['Time']].applymap(int)
```
- `.astype` might be better... 


### Perform grouping and aggregating operations using `df.groupby()` and `df.agg()`.
### Perform aggregating methods on grouped or ungrouped objects such as finding the minimum, maximum and sum of values in a dataframe using `df.agg()`.

#### `df.groupby()`
```python
dfg = df.groupby(by='Name')
dfg
```
![](https://pages.github.ubc.ca/MDS-2020-21/DSCI_511_py-prog_students/_images/groupby_1.png)

##### `.get_group()`
```python
dfg.get_group('Afternoon Ride')
```

##### Apply aggregate functions to the groupby object
![](https://pages.github.ubc.ca/MDS-2020-21/DSCI_511_py-prog_students/_images/groupby_2.png)
```python
dfg.mean()
```

###### `df.aggregate()`
```python
dfg.aggregate(['mean', 'sum', 'count'])
```
- apply multiple function
- even apply different functions to different columns
- you can use aggregate for non-grouped dataframes too

```python
def num_range(x):
    return x.max() - x.min()

dfg.aggregate({"Time": ['max', 'min', 'mean', num_range], 
               "Distance": ['sum']})
```

```python
# ungrouped df
df.agg(['mean', 'min', 'count', num_range])
```

### Remove or fill missing values in a dataframe with `df.dropna()` and `df.fillna()`.
#### `df.dropna()`
- drop missing values

#### `df.fillna()`
```python
df.fillna(0)  # fill with 0
df.fillna(df.mean())  # fill with the mean
df.fillna(method='bfill')  # backward (upwards) fill from non-nan values
df.fillna(method='ffill')  # forward (downward) fill from non-nan values
```

### Manipulate strings in Pandas by accessing methods from the `Series.str` attribute.
#### `.str` atrribute
- String methods can be accessed by the `.str` attribute of Pandas Series and Index objects
- Pretty much all built-in string operations (`.upper()`, `.lower()`, `.split()`, etc) and more are available
```python
s = pd.Series(x)
s
0        Tom
1       Mike
2       None
3    Tiffany
4       Joel
5     Varada
dtype: object

s.str.upper()
0        TOM
1       MIKE
2       None
3    TIFFANY
4       JOEL
5     VARADA
dtype: object

s.str.split("ff", expand=True)
	0	1
0	Tom	None
1	Mike	None
2	None	None
3	Ti	any
4	Joel	None
5	Varada	None

s.str.len()
0    3.0
1    4.0
2    NaN
3    7.0
4    4.0
5    6.0
dtype: float64
```

- `.str` operation on column names and index objects
```python
df.columns = df.columns.str.capitalize().str.replace("feature", "").str.strip()
df.index = df.index.str.lower().str.replace("w", "w_")
```

[More string method here](https://pages.github.ubc.ca/MDS-2020-21/DSCI_511_py-prog_students/lectures/lecture8-wrangling-advanced.html#string-methods)

### Understand how to use regular expressions in Pandas for wrangling strings.
- Use RegEx with str.methods
```python
s.str.findall(r'^[^AEIOU].*[^aeiou]$')

0        [Tom]
1           []
2         None
3    [Tiffany]
4       [Joel]
5           []
dtype: object

df.loc[df['Comments'].str.contains(r"[Rr]ain")]

df['Comments'].str.split(r"[.,!]", expand=True)		## split strings and separate them into new columns
```

### Differentiate between datetime object in Pandas such as `Timestamp, Timedelta, Period, DateOffset`.
- Timestamp (like np.datetime64)
- Timedelta (like np.timedelta64)
  - adding days, weeks, ...
  ```python
  date + timedelta(days=7)
  ```
- Period (custom object for regular ranges of datetimes)
```python
dates = np.arange("2020-07", "2020-12", dtype='datetime64[M]')
dates
```
- DateOffset (custom object like timedelta but factoring in calendar rules)


### Create these datetime objects with functions like `pd.Timestamp()`, `pd.Period()`, `pd.date_range()`, `pd.period_range()`.
#### `pd.Timestamp()`
- create a single point in time
```python
print(pd.Timestamp('2005-07-09'))  # parsed from string
print(pd.Timestamp(year=2005, month=7, day=9))  # pass data directly
print(pd.Timestamp(datetime(year=2005, month=7, day=9)))  # from datetime object
```

#### `pd.Period()`
- to speciffy a span of time
```python
span = pd.Period('2005-07-09')
print(span)
print(span.start_time)
print(span.end_time)

2005-07-09
2005-07-09 00:00:00
2005-07-09 23:59:59.999999999
```

#### `pd.date_range()`, `pd.period_range()`
- create arrays of datetimes
```python
pd.date_range('2020-09-01 12:00',
              '2020-09-11 12:00',
              freq='D')
DatetimeIndex(['2020-09-01 12:00:00', '2020-09-02 12:00:00',
               '2020-09-03 12:00:00', '2020-09-04 12:00:00',
               '2020-09-05 12:00:00', '2020-09-06 12:00:00',
               '2020-09-07 12:00:00', '2020-09-08 12:00:00',
               '2020-09-09 12:00:00', '2020-09-10 12:00:00',
               '2020-09-11 12:00:00'],
              dtype='datetime64[ns]', freq='D')

pd.period_range('2020-09-01',
                '2020-09-11',
                freq='D')
PeriodIndex(['2020-09-01', '2020-09-02', '2020-09-03', '2020-09-04',
             '2020-09-05', '2020-09-06', '2020-09-07', '2020-09-08',
             '2020-09-09', '2020-09-10', '2020-09-11'],
            dtype='period[D]', freq='D')			      
```

#### `pd.to_datetime()` 
- convert an array of dates as strings to datetime
```python
string_dates = ['July 9, 2020', 'August 1, 2020', 'August 28, 2020']
pd.to_datetime(string_dates)
DatetimeIndex(['2020-07-09', '2020-08-01', '2020-08-28'], dtype='datetime64[ns]', freq=None)
```

#### `parse_date` argument
- in `pd.read_csv()`, use `parse_dates=True` to parse an array of strings to a datatime object

### Index a datetime index with partial string indexing.
#### Partial string indexing
```python
df.loc['2019-09']
```

#### Exact matching
```python
df.loc['2019-10-10']
```

#### Slicing
```python
df.loc['2019-10-01':'2019-10-13']
```

#### `df.query()` and `df.between_time()`
```python
df.query("'2019-10-10'")
df.between_time('00:00', '01:00')
```

### Perform basic datetime operations like splitting a datetime into constituent parts (e.g., year, weekday, second, etc), apply offsets, change timezones, and resample with `.resample()`.

#### Basic datetime operations
```python
df.index.year
df.index.second
df.index.weekday
df.index.day_name()
df.index.month_name()
```

#### Offsets
- `pd.Dateoffset` : can apply some cases where time is not regular
```python
t1 = pd.Timestamp('2020-03-07 12:00:00', tz='Canada/Pacific')
t2 = t1 + pd.Timedelta("1 day")
print(f"Original time: {t1}")
print(f" Plus one day: {t2}")  # note that time has moved from 12:00 -> 13:00

Original time: 2020-03-07 12:00:00-08:00
 Plus one day: 2020-03-08 13:00:00-07:00

t3 = t1 + pd.DateOffset(days=1)
print(f"Original time: {t1}")
print(f" Plus one day: {t3}")  # note that time has stayed at 12:00

Original time: 2020-03-07 12:00:00-08:00
 Plus one day: 2020-03-08 12:00:00-07:00
```
#### Timezones
```python
print(f"        No timezone: {pd.Timestamp('2020-03-07 12:00:00').tz}")
print(f"             tz arg: {pd.Timestamp('2020-03-07 12:00:00', tz='Canada/Pacific').tz}")
print(f".tz_localize method: {pd.Timestamp('2020-03-07 12:00:00').tz_localize('Canada/Pacific').tz}")
```

- Converting timezone
```python
df.index = df.index.tz_localize("Canada/Pacific")  # first specify the current timezone
df.index = df.index.tz_convert("Australia/Sydney")  # then convert to the proper timezone
df
```

#### Resampling and Aggregating
- `.resample()`
  - resampling the time series to a coarser/finer/regular resolution
  - For example, you may want to resample daily data to weekly data
  ```python
  df.resample("1D")	## similar to .groupby()
  df.resample("1M").groups
  df.resample("1D").mean()
  ```

### Make basic plots in Pandas by accessing the `.plot `attribute or importing functions from `pandas.plotting`.
#### `.plot`
```python
df['Distance'].plot.line();
df['Distance'].cumsum().plot.line();
df['Distance'].plot.density();
```
#### Pandas Plotting
```python
from pandas.plotting import scatter_matrix
scatter_matrix(df);
```

# Practice Questions

## Question 1

In your own words, briefly explain key differences between a NumPy ndarray, a Pandas Series and a Pandas Dataframe (< 3 sentences).


## Question 2

What does term "broadcasting" refer to in NumPy (< 2 sentences)?


## Question 3

Briefly describe the difference between `.apply()` and `.applymap()` in Pandas (< 2 sentences)?


## Question 4

When loading data using the `pd.read_csv()` function, what parameter is used to:

1. specify which row contains the column names?

2. select a subset of columns to load?

3. automatically parse dates to datetime format?


The above are the most obvious answers, but other answers my be accepted too.

## Question 5

What functions in the R tidyverse are equivalent to `.melt()` and `.pivot()` in Pandas?
