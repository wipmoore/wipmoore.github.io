---
layout: post
title: "Python 2.7 strings and text"
date: 2017-11-01 10:40:00 +0000
categories: programming python
---

In python, the _unicode_ type stores an abstract sequence of code points where as the _str_ type stores a sequence of raw bytes.  A unicode encoding (UTF-8, UTF-7, UTF-16, UTF-32, etc) maps sequences of bytes (_str_) to the unicode code points (_unicode_) and is used to convert between the two.


## Converting between strings and unicode

_str.decode_ converts raw bytes to _unicode_ using a supplied encoding, it is called decode as the raw bytes are seen as an encoded version of the pure unicode code points. Conversely _unicode.encode_ converts unicode code points to an _str_ using a supplied encoding, it is called encode as the unicode code points are seen as being encoded into a stream of raw bytes.

e.g. encoding unicode to a string

```python
# coding: utf-8
u = u'∀x ∃y P(x,y)'

with open( 'written.txt', 'w' ) as f:
    s = u.encode('utf-8')
    f.write( s )

```
The ```# coding: utf-8``` is to tell the python interpreter what coding has been used to stored the unicode characters in the string literal.


e.g. decoding a string to unicode

```
with open( 'Unicode.txt', 'r' ) as f:
    s = f.read()
    print s.decode('utf-8')
```



## Type coercion

When an instance of a _str_ and _unicode_ type are combined in a statement Python will convert the _str_ to _unicode_ (It assumes the bytes in the _str_ are ascii).

```python
>>> s = 'This is a str'
>>> type(s)
<type 'str'>
>>> u = u'This is unicode'
>>> type(u)
<type 'unicode'>
>>> type( s + u )
<type 'unicode'>
```

## Immutability

_str_ and _unicode_ are immutable once created they can not be changed.


```python
>>> a = u'This is unicode'
>>> b = a
>>> b is a
True
>>> b += u' b'
>>> b is a
False
>>> a
u'This is unicode'
>>> b
u'This is unicode b'
```

## Interning

There is support for interning _str_ but not _unicode_.

```
>>> s1 = 'foo!'
>>> s2 = 'foo!'
>>> s1 is s2
False
>>> s1 = intern('foo!')
>>> s2 = intern('foo!')
>>> s1 is s2
True
```


## Text manipulation

In general _unicode_ should be used when dealing with text manipulation ( finding sub strings, splitting on word boundaries, etc ) and _str_ should be used when dealing with I/O. You should decode to unicode as early as possible, perform all text manipulation on the unicode and then encode as late as possible preferably just before the IO operations.


## String literals

String literals can be enclosed in either single or double quotes and the choice is arbitrary but when using a single quote as a terminator you can use the double quotation mark inside the string without having to escape it and vice versa.

e.g.

```python
a = "this has a single quote ' "
b = "this has a double quote \" "
```

Special characters are introduce via escaping (via a backslash) within both single and double quoted literals -- e.g. \n \' \" \\

By default string literals are created as an instance of str, to follow the advice that text manipulation should always be done with unicode you can specify that a string literal is created as unicode by prefixing the literal with a u e.g.

```python
u1 = u'This is a unicode string literal.'
```

### Raw string literal

A raw string does not treat \ as an escape character, so a raw string of 'a\nb' evaluates to a length of 4 where as an ordinary string of 'a\nb' would evaluate to a length of 3.  You can specify that a string literal is treat as a raw string by prefixing it with an _r_ e.g.

```python
>>> s1 = ur'x\nx'
>>> s1
u'x\\nx'
>>> s2 = u'x\nx'
>>> s2
u'x\nx'
```

When a raw string is displayed in the console the \ will be escaped.


## Appendix

* [Google developers](https://developers.google.com/edu/python/strings)
* [Ned Batchelder](https://nedbatchelder.com/text/unipain.html)
