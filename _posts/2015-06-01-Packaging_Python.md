---
layout: post
title: Python Packaging
---

Packaging applications properly is one of the most important ways
in which we can improve user experience, and indeed make our own lives
easier. Python offers excellent methods to easily package and
deploy applications in a cross-platform way. This post demonstrates
how we can package up a Python application using the current
best-practices.

To illustrate this, I've made a simple Python package called 
*kingman*, which simulates the classical single-locus
[http://en.wikipedia.org/wiki/Coalescent_theory](Kingman's Coalescent).
The simulation itself is pretty trivial, and something that 
anyone could reproduce themselves in minutes. I think it's 
a good idea to have an example which actually does something useful
to avoid the [http://knowyourmeme.com/memes/how-to-draw-an-owl](drawing
an owl) problem. The package is available on 
[https://pypi.python.org/pypi/kingman](PyPI) and 
[https://github.com/jeromekelleher/kingman](GitHub).

This tutorial is based on current best practises as given in the 
[http://python-packaging-user-guide.readthedocs.org/en/latest/](
Python Packaging User Guide).

## Code Layout

In our [https://github.com/jeromekelleher/kingman](git repository), 
we have the following layout:



