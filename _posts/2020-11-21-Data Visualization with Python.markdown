---
layout: single
title:  "Data Visualization with Python"
date:   2020-11-21 23:00:00 -0500
categories: Programming Python Data
comments: true
typora-root-url: ..
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

# Plotting with matplotlib

The below examples are from the Zenva tutorial I mentioned earlier. I did not include the data that was included in the tutorial as I don't have permission to publish it, but the data is stored as a bytestream and is mostly a list of tuples. I have uzed the zip function to unpack the iterables while maintaining their sequences, as this is very useful for later plotting.

## Column Chart

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
Plotting the data in a column chart

Column charts are great for plotting categorical data.
'''

bar_coords = range(len(fruit))  # Determines x coordinates
plt.bar(bar_coords, num_sold)  # Plots a column chart
plt.xticks(bar_coords, fruit)  # Replace the xticks with the names of fruit

# Chart annotations
plt.title('Number of Fruits Sold (2017)')
plt.ylabel('Number of Fruit (millions)')

plt.show()  # Shows the bar chart
```

This code results in the below chart:

![ColumnChart](/assets/images/Data Visualization/ColumnChart.png)

## Horizontal Bar Chart

You can plot data into a horizontal bar chart using the below code snippet:

```python
import matplotlib.pyplot as plt
import pickle

# Load Data
with open('coding-exp-by-dev-type.pickle', 'rb') as f:
    data = pickle.load(f)
    
# Split into two lists
dev_types, years_exp = zip(*data)

'''
Plotting the data in a bar chart

Horizontal Bar charts are also great for plotting categorical data
'''
bar_coords = range(len(dev_types))
plt.barh(bar_coords, years_exp)  # Plot a horizontal bar chart
plt.yticks(bar_coords, dev_types, fontsize=8)
plt.tight_layout()  # Tightens the layout, eliminating white space

# Chart Annotations
plt.title('Years of Coding Experience by Developer Type')
plt.xlabel('Years')

plt.show()
```

This code results in the below chart:

![HorizontalBarChart](/assets/images/Data Visualization/HorizontalBarChart.png)

## Pie Chart

You can plot data into a pie chart using the below code snippet:

```python
import matplotlib.pyplot as plt
import pickle


# Load Data
with open ('devs-outside-time.pickle', 'rb') as f:
    data = pickle.load(f)
    
# Split into two lists
time, responses = zip(*data)

'''
Plotting the data in a pie chart

Pie charts are great for plotting fractional data
'''
# autopct='%.2f%%' means: floating point with 2 sig digits, and percent sign
plt.pie(responses, labels=time, autopct='%.2f%%')

# Forces x/y axis to have the same scale and result in a circle
plt.axis('equal')  

# Chart annotations
plt.title('Daily Time Developers Spend Outside')

plt.show()
```

This code results in the below chart:

![PieChart](/assets/images/Data Visualization/PieChart.png)

## Line Chart

You can plot data into a line chart using the below code snippet:

```python
import matplotlib.pyplot as plt
import pickle

# Load Data
with open ('prog-langs-popularity.pickle', 'rb') as f:
    data = pickle.load(f)
    
'''
Data is organized as:
[
 (language1, [(year1, ranking1), (year2, ranking2)...]),
 (language2...), ...,
]
'''

# Split into two lists
languages, rankings = zip(*data)

# Get the Java years and ranks | Split Java into two lists
java_years, java_ranks = zip(*rankings[0])

'''
Plotting the data in a line chart

Line charts are great for plotting time series data
'''

plt.plot(java_years, java_ranks)  # Use plt.plot to plot a line graph
plt.xticks(java_years)

# Annotations
plt.title('Java Ranking')
plt.xlabel('Year')
plt.ylabel('Ranking')

plt.show()
```

This code results in the below chart:

![LineChart](/assets/images/Data Visualization/LineChart.png)

## Multi-line Chart

You can plot data into a multi-line chart using the below code snippet:

```python
import matplotlib.pyplot as plt
import pickle

# Load Data
with open ('prog-langs-popularity.pickle', 'rb') as f:
    data = pickle.load(f)
    
'''
Data is organized as:
[
 (language1, [(year1, ranking1), (year2, ranking2)...]),
 (language2...), ...,
]
'''

# Split into two lists
languages, rankings = zip(*data)

'''
Plotting the data in a multi-line chart

