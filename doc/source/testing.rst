=====================
 Testing (QA and CI)
=====================

OpenStack projects have robust automated testing.  In general, we
believe something not tested is broken.  OpenStack is an extremely
complex suite of software largely designed to interact with other
software that can be operated in a variety of configurations.
Manual localized testing is unlikely to be sufficient in this
situation.

Types of Tests
==============

OpenStack projects employ a number of approaches to testing:

**Unit tests**
  These tests are generally designed to validate a single method or
  class and verify that the code operates as designed across all
  inputs.  They may employ mocks or fakes to simplify interaction with
  components outside the immediate area of the test.

  Most projects have a large number of unit tests and expect to have
  near-complete code coverage with those tests.  Changes to project
  code should generally include changes to add testing as well.

**Functional tests**
  Functional tests validate a project in operation (e.g., if the
  project is an API server, the server will actually be running),
  though interactions with external components are kept to a minimum.
  This may be accomplished by using fake drivers for interactions with
  other components, or simpler implementations of those components.

  Functional tests are expected to catch most actual operational bugs
  in an environment that is easier to understand and debug than a full
  integration test environment.

**Integration tests**
  Integration testing includes the project and all related components
  operating in as realistic manner as possible.  The goal is to
  identify bugs that are only likely to appear in production, before
  they are found in production.  Often, when a bug is found in
  integration testing, it may indicate a gap in functional testing.

**Style checks**
  When large numbers of developers work together on a project's
  codebase, it is often useful to have a set style for code.  Style
  checks (such as pep8 for Python) are an automated way to ensure a
  consistent code style from all contributors.

Automated Test Systems
======================

OpenStack projects can generally be tested locally by developers
before submitting changes.  However, not all developers have access to
sufficient test resources to run all tests, and moreover, project
reviewers can not assume that a developer has performed all of the
relevant testing.  Therefore, OpenStack projects can define a set of
test jobs that automatically run on every change to the project.
These are run by a tool called *Zuul* and results appear in the
Gerrit code review system under the name *Jenkins*.

In order to facilitate projects interacting with the automated test
system in a standardized way, the Technical Committee has adopted the
`Consistent Testing Interface
<http://governance.openstack.org/reference/project-testing-interface.html>`_
which describes what facilities a project must provide for both
developers and the automated systems to run tests on the project.

Project Gating
==============

.. TODO: link to core reviewer guidelines

OpenStack projects do not permit anyone to directly merge code to a
source code repository.  Instead, any member of a core reviewer team
may approve a change for inclusion, but the actual process of merging
is completely automated.  After approval, a change is automatically
run through tests again, and only if the change passes all of the
tests, is it merged.

This process ensures that the main branch of development is always
working (at least as well as can be determined by its testing
infrastructure).  This means that a developer can, at any point, check
out a copy of the repository and begin work on it without worrying
about whether it actually functions.

This is also an important part of the egalitarian structure of our
community.  With no member of the project able to override the results
of the automated test system, no one is tempted to merge a change on
their own authority under the perhaps mistaken impression that they
know better than the test system whether a change functions correctly.

For more information on the automated testing infrastructure itself,
including how to configure and use it, see the `Infrastructure User
Manual <http://docs.openstack.org/infra/manual/>`_.

Test Failures
=============

OpenStack project integration tests have logs from running services
automatically uploaded to a logstash-based processing system.  An
automated system finds signatures related to known bugs and leaves
code review comments about them.  Additionally, aggregate statistics
about the number of times a known bug has caused a test failure are
reported.

If your project participates in an integration test with other
OpenStack projects, you are welcome to add definitions for your
project's logs and signatures for known bugs to the log processing
system.

For more information on how to deal with integration test failures,
see the `Developer's Guide
<http://docs.openstack.org/infra/manual/developers.html#automated-testing>`_.
