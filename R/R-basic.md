## R basic 

### Learning Objectives
- Choose and use the appropriate readr::read_* function and function arguments to load a given rectangular, plain text data set into R
    - read_csv
    - read_delim("data/can_lang.tsv", delim = "\t", col_names = FALSE)
    - or.. col_names = c('col', 'names')
    - read_excel
    - read_tsv

- Use the assignment symbol, <-, to assign values to objects in R
- Write a dataframe to a .csv file using readr::write_csv
    - write_csv(data_frame, "folder/file_name.csv")
- Use readr::read_csv to bring data from standard comma separated value (.csv) files into R
- Recall and use the following dplyr functions and operators for their intended data wrangling tasks:
    - select
    - filter
    - mutate
    - arrange
    - desc
    - slice (select row) (getting a single value)
    ```
    gapminder %>% 
    arrange(desc(lifeExp)) %>% 
    slice(1) %>% 
    pull(lifeExp)
    ```
    - pull
    - %in%
    `filter(gapminder, country %in% c("Mexico", "United States", "Canada"))`
- Use the pipe operator, %>%, to combine two or more functions
- Define the term “tidy data”
    - each row = observation
    - each column = variable (or attribute)
    - each value is a single cell
    - column header should be variable names(grouping together), not values.
- Discuss the advantages and disadvantages of the tidy data format
    - Tidy data is particularly well suited for vectorised programming languages like R, because the layout ensures that values of different variables from the same observation are always paired. 
    - The big problem with this definition is the last word: visualize. Tidy data is extremely difficult to visualize. More traditional table formats are often superior.
- Use tidyr::pivot_wider & tidyr::pivot_longer in R to make untidy data tidy
    - `pivot_longer(col_start:col_end, names_to = "new_column_for_names", values_to = "new_column_for_values")`
    - `pivot_wider(names_from = 'col_name_you_want_to_make_name', values_from = 'col_name_you_want_to_make_value')`

### Learning Objectives
- Explain how the assignment symbol, <- differs from = in R
- Create in R, and define and differentiate in English, the below listed key datatypes in R:
    - logical, numeric, character and factor vectors
    - lists
    - data frames and tibbles: data frames have rows as observations, columns as variables, which are vectors (subtype of lists)
- Use R to determine the type and structure of an object
    - character → double → integer → logical (hierarchy for coercion)
- Explain the distinction between names and values, and when R will copy an object.
    - copy-on-modify: when you modify a vector, it copies the original vector and then modify the vector. Vectors are essentially immutable. If you change one element of the vector, you have to copy the whole thing to update it
    - However, if an object has a single name bound to it, R will modify it in place: `v <- c(1,2,3) then v[[3]] <- 4`

- Use the three subsetting operators, `[[`, `[`, and `$`, to subset single and multiple elements from vectors and data frames, lists and matrices

