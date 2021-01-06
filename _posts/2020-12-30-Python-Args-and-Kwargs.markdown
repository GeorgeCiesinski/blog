---
layout: single
title:  "Python Args and Kwargs"
date:   2020-12-31 23:00:00 -0500
categories: Programming Python
comments: true
typora-root-url: ..

---

# Python *args and *kwargs

Yesterday I was tasked with taking a class method my colleague created, and creating a universal method that all classes can access. This way, instead of being repeated dozens of times across many different modules, it can be written once in a single place, and can be modified easier in the future. This task would have been quite easy under normal circumstances, but this class method made use of **\**kwargs**, while the modules that called this class method relied on ***args**. My colleague tried to explain the concept of ***args** and **\**kwargs** to me, but my brain was refusing to cooperate and I understood none of it. I decided to make this a topic of today's blog as I figure it out.

# What are *args and **kwargs

***args** and **\**kwargs** are a way of passing variables to a method. First and foremost, it is not necessary to write the names ***args** and **\**kwargs**. The asterisks (one for args, two for kwargs) is what denotes whether the method is expecting args or kwargs. The name can be written as anything. For instance, instead of writing args and kwargs, you can name it var and vars, or anything else, so long as they have the correct amount of asterisks for the type of variables expected.

***args** is short for arguments, and is used in function definitions to indicate an unspecified number of arguments. This allows you to write functions that can be called with 3 variables, or 5 variables, without having to rewrite the function definition. It is similar to a list.

**\**kwargs** is short for keyword arguments and is similar to ***args** except that it indicates an unspecified number of key value pairs. It is similar to a list.



## Unpacking Operators - How *args and **kwargs work

A key to understanding args and kwargs is that * unpacks iterable objects such as tuples, lists and strings, and ** unpacks dictionaries. 

### * Operator

Below is an example of how * can be used to unpack an iterable: 

```python
numbers = [1, 2, 3, 4, 5]
print(*numbers, 6, 7, 8, 9)
```

Results in the console output: 

```
1 2 3 4 5 6 7 8 9
```

Here is one more example but using a string, which is iterable:

```python
def print_chars(*string):
	for char in string:
		print(char)
    
string = "Hello"
print_chars(*string)
```

Which outputs:

```
H
e
l
l
o
```

This works because `print_chars(*string)` is expecting an args called \*string. In other words, it is expecting an unknown number of variables. `print_chars(\*string)` first unpacks the string into multiple variables representing the characters and sends them to the function, which iterates through each character and outputs it.

### \*\* Operator

The ** operator can similarly be used to unpack dictionaries. Consider the below example:

```python
def print_student(name, age, student_number):
	print(f"The student's name is {name}, their age is {age}, their student number is {student_number}")
	
student = {
	'name': 'Mr. Student',
	'age': 80,
	'student_number': 123456
	}

print_student(**student)
```

The output is:

```
The student's name is Mr. Student, their age is 80, their student number is 123456
```

In this example, we defined a function named `print_student()` which is expecting a name, age, and student_number. We also created a dict called "student" with those exact variables set as the keyword. This allows us to call `print_student()` and pass `**student` which unpacks the dict into keyword arguments before calling the function. The function recognized these keyword arguments as matching it's definition, and carries out the desired function. 

# How to use *args

In this section, I wanted to talk about the most basic use of ***args**. There are many other ways to use it where the Python documentation may need to be referenced, but below I have covered the basic concept and usage.

## Defining the function to accept *args

Below is a basic function that takes in a regular argument (reg_arg), and ***args**, which indicates an unspecified number of additional arguments: 

```python
def args_example(reg_arg, *args):
	print(reg_arg)
	for arg in args:
		print(arg)
```

When calling a function like this, the regular variables are accounted for first, and in order. For instance, if we were to call:

```
args_example("Regular Argument", "another argument", "third argument")
```

The first argument "Regular Variable" would be accepted as reg_variable in the function. This would continue for each explicitly defined variable. Since there are no other regular variables defined and the function definition includes ***args**, all additional variables would be taken in as ***args**. These can be iterated through like a regular list: 

```python
for arg in args:
	do_thing(arg)
```

I also want to stress that the asterisks is only needed in the function definition, but is not needed within the function.

## Calling a function and passing *args

In this first example, we will call the method we decribed above by passing the expected reg_variable, and two additional variables: 

