---
layout: post
title: Cython, CPython and Jython
date: 2018-05-31
categories: 
  - Software Development
tags: 
  - design pattern
  - java
  - programming
---

Recently, I was confused with Cython and Cpython. In this blog, I will talk about Cython, CPython, Jython, and compare them with Java, C/C++ and related concepts.

## 1. Cython and CPython

Although Cython and CPython look very similar, they are very different. 

### 1.1 Cython 

Cython is actually a programming language, like Java, Python, and C++. It is a superset of Python. Let's see what wikipedia said.

Wikipedia: [Cython](https://en.wikipedia.org/wiki/Cython)
> Cython is a superset of the Python programming language, designed to give C-like performance with code that is mostly written in Python.

### 1.2 CPython

CPython is a Python interpreter implemented by C. 

Wikipedia: [CPython](https://en.wikipedia.org/wiki/CPython)
> CPython is the reference implementation of the Python programming language. Written in C, CPython is the default and most widely-used implementation of the language. 
> CPython is an interpreter. It has a foreign function interface with several languages including C, in which one must explicitly write bindings in a language other than Python.


## 2. Jython

Jython is another kind of Python interpreter. Jython is a Python interpreter implemented by Java and Python.

Wikipedia: [Jython](https://en.wikipedia.org/wiki/Jython)
> Jython is an implementation of the Python programming language designed to run on the Java platform. It is the successor of JPython.

Jython programms will compile into Java bytecode by JVM, so they can import and use any Java class.

The main difference between Jython and CPython is Jython is implemented by Java, while CPython is implemented by C. 


## 3. Other Python Interpreters

There are several Python interpreters, including CPython and Jython. 

[CPython](https://en.wikipedia.org/wiki/CPython): Python interpreters implemented by C. Platform: Gernal Cross-platform

[Jython](https://en.wikipedia.org/wiki/Jython): Python interpreters implemented by Java. Platform: JVM

[IronPython](https://en.wikipedia.org/wiki/IronPython): Python interpreters implemented by C#. Platform: .NET

[PyPy](https://en.wikipedia.org/wiki/PyPy): Python interpreters implemented by Python. Platform: Cross-platform

The PyPy interpreter itself is written in a restricted subset of Python, called RPython (Restricted Python).


## 4. Cython V.S. C/C++

In the Quora [What are the disadvantages of using Cython vs. C/C++?](https://www.quora.com/What-are-the-disadvantages-of-using-Cython-vs-C-C):

> Cython is a language that is a superset of Python. When using it, one codes in "mostly-Python" with optional static typing and the ability to call C code quickly and painlessly. Cython is compiled down to C, and is intended to wrap C extension modules for calling from Python. 
> Cython's primary function is to write C extension modules for Python.

Cython is compiled into C, so it is fast but still slower than native C

## 5. Python V.S. Java

1. Type:

Python is dynamic typing, while Java is static typing. In Java, the type of a variable should be explicitly declared, checked at compile time, and cannot change the type later in the program. This is known as static typing. In contrast, Python uses dynamic typing. In Python, the type of a variable the type of a variable is not declared and can change the type later in the program.

2. Portability:

Java is more portable than Python. Java is available just about everywhere. Python isn’t especially portable, the actual runtime is available many places, but modules tend to be a mixture of C and Python, and just about any compatibility is off the cards.

3. Indentation vs Braces:

Python uses indentation to separate code, while Java uses curly braces.

## 6. Python V.S. C++

1. Memory management: 

C++ doesn't have garbage collection, and encourages use of raw pointers to manage and access memory. In contrast, Python has garbage collection. The raw pointers are hidden in Python.


2. Type: 

C++ is static typing, while Python is dynamic typing. In C++, the type of a variable should be explicitly declared, checked at compile time, and cannot change the type later in the program. In contrast, in Python, the type of a variable the type of a variable is not declared and can change the type later in the program.

3. Language Complexity:

C++ is a complexity language. Python is much simpler, which leads to faster development and less mental overhead.

4. Interpreted vs Compiled:

C++ is almost always explicitly compiled. Generally, Python is not. It's common practice to develop in the interpreter in Python, which is great for rapid testing and exploration. C++ developers almost never do this, gdb notwithstanding.

5. Speed:

C++ typically always run faster than Python.

6. Indentation vs Braces:

Python uses indentation to separate code, while C++ uses curly braces.


## Reference:

1. Wikipedia: https://www.wikipedia.org/

2. Quora: https://www.quora.com/What-are-the-advantages-of-Python-over-C++

3. Quora: https://www.quora.com/What-are-the-key-differences-between-Python-and-Java-Which-technology-would-be-faster
