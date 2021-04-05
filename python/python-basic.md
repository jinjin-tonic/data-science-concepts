### Learning Objectives

- Create, describe and differentiate standard Python datatypes such as int, float, string, list, dict, tuple, etc

| English name          | Type name | Type Category  | Description                                   | Example                                  |
|-----------------------|-----------|----------------|-----------------------------------------------|------------------------------------------|
| integer               | int       | Numeric Type   | positive/negative whole numbers               | 42                                       |
| floating point number | float     | Numeric Type   | real number in decimal form                   | 3.14159                                  |
| boolean               | bool      | Boolean Values | true or false                                 | True                                     |
| string                | str       | Sequence Type  | text                                          | "I Can Has Cheezburger?"                 |
| list                  | list      | Sequence Type  | a collection of objects - mutable & ordered   | ['Ali', 'Xinyi', 'Miriam']               |
| tuple                 | tuple     | Sequence Type  | a collection of objects - immutable & ordered | ('Thursday', 6, 9, 2018)                 |
| dictionary            | dict      | Mapping Type   | mapping of key-value pairs                    | {'name':'DSCI', 'code':511, 'credits':2} |
| none                  | NoneType  | Null Object    | represents no value                           | None                                     |
| set                   | set       | Sequence Type  | a collection of objects - mutable & unordered | {0, 1, 2, 3}

- Dictionaty values are mutable, keys are immutable

- Perform arithmetic operations like +, -, *, ** on numeric values
    - // :  divide with integral result (discard remainder)

- Perform basic string operations like .lower(), .split() to manipulate strings

- Compute boolean values using comparison operators operations (==, !=, >, etc.) and boolean operators (and, or, not)

- Assign, index, slice and subset values to and from tuples, lists, strings and dictionaries

- Write a conditional statement with if, elif and else

- Identify code blocks by levels of indentation

- Explain the difference between mutable objects like a list and immutable objects like a tuple
    | class | immutable? |
    |-------|------------|
    | bool  | yes        |
    | int   | yes        |
    | float | yes        |
    | list  | no         |
    | tuple | yes        |
    | str   | yes        |
    | set   | no         |
    | dict  | no         |


### Learning Objectives

- Write for and while loops in Python
    - generators
    ```
    gen = (n for n in range(10))
    for i in gen:
    print(i)
    ```
    - or
    ```
    def gen():
    for n in range(10):
        yield (n, n ** 2)
    ```

- Identify iterable datatypes which can be used in for loops
    - Iterable datatypes: Strings, Tuples, Lists, Sets, Dictionaries

- Create a list, dictionary, or set using comprehension
    - list comprehension
    `[num*2 if not num % 2 else -num for num in nums]`
    - dict comprehension
    `keys_and_vals = {k:v for k, v in zip(keys, vals)}`
    - set comprehension
    `{word[-1] for word in words}`

- Write a try/except statement
    ```
    try:
        this_variable_does_not_exist
    except Exception as ex:
        print("You did something bad!")
        print(ex)
        print(type(ex))
    ```

    ```
    try:
    this_variable_does_not_exist  # name error
        #     (1, 2, 3)[0] = 1  # type error
        #     5/0  # ZeroDivisionError
    except TypeError:
        print("You made a type error!")
    except NameError:
        print("You made a name error!")
    except:
        print("You made some other sort of error")
    ```

    - Custom error
    ```
    class ThisIsNotWhatIWanted(Exception):
    pass

    def website(url):
    if not isinstance(url, str):
        raise ThisIsNotWhatIWanted(f"Sorry, enter different thing")
    return [domain for domain in url.split(".")][1]
    ```

- Define a function and an anonymous function in Python
    ```
    def evaluate_function_on_x_plus_1(fun, x):
    return fun(x+1)

    evaluate_function_on_x_plus_1(lambda x: x ** 2, 5)  ## Anonymous function
    ```

    ```
    custom = (lambda x: sum(x))
    custom([1,2,3,4,5])
    >> 15
    ```
- Describe the difference between positional and keyword arguments
    - Positional arguments must be included in the correct order. 
    - Keyword arguments are included with a keyword and equals sign.

- Describe the difference between local and global arguments
    - A global variable is a variable that is accessible globally. 
    - A local variable is one that is only accessible to the current scope, such as temporary variables used in a single function definition.

