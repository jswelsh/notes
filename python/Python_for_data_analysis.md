#Python for Data Analysis

- Python has become one of the most popular dynamic, programming languages, along with Perl, Ruby, and others. 
- Python and Ruby have become especially popular in recent years for building websites using their numerous web frameworks, like Rails (Ruby) and Django (Python).
- Such languages are often called scripting languages as they can be used to write quick-and-dirty small programs,or scripts.
- Python is distinguished by its large and active scientific computing community.
- Python’s strength in general purpose programming, it is an excellent choice as a single language for building data-centric applications.

#### Solving the “Two-Language” Problem
In many organizations, it is common to research, prototype, and test new ideas using a more domain-specific computing language like MATLAB or R then later port those ideas to be part of a larger production system written in, say, Java, C#, or C++. What people are increasingly finding is that Python is a suitable language not only for doing research and prototyping but also building the production systems, too.

#### Why Not Python?

As Python is an interpreted programming language, in general most Python code will run substantially slower than code written in a compiled language like Java or C++. As programmer time is typically more valuable than CPU time, many are happy to make this tradeoff. However, in an application with very low latency requirements (for ex- ample, a high frequency trading system), the time spent programming in a lower-level, lower-productivity language like C++ to achieve the maximum possible performance might be time well spent.

### Essential Python Libraries

#### NumPy
NumPy - Numerical Python, is the foundational package for scientific computing in Python.
- A fast and efficient multidimensional array object ndarray
- Functions for performing element-wise computations with arrays or mathematical
operations between arrays
- Tools for reading and writing array-based data sets to disk
- Linear algebra operations, Fourier transform, and random number generation
- Tools for integrating connecting C, C++, and Fortran code to Python

Beyond the fast array-processing capabilities that NumPy adds to Python, one of its primary purposes with regards to data analysis is as the primary container for data to be passed between algorithms. For numerical data, NumPy arrays are a much more efficient way of storing and manipulating data than the other built-in Python data structures. Also, libraries written in a lower-level language, such as C or Fortran, can operate on the data stored in a NumPy array without copying any data.

##### pandas
pandas - panel data, an econometrics term for multidimensional structured data sets. pandas provides rich data structures and functions designed to make working with structured data fast, easy, and expressive. pandas combines the high performance array-computing features of NumPy with the flexible data manipulation capabilities of spreadsheets and relational databases (such as SQL).

##### matplotlib
matplotlib is the most popular Python library for producing plots and other 2D data visualizations.


##### IPython
IPython is the component in the standard scientific Python toolset that ties everything together. It provides a robust and productive environment for interactive and exploratory computing. It is an enhanced Python shell designed to accelerate the writing, testing, and debugging of Python code. It is particularly useful for interactively working with data and visualizing data with matplotlib.

##### SciPy
SciPy is a collection of packages addressing a number of different standard problem domains in scientific computing. Together NumPy and SciPy form a reasonably complete computational replacement for much of MATLAB along with some of its add-on toolboxes.

##### The Python Interpreter
Python is an interpreted language. The Python interpreter runs a program by executing
one statement at a time.

##### Language Semantics
The Python language design is distinguished by its emphasis on readability, simplicity,
and explicitness. Some people go so far as to liken it to “executable pseudocode”.
###### 1. Indentation, not braces
- Python uses whitespace (tabs or spaces) to structure code instead of using braces as in many other languages
- statements also do not need to be terminated by semicolons. Semicolons can be used, however, to separate multiple statements on a single line: 
`a = 5; b = 6; c = 7`
Putting multiple statements on one line is generally discouraged in Python as it often
makes code less readable.
###### 2. Everything is an object
An important characteristic of the Python language is the consistency of its object model. Every number, string, data structure, function, class, module, and so on exists in the Python interpreter in its own “box” which is referred to as a Python object. Each object has an associated type (for example, string or function) and internal data.

###### 3. Variables and pass-by-reference
When you pass objects as arguments to a function, you are only passing references; no copying occurs. Thus, Python is said to pass by reference, whereas some other languages support both pass by value (creating copies) and pass by reference. This means that a function can mutate the internals of its arguments. Suppose we had the following function:
```
def append_element(some_list, element):
some_list.append(element)
```
Then given what’s been said, this should not come as a surprise:
```
In [2]: data = [1, 2, 3]
In [3]: append_element(data, 4)
In [4]: data
Out[4]: [1, 2, 3, 4]
```
###### 4. Dynamic references, strong types
In contrast with many compiled languages, such as Java and C++, object references in
Python have no type associated with them. There is no problem with the following:
```
In [245]: a = 5 In [246]: type(a)
Out[246]: int
In [247]: a = 'foo' In [248]: type(a)
Out[248]: str
```

Variables are names for objects within a particular namespace; the type information is
stored in the object itself. Some observers might hastily conclude that Python is not a
“typed language”. This is not true; consider this example:
```
In [249]: '5' + 5
---------------------------------------------------------------------------
TypeError
Traceback (most recent call last)
<ipython-input-249-f9dbf5f0b234> in <module>()
----> 1 '5' + 5
TypeError: cannot concatenate 'str' and 'int' objects
```
In some languages, such as Visual Basic, the string '5' might get implicitly converted (or casted) to an integer, thus yielding 10. Yet in other languages, such as JavaScript, the integer 5 might be casted to a string, yielding the concatenated string '55' . In this regard Python is considered a strongly-typed language, which means that every object has a specific type (or class), and implicit conversions will occur only in certain obvious circumstances