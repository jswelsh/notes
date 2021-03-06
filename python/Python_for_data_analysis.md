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

#### matplotlib
matplotlib is the most popular Python library for producing plots and other 2D data visualizations.


#### IPython
IPython is the component in the standard scientific Python toolset that ties everything together. It provides a robust and productive environment for interactive and exploratory computing. It is an enhanced Python shell designed to accelerate the writing, testing, and debugging of Python code. It is particularly useful for interactively working with data and visualizing data with matplotlib.

#### SciPy
SciPy is a collection of packages addressing a number of different standard problem domains in scientific computing. Together NumPy and SciPy form a reasonably complete computational replacement for much of MATLAB along with some of its add-on toolboxes.