- Apply the DRY principle to write modular code
    - Don't Repeat Yourself

- Assess whether a function has side effects
    - If a function changes the variables passed into it, then it is said to have side effects
    ```
    def silly_sum(my_list):
    my_list.append(0)
    return sum(my_list)

    >>> l
    [1, 2, 3, 4, 0]
    ```

- Write a docstring for a function that describes parameters, return values, behaviour and usage
    ```
    def function_name(param1, param2, param3):
    """First line is a short description of the function.
    
    A paragraph describing in a bit more detail what the
    function does and what algorithms it uses and common
    use cases.
    
    Parameters
    ----------
    param1 : datatype
        A description of param1.
    param2 : datatype
        A description of param2.
    param3 : datatype
        A longer description because maybe this requires
        more explanation and we can use several lines.
    
    Returns
    -------
    datatype
        A description of the output, datatypes and behaviours.
        Describe special cases and anything the user needs to
        know to use the function.
    
    Examples
    --------
    >>> function_name(3,8,-5)
    2.0
    """
    ```

### Learning Objectives

- Formulate a test case to prove a function design specification
- Use an assert statement to validate a test case
    - `assert expression, "Error message if expression is False`
- Debug Python code with the pdb module, or by using %debug in a Jupyter code cell
    - `breakpoint()`
- Describe the difference between a class and a function in Python
    - class: creating our **own data types**. An instance is called an object.
    - class can have methods, they group functions, store data (attributes or properties)
    - function: functions do things. they cannot have attributes.
- Be able to create a class
- Differentiate between instance attributes and class attributes
    - Attributes like `mds_1.first` are sometimes called `instance attributes`
    - They are specific to the object we have created
    - But we can also set `class attributes` which are the same amongst all instances of a class, they are defined **outside** of the `__init__ method`

    ```
    class mds_member:
    
    role = "MDS member" # class attributes
    campus = "UBC"
    
    def __init__(self, first, last):
        self.first = first
        self.last = last
        self.email = first.lower() + "." + last.lower() + "@mds.com"
        
    def full_name(self):
        return f"{self.first} {self.last}"
    ```
    - You can also change the class attribute for just a single instance
    - But this is typically not recommended because if you want differing attributes for instances, you should probably use instance attributes

- Differentiate between methods, class methods and static methods 
    - Regular methods: act on an instance of the class (take self as an argument)
    - Class methods: act on the actual class
    ```
    class mds_member:

    role = "MDS member"
    campus = "UBC"
    
    def __init__(self, first, last):
        self.first = first
        self.last = last
        self.email = first.lower() + "." + last.lower() + "@mds.com"
        
    def full_name(self):
        return f"{self.first} {self.last}"
    
    @classmethod
    def from_csv(cls, csv_name):
        first, last = csv_name.split(',')
        return cls(first, last)
    ```
    - Identify our method as class method using the decorator `@classmethod` (more on decorators in a bit)
    - Pass `cls` instead of self as the first argument
    
    - Static methods: do not operate on either the instance or the class, they are just simple functions (self arguments X)

    ```
    class mds_member:
    
    role = "MDS member"
    campus = "UBC"
    
    def __init__(self, first, last):
        self.first = first
        self.last = last
        self.email = first.lower() + "." + last.lower() + "@mds.com"
        
    def full_name(self):
        return f"{self.first} {self.last}"
    
    @classmethod
    def from_csv(cls, csv_name):
        first, last = csv_name.split(',')
        return cls(first, last)
    
    @staticmethod
    def is_quizweek(week):
        return True if week in [3, 5] else False
    ```
- Understand and implement subclassing/inheritance with Python classes

### Learning Objectives

- Describe why code style is important
- Differentiate between the role of a linter like flake8 and an autoformatter like black
- Implement linting and formatting from the command line or within Jupyter or another IDE
- Write a Python module (.py file) in VSCode or other IDE of your choice
- Import installed or custom packages using the import syntax
- Explain the notion of a reference in Python
- Explain the notion of scoping in Python
- Anticipate whether changing one variable will change another in Python
- Anticipate whether a function changes the caller’s version of an argument variable in Python
- Select the appropriate choice between == and is in Python
    - `==` are the contents the same? (just the contents are the same)
    - `is` are the objects the same? (same memory)
