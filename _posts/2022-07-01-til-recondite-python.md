---
layout: post
title: "TIL: Recondite Python"
---

This page is a collection of obscure facts about the Python programming language.

## Table of Contents
{:.no_toc}

* Table of Contents
{:toc}

## the `__debug__` constant

Python has a [built-in boolean constant](https://docs.python.org/3/library/constants.html#debug__){:target="_blank"}
named `__debug__`.

It is set to `True` whenever you run a REPL shell or call a script with the
`python script.py` i.e. by default.

It is set to `False` only when you pass the `-O` or the `-OO` flags to the python executable.

These flags are described in the Python manual page as:

`-O` Remove assert statements and any code conditional on the value of __debug__;
augment the filename for compiled (bytecode) files by adding .opt-1 before the .pyc
extension.

`-OO` Do `-O` and also discard docstrings; change the filename for compiled
(bytecode) files by adding .opt-2 before the .pyc extension.

### `__debug__` in REPL shell

```bash
$ python3
Python 3.9.13 (main, May 24 2022, 21:28:31)
[Clang 13.1.6 (clang-1316.0.21.2)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> __debug__
True

```

```bash
$ python3 -O
Python 3.9.13 (main, May 24 2022, 21:28:31)
[Clang 13.1.6 (clang-1316.0.21.2)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> __debug__
False

```

### `__debug__` in scripts

```bash
$ cat script.py
print(__debug__)

$ python3 script.py
True
$ python3 -O script.py
False
$ python3 -OO script.py
False
```