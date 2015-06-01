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

To illustrate this, I've made a simple Python package called ``kingman``, which
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
See also the [example package](https://github.com/pypa/sampleproject)
from the Python Packaging authority.
This package is based on my own experience, and may not fully conform
to what others may think. Please file an issue on github, or send a 
pull request if you see a problem!

## Python versions

Python 3 is here to stay, and we should aim to support it fully in any 
module that we release. Supporting both Python 2.7 and Python 3.x 
is actually quite straightforward in the majority of cases, once 
we follow a few simple rules. For the more complex cases, the 
[six module](https://pypi.python.org/pypi/six) provides a very useful
compatability layer.

In the ``kingman`` module, every file starts with the lines
{% highlight python %}
from __future__ import print_function
from __future__ import division
{% endhighlight %}

Including these two lines will ensure that the vast majority of the 
code you write is compatible with both Python 2.7 and 3.x.  We also 
ensure a good level of compatibility by included several Python 
versions in our Travis Continuous integration tests (see below).
See [here](http://python-future.org/compatible_idioms.html) for 
further information on issues regarding Python version compatibility.

## Code Layout

In our [git repository](https://github.com/jeromekelleher/kingman), 
we have the following layout:


## Continuous Integration Testing.
[Continuous integration testing](http://en.wikipedia.org/wiki/Continuous_integration)
is one of the most effective ways of ensuring that your package is always in a 
useful state. Using [Travis CI](https://travis-ci.org/) we can test our code 
after every push to git, which means many bugs are caught earlier than they 
might otherwise be. To use Travis in the ``kingman`` package, we do the 
following:

1. Enable [Travis integration with GitHub](http://docs.travis-ci.com/user/getting-started/);
2. Create the ``.travis.yml`` file;
3. Push an update to GitHub.

## Code linting

To ensure that our code follows the 
[PEP 8 style guide](https://www.python.org/dev/peps/pep-0008)
and to catch many common programming mistakes, we use the 
[Flake8 tool](https://pypi.python.org/pypi/flake8). We ensure this 
is run every time we push code to GitHub by running it in our 
``.travis.yml`` file.

## Test Coverage

To ensure we maintain good test coverage, we use the nose 
[coverage plugin](http://nose.readthedocs.org/en/latest/plugins/cover.html). 
We enable this in the ``.travis.yml`` file and set a minimum coverage 
threshold of 85%%. If our test coverage falls below this, the Travis run will
fail.

## Command line interface development and deployment.

A very common task in Bioinformatics is to develop a standalone program 
with a command line interface which performs some well defined task. Python 
provides some excellent tools to make developing and deploying CLIs very
simple. To write our CLI, we use the 
[argparse module](https://docs.python.org/3.4/library/argparse.html) from 
the Python standard library. This replaces the old ``optparse`` module, 
which should not be used in new code.


## Uploading to PyPI

Uploading our package to the [Python Package index](https://pypi.python.org/pypi)
makes it available for anyone to install easily. After creating a PyPI account
and setting up the credentials, we first register our package:
{% highlight bash %}
$ python setup.py register
{% endhighlight %}

Once we have registered, we can then upload versions of the package as follows:
{% highlight bash %}
$ python setup.py sdist
$ twine upload kingman-{VERSION}.tar.gz
{% endhighlight %}
We use the [Twine](https://pypi.python.org/pypi/twine) package to securely upload
the package.

## Documentation 

A package is only as good as its documentation, and documentation needs to 
look good to keep users engaged. [Sphinx](http://sphinx-doc.org/) and 
[Read The Docs](https://readthedocs.org/) allow us to easily write 
beautiful looking documentation that is automatically updated when we 
push new updates to GitHub. 

### Sphinx

The documentation is built using [Sphinx](http://sphinx-doc.org/). See the 
[sphinx tutorial](http://sphinx-doc.org/tutorial.html) for a quick introduction
to using Sphinx. To get started, we simply run 
{% highlight bash %}
$ sphinx-quickstart
{% endhighlight %}
from within the ``docs`` directory, taking the default for the majority of the 
options.

We then add two new [reStructured Text](http://docutils.sourceforge.net/rst.html)
files to the docs directory, ``api.rst`` and ``cli.rst``, which hold the documentation
for the Python API and the command line interface, respectively. For the API
documentation, we use the [Sphinx autodoc](http://sphinx-doc.org/ext/autodoc.html)
extension. This allows us to import the API documentation directly from the 
source code.

### ReadTheDocs


