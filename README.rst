=======
testrig
=======

Python package integration testing.

Runs the test suite of some package against 'old' and 'new' versions
of given dependencies. Failures that appear in 'new' are reported.

Each test suite run is run in a virtualenv constructed from scratch.
You should install ``ccache`` (and possibly also ``f90cache``) to
avoid drinking too much coffee.

Currently, this is POSIX-only, and tested only on Linux.

Configuration
-------------

Configuration is read from ``testrig.ini`` by default.  It contains
sections, one per test environment.  Section named ``DEFAULT`` can be
used to specify (overridable) default values for the configuration
items.

An example first (runs scipy test suite against old and new numpy
versions)::

  [DEFAULT]
  old=numpy==1.7.2
  new=Cython==0.22 git+https://github.com/numpy/numpy@master

  [scipy]
  base=nose tempita Cython==0.22 scipy==0.17.0
  run=python -c 'import numpy; numpy.test("fast", verbose=2)'
  parser=nose


The configuration items in each section are:

* ``old``: package specifications for the 'old' configuration.
* ``new``: package specifications for the 'new' configuration.
* ``base``: packages for both configurations. These are installed
  after those specified by ``old`` or ``new``.
* ``run``: command that runs the tests.
* ``parser``: parser for the test output. Available options:

  - ``nose``: parses nose stdout
  - ``pytest-log``: parses contents from ``py.test --result-log=pytest.log ...``

Usage
-----

Run::

    python run.py --help
    python run.py $TESTENV

The runs may take a long time, as it builds everything from source.

Note that parallel testing (``-j``) results to somewhat less efficient
ccache usage.