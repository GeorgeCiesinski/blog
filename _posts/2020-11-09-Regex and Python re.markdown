---
layout: single
title:  "Regex and Python re"
date:   2020-11-09 23:30:00 -0500
categories: Programming Regex Re
comments: true
---

# Regex

A regular expression (regex) is a sequence of characters that define a search pattern. 

[RexEgg - Regex Syntax](https://www.rexegg.com/regex-quickstart.html)

[RegExr - Regex Testing](https://www.rexegg.com/regex-quickstart.html)

# re library

The python re module provides regular expression matching operations similar to those found in Perl.

[Documentation](https://docs.python.org/3/library/re.html#module-re)

```python
import re
```

## re.match()

Match is anchored to the beginning of the string, so it will only find a pattern if it starts at the beginning of the string.

> If zero or more characters at the beginning of *string* match the regular expression *pattern*, return a corresponding [match object](https://docs.python.org/3/library/re.html#match-objects).  Return `None` if the string does not match the pattern; note that this is different from a zero-length match.
>
> Note that even in [`MULTILINE`](https://docs.python.org/3/library/re.html#re.MULTILINE) mode, [`re.match()`](https://docs.python.org/3/library/re.html#re.match) will only match at the beginning of the string and not at the beginning of each line.
>
> If you want to locate a match anywhere in *string*, use [`search()`](https://docs.python.org/3/library/re.html#re.search) instead (see also [search() vs. match()](https://docs.python.org/3/library/re.html#search-vs-match)).

[Source](https://docs.python.org/3/library/re.html)

```python
re.match(regex, element)
```

The above code results in a match object that looks like this:

```
<re.Match object; span=(0, 5), match='Match result'>
```

**Span:** The start and end of the result.

**Match:** The string showing the found result.

In order to simply confirm if there is a match without showing the match object, you can use:

```
bool(re.match(regex, element))
```

results in a **True** or **False** output depending on whether or not a match is found.

## re.search()

> Scan through *string* looking for the first location where the regular expression *pattern* produces a match, and return a corresponding [match object](https://docs.python.org/3/library/re.html#match-objects).  Return `None` if no position in the string matches the pattern; note that this is different from finding a zero-length match at some point in the string.

```
re.search(regex, element)
```

This returns a list of strings

```
['result1', 'result2']
```

