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

To illustrate this, I've made a simple Python package called *kingman*, which
simulates the classical single-locus [Kingman's
Coalescent](http://en.wikipedia.org/wiki/Coalescent_theory).  The simulation
itself is pretty trivial, and something that anyone could reproduce themselves
in minutes. I think it's a good idea to have an example which actually does
something useful to avoid the [drawing an
owl](http://knowyourmeme.com/memes/how-to-draw-an-owl) problem.  The package is
available on [PyPI](https://pypi.python.org/pypi/kingman) and
[GitHub](https://github.com/jeromekelleher/kingman).

This tutorial is based on current best practises as given in the [Python
Packaging User
Guide](http://python-packaging-user-guide.readthedocs.org/en/latest/).

## Code Layout

In our [git repository](https://github.com/jeromekelleher/kingman), 
we have the following layout:


## Continuous Integration Testing.
(Continuous integration testing)[http://en.wikipedia.org/wiki/Continuous_integration]
is one of the most effective ways of ensuring that your package is always in a 
useful state. Using (Travis CI)[https://travis-ci.org/] we can test our code 
after every push to git, which means many bugs are caught earlier than they 
might otherwise be. To use Travis in the ``kingman`` package, we do the 
following:

1. Enable [Travis integration with GitHub](http://docs.travis-ci.com/user/getting-started/);
2. Create the ``.travis.yml`` file;
3. Push an update to GitHub.