```python
regular_argument = "This is a regular argument"
x = 1
y = 2
z = 3
args_example(regular_argument, x, y, z)
```

As expected, the function prints out reg_arg, then it iterates through the args and prints them out: 

```
This is a regular variable
1
2
3
```

In this second example, we will call the same function with one additional variable and without changing the first function: 

```python
w = 0
args_example(regular_argument, w, x, y, z)
```

And the console printout looks like this: 

```
This is a regular argument
0
1
2
3
```

This is the benefit of using ***args**. It allows us the flexibility to define a function without knowing how many arguments we may need to send to it.

# How to use **kwargs

Below I have created another example function. This time it does not take in any regular arguments, but does take in **\**kwargs**. It then iterates through the **\**kwargs** and extracts each key and corresponding value before formatting it and printing it to console: 

```python
def display_person(**kwargs):
    for key, value in kwargs.items():
        output = f"The person's {key} is {value}."
        print(output)
```

This function can be called as so:

```python
display_person(name="Joe", age=28, weight="200 lbs", height="180cm")
```

Which results in the console printout: 

```
The person's name is Joe.
The person's age is 28.
The person's weight is 200 lbs.
The person's height is 180cm.
```

# Different ways to use *args and **kwargs 

There are many different ways to use ***args** and **\**kwargs**. I will not be covering every way to use them or the accepted conventions, although you can read more about this by checking out [this blog](https://treyhunner.com/2018/10/asterisks-in-python-what-they-are-and-how-to-use-them/) by Trey Hunter, who is far, far more talented when it comes to Python than I am. 

Instead, I wanted to discuss using * and ** in both the function definition, and when calling the function. 

## Using * when calling the function

In this first example, we will define a function expecting three arguments. These arguments are then printed individually. 

```python
def print_arguments(x, y, z):
	print(x)
	print(y)
	print(z)
  
to_print = 1, 2, 3
print_arguments(*to_print)
```

The `to_print = 1, 2, 3`  line creates a tuple that looks like: `(1, 2, 3)`. The second line calls `print_arguments()` and passes the items within `to_print` by first unpacking it with the `*` operator. This results in the console printout: 

```
1
2
3
```

## Using * when defining a function

We can achieve the same behaviour by defining the function and calling it a bit differently:

```python
def print_arguments(*args):
	for arg in args:
		print(arg)

print_arguments(1, 2, 3)
```

This also results in the console printout: 

```
1
2
3
```

In this example, we are not creating a tuple like `(1, 2, 3)` but are calling the `print_arguments()` function and passing 1, 2, and 3 as individual arguments. The `*args` in the function definition is expecting multiple arguments which it takes in as an iterable. We then iterate through the items in this iterable and print them, resulting in the same behaviour as "Using * when calling the function". 

# Creating a function with regular arguments, *args, and *kwargs

One of the most interesting things about defining a function with *args and **kwargs is that the function can intelligently determine what to do with the arguments you pass to it, depending on which definition you use. 

Below is a function that is using an explicitly defined `name` variable, a tuple containing scores, and a dictionary containing student details. It then prints everything, iterating through the iterables:

```python
def output_student_details(name, *scores, **details):
	print(f"The student's name is: {name}")
	print(f"The student's scores are: ")
	for score in scores:
		print(score)
	for key, value in details.items():
		print(f"The student's {key} is {value}")
    
name = "Joseph"
scores = 80, 80, 85, 90, 65
details = {
	'age': 32,
	'student number': 123456,
	'address': '123 Fake St.'
	}

output_student_details(name, *scores, **details)
```

Amazingly, the function is able to correctly interpret all of the arguments and print them out accordingly: 

```
The student's name is: Joseph
The student's scores are: 
80
80
85
90
65
The student's age is 32
The student's student number is 123456
The student's address is 123 Fake St.
```

# Conclusion

So this covers many of the basics of using *args and **kwargs. As I mentioned earlier in this post, there are so many different ways they can be used. Trey Hunter talks about many different applications, most of which are more complex than any of the examples I have listed. 

This has been a really fun post to make and has made me feel much more comfortable writing code with these operators. I also discovered that I learn really well when I create blog posts like this. I don't know if this will end up helping anybody in the future who was stuck like I was, but it certainly helped me! I will continue to create and post these blogs in the future, so look out for more of these! 

Thank you for stopping by, and happy new years!

