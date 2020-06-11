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
* Functions are a **first-class object** - the idea is that you can assign functions to a variable or pass them to another funciton or store them in a data structure. All functions in Python are first-class. First-class object can:  
 * be created at the runtime  
 * assigned to a variable  
 * passed to a function  
 * returned as a funciton  

* **Build-in** functions:  
 * Reduction functions: ``sum, all, any``  
 * Higher-order funcitons (a function that takes another function as an argument): ``min, max, sorted`` with a use of key parameter that can take a function e.g. ``len()``
  
## Object Oriented Programming (OOP)
Wrapping the functionality and the data in *objects*. **Class** a type of objects and **object** itself is an instance of a class. Calls can have data, which are then called **fields** and functions, which are called **methods**. TOgether fields and methods are called **atributes**

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
All methods and fields are public in Python. If you add double underscore prefix, the Python will use name-mangling to make it private, so it will automatically add the name of the class as a prefix eg. if class ``Dog`` has an atribute ``__mood``, then to cal it outside of this class, you would need to call ``_Dog__mod``. This is for sefety (accidentely calling this method, not knowing that it is private), not for security (you can still do it, but you are aware that it is private and you should not). Some do not like the look of this so by convention if you add _ before the name it should be treated as a protected atribute. This is just convention (as ALL_CAPS mean constant value).

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
**Decorator** is a callable that takes another function as an argument (the decorated function). 
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
