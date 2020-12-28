layout: single
title:  "Learning Sentiment Analysis with Python"
date:   2020-12-13 23:00:00 -0500
categories: Programming Python
comments: true
typora-root-url: ..

Like everyone else, I have been stuck at home almost every day due to the pandemic, except when I go out to grab essentials (weed and alcohol of course, but sometimes food too). Most fun things like watching movies and playing games has started to get boring, so I have started taking more projects on instead. My thought process on this is that if I am stuck inside during the pandemic anyways, I should make the most of it by learning as much as I can!

One of my latest projects that I have taken on is a small project (maybe 1-2 months) which I am working on with a few of my friends, Rob and Saif. This project is about taking tweets from celebrities or politicians, and analyzing the replies to those tweets to guess the general sentiment. I am very excited about this project because it is my first foray into data analytics. My friend Saif is currently studying data analytics in a bootcamp program, and my friend Rob is very knowledgeable about data analysis as well, although he is not a programmer. 

# What is Sentiment Analysis

Sentiment analysis, also known as opinion mining, is a natural language processing technique used to interpret and classify sentiment in subjective data. It is used to analyze text data like news articles, blogs, or comments, and understand things like positive or negative sentiment, and the context of the sentiment.

Sentiment analysis is useful for a wide variety of applications. For example, companies might use it to guage brand reputation or analyze the sentiment around certain products. It can also be used for analyzing customer feedback, and analyzing social data such as dialogue across social networks. Our project is considering a number of different libraries which can be used for sentiment analysis including a really cool library called TextBlob.

# What is TextBlob

[TextBlob](https://textblob.readthedocs.io/en/dev/) is a Python library for processing textual data. It provides an API with a large number of features and common natural language processing tasks, including part-of-speech tagging, noun phrase extraction, sentiment analysis, and much more. Textblob is built upon [NLTK](https://www.nltk.org) (natural language toolkit) which is a huge language processing library, and provides an easy to use interface to make it easier than ever to use.

# TextBlob Basic Usage

Textblob is very easy to use. As you can see below, you can get basic sentiment analysis in just four lines of code:

```python
from textblob import TextBlob

input = input('Please paste the blob for analysis: ')

blob = TextBlob(input)

print(blob.sentiment)
```

This code results in the terminal asking you to enter a text blob. It converts this into a **TextBlob** object and finally, prints this object. To test this, I entered the test text:

> A pine is any conifer in the genus Pinus of the family Pinaceae. Pinus is the sole genus in the subfamily Pinoideae. The Plant List compiled by the Royal Botanic Gardens, Kew and Missouri Botanical Garden accepts 126 species names of pines as current, together with 35 unresolved species and many more synonyms.

Once you enter this text, the program then converts it to a TextBlob object, analyzes it, and prints the result:

```
Sentiment(polarity=0.25, subjectivity=0.4125)
```

This result is a tuple containing polarity, and subjectivity. The **polarity** is a float value between -1 and 1, which is a score of how positive or negative the sentiment is, with 0 being neutral. The **subjectivity** is a float value between 0 and 1 where 0 is objective, and 1 is subjective. Objective text would contain only factual information, whereas subjective text contains judgement and opinion. In the above example, I used a fairly neutral paragraph presenting facts only, and this was analyzed to be nearly neutral (0.25 polarity) and mostly objective (0.4125 subjectivity). 

This library seems to have varying levels of accuracy, but I suspect it is because in order to be accurate, it would need to analyze a larger text sample such as an article. 

# Vader

[Vader](https://pypi.org/project/vaderSentiment/) (Valence Aware Dictionary and sEntiment Reasoner) is another sentiment  sentiment analysis library for python. This library is specifically designed for social media 

