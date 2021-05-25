---
layout: single
title:  "More Python Features I didn't know about"
date:   2021-05-24 12:00:00 -0500
categories: Programming
comments: true
typora-root-url: ..
---

A few days ago I made part 1 of this series [here](https://georgeciesinski.github.io/programming/Python-Features-Elusive/) about Python features I didn't know about. The post was getting quite long, so this is a continuation of the previous post. Some of these things I know, but want to get a better understanding of. Others, I may have never seen before. You can also find part 3 [here](https://georgeciesinski.github.io/programming/Python-Features-Decorators/). 

# Class Inheritance

This will be a brief one. Like in other languages, Python's OOP (object oriented programming) includes class inheritance. This means that you can make a super class, as well as subclasses that are children of this super class. The subclass can call super class methods, including the `__init__()` method:

```python
class Computer:
	
	def __init__(self, cores, ram, hard_drive, ip):
	
		self.cores = cores
		self.ram = ram
		self.hard_drive = hard_drive
    self.ip = ip

  def ping_computer(self):
    
    print(f'Pinging {self.ip}')
  
# Subclass has the superclass in brackets
class Macbook(Computer):
	
  # Init method can call the super().__init__ method, and can instantiate additional variables
	def __init__(self, cores, ram, hard_drive, ip, model):
	
		super().__init__(cores, ram, hard_drive, ip)
		self.model = 'Macbook'
	
new_mac = Macbook(4, 8, 250, '192.168.44.67', 'Macbook Air')

# Subclass can call super class methods
new_mac.ping_computer()
```

# Using Composition instead of Class Inheritance

Sometimes you might have related classes, but cannot use inheritance because the classes are not sufficiently similar. For example, you might have a garage class which keeps track of the number of vehicles in it: 

```python
class Garage:
	
	def __init__(self, quantity):
		self.quantity = quantity
		
	def __str__(self):
		return f'The garage has {self.quantity} cars.'
```

The class Car might be related to the garage, but a car does not have a quantity. If you inherited the `__init__()` method, then you would have to supply a quantity. Instead, you can use composition:

```python
class Garage:
	
	def __init__(self, *cars):
		self.cars = cars
		
	def __str__(self):
		return f'The garage has {len(self.cars)} cars.'

# No need to provide a superclass
class Car:
	
  # Doesn't use Garage's init method
	def __init__(self, license_plate):
		self.license_plate = license_plate
		
	def __str__(self):
		return f'Car: {self.license_plate}.'
  
# Create car instances  
car_1 = Car('BMYW-556')
car_2 = Car('CWTW-300')

# Create a new garage instance, and pass the cars
garage = Garage(car_1, car_2)

print(garage)

>> The garage has 2 cars.
```

# Custom Error Classes

You can create a custom error by creating a new class with either `Exception` as the superclass, or a custom exception if there is a similar one. 

```python
# You can write pass for the error details as the ValueError already has the required methods and details
class TooManyCarsError(ValueError):
  pass

if parked_cars > allowed_cars:
  raise TooManyCarsError("Unable to park any more cars in the garage.")
```

# First Class Functions

A first class function is a function you pass into another function like you would any object or variable. If a function can be passed into another function, or returned by a function, it is said to be first class. 

```python
# Takes in any number of values and sums them
def add(*values):
  
	sum_of_values = 0
  
	for value in values:
		sum_of_values += value
    
	return sum_of_values

# Takes in any number of values and the operator to use (example: add)
def calculate(*values, operator):
	return operator(*values)

# Call the calculate function. Pass in variables, and the name of the first class function to use without the ()
result = calculate(1, 2, 3, operator=add)

# The function is called and the variables are passed in to get an expected result
print(result)

>> 6
```

# Mutable Parameters & Optional Parameters

This section will just be about why you shouldn't use mutable parameters, and how you can get around it. 

First, I just want to briefly touch on the `typing` library. Instead of using Python's native `list` type, I will be doing something like this: 

```python
from typing import List
```

This library adds a lot more functionality to the types, and supports the various PEP standards. Without further adieu:

```python
from typing import List

class Car:
	def __init__(part_list: List[int] = []): # Very bad, don't do this
  	self.part_list = part_list
```

The problem with using mutable parameters is that the values in the functions are evaluated when it is defined, not when it is called. If you create two different cars (car A and car B) without passing `part_list`, then `self.part_list` will refer to the same list. If you modify the part list for car A, car B's `part_list` will also be modified becuase the name is pointing at the same list. 

This is where optional parameters come in. Instead, we can write this function as so: 

```python
# Import Optional from typing as well
from typing import List, Optional

class Car:
	def __init__(part_list: Optional[List[int]] = None):
  	self.part_list = part_list or []
```

This way, the `__init__()` method doesn't create an empty list as soon as it is evaluated. If there is no list, it assigns it the value `None`. The `Optional` type informs Python that this argument is optional and evaluates explicitly to `None` if it is not provided. Once `self.part_list` tries to evaluate, it will check if a `part_list` is provided, and will create an empty list if not. 

This is far safer, and will prevent objects from sharing the same mutable objects. 
