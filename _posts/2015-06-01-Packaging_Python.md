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
in minutes. The package is
available on [PyPI](https://pypi.python.org/pypi/kingman) and
[GitHub](https://github.com/jeromekelleher/kingman), and has 
documentation at [ReadTheDocs](http://kingman.readthedocs.org/en/latest/).

This tutorial is based on current best practises as given in the [Python
Packaging User
Guide](http://python-packaging-user-guide.readthedocs.org/en/latest/).
See also the [example package](https://github.com/pypa/sampleproject)
from the Python Packaging authority for an alternative framework.
This package is based on my own experience, and may not fully conform
to what others may think. Please file an 
[issue](https://github.com/jeromekelleher/kingman/issues)
on GitHub, or send a 
[pull request](https://github.com/jeromekelleher/kingman/pulls)
 if you see a problem!

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
we have the following files:

- **kingman** The directory holding the code for the ``kingman`` package.
- **tests** The directory containing the unit tests.
- **docs** The directory containing the Sphinx documentation.
- **README.txt** The README file for the project, written in reStructuredText 
  format. The reason for using reStructuredText is so we can have nicely 
  formatted information on both GitHub and PyPI generated directly from the same README.
- **README.rst** A soft link to the README.txt file above for convenience.
- **LICENSE** The file setting out the terms under which the software is shared.
  Should be an [OSI approved](http://opensource.org/licenses) open source 
  license.
- **MANIFEST.in** A file describing the extra files to include in the source
  distribution. See 
  [here](https://docs.python.org/3.4/distutils/sourcedist.html#manifest-template)
  for more details.
- **requirements.txt** A file describing the packages dependencies. See below 
  for further details.
- **setup.py** The main setup configuration file. See below for discussion.
- **ez_setup.py** A file that we copy in to the source distribution so we 
  can bootstrap ``setuptools``, if we need to.
- **cli_dev.py** This is a simple shim to allow us work with the command 
  line application more easily during development, so we can call
  ``python cli_dev.py`` from the shell to run the application during 
  development.

## The setup.py file

The ``setup.py`` file is the most important file in defining our package.
Following the 
[advice](http://python-packaging-user-guide.readthedocs.org/en/latest/projects.html#setuptools)
of the Python Packaging Authority, we use 
[setuptools](http://pythonhosted.org/setuptools/) to create 
the distribution. This complicates things a little, as some users may
not have setuptools installed. To work around this, we first attempt to import 
setuptools, and if it doesn't exist fall back on bootstrapping setuptools
using [ez_setup.py](https://pypi.python.org/pypi/ez_setup). This seems to work
well enough in practice.

## Requirements

One of the most useful aspects of using [pip](https://pypi.python.org/pypi/pip)
to install packages is the automatic installation of dependencies. To enable 
this we need to tell pip what packages we depend on. There are two distict
scenarious in which we need to be explicit about our dependencies, which 
are handled in different ways:

1. In testing and development, we need to list all the packages we require when
   we want to run our unit tests (e.g. on Travis). This is done using the 
   [requirements.txt file](https://pip.readthedocs.org/en/1.1/requirements.html).
2. When installing the package on a users machine, we need to tell the installer
   what extra packages we depend on for normal use. This is done in ``setup.py``
   using the **install_requires** argument. See the 
   [setuptools documentation](https://pythonhosted.org/setuptools/setuptools.html)
   for more details.

## Unit tests

[Unit testing](http://en.wikipedia.org/wiki/Unit_testing) is an essential
part of developing reliable software. We use the 
[nose](https://nose.readthedocs.org/en/latest/) testing framework for Python.
If you follow some simple naming rules for your test cases, nose can 
autodiscover and run them for you. For example, we can run:
{% highlight bash %}
$ nosetests -v
test_random_seed (test_cli.TestInput) ... ok
test_sample_size (test_cli.TestInput) ... ok
test_sample_size (test_cli.TestOutput) ... ok
test_bad_sample_size (test_simulate.TestInput) ... ok
test_random_seed (test_simulate.TestOutput) ... ok
test_sample_size (test_simulate.TestOutput) ... ok

----------------------------------------------------------------------
Ran 6 tests in 0.016s
{% endhighlight %}

One minor point to note is that nose will not run unit tests by 
default in files that are 
[marked executable](http://nose.readthedocs.org/en/latest/usage.html).


## Continuous Integration Testing
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
threshold of 85%. If our test coverage falls below this, the Travis run will
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

We then add two new [reStructuredText](http://docutils.sourceforge.net/rst.html)
files to the docs directory, ``api.rst`` and ``cli.rst``, which hold the documentation
for the Python API and the command line interface, respectively. For the API
documentation, we use the [Sphinx autodoc](http://sphinx-doc.org/ext/autodoc.html)
extension. This allows us to import the API documentation directly from the 
source code.

### ReadTheDocs

Read The Docs allows us to automatically publish the Sphinx generated documentation
to the web. It tracks changes from GitHub, so that our documentation is always 
up to date. See the 
[tutorial](https://docs.readthedocs.org/en/latest/getting_started.html) to 
get started with Read The Docs. After importing the ``kingman`` project from 
GitHub, we then have automatically updated 
[documentation](http://kingman.readthedocs.org/en/latest/index.html).

