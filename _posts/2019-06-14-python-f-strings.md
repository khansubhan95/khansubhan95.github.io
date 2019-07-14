---
layout: post
title:  "Python f strings"
date:   2019-06-14 18:50:00 -0500
permalink: /python-f-strings/
---
Python f strings allow for string interpolation similar to Swift or JavaScript. They are a new feature introduced in Python 3.6. Let's look at an example 

```python
hello = 'hello'
world='world'
print(hello + ' ' + world) 
```

You can do a similar thing using f strings as follows 

```python
hello = 'hello'
world='world'
print(f'{hello} {world}')
```

String interpolation allows to embed variables into the string. This is especially useful because it does away with casting data types into strings to embed in a string. f strings even allow to embed expressions in {} 

```python
x=5
print(f'I have {x+1} apples!')
```

**Usage**: f followed by single or double quotes.