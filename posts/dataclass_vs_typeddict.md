# Dataclass vs TypedDict in Python

Type annotations were introduced to Python with [PEP484](https://peps.python.org/pep-0484/) and since then they have been a staple tool for most Python programs. They open up Python code to easier static analysis and refactoring and potential runtime type checking. Not to mention making the code a lot easier to reason with in general.

Take the following example:

```python
def hello(a):
	return "hello " + a

hello(2)
```

The above code would rase this error:
```python
TypeError: can only concatenate str (not "int") to str
```

However, using type annotations would help us catch this error before the code is even executed:

```python
def hello(a: str) -> str:
	return "hello " + a

hello(2)
```

Trying to compile this code with mypy would yield the following error:
```
error: Argument 1 to "hello" has incompatible type "int"
```

Of course, using a good IDE or text editor with good static analysis tools would help us catch this error while writing the code. 

Obviously, Python type annotations work with classes as well. For example:
```python
import requests
from typing import List

class Book:
	title: str
	authors: List[str]

def get_book_from_api(endpoint: str) -> Book:
	response = requests.get(endpoint)
	response_json = response.json()
	book = Book()
	book.title = response_json["title"]
	book.authors = response_json["authors"]

	return book
```

In the example above, the `Book` class works as a wrapper around some data we get from a rest endpoint. It contains two fields, a title field and an authors field. Using Python type annotations we have defined the types of both those fields. However, assigning the values to each field one by one is a bit annoying, and the OOP way to do that would be to assign attributes inside the `__init__` method:

```python
class Book:
	title: str
	authors: List[str]

	def __init__(self, title: str, authors: List[str]):
		self.title = title
		self.authors = authors

def get_book_from_api(endpoint: str) -> Book:
	response = requests.get(endpoint)
	response_json = response.json()
	book = Book(title=response_json["title"], authors=response_json["authors"])

	return book
```

This is great, we're making important steps here. We managed to create a Python class that uses type annotations and can be constructed using the `__init__` method. But this class is still just a wrapper around some data, so it would be great if we can reduce the boilerplate for this code while still keeping all the advantages that the Python type system gives us

This is where `dataclass` becomes useful. It's a Python decorator that automatically adds the `__init__` method (including types!) and some additional goodies to a Python class. It can be used as follows:

```python
import requests
from dataclasses import dataclass
from typing import List

@dataclass
class Book:
	title: str
	authors: List[str]

def get_book_from_api(endpoint: str) -> Book:
	response = requests.get(endpoint)
	response_json = response.json()
	book = Book(title=response_json["title"], authors=response_json["authors"])

	return book
```

As you we can see, there's no need to manually define the `__init__` method anymore. Not only does this reduce the time needed to write classes since it reduces the amount of boilerplate code, it also reduces the probability of errors since there's less boilerplate code for us to type in.

However, dataclasses are not a silver bullet and have some drawbacks. But in order to understand those drawbacks we must first introduce the notion of [duck typing](https://en.wikipedia.org/wiki/Duck_typing). The idea behind it is simple: if it quacks like a duck and walks like a duck then it's a duck. In other words, an object is of a particular type if it has all the methods and attributes required by that type.

Duck typing cannot be followed with dataclasses though. Consider the following example:

```python
@dataclass
class Table:
	color: str
	age: int

@dataclass
class Bookshelf:
	color: str
	age: int

Table(color="brown", age=12) == Bookshelf(color="brown", age=12) # False
```

Following the logic of duck typing, then both the `Table` and the `Bookshelf`  objects should represent the same type since they have the same attributes and the same values. However,  they are not treated as equal objects because they belong to different classes. In order to strip that difference between both objects we have to resort to dictionaries:

```python
{"color": "brown", "age": 12} == {"color": "brown", "age": 12} # True
```

But using Python dicts would make us lose all the advantages that Python types gives us, right? Wrong. This is where `TypedDict` comes into play. `TypedDict` basically allows Python dicts to work as types:

```python
from typing import TypedDict

class Table(TypedDict):
    color: str
    age: int

class Bookshelf(TypedDict):
    color: str
    age: int

Table(color="brown", age=12) == Bookshelf(color="brown", age=12) # True
```

However, even though `TypedDict` can give us all the advantages of duck typing, it has two big disadvantages compared to dataclasses:

1. Validation is harder
2. Not immutable

Let's start with the first point. Since dataclasses can have explicit `__init__` methods, then they can be validated whenever an object of that type is constructed. This is not true for `TypedDict` and every time we construct a dictionary of a specific type, then we'd have to validate all its values at runtime.

Whereas for immutability, in dataclasses its ensured by the `frozen=True` argument. If set, it ensures that any object that belongs to that type would be immutable, which is not possible with `TypedDict`.