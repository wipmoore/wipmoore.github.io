---
layout: post
title: "Character sets"
date: 2017-10-28 19:00:00 +0100
categories:
---
## ASCII

American standard code form information interchange is an eight bit code ( 256 representations 0-255) that represents all characters in the english alphabet plus some extra characters.  

As there are only twenty six characters in the english alphabet with two representations upper and lower case for each character even with the standard extra characters added it only uses 127 of the 255 possible characters. This lead to various different creative minds deciding to use the remaining characters for their own needs. The problem with people inventing their own extensions to the character set is that the interpretation of what characters had been written was in the hands of the receiver regardless of what the person who wrote it intended.  This results in different people seeing different messages and it being random luck if you see the text as the author intended.

To solve this problem the American National Standards Institute (ANSI) came up with the concept of standard code pages.  The first 128 (0-127) characters were standard across all code pages, the remaining characters were dependent on the code page that you had chose to interpret a text file with.

While code pages solved the problem of being able to correctly represent an author's text so long as you know what code page they had use there were still problems.  You could not include characters from different code pages in the same document, for certain alphabets 256 characters are not enough.


## Unicode

Unicode was an attempt to make a character set that included every reasonable writing system on the planet.  

### Code Points

In unicode every character is assigned a unique number by the Unicode consortium e.g. U+0639 for the Arabic letter Arin, this number is called a code point.  The U+ means "Unicode" and 0639 is a hexadecimal number. "Hello" in unicode is represented by the following five code points U+0048 U+0065 U+006C U+006C U+006F.


### Encodings

Encodings are the way that code point are physically represented in a computer either in main memory of physical storage. There are many different encoding  (ways of representing code point) which came about to meet specific needs.  

UTF-8 is an encoding standard that is capable of representing all 1,112,064 unicode code points, it is also backwards compatible with ASCII.  The first 128 characters of Unicode, which correspond one-to-one with ASCII, are encoded using a single octet with the same binary value as ASCII so valid ASCII text is valid UTF-8-encoded Unicode as well, ASCII is a subset of UTF-8.  It uses between one to four bytes to represent a character.


* 1 Byte, allows 7 bits for code points and covers U+0000 to U+007F. 0xxxxxxx
* 2 Bytes, allows 11 bits for code points and covers U+0080 to U+07FF, 110xxxxx 10xxxxxx
* 3 Bytes, allows 16 bits for code points and covers U+0800 to U+FFFF, 1110xxxx 10xxxxxx 10xxxxxx
* 4 Bytes, allows 21 bits for code points and covers U+1000 to U+10FFFF, 11110xxx 10xxxxxx 10xxxxxx 10xxxxxx

It is always safe to treat ASCII as unicode but not the other way round.

Regardless of the the encoding used you always need to identify the encoding used for a documents it should not just be assumed.  The encoding used is a property of the document rather than of the service providing access to it and so should be stored in the document metadata.  E.g. for html files there should always be a content meta tag that is the first entry in the header.

```
<html>
<head>
<meta http-equiv=“Content-Type” content=“text/html; charset=utf-8”>
...
</head>
```

This tells the browser which encoding has been used for document. As soon as the browser see the Content-Type meta tag it stops parsing the document and starts again using the specified encoding so this should be the first entry in the head section.  The http server should not be responsible for specifying the encoding in the http headers as it is a property of the document and different documents served by the same server may have been written using different encodings.


### Byte Order Marks (BOM)

This is two bytes (FF FE or FE FF) at the start of a stream that indicate the order in which the bytes in a multi byte string would be stored. That is all it does it does not tell you the encoding that has been used on the stream this still has to be specified.  The property is to do with data interchange rather that interpretation.
