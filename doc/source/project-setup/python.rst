:title: Python Project Guide

Python Project Guide
####################

The goal of this document is to explain OpenStack wide standard practices
around the use of Python.

It describes the use of a Python virtual environment, the install of both
system and project-specific dependencies and, finally, running the tests.

Virtual Environment
===================

It is recommended that you use `virtualenv`_ to create an isolated Python
environment, with no reliance on system packages other than truly global
things like Python itself.

`virtualenv`_  bundles three other important Python tools – pip, wheel and
setuptools. All combined, this will get you a fully up to date development
environment going without root access – and without confusing your package
manager.

Installing
^^^^^^^^^^

#. Pick a place to make the virtual enviroment, e.g `~/workspace`;
#. Get a copy of `virtualenv`_ to run locally from source, according to the
   `virtualenv installation`_ guide. Note that you need to invoke the
   `virtualenv.py` script against `~/workspace`;
#. `Activate your virtualenv`_.

.. _`virtualenv`: https://virtualenv.pypa.io/en/latest/
.. _`virtualenv installation`: https://virtualenv.pypa.io/en/latest/installation.html
.. _`Activate your virtualenv`: https://virtualenv.pypa.io/en/latest/userguide.html#activate-script

Installing System Dependencies
==============================

Prior to installing the Python packages and running the tests, there are system
packages that will need installing in order to allow various Python packages
to compile.

`bindep`_ project exists to address this need. It is a tool for checking the
presence of binary packages needed by an application or library::

  $ pip install bindep

If the project you are working on has a `other-requirements.txt`, bindep will
read system requirements from there. Just call it along with your package
manager::

  $ sudo [apt-get | yum] install $(bindep -b)

If there is no such file, in order to learn what system dependencies need to be
installed, you should look at the documentation of the specific project you are
interested in.

.. _`bindep`: https://git.openstack.org/cgit/openstack-infra/bindep

Running Python Unit Tests
=========================

Before submitting your change, you should test it. Repositories generally have
several categories of tests:

* Style Checks -- Check source code for style issues
* Unit Tests --  Self contained in each repository
* Integration Tests -- Require a running OpenStack environment

The tests available and the tools used to implement these tests vary from
project to project. This section assumes you have all system dependencies
needed by the project installed. It covers how to run the style check and unit
tests. Both are run through `tox`_, so you need to install it::

  $ sudo pip install tox

.. _`tox`: https://tox.readthedocs.org/en/latest/

Run The Tests
^^^^^^^^^^^^^

Navigate to the repository's root directory and execute::

  $ tox

.. note::
  Completing this command may take a long time, depending on system resources.
  You might not see any output until tox is complete.

Run One Set of Tests
^^^^^^^^^^^^^^^^^^^^

tox will run your entire test suite in the environments specified in the
repository tox.ini::

  [tox]

  envlist = <list of available environments>

To run just one test suite in envlist execute::

  $ tox -e <env>

so for example, run the test suite in py27::

  $ tox -e py27

Running the style checks
^^^^^^^^^^^^^^^^^^^^^^^^^

Just run::

  $ tox -e pep8

Run One Test
^^^^^^^^^^^^

To run individual tests with tox:

If `testr`_ is in tox.ini, for example::

  [testenv]

  ... "python setup.py testr --slowest --testr-args='{posargs}'"

Run individual tests with the following syntax::

  $ tox -e <env> -- path.to.module.Class.test

So for example, run the test_memory_unlimited test in openstack/nova::

  $ tox -e py27 -- nova.tests.unit.compute.test_claims.ClaimTestCase.test_memory_unlimited

If `nose`_ is in tox.ini, for example::

  [testenv]

  ... "nosetests {posargs}"

Run individual tests with the following syntax::

  $ tox -e <env> -- --tests path.to.module:Class.test

So for example, run the list test in openstack/swift::

  $ tox -e py27 -- --tests test.unit.container.test_backend:TestContainerBroker.test_empty

.. _`testr`: https://wiki.openstack.org/wiki/Testr
.. _`nose`: https://nose.readthedocs.org/en/latest/

Debugging Python Unit Tests
===========================

You can debug tests with `pdb`_. To begin, insert ``set_trace()`` where you
wish to break::

  import pdb; pdb.set_trace()

If testr is in tox.ini, the ``testtools.run`` command should be used to run
tests. However, due to a `bug`_, it is not possible to simply pass a regex to
this tool. Instead, first generate a list of tests to run and then pipe this
list through ``testtools.run``::

  $ source .tox/py27/bin/activate
  $ testr list-tests test_name_regex > my-list
  $ python -m testtools.run discover --load-list my-list

Alternatively, some projects provide a ``debug`` in their tox envlist, which is
based on `oslo_debug_helper`_. Run individual tests with pdb enabled with the
following syntax::

  $ tox -e debug -- path.to.module.Class.test

.. TODO(stephenfin): How to debug nose tests?

.. _`pdb`: https://docs.python.org/3/library/pdb.html
.. _`bug`: https://bugs.launchpad.net/testrepository/+bug/902881
.. _`oslo_debug_helper`: http://docs.openstack.org/developer/oslotest/features.html