| Operator | Example use       | What it returns                                                        |
|----------|-------------------|------------------------------------------------------------------------|
| [        | mtcars[1:10, 2:4] | rows 1-10 for columns 2-4 of the data frame, as a data frame           |
| [        | mtcars[1:10, ]    | rows 1-10 for all columns of the data frame, as a data frame           |
| [        | mtcars[1]         | the first column of the data frame, as a data frame                    |
| [[       | mtcars[[1]]       | the first column of the data frame, as a vector                        |
| $        | mtcars$cyl        | the column the corresponds to the name that follows the $, as a vector |

- Compute numeric and boolean values using their respective types and operations

### Learning Objectives
- Tibble vs Data frame
    - In RStudio, tibbles only output the first 10 rows
    - When you numerically subset a data frame to 1 column, you get a vector. However, when you numerically subset a tibble you still get a tibble back.
- Manipulate dates and times using the `{lubridate}` package
    - today()
    - now()
    - ymd, mdy, dmy, .. ()
    - make_date, make_datetime
- Be able to modify strings in a data frame using regular expressions and the `{stringr}` package
    - If you want to operate on data frames, you will need to use these functions inside of data frame/tibble functions, like filter and mutate.
    - str_detect(vector, "pattern")
    - str_subset(vector, "pattern") : keep only the matching elements (cf. str_sub: substrings)
    - str_split(my_fruit, " ") : split string on a delimiter (returns list)
    - str_c : collapse
    - str_replace(data, "A", "B") : Replace "A" to "B"
- Cast categorical columns in a data frame as factors when appropriate, and manipulate factor levels as needed in preparation for data visualisation and statistical analysis (using base R and `{forcats}` package functions)
    - as.factor(dataframe$column), factor()
    - nlevels()
    - fct_drop()
    - levels()
    - fct_infreq()
    - fct_rev()
    - fct_reorder(gapminder$country, gapminder$lifeExp) : order country by lifeExp
    - fct_relevel("Asia", "Africa")

### Learning Objectives
- Compare and contrast mutating joins, filtering joins and set operations
    - inner: return all rows from x where there are matching values in y, and all columns from x and y. All combinations are retruned.
    - semi: return all rows from x where there are matching values in y, keeping just columns from x. No duplicate.
    - left: return all rows from x(even though there is no matching with y), and all columns from x and y. All combinations, missing values are returned.
    - anti: return all rows from x where there are not matching values in y, keeping just columns from x.
    - full: return all rows and all columns from both x and y. Not matching values -> NA. 
- Choose the appropriate two table `dplyr` function based on the type of join desired between two data frames, and use it in R to obtained the desired data frame from joining two tables
- Write conditional statements in R with `if`, `else if` and `else` to run different code depending on the input
- Write for loops in R to repeatedly run code
    ```
    for (i in seq_along(sequence)){
        print(sequence[i]^2)
    }

    if (a > b) {
        print("Something")
    } else if (a < b) {
        print("Something else")
    } else {
        print("Another thing!")
    }
    ```




## {dplyr}
### Explain what a grouped data frame is, and how it can be used

- Most data operations are done on groups defined by variables.
- `group_by()` takes an existing tbl and converts it into a grouped tbl where operations are performed "by group". ungroup() removes grouping. (source: dplyr)
- `group_by()` interates over groups of rows (similar to `for` loops)

### Use {dplyr}’s `group_by` + `summarize` to perform the split-apply-combine approach in R to iterate over and summarize data by groups

#### `group_by` + `summarise`

- useful when you want to do somethign repeatedly to a group of rows
- example, we want to calculate the avg. life expectancy for each continent from the gapminder data set

```r
# calculate the average life expectancy for each continent
gapminder %>%
    group_by(continent) %>%
    summarise(mean_life_exp = mean(lifeExp))
```

- we can also `group_by` multiple columns

```r
# calculate the mean life expectancy for each continent and each year
gapminder %>%
    group_by(continent, year) %>%
    summarise(mean_life_exp = mean(lifeExp))
```

#### Grouped mutate

- Sometimes you don’t want to collapse the n rows for each group into one row. You want to keep your groups, but compute within them.
- Let’s make a new variable that is the years of life expectancy gained (lost) relative to 1952, for each individual country. We group by country and use mutate to make a new variable. The first function extracts the first value from a vector. Notice that first is operating on the vector of life expectancies within each country group.

```r
# calculate life expectancy gained (or lost) relative to 1952
gapminder %>%
    group_by(country) %>%
    mutate(life_exp_gain = lifeExp - first(lifeExp)) %>%
    head()
```

### Identify missing and erroneous values and manage them by removing (via {dplyr}’s `filter`) or replacing (using {dplyr}’s `mutate + case_when`)

#### `mutate + case_when`

- Selectively change values in a data frmae (`if` statements)

```r
french_contients <- gap %>%
  mutate(continent = case_when(#continent == "Asia" ~ "Asie",
                               continent == "Europe" ~ "L'Europe",
                               continent == "Africa" ~ "Afrique",
                               continent == "Americas" ~ "les amériques",
                               continent == "Oceania" ~ "Océanie",
                               TRUE ~ continent))         ## Tell R to not put in NA's in the non-specified class
head(french_contients)
```

#### `drop_na()`

- removing all rows where there is a NA in any column

### `drop_na(df, x)`

- removing all rows where the column x has NAs

#### `na.rm = TRUE`

- By default, summary statistics functions in R return NA if there are any NA observations in the data… This is often not ideal, what we’d like would be for R to just ignore the NA’s and kindly return us the statistic we asked for. We can specify this by setting the na.rm argument to TRUE.

```r
penguins %>%
    group_by(species) %>%
    summarise(max_bill_length = max(bill_length_mm, na.rm = TRUE))
```

### Identify where in R code a commonly used functional, a {purrr}’s `map*` function, could be used in place of for loops and write code to do this

#### Functionals

- A functional is a function that takes a function as an input and returns a vector as output.
- Common use is as an alternative to for loops
- For loops are actually quite effective for iteration, and efficient when used, however it is easy to make mistakes when setting them up as you have to:
  - pre-allocate space for the output
  - iterate over the thing the right amount of times
  - properly use the iteration index

#### `map_*` functions

- alternative to `for` loops that help you make less syntax errors and much faster!
![](https://d33wubrfki0l68.cloudfront.net/12f6af8404d9723dff9cc665028a35f07759299d/d0d9a/diagrams/functionals/map-list.png)

##### `map()`

- Return a list

```r
library(purrr)
map(mtcars, median)
```

##### `map_dbl()`

- Return a vector of type double

```r
map_dbl(mtcars, median)
```

#### `map_df()`

- Return a tibble

```r
map_df(mtcars, median)
```

#### Ignoring NA values

```r
map_dbl(mtcars_NA, median, na.rm = TRUE)
```

### Mutate vs Summarise

Mutate won't reduced number of rows while summarise will did and return number of groups in rows.


## R function
### In R, define and use a named function that accepts parameters and returns values

```r
add <- function(x, y) {
  x + y
}

add(5, 10)
```

- The last line of the function returns a value, to return a value early use the special word `return`

- Default function arguments

```r
repeat_string <- function(x, n = 2) {
    repeated <- ""
    for (i in seq_along(1:n)) {
        repeated <- paste0(repeated, x)
    }
    repeated
}

repeat_string("MDS")
```

### Describe lazy evaluation and `...` (variable arguments) and how it affects functions in R

- In R, function arguments are lazily evaluated: they're only evaluated if accessed. (cf. Python)

```r
# R code (this would work)
add_one <- function(x, y) {
    x <- x + 1
    return(x)
}
add_one(1) # won't cause Error since R uses lazy evaluation and y does not get referred here which is fine.
```

```python
# Python code (this would not work)
def add_one(x, y):
    x = x + 1
    return x
add(1) # Throw an Error since python does not know what y refers to
```

- Python evaluates the function arguments before it evaluates the function and because it doesn’t know what y is, it will break even though it is not used in the function.
- R performs lazy evaluation, meaning it delays the evaluation of the function arguments until its value is needed within/inside the function. Since y is never referenced inside the function, R doesn’t complain, or even notice it.

### explain the importance of scoping and environments in R as they relate to functions

#### Name masking

- Names defined inside a function mask names defined outside a function
- If a name isn’t defined inside a function, R looks one level up (and then all the way up into the global environment and even loaded packages!)

```r
  x <- 1
g04 <- function() {
  y <- 2
  i <- function() {
    z <- 3
    c(x, y, z)
  }
  i()
}
g04()

1 2 3
```

#### Dynamic lookup

- R looks for values when the function is run, not when the function is created.
- This means that the output of a function can differ depending on the objects outside the function’s environment

```r
g12 <- function() x + 1
x <- 15
g12() ## 16

x <- 20
g12() ## 21
```

#### A fresh start

- Every time a function is called, a new environment is created to host its execution.
- This means that a function has no way to tell what happened the last time it was run; each invocation is completely independent.

```r
g11 <- function() {
  if (!exists("a")) {
    a <- 1
  } else {
    a <- a + 1
  }
  a
}

g11() ## 1
g11() ## 1
g11() ## 1
```

### Use testthat to formulate a test case to prove a function design specification

```r
test_that("Message to print if test fails", expect_*(...))

x <- c(3.5, 3.5, 3.5)
y <- c(3.5, 3.5, 3.5)
test_that("x and y should contain the same values", {
    expect_equal(x, y)
})
```

#### Common `expect_*` statements for use with `test_that`

| `expect_*`          | description                                                                                            |
|---------------------|--------------------------------------------------------------------------------------------------------|
| `expect_identical`  | test two objects for being exactly equal                                                               |
| `expect_equal`      | compare R objects x and y testing ‘near equality’ (can set a tolerance)                                |
| `expect_equivalent` | compare R objects x and y testing ‘near equality’ (can set a tolerance) and does not assess attributes |
| `expect_error`      | tests if an expression throws an error                                                                 |
| `expect_warning`    | tests whether an expression outputs a warning                                                          |
| `expect_output`     | tests that print output matches a specified value                                                      |
| `expect_true`       | tests if the object returns `TRUE`                                                                     |
| `expect_false`      | tests if the object returns `False`                                                                    |

##### Example

```r
celsius_to_fahr <- function(temp) {
  (temp * (9 / 5)) + 32
}


test_that("Temperature should be the same in Celcius and Fahrenheit at -40", {
        expect_identical(celsius_to_fahr(-40), -40)
    })
test_that("Room temperature should be about 23 degrees in Celcius and 73 degrees Fahrenheit", {
        expect_equal(celsius_to_fahr(23), 73, tolerance = 1)
    })
```

### Use test-driven development principles to define a function that accepts parameters, returns values and passes all tests

- Create a function for tests

```r
test_fahr_to_celsius <- function() {
    test_that("Temperature should be the same in Celcius and Fahrenheit at -40", {
        expect_identical(fahr_to_celsius(-40), -40)
    })
    test_that("Room temperature should be about 73 degrees Fahrenheit and 23 degrees in Celcius", {
        expect_equal(fahr_to_celsius(73), 23, tolerance = 1)
    })
}
```

### Handle errors gracefully via exception handling

- Defensive programming at the beginning of a function

```r
fahr_to_celsius <- function(temp) {
    if(!is.numeric(temp)){
        stop("Cannot calculate temperature in Farenheit for non-numerical values")
    }
    (temp - 32) * 5/9
}
```

#### `try` in R

```r
try({
    # some code
    # that can be
    # split across several
    # lines
})

# code to continue even if error in code
# in try code block above
```

- R has a `try` function to attempt to run code, and continue running subsequent code even if code in the try block does not work

### Use `roxygen2` friendly function documentation to describe parameters, return values, description and example(s)

```r
#' Converts temperatures from Fahrenheit to Celsius.
#'
#' @param temp a vector of temperatures in Fahrenheit
#'
#' @return a vector of temperatures in Celsius
#'
#' @examples
#' fahr_to_celcius(-20)
fahr_to_celsius <- function(temp) {
    (temp - 32) * 5/9
}

```

### Why Roxgen2

Because when you create an R package to share them, they will be set up to have the fancy documentation that we get using ?function_name

### Write comments within a function to improve readability

### Evaluate the readability, complexity and performance of a function

### Source and use functions stored as R code in another file, as well as those in R packages/libraries

- Functions in another script that you want to read into your analysis

```r
source("src/kelvin_to_celsius.R")

kelvin_to_celsius(273.15)
```

### Describe what R packages/libraries are, as well as explain when and why they are useful

- In R, a package is a collection of R functions, data and compiled code.
- The location where the packages are stored is called the library.
- Packaging your R code so that it can be installed and then used across multiple projects on your (and others) machines without directly pointing to where the code is stored, but instead accessed using the `library` function.

```r
library(convertemp)
```

## {purrr}

### Write and use anonymous functions in R in isolation and in combination the functional {purrr} map

#### Writing an anonymous function with map_*

```r
(function(x) x + 1)(1)
```

```r
map_*(data, function(arg) function_being_called(arg, other_arg))    ## long form
map_*(data, ~ function_being_called(., other_arg))                  ## short form
```

#### Example

```r
map_df(data_entry, function(vect) str_replace(vect, pattern = "Cdn", replacement = "Canadian"))
map_df(data_entry, ~str_replace(., pattern = "Cdn", replacement = "Canadian"))
```

### group_by + nest, map, unnest, list-columns

- Create and modify nested data frames using {dplyr} `group_by` + {tidyr} `nest` and {purrr} `map*` functions
- Describe situations where nested data frames are useful
- Create unnested data frames using {tidyr} `unnest`

#### What is a nested data frame

- A data frame which contains other data frames
- Extremely useful when **fitting many models**
- Allows you to **keep the data, the model meta data, and the model and its results** all associated together as a single row

#### How to

- We start with a grouped data frame, and nest it

```r
# create a nested data frame
by_country <- gapminder %>%
    group_by(continent, country) %>%
    nest()
print(by_country)

# A tibble: 142 x 3
# Groups:   country, continent [142]
   country     continent data
   <fct>       <fct>     <list>
 1 Afghanistan Asia      <tibble [12 × 4]>
 2 Albania     Europe    <tibble [12 × 4]>
 3 Algeria     Africa    <tibble [12 × 4]>
 4 Angola      Africa    <tibble [12 × 4]>
 5 Argentina   Americas  <tibble [12 × 4]>
 6 Australia   Oceania   <tibble [12 × 4]>
 7 Austria     Europe    <tibble [12 × 4]>
 8 Bahrain     Asia      <tibble [12 × 4]>
 9 Bangladesh  Asia      <tibble [12 × 4]>
10 Belgium     Europe    <tibble [12 × 4]>
# … with 132 more rows
```

- `data` column is a list of data frames(tibbles) (list column)
  - a column that is a list of other data frames

#### Difference between a standard grouped data frame and a nested data frame

- In a grouped data frame, each row is an observation
- In a nested data frame, each row is a group. (or meta-observation!)

#### Difference between list-columns in data frames and in tibbles

- List columns are possible in base R data frames, but making them is not easy.

```r
# try to make a list column in a data frame by using I
data.frame(x = I(list(c(1, 2, 3), c(3, 4, 5))))
```

#### List column workflow

1. Create a list column using a function such as `nest` (or `summarise` + `list`, `mutate` + `map_*`)
2. Create other intermediate list-columns by transforming existing list columns with `map`, `map2`, or `pmap`
3. Simplify the list-column back down to a data frame or atomic vector, often by `unnest`, `mutate` + `map_*` functions hat return atomic vectors as opposed to lists.

```r
gap_lifeExp_ci <- function(df, statistic) {
  df %>%
        specify(response = lifeExp) %>%
        generate(reps = 1000, type = "bootstrap")  %>%
        calculate(stat = statistic)  %>%
        get_ci()
}

by_country <- gapminder %>%
    group_by(continent, country) %>%                        # group
    nest() %>%                                              # nest
    mutate(mean_life_exp = map_dbl(data, ~mean(.$lifeExp)), # unnest with mutate+map_*
    life_exp_ci = map(data, ~gap_lifeExp_ci(., "mean")))    # making an intermediate list-column
print(by_country)

# A tibble: 142 x 5
# Groups:   country, continent [142]
   country     continent data              mean_life_exp life_exp_ci
   <fct>       <fct>     <list>                    <dbl> <list>
 1 Afghanistan Asia      <tibble [12 × 4]>          37.5 <tibble [1 × 2]>
 2 Albania     Europe    <tibble [12 × 4]>          68.4 <tibble [1 × 2]>
 3 Algeria     Africa    <tibble [12 × 4]>          59.0 <tibble [1 × 2]>
 4 Angola      Africa    <tibble [12 × 4]>          37.9 <tibble [1 × 2]>
 5 Argentina   Americas  <tibble [12 × 4]>          69.1 <tibble [1 × 2]>
 6 Australia   Oceania   <tibble [12 × 4]>          74.7 <tibble [1 × 2]>
 7 Austria     Europe    <tibble [12 × 4]>          73.1 <tibble [1 × 2]>
 8 Bahrain     Asia      <tibble [12 × 4]>          65.6 <tibble [1 × 2]>
 9 Bangladesh  Asia      <tibble [12 × 4]>          49.8 <tibble [1 × 2]>
10 Belgium     Europe    <tibble [12 × 4]>          73.6 <tibble [1 × 2]>
# … with 132 more rows
```

```r
# unnest the ci column
by_country %>%
    unnest(life_exp_ci) %>%         ## unnest() with column name to specify which column you would like to unnest.
    print()

# A tibble: 142 x 6
# Groups:   country, continent [142]
   country     continent data              mean_life_exp `2.5%` `97.5%`
   <fct>       <fct>     <list>                    <dbl>  <dbl>   <dbl>
 1 Afghanistan Asia      <tibble [12 × 4]>          37.5   34.4    40.3
 2 Albania     Europe    <tibble [12 × 4]>          68.4   64.8    71.7
 3 Algeria     Africa    <tibble [12 × 4]>          59.0   53.4    64.6
 4 Angola      Africa    <tibble [12 × 4]>          37.9   35.6    39.9
 5 Argentina   Americas  <tibble [12 × 4]>          69.1   66.9    71.2
 6 Australia   Oceania   <tibble [12 × 4]>          74.7   72.4    77.0
 7 Austria     Europe    <tibble [12 × 4]>          73.1   70.8    75.2
 8 Bahrain     Asia      <tibble [12 × 4]>          65.6   61.0    69.9
 9 Bangladesh  Asia      <tibble [12 × 4]>          49.8   45.0    54.6
10 Belgium     Europe    <tibble [12 × 4]>          73.6   71.5    75.6
# … with 132 more rows
```

## Programming with R

### Describe data masking as it relates to the dplyr functions. Explain the problems it solves for interactive programming and the problems it creates for programming in a non-interactive setting

#### Data masking

- `arrange()`, `count()`, `filter()`, `group_by()`, `mutate()`, and `summarise()` use **data masking** so that you can use data variables as if they were variables in the environment (i.e. you write my_variable not df$myvariable).

```r
starwars[starwars$homeworld == "Naboo" & starwars$species == "Human", ,]
```

```r
starwars %>% filter(homeworld == "Naboo", species == "Human")       # data masking allows you to use this kind of syntax
```

- When functions like `filter` are called, there is a delay in evaluation and the data frame is temporarily promoted as first class objects, we say the data masks the workspace
- This is to allow the promotion of the data frame, such that it masks the workspace (global environment)
- When this happens, R can then find the relevant columns for the computation

- Data masking makes interactive data exploration fast and fluid, but they add some new challenges when you attempt to use them indirectly such as in a for loop or a function. --> `enquo()` + `!!` or `{{}}`

### Explain what the `enquo()` function and the `!!` operator do in R in the context of data masking as it relates to the dplyr functions

```r
filter_gap <- function(col, val) {
    col <- enquo(col)                   ## `enquo` quotes the column name
    filter(gapminder, !!col == val)     ## `!!` unquotes the column name
}

filter_gap(country, "Canada")

```

### Use the `{{` (read: curly curly) operator (abstracts quote-and-unquote into a single interpolation step), the `:=` (read: walrus) operatpr in R, and `...` (read: pass the dots) to write functions which wrap the dplyr functions

#### `{{` operator

```r
filter_gap <- function(col, val) {
    filter(gapminder, {{col}} == val)       ## `enquo` + `!!`, but easier to use
}

filter_gap(country, "Canada")
```

- Why do we need to embrace the columns with `{{` when writing functions that wrap {tidyverse} functions when we want to pass unquoted column names in as function arguments?
    - A data frame column name is not something that is known/accessible in the global environment
    - `{{` quotes the column names, and then unquotes and evaluates them inside the data mask - where they can be successfully evaluated


#### `:=` operator (walrus)

- It is needed when adding values with tidy evaluation

```r
group_summary <- function(data, group, col, fun) {
    data %>%
        group_by({{ group }}) %>%
        summarise( {{ col }} := fun({{ col }}))
}

group_summary(gapminder, continent, gdpPercap, mean)
```

#### `...` Passing the dots

```r
square_diff_n_select <- function(data, col_to_change, ...) {        ## ... should locate at the end
    data %>%
        mutate({{ col_to_change }} := ({{ col_to_change }} - mean({{ col_to_change }}))^2) %>%
        select(..., {{ col_to_change }})
}

square_diff_n_select(mtcars, mpg, drat, carb)
```

#### Programming defensively with tidy evaluation

```r
square_diff_n_select <- function(data, col_to_change, ...) {
    if (!is.numeric(data %>% pull({{ col_to_change }}))) {
        stop('col_to_change must be numeric')
    }

    data %>%
        mutate({{ col_to_change }} := ({{ col_to_change }} - mean({{ col_to_change }}))^2) %>%
        select(..., {{ col_to_change }})
}

square_diff_n_select(gapminder, lifeExp, country, year)
```

# Practice Quiz

## Question 1

Using `group_by()` and `summarise()` provide code to calculate the minimum lifeExp for each year in the gapminder dataset. To help you with this question, the first 5 rows of gapminder are also printed below.

```
# A tibble: 5 x 6
  country     continent  year lifeExp      pop gdpPercap
  <fct>       <fct>     <int>   <dbl>    <int>     <dbl>
1 Afghanistan Asia       1952    28.8  8425333      779.
2 Afghanistan Asia       1957    30.3  9240934      821.
3 Afghanistan Asia       1962    32.0 10267083      853.
4 Afghanistan Asia       1967    34.0 11537966      836.
5 Afghanistan Asia       1972    36.1 13079460      740.
```

Jeremy:

```{r}
gapminder %>%
  group_by(year) %>%
  summarise(min_lifeExp = min(lifeExp))
```

## Question 2

Briefly explain the difference between a grouped and nested dataframe.

Jeremy:

- Grouped dataframe performs operations by groups and if we apply aggregate functions on them, the final table won't include those columns the aggregate function performs on.
  - Each row of a grouped dataframe is an observation. (Jin)

- Nested dataframe has list columns which can contain other dataframes. We can perform unnest on it to transform to original.
  - Each row of a nested dataframe is a group, which is a meta-observation. (Jin)
- After applying operations such as `summarise` or `mutate`, a nested dataframe contains the original data, but a grouped dataframe doesn't. (Jin)

## Question 3

Fill in the missing code in the function below, such that running `below_threshold(penguins, body_mass_g, 3000)` would return the rows of a data frame where the column name specified had values below the specified threshold. You just need to provide the code to make this function work, you do not need to write tests nor throw errors for erroneous user input.

```
below_threshold <- function(df, col_name, threshold) {


}
```

Jeremy:

```{r}
below_threshold <- function(df, col_name, threshold) {
  filter(df, {{ col_name }} < threshold)
}
```

Jin:

```r
below_threshold <- function(data, col_name, threshold) {
    data %>%
        filter({{ col_name }} < threshold)
}
```

## Question 4

Consider the function defined below:

```
#' Calculates the average from a vector or list of numbers
#'
#' @param x vector or list containing numeric elements
#'
#' @return vector of length one and type double
#'
#' @examples
#' average(c(3, 5.5, 7))
average <- function(x) {
  total <- 0
  for (i in seq_along(x)) {
    total <- total + x[i]
  }
  total / length(x)
}
```

Fill in the blanks (___) in the code below to write two useful tests you would write to demonstrate that this function works.

```
library(testthat)
test_that("_______________________", {
  ____________________________________
})
test_that("_______________________", {
  ____________________________________
})
```

Jeremy:

```{r}
library(testthat)
test_that("The output should be whatever you input when input vector has length 1", {
  expect_equal(average(c(2)), 2)
})
test_that("The output should be 3 when you pass a vector c(1, 2, 3, 4, 5)", {
  expect_equal(average(c(1, 2, 3, 4, 5)), 3)
})
```

Jin:

```r
test_that("The length of the output should be 1.", {
    expect_equal(length(average(c(3, 5.5, 7))), 1)
})

test_that("The function is not calculating properly.", {
    expect_equal(average(c(3, 5.5, 7)), 5.17, tolerance = 0.01)
})
```

## Question 5

Consider the dplyr functions in R that you have already met (`select`, `filter`, `arrange`, `mutate`). These functions allow you to refer to column names without using quotes or the `$` operator. Explain the advantages and disadvantages of referring to column names in this way. Hint: think about writing functions. Answer in 2-3 sentences and in your own words.

Jeremy:

- advantages:
  - Don't need to type data frame name again and again to acess its columns.
  - Add readability and sanity to your code.
  - Easy to use dplyr functions. (Jin)
- Disadvantages:
  - When passing unquoted column name to the function, the function would not know its an unquoted column name unless you use {{}} to specify that.
  - Adds a bit of complexity when programming in tidy evaluation.
