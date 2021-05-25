---
layout: single
title:  "Python Decorators"
date:   2021-05-25 23:00:00 -0500
categories: Programming
comments: true
typora-root-url: ..
---

This is the third part of my 3 part series about Python Features I didn't know about. The first part can be found [here](https://georgeciesinski.github.io/programming/Python-Features-Elusive/) and the second part here. This blog covers some basics about decorators. 

Decorators let you add new functionality to an existing function without modifying its structure. 

# Built-in Method Decorators

Before we learn how to make our own decorators, there are a few built-in ones we can look at. You can use these decorators on your methods to change the way Python lets you use them. 

## Three types of methods

There are three types of methods. An **instance method** has no decorators, and it is the method you usually use in object-oriented programming. This requires an instance to be created to be used, and has information about the instance. Next is a **class method**. This is a method with the @classmethod decorator, and has information about the class. Finally, there is the **static method**, which has the @staticmethod decorator. This one has neither the information about the instance or the class. 

Instance methods are used for most things. They often use date that is in the object, and use `self` frequently to access this data.

## @classmethod

This decorator lets you use the method without passing an instance of the class to it. The class method requires you to pass the class (`cls`) as the argument:

```python
class ClassName:

	def instance_method(self):
		print(f"This is the instance method of {self}")
		
	# Use @classmethod decorator and use the argument cls instead of self
	@classmethod
	def class_method(cls):
		print("This is the class method of {cls}")
```

```python
# 1. Calling the instance_method requires you to create an instance first
new_instance = ClassName()

# Invoking the instance_method from the new instance
new_instance.instance_method()

This is the instance method of <__main__.ClassName object at 0x7fb460780790>

# Invoking the instance_method from the class and passing the new instance
ClassName.instance_method(new_instance)

This is the instance method of <__main__.ClassName object at 0x7fb460780790>
```

```python
# 2. Calling the class instance does not require an instance to be created first
new_class.class_method()

This is the class method of {cls}
```

Class methods are used like factories, which I will explain later in this section. 

## @staticmethod

A static method isn't really a method, which is another name for a function of a class. It is more of a function <u>in</u> a class. This doesn't have information about the instance or the class. 

```python
ClassName:
	@staticmethod
	def static_method():
		print("This is a static method.")

		
ClassName.static_method()

This is a static method.
```

If you have a method that feels like it belongs in a certain class, even if it doesn't use the instance or class data, then that is when you would use a static method.

## Using @classmethod as a factory

Sometimes, you might not want to use the `__init__()` method to create a new object. In the below example, you can create a new Train and create an object no matter what you pass as train_type, even if it isn't one of the types. 

```python
class Train:

	types = ("Steam Train", "Maglev")
	
	def __init__(self, name, train_type, speed):
		self.name = name
		self.train_type = train_type
		self.speed = speed
	
	def __repr__(self):
		return f"<Train {self.name}, {self.train_type}, {self.speed}kph>"
	
	@classmethod
	def steam_train(cls, name, speed):
		return cls(name, cls.types[0], speed)
	
	@classmethod
	def maglev(cls, name, speed):
		return cls(name, cls.types[1], speed)

	
new_steam_train = Train.steam_train("Line 17", 70)
new_maglev = Train.maglev("Line 501", 500)

print(new_steam_train)
<Train Line 17, Steam Train, 70kph>

print(new_maglev)
<Train Line 501, Maglev, 500kph>
```

Of course, you can also use an `if statement` to make sure it is an allowed type, but you can also use a class method to call the class itself and to pass an accepted type in right away. 

# Building your own Decorators

Now that you are familiar with the built-in decorators, you can learn how to make your own.

## Simple Decorators

The below example shows how you can add some security functionality to an existing function: 

```python
# make_secure() is the decorator, while secure_function() is what is replacing func. 
# make_secure() adds functionality to func which (cont in next comment...)
def make_secure(func):
	def secure_function():
    # Checks if user is an admin, and returns the function
		if user['role'] == 'admin':
			return func()
    # And prints message if the user is not an admin
		else:
			print(f"{user['name']} does not have admin rights.")
	return secure_function

# Basic function which logs user in
def root_login():
	print(f"{user['name']} has logged in successfully.")

# Overwrite root_login function by passing it to make_secure decorator
root_login = make_secure(root_login)

# Attempt to call function with a guest role
user = {
  'name': 'Billy',
  'role': 'guest'
}

# Login fails
root_login()
Billy does not have admin rights.

# Attempt to call function with an admin role
user = {
  'name': 'Billy',
  'role': 'admin'
}

# Login successful
root_login()
Billy has logged in successfully.
```

## Using @ syntax for decorators

The @ syntax lets you use a decorator without overwriting the original function explicitly. To show you what I mean, I will recreate the previous example using the @ syntax. Notice that `root_login = make_secure(root_login)` is no longer needed:

