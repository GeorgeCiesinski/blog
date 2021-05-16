---
layout: single
title:  "Python Features I didn't know about"
date:   2021-05-16 23:00:00 -0500
categories: Programming
comments: true
typora-root-url: ..
---

As usual, it has been a while since my last post. I have been devoting all of my free time to programming instead of blogging! Today, I finally found a good reason to write a new blog. I recently picked up my first few Udemy courses, including [REST APIs with Flask and Python](https://www.udemy.com/course/rest-api-flask-and-python/) by [Jose Salvatierra](https://www.udemy.com/course/rest-api-flask-and-python/#instructor-1). The first section of this tutorial is "A full Python refresher" and covers what I thought were Python basics. I was going to skip this tutorial, but one of the first few videos in the tutorial said to click through the videos even if you think you know the stuff. Well, Jose was right. I was very surprised by the Python basics I thought I knew but somehow completely missed. This blog is about those things. It is mostly random Python features, and isn't centered around any single topic. Without wasting time, I will get started!

# Lists, Tuples, Dicts...... and Sets? 

The first thing that absolutely blew my mind is known as **sets**. This one shooketh me. I don't know how, but I completely overlooked sets when I was learning Python. Before I get to sets, they are a way to store multiple values in a single variable, similar to lists, tuples, and dicts. I will cover those briefly below, then talk about sets.

As you might know, you can make a list with square brackets:

```
list = ['this', 'is', 'a', 'list']
```

You can make tuples similarly but with regular brackets:

```
tuple = (1, 2, 3)
```

 Lists and tuples are similar. Both can be iterated, and the individual elements inside are ordered, so they can be accessed with an index: 

```
item = list[0]
item = tuple[0]
```

The main difference is that a list is mutable where the values can be changed, while a tuple is immutable. I wont dive too deep into these. 

Dictionaries or 'dicts' are another way of storing multiple pieces of data in a variable. They can be created with curly brackets: 

```python
dict = {
	'key': 'value',
	'another_key': 'another_value',
	'an_int': 5
}
```

Dicts are "key-value pairs" which means they contain a key, and a value, for each item. They are mutable and unordered, which means that they can be changed, but cannot be accessed with an index. Instead, the value is accessed with the key, as so: `value = dict['key']`. 

## What is a set?

So far, all of this should ring a bell if you have learned Python. What I didn't know is that there is another way to store multiple values known as a **set**. A set also uses curly brackets, but stores single values like a list or tuple, instead of key-value pairs like a dict: 

```
set = {'this', 'is', 'a', 'set'}
```

Sets are mutable, so they can be changed, but they are unordered, so they cannot be accessed with an index. Instead, you need to use methods such as `.add()` or `.remove()` to add or remove individual values. Additionally, they cannot contain duplicates. If you try to do:

```python
friends = {'john', 'adam', 'mary'}
friends.add('john')
```

Then the set will still only contain john, adam, and mary. 

## Set Operations

There are some powerful operations you can perform with sets. I will cover these below: 

### Union

Lets say you have two sets called `men` and `women`:

```python
men = {'bob', 'john', 'jerry'}
women = {'mary', 'stacy', 'tara'}
```

You can combine them into a new set called `friends` with the `union` function:

```python
friends = men.union(women)
print(friends)

{'bob', 'john', 'jerry', 'mary', 'stacy', 'tara'}
```

### Difference

What if we have a set with all of our friends, and another set containing those who are gamers:

```python
friends = {'bob', 'john', 'jerry', 'mary', 'stacy', 'tara'}
gamers = {'bob', 'stacy'}
```

We can perform `difference` to find out who isn't a gamer: 

```python
non_gamers = friends.difference(gamers)
print(non_gamers)

{'john', 'jerry', 'mary', 'tara'}
```

### Intersection

What if we have two sets for people who `drive_stick` and `drive_automatic`:

```python
drive_stick = {'bob', 'mary', 'stacy'}
drive_automatic = {'bob', 'stacy', 'tara'}
```

We could extrapolate who can drive using both using `intersection`

```python
drive_both = drive_stick.intersection(drive_automatic)
print(drive_both)

{'bob', 'stacy'}
```

# Lambda Functions

Lambda functions are a concept I heard of but never learned. I am not a huge fan of this if I have to be honest, but many programmers claim there are benefits that lambdas provide. In short, lambda functions are also known as *anonymous functions*, and basically are functions that can be used without a name. A very basic lambda function looks like: 

```python
lambda x, y: x + y
```

This lambda function defines variables `x, y` before the colon, and the function `x + y` after it. Unfortunately, it is not usable in this form. There are a couple ways to make it usable.

## Named lambda function

One way is to use it by naming it. You can then call it like a regular function: 

```python
add_nums = lambda x, y: x + y
add_nums(1, 2)

3
```

## In line lambda function

Another way to do this is to use it in line. To do this, you must wrap the lambda function in brackets, and put the expected variables in another set of brackets beside it: 

```python
(lambda x, y: x + y)(1, 2)

3
```

## Utility of lambda functions

Upon learning this, I was wondering what is the point of one. To me, it almost seems to make the function less readable. You need to understand lambda notation, and what would otherwise be an easy to read code block is compressed down to a line. This said, there are some very useful ways to use it. 

One way is it can shorten code. For example:

```python
# This filters all of the even numbers from the list using the lambda function in only 1 line (not including print statement)
even_nums = list(filter(lambda x: x%2 == 0, [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]))
print(even_nums)

[2, 4, 6, 8, 10]

# The regular way is longer
def filter_even_nums(x):
	return x % 2 == 0

even_nums = list(filter(filter_even_nums, [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]))
print(even_nums)

[2, 4, 6, 8, 10]

```

Another way is that it can return a function, creating it dynamically: 

```python
# Creates a multiplication function dynamically
def dynamic_multiply(n):
	return lambda x: x * n

# Create a doubling function by passing 2 to dynamic_multiply
double_func = dynamic_multiply(2)
# Doubles 10
double_func(10)

20

# Create a tripling function by passing 3 to dynamic_multiply
triple_func = dynamic_multiply(3)
triple_func(10)

30
```

There are many other utilities, but these are the ones I found most useful and most interesting. 

# Packing and Unpacking Args and Kwargs

This is a feature I have used before, but I still wanted to cover it as many people aren't aware about it. I could also use a bit of a refresher on the various ways to use it.

## Args

The `*` symbol can be used to unpack elements such as tuples or strings. This is known as args or "arguments", although it can be given any name as long as the asterix is used.

```python
nums = [1, 2, 3, 4, 5]

# Prints out the nums list
print(nums)

[1, 2, 3, 4, 5]

# Unpacks the list into individual ints and prints those out
print(*nums)

1 2 3 4 5
```

This can be used to pass values to functions as well: 

```python
add_nums(x, y): 
	return x + y

nums = [5, 7]

# Unpacks nums into two variables and calls add_nums
print(add_nums(*nums))

12

```

It is important to note here that if there are more items in this list than the function has arguments, then this will result in an error as the function will detect more variables than it has arguments.

This can also be used to write a function with an unknown amount of arguments. Using the args indicator in the function definition packs the variables into a tuple. For example: 

```python
# Add_nums packs the variables into a tuple, then loops through them with a for loop in the function
def add_nums(*args):
	total = 0
	for arg in args:
		total += arg
	return total

# Call add_nums and pass 3 variables
print(add_nums(1, 2, 3))

6

# Call add_nums and pass 4 variables
print(add_nums(1, 2, 3, 4))

10

```

## Kwargs

This is similar to args but is known as "keyword arguments".

For example, when used in a function definition, it packs the kwargs into a dict: 

```python
# Packs kwargs into a dict and prints it
def print_kwargs(**kwargs):
	print(kwargs)
  
print_kwargs(name='bob', surname='bobington', age=29)

{'name': 'bob', 'surname': 'bobington', 'age': 29}
```

When used while calling a function, it unpacks the dict into variables:

```python
def print_details(name, surname, age):
	print(name, surname, age)

# Define dictionary with the same keys as the args in print_details()
details = {
	'name':'bob',
	'surname':'bobington',
	'age': 29
	}

# Unpacks dict into keyword arguments
print_details(**details)
bob bobington 29
```

# Magic or Dunder Methods (Object-oriented Programming)

Magic methods (also known as Dunder Methods), and are a part of object-oriented programming. These are special methods that start or end in double underscores. These are not invoked by you, but an invocation happens automatically on a certain action. 

## `__init__()`

The `__init__()` method runs automatically when a class is created. It is often used to initialize some values of the object. 

```python
class Friend:
	
	def __init__(self, name, age):
		self.name = name
		self.age = age
		
new_friend = Friend('Bob', 25)
print(new_friend.name, new_friend.age)

Bob 25
```

## `__str__()`

The `__str__()` method tells the class what to output as a string representation. The best way to understand what this means is to see the two examples below:

```python
# 1. Basic class with no __str__() method
class Friend:
  def __init__(self): 
		pass
  
new_friend = Friend()
print(new_friend)

# Output is a string representation of the object
<__main__.Friend object at 0x7fa2b8780220>
```

```python
# 2. Class with an __str__() method
class Friend:
  
	def __init__(self):
		pass
  
	def __str__(self):
		return "Friend Object"

	
new_friend = Friend()
print(new_friend)

# Output is the defined output in the __str__() method
Friend Object
```

## `__repr__()` 

The `__repr__()` method outputs a representation of the object, and if possible, it should be able to reproduce the object from this output. 

```python
class Friend:
	
  def __init__(self, name, age):
		self.name = name
		self.age = age
    
  # Defines a string representation of the object output
	def __repr__(self):
		return f"<Person('{self.name}', {self.age})>"

	
new_friend = Friend('Nick', 20)
print(new_friend)

<Person('Nick', 20)>
```

It is important to note that if there is an `__str__()` method, then this will override the print output, so to test it, you should comment out the `__str__()` method temporarily. This is frequently used in debugger output.

# Conclusion

I wanted to wrap these ideas up as this post has become quite long and full of code examples. I will probably create one more blog post to summarize any other interesting points from "A full Python refresher" that I come across. As for this post, I hope that it better explains some of the ideas, especially if you heard of them and use them but don't fully understand them. This was definitely the case for me.

Thanks for checking out my post! See ya in the next one.

