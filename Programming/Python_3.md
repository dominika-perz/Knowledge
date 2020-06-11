# Python 3

## Assignments
**Augmented assigment**: performs as inplace modification of  the variable: e.g. ``+=``, ``-=``, etc.
Can be implemented in a class by ``__iadd__``, ``__idiv__`` functions and so on.

Rules for assignment:  
* A simple assigment does not create copy  
* An augmented assignment will create a new object only if a lefthand variable is immutable.  
* Assigning a new value does not modify this value, but creates a new one that is now referenced (rebinding). If the old value does not have a strong references to it anymore it is garbage collected.  
* If a mutable object is passed to a functions, it can modify it, to prevent this we have to mae a local copy or revert the object back to its original state before returning.  
* Be careful (or better don't) mutable objects as default values for functions parameters,because that can be changed inplace

### ``Assert`` statement
The ``assert`` statement is used to assert that something is true, if it is not an exeption will be raised.Normally it is better to handle the abnormality / catch the exception and quit then just quit by throwingan exception by ``assert``.

## Type annotation  
**Annotation** - are optional and on their own do not mean anything, but can be used but third libraries or IDEs for verification
  * vairiable: e.g. ``primes: List[int] = []`` or ``name: str``
  * functions: ``def sum(a: int, b: int) -> int``
  
## Generators  
**Simple generators** - the function can retrurn a generator, that will be used to generated a new value
every time that the function is called. This can be implemented with a use of a word ``yield``:  
```python
def fib():
a, b = 0, 1 #this is initialization, happens only once
while 1:
  yield b # every time the fib() is called, w new value for b will be return
  a, b = b, a+b # an update that is performed after evry call
```
## Functions  
Functions are a **first-class object** - the idea is that you can assign functions to a variable or pass them to another funciton or store them in a data structure. All functions in Python are first-class. First-class object can:  
 * be created at the runtime  
 * assigned to a variable  
 * passed to a function  
 * returned as a funciton  

**Build-in** functions:  
 * Reduction functions: ``sum, all, any``  
 * Higher-order funcitons (a function that takes another function as an argument): ``min, max, sorted, map, filter, reduce`` with a use of key parameter that can take a function e.g. ``len()``
  
## Object Oriented Programming (OOP)
Wrapping the functionality and the data in *objects*. **Class** a type of objects and **object** itself is an instance of a class. Calls can have data, which are then called **fields** and functions, which are called **methods**. TOgether fields and methods are called **atributes**. The main adventage of OOP is the reuse of the code.

For instances we have two types: **instance variables** tha are object specific and **class variables** that are shared within the instances of the class. The same can be done with method by adding a ``@methodclass`` decorator before a class method. All the methods have additional first parameter caleed ``self``, which referes to the object itself.  
```python
class MyClass:

   numOfObjects = 0 #this is a class variable
   
   def __init__(self, var):
      self.var = var # this is an instance variable
      MyClass.numOfObjects += 1 #or self.__clas__.numOfObjects
      
   @classmethod
   def how_many(cls):
      return cls.numOfObject


object_myclass = MyClass(5)
MyClass.how_many()
```
``__init__`` method is run as soon as an object is instantiated - used for initalization  
All methods and fields are public in Python. If you add double underscore prefix, the Python will use name-mangling to make it private, so it will automatically add the name of the class as a prefix eg. if class ``Dog`` has an atribute ``__mood``, then to cal it outside of this class, you would need to call ``_Dog__mod``. This is for sefety (accidentely calling this method, not knowing that it is private), not for security (you can still do it, but you are aware that it is private and you should not). Some do not like the look of this so by convention if you add _ before the name it should be treated as a protected atribute. This is just convention (as ALL_CAPS mean constant value). Trying to conceal the private atrributs of a class is called **encapsulation**.

### Inheritance
An idea of having a general class with common functionality and atributes and having a subclasses that inherit this common methods and atributes from the parent class (**base class** or **superclass**), but also implement their functionality (**subclass** or **derived class**).

We can refer to the base class by the keyword ``super``. The subclasses can implement the same methods are base class, in that case the subclass implementation will be called by the subtype object (the parent implementation is overridden). However, inside the subclass implementation, a implementation of a parent class can be called by ``super.func_name()``. If the method is only implemented in a parent class, then this impleemntation will be call via subtype object.

```python
class baseClass():
   def __init__(self, name):
      self.name = name
     
class subClass(baseClass):
   def __init__(self, name, salary):
      super.__init__(self, name)
      self.salary = salary
```

**Polymorphism** - fact that the subtype classes can be used inevery place the base class object is expected. So we can create a function that operates on a base class and then call it with any of the subclasses objects. Another example of this are operators like ``+`` or functions like ``len()`` which can do different things for different data types.
 

## Docstring
Docstring can be accessed by calling ``func.__doc__``
It is added to a function or class by adding a comment
```python
def func1():
"""This is a docstring for func1"""
```

## Dunder
The functions with double underscore before and after the name are called **special method** or **magic methods** and are pronounced dunder e.g. ``__getitem__`` is pronouced *dunder getitem*. These functions are defined by Python and if implemented in a class allow for additonal funcitonality. e.g. implementiong ``__len__`` allow to call ``len(*)`` on the object, ``__getitem__`` allows to use ``[]`` to grab an instance form an object. 

The list of special methods:
* String/bytes representation: ``___repr__, __str__, __format__, __bytes__``  
* Conversion: ``__abs__, __bool__, __complex__, __int__, __float__, __hash__, __index__``  
* Collection emulating ``__len__, __getitem__, __setitem__, __delitem__, __contains__``   
* init&deletion: ``__new__, __init__, __del__``  
* Attribute management ``__getattr__, __getattribute__, __setattr__, __delattr__, __dir__``  

* Unary numeric operators ``__neg__ -, __pos__ +, __abs__ abs()``  
* Rich comparison operators ``__lt__ >, __le__ <=, __eq__ ==, __ne__ !=, __gt__ >, __ge__ >=``  
* Arithmetic operators ``__add__ +, __sub__ -, __mul__ *, __truediv__ /, __floordiv__ //, __mod__%, __divmod__ divmod() , __pow__ ** or pow(), __round__ round()``   
* Reversed arithmetic operators ``__radd__,__rsub__,__rmul__,__rtruediv__,__rfloordiv__,__rmod__,__rdivmod__, __rpow__``  
* Augmented assignment,arithmetic operators: ``__iadd__,__isub__,__imul__,__itruediv__,__ifloordiv__,__imod__,__ipow__``  
* Bitwise operators ``__invert__ ~, __lshift__ <<, __rshift__ >>, __and__ &, __or__ |,__xor__ ^``  

* Reversed bitwise operators ``__rlshift__, __rrshift__, __rand__, __rxor__, __ror__``  
* Augmented assignment bitwise operators ``__ilshift__, __irshift__, __iand__, __ixor__, __ior__``  

### Decorators
**Decorator** is a callable that takes another function as an argument (the decorated function). They are an example of **metaprogramming**
They can replace the decorated function with another one or perform a check or verification before the decorated function is run.
The decorators are executed imediately after the declarated funciton is defined, so usually when the module is imported. Syntex:    
```python
@deco
def func(args):
  pass
  
func = deco(func)
```
Useful decorators:
* ``@clock()``
* ``@register`` - register this call to 
* ``@classmethod`` - useful, allows to declare a class method, its first argument is always a class ``cls`` (as object for a regular method).
* ``@staticmethod`` - behaves as regular function, does not get class nor object as an argument, not so useful, module-level functions are simpler

We can also defined our own decorators:
```python
def smart_divide(func):
   def inner(a,b):
      print("I am going to divide",a,"and",b)
      if b == 0:
         print("Whoops! cannot divide")
         return

      return func(a,b)
   return inner

@smart_divide
def divide(a,b):
    return a/b
```
```python
>>> divide(2,5)
I am going to divide 2 and 5
0.4

>>> divide(2,0)
I am going to divide 2 and 0
Whoops! cannot divide
```
For a decorator to work for ay function we need to allow it to receive a tuple of positional and keyword arguments:
```python
def works_for_all(func):
    def inner(*args, **kwargs):
        print("I can decorate any function")
        return func(*args, **kwargs)
    return inner
```

### Property
``@property`` decorator is used for getter and setter implementations. If we have an instance eg.``var`` with a property functions of setter and getter, then every time we try to access or change the value of this instance, the getter and setter functions will be called. We can assing his as follows: ``var = property(get_var, set_var)``. The full function is: ``property(fget=None, fset=None, fdel=None, doc=None)``, fdel - deletes the instance, doc - is a string describing an instance. We can also implement this with a decorator:  
```python
# Using @property decorator
class Celsius:
    def __init__(self, temperature=0):
        self.temperature = temperature

    def to_fahrenheit(self):
        return (self.temperature * 1.8) + 32

    @property
    def temperature(self):
        print("Getting value...")
        return self._temperature

    @temperature.setter
    def temperature(self, value):
        print("Setting value...")
        if value < -273.15:
            raise ValueError("Temperature below -273 is not possible")
        self._temperature = value
```

## Closure
A technique with which some data is attached to the code, e.g. when a nested function references a value in its enclosing scope and then this inner function is returned by the outer function. We can see this here: ``print_msg`` have a another function inside: this is called a **nested function**. The inside (nested) function ``printer`` has access to local variables of the outer function. 
```python
def print_msg(msg):
    def printer():
        print(msg)
    return printer

# Output: Hello
another = print_msg("Hello")
another()
```
Decorators in Python make an extensive use of closures as well. Values that get enclosed in the closure function can be found in the ``__closure__`` attribute.

## Context Manager
It is useful when we have a procedure to open and close connection or file. With context manager we make sure that even if excaption will occur while working with an object, the opened connection or file will be closed before exiting the program.
```python
with ... as <name>:
   <name>...
```
``__enter__`` - a setup for the context manager
``__exit__`` - a tear down for the context manager

# PEP 8
* **Indentation: 4 spaces** per level, spaces are prefered over tabs
* Max line length should be **79 characters**. It can be ok to extend to 99 characters for code, but docs and comments should be up to 72 characters.
* If you have to break the line, you can wrap everything in parenthesis or add backslash at the end of the line, before breakage
* The line should **break before the operator**
```python
income(gross_wages
       + tax_interest
       - loan_payment)
```
* **Spacing**: surround top-level functions and clas definitions with two blank lines, method inside a class - single blank line. Extra blank lines can be used (rarely) to seperate a group of functions in class or logical sections in functions. Single line functions relatted to each other may not have a blanck line in between.
* **Enconding** Python 3 uses **UTF-8**
* ``import`` should be on seperate lines at the top of the file, grouped by standard library, related libraries or local specific imports.
* Both single and double quotes work for Python - just stick with one.
* No spaces: after and before parenthesis, before comma, semicolon or colon, after semicolon in slicing (except if the values are long e.g. call to a function), 
* Single space: around equal sign or other operators(+,-,+=,and,or,in,not etc.), after comma (expect if is it trailing comma after which is closed parenthesis``,)``), after semicolon (except for slicing) or colon
* If two different operants are used, the one evaluated first should not have spaces: 
```python
hypot2 = x*x + y*y
c = (a+b) * (a-b)
```
* Do not use spaces with ``=`` in parameters assignment
* **Trailing comma**