Multi-line charts allow multiple items to be plotted
'''

# Iterate through the languages and "plot" them
for i in range(len(languages)):
    # For each language, split the data into years and rankings lists
    years, ranks = zip(*rankings[i])
    plt.plot(years, ranks)
    
# Annotations
plt.title('Programming Language Rankings')
plt.xlabel('Year')
plt.ylabel('Ranking')
plt.legend(languages)
    
plt.show()
```

This code results in the below chart:

![MultiLineChart](/assets/images/Data Visualization/MultiLineChart.png)

## Scatter Plot

You can plot data into a scatter plot using the below code snippet:

```python
import matplotlib.pyplot as plt
import pickle

# Load data
with open('iris.pickle', 'rb') as f:
    iris = pickle.load(f)
    
'''
iris.pickle contains a dictionary with a number of key-value pairs, including
data, which is the array of different data types for each flower,
feature_names which is the name of the data type. In this example we will
only be plotting the sepal length and sepal width, which are the first two
columns in the data table. 
'''

# Extract the first column from the data table
sepal_length = iris['data'][:,0]  # [:,0] stands for all rows, first index
sepal_width = iris['data'][:,1] 
classes = iris['target']

'''
Plotting the data in a scatter plot

Scatter plots are used to plot data points on a horizontal and a vertical 
axis in the attempt to show how much one variable is affected by another.
'''

# Plots the datapoints, and colors them by class
plt.scatter(sepal_length, sepal_width, c=classes)

# Annotations
plt.title('Sepal Length v. Sepal Width')
plt.xlabel('Sepal Length (cm)')
plt.ylabel('Sepal Width (cm')

plt.show()
```

This code results in the below chart:

![ScatterPlot](/assets/images/Data Visualization/ScatterPlot.png)

## Multi Scatter Plot (subplot)

You can plot data into a multi scatter plot using the below code snippet:

```python
import matplotlib.pyplot as plt
import pickle

# Load data
with open('iris.pickle', 'rb') as f:
    iris = pickle.load(f)
    
'''
iris.pickle contains a dictionary with a number of key-value pairs, including
data, which is the array of different data types for each flower,
feature_names which is the name of the data type. In this example we will
only be plotting the sepal length and sepal width, which are the first two
columns in the data table. 
'''

# Extract the first column from the data table
sepal_length = iris['data'][:,0]  # [:,0] stands for all rows, first index
sepal_width = iris['data'][:,1] 
petal_length = iris['data'][:,2]
petal_width = iris['data'][:,3]
classes = iris['target']

'''
Plotting the data in a multi scatter plot

Multi scatter plots (subplots) arrange plots into a grid fashion. These are
good for showing multiple different relationships.
'''

# Fig is the figure containing the subplot grid
# Axes contains the subplots that can be indexed into
fig, axes = plt.subplots(2, 2)  # Creates a 2x2 grid of plots (column, row)
fig.suptitle('Iris Dataset')  # figure title

# Populate subplots
# Sepal length vs sepal width
axes[0,0].scatter(sepal_length, sepal_width, c=classes)  # Top left
axes[0,0].set_xlabel('Sepal Length (cm)')
axes[0,0].set_ylabel('Sepal Width (cm)')
axes[0,0].title.set_text('Sepal length vs sepal width')
# Petal length vs petal width
axes[0,1].scatter(petal_length, petal_width, c=classes)  # Top Right
axes[0,1].set_xlabel('Petal Length (cm)')
axes[0,1].set_ylabel('Petal Width (cm)')
axes[0,1].title.set_text('Petal length vs petal width')
# Petal length vs petal width
axes[1,0].scatter(sepal_length, petal_length, c=classes)  # Bottom Left
axes[1,0].set_xlabel('Sepal Length (cm)')
axes[1,0].set_ylabel('Petal Length (cm)')
axes[1,0].title.set_text('Petal length vs petal width')
# Sepal width vs petal width
axes[1,1].scatter(sepal_width, petal_width, c=classes)  # Bottom Right
axes[1,1].set_xlabel('Sepal Width (cm)')
axes[1,1].set_ylabel('Petal Width (cm)')
axes[1,1].title.set_text('Sepal width vs petal width')

fig.tight_layout()  # Optimizes layout so xlabels can be seen
plt.show()
```

This code results in the below chart:

![MultiScatterPlot](/assets/images/Data Visualization/MultiScatterPlot.png)