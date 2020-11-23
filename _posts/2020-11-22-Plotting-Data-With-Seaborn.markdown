---
layout: single
title:  "Plotting Data with Seaborn"
date:   2020-11-22 23:00:00 -0500
categories: Programming Python Data
comments: true
typora-root-url: ..
---

This is a continuation of my "Data Visualization with Python" post. In this post, I have decided to cover plotting in the Python library seaborn which is an alternative to 

# Plotting with Seaborn

As I mentioned earlier, seaborn is an alternative data visualization library based on matplotlib but has a different api and different methods to plot data.

## Column Chart

You can plot data into a column chart using the below code snippet:

```python
import seaborn as sns
import pickle

# Load data which is stored as binary
with open ('fruit-sales.pickle', 'rb') as f:
    data = pickle.load(f)
    
# Splitting a list of tuples into two lists
fruit, num_sold = zip(*data)  # The * unpacks the iterable
# Convert into lists as seaborn can't read tuples for all functions
fruit = list(fruit)
num_sold = list(num_sold)

'''
Plotting the data in a column chart
'''

axes = sns.barplot(x=fruit, y=num_sold)

# Chart annotations
axes.set_title('Number of Fruits Sold (2017)')
axes.set_ylabel('Number of Fruit (millions)')

```

This code results in the below chart:

![SeabornColumnChart](/assets/images/Data Visualization/SeabornColumnChart.png)

## Bar Chart

You can plot data into a Bar chart using the below code snippet:

```python
import seaborn as sns
import pickle

# Load Data
with open('coding-exp-by-dev-type.pickle', 'rb') as f:
    data = pickle.load(f)
    
# Split into two lists
dev_types, years_exp = zip(*data)
dev_types = list(dev_types)
years_exp = list(years_exp)

'''
Plotting the data in a bar chart
'''

# Switching the x/y order to y/x makes the bar chart horizontal
axes = sns.barplot(y=dev_types, x=years_exp)

# Chart Annotations
axes.set_title('Years of Coding Experience by Developer Type')
axes.set_xlabel('Years')

```

This code results in the below chart:

![SeabornBarChart](/assets/images/Data Visualization/SeabornBarChart.png)