```python
# Adds functionality to func which (cont in next comment...)
def make_secure(func):
	def secure_function():
    # Checks if user is an admin, and returns the function
		if user['role'] == 'admin':
			return func()
    # And prints message if the user is not an admin
		else:
			print(f"{user['name']} does not have admin rights.")
	return secure_function

# Automatically overwrites root_login by using above decorator
@make_secure
def root_login():
	print(f"{user['name']} has logged in successfully.")

# Attempt to call function with a guest role
user = {
  'name': 'Billy',
  'role': 'guest'
}

# Login fails
root_login()
Billy does not have admin rights.

# Attempt to call function with an admin role
user = {
  'name': 'Billy',
  'role': 'admin'
}

# Login successful
root_login()
Billy has logged in successfully.
```

## Decorators and function names

Decorators rewrite the function's name within Python when they replace the original function. This can affect some libraries who refer to the function's name, so it is important to keep this in mind. For example, if I run the below line on the `root_login()` function in my above example, you will see that the name is not `root_login` any more:

```python
print(root_login.__name__)

secure_function
```

It has been renamed within the decorator to `secure_function` instead. This also affects any documentation that might exist for the original function, as it is now replaced with secure_function.

This can be resolved with a built-in Python library called `functools` as well as another decorator:

```python
# Functools library must be imported
import functools

def make_secure(func):
  # Decorator informs python that the name should be the same as func
  @functools.wraps(func)
	def secure_function():
		if user['role'] == 'admin':
			return func()
		else:
			print(f"{user['name']} does not have admin rights.")
	return secure_function
```

Now if we check the name of the function again, we get:

```python
print(root_login.__name__)

root_login
```

You will need to use this on every decorator that you write.

## Decorating functions with parameters

Sometimes, the function you want to decorate has parameters. Lets say you have: 

```python
def root_login(department):
	if department == 'finance':
		print(f"{user['name']} has logged into finance system successfully.")
	if department == 'development':
		print(f"{user['name']} has logged into development console successfully.")
```

You can account for this issue by adding those parameters into the decorator, but this is **bad practice** because the decorator will be coupled with that specific function and will not work with other functions that are missing this parameter.

```python
# Example of BAD PRACTICE function
def make_secure(func):
  @functools.wraps(func)
  # Added department to secure_function() and func()
	def secure_function(department):
		if user['role'] == 'admin':
			return func(department)
		else:
			print(f"{user['name']} does not have admin rights.")
	return secure_function
```

Instead, you should make those funcitons take in unlimited number of arguments by adding `*args, **kwargs`:

```python
def make_secure(func):
  @functools.wraps(func)
  # Added *args, **kwargs
	def secure_function(*args, **kwargs):
		if user['role'] == 'admin':
			return func(*args, **kwargs)
		else:
			print(f"{user['name']} does not have admin rights.")
	return secure_function
```

Doing this yields the expected result: 

```python
root_login('finance')

Billy has logged into finance system successfully.

root_login('development')

Billy has logged into development console successfully.
```

## How to make decorators with parameters

This part is pretty weird, and a bit complicated. Lets say you want to want to have two functions. One logs admins into root, and the other logs in users regularly:

```python
def root_login():
	print(f"Admin {user['name']} has logged into root successfully.")
  
def regular_login():
  print(f"User {user['name']} has logged in successfully.")
```

We want Admins to access the `root_login()` function, Regular Users to access the `regular_login()` function, and guests to be able to access neither. To do this, we need to create a decorator factory - a function that creates decorators by taking in a parameter:

```python
# make_secure() creates the below decorator
# make_secure takes in access_level parameter passed in by any function which calls it
def make_secure(access_level):
	def decorator(func):
  	@functools.wraps(func)
		def secure_function(*args, **kwargs):
      # secure_function will check user's role across the access_level
			if user['role'] == access_level:
				return func(*args, **kwargs)
			else:
				print(f"{user['name']} is unable to login.")
		return secure_function
  return decorator

# Call @make_secure with 'admin' parameter
@make_secure('admin')
def root_login():
	print(f"Admin {user['name']} has logged into root successfully.")

# Call @make_secure with 'regular_user' parameter
@make_secure('regular_user')
def regular_login():
  print(f"User {user['name']} has logged in successfully.")
```

Now lets see the results if we try to login as different users:

```python
# Attempt to login as guest (root_login and regular_login fail)
user = {
  'name': 'Billy',
  'role': 'guest'
}

root_login()

Billy is unable to login.

regular_login()

Billy is unable to login.

# Attempt to login as regular_user (root_login fails, regular_login works)
user = {
  'name': 'Nancy',
  'role': 'regular_user'
}

root_login()

Nancy is unable to login.

regular_login()

User Nancy has logged in successfully.

# Attempt to login as an admin (root_login works, regular_login fails)
user = {
  'name': 'Agent Smith',
  'role': 'admin'
}

root_login()

Admin Agent Smith has logged into root successfully.

regular_login()

Agent Smith is unable to login.
```

# Conclusion

This wraps up what I learned about decorators. To be honest, some of this stuff is pretty complicated and I still don't think I can use this without referring back to my blog later. I hope this helps somebody out there who is trying to learn about decorators. 

See ya on my next blog/series!
