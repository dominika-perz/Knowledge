# Python Glossary

* Augmented assigment: performs as inplace modification of  the variable: e.g. ``+=``, ``-=``, etc.
Can be implemented in a class by ``__iadd__``, `__idiv__`` functions and so on.
* Annotation - are optional and on their own do not mean anything, but can be used but third libraries or IDEs for verification
  * vairiable: e.g. ``primes: List[int] = []`` or ``name: str``
  * functions: ``def sum(a: int, b: int) -> int``
* Decorators - a callable that takes another function as an argument (the decorated function). 
They can replace the decorated function with another one or perform a check or verification before the decorated function is run.
The decorators are executed imeediately after the decarated funciton is defined, so usually when the module is imported. Syntex:    
```python
@deco
def func(args):
  pass
  
func = deco(func)
```
* Simple generators - the function can retrurn a generator, that will be used to generated a new value
every time that the function is called. This can be implemented with a use of a word ``yield``:  
```python
def fib():
a, b = 0, 1 #this is initialization, happens only once
while 1:
  yield b # every time the fib() is called, w new value for b will be return
  a, b = b, a+b # an update that is performed after evry call
```
  
