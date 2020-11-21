---
layout: single
title:  "Data Visualization with Python"
date:   2020-11-21 23:00:00 -0500
categories: Programming Python Data
comments: true
---

Hi everyone/noone. I realize that probably nobody reads these blog posts at this time, but I have found enormous value in writing these posts as I have been learning a lot by writing these. Today's post is about visualizing data using Python, Anaconda, Matplotlib, Seaborn, & Bokeh. The information in this post is based off of a number of sources including "The Complete Python Data Visualization Course" tutorial by Zenva, wikipedia for definitions, and some other supplemental sources whenever I have a question the tutorial doesn't answer. 

The purpose of this post will be to learn the basics about plotting data with Python. 

# Data Science

The process of using scientific methods, processes, and algorithms to extract insights from structural and unstructured data is known as Data Science. It is utilized in data mining, machine learning, and big data. Data science combines principles from many fields including mathematics, statistics, computer science, and much more to help us understand and analyze phenomena with data. 

# Tools

## Anaconda

A python and r programming distribution for scientific computing (data science, machine learning, etc) that simplifies package management and deployment. It includes data science packages for all of the common operating systems. The packages in Anaconda are managed by the package management system **conda**.

## Spyder

An IDE for python.

## Jupyter

A popular application used for data analysis. It is an IPython notebook ("interactive python"). You can run each block of code separately using this. 

# Python Libraries

## matplotlib

A comprehensive library for creating static, animated, and interactive visualizations in python.

## pickle 

A library that serializes and deserializes a Python object structure. Serialization, or "pickling" converts an object into a bytestream, and deserialization, or "unpickling" converts a bytestream into an object. This library is not secure and should only be used on data you trust.

# Plotting

## Column Chart with matplotlib

You can plot data into a column chart using the below code snippet:

```python
import matplotlib.pyplot as plt
import pickle

# Load data which is stored as binary
with open ('fruit-sales.pickle', 'rb') as f:
    data = pickle.load(f)
    
# Splitting a list of tuples into two lists
fruit, num_sold = zip(*data)  # The * unpacks the iterable

'''
Plotting the data in a bar chart

Bar Charts (or column charts) are great for plotting categorical data.
'''

bar_coords = range(len(fruit))  # Determines x coordinates
plt.bar(bar_coords, num_sold)  # Plots a bar chart
plt.xticks(bar_coords, fruit)  # Replace the xticks with the names of fruit

# Chart annotations
plt.title('Number of Fruits Sold (2017)')
plt.ylabel('Number of Fruit (millions)')

plt.show()  # Shows the bar chart
```

This results in the below column chart:



