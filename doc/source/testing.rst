=====================
 Testing (QA and CI)
=====================

OpenStack projects have robust automated testing.  In general, we
believe something not tested is broken.  OpenStack is an extremely
complex suite of software largely designed to interact with other
software that can be operated in a variety of configurations.
Manual localized testing is unlikely to be sufficient in this
situation. By default, all the testing is done with current versions
of dependencies managed centrally in the ``upper-constraints.txt``
file within the ``openstack/requirements`` repository. Testing
lower bounds of dependencies using a ``lower-constraints.txt`` file
is optional for projects.

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
<https://governance.openstack.org/tc/reference/project-testing-interface.html>`_
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
including how to configure and use it, see the `OpenDev
Manual <https://docs.opendev.org/opendev/infra-manual/latest/>`_.

How to Handle Test Failures
===========================

If Zuul reports a test failure on a patch, the first step should be
identifying what went wrong. You will be tempted to just recheck the
patch to see if it fails again, but please **DO NOT DO THAT.** CI test
resources are a very scarce resource (and becoming more so all the
time), so please be extremely sparing when asking the system to re-run
tests.

.. note:: Please do not **EVER** simply ``recheck`` without a
          reason. Please always *attempt* to determine what caused the
          failure and give some meaningful description it. It can be something
          like name of the test(s) which have failed or some other info about
          what and in which job/test case went wrong.
          Please try to avoid comments like ``recheck - unrelated failure`` as
          such comments are almost the same as simply ``recheck``.
          Please check below examples of the good recheck comments.

It is important that before you request a recheck, you adhere to the
following guidelines:

#. First, you should examine the logs of the jobs that failed. Look
   for the reason why the job failed, be it failed tests, or a setup
   failure, such as a failed devstack run, or job timeout. You should
   always begin this process suspecting that the failure is a result
   of the proposed patch itself, but with an eye to the problem being
   unrelated. Try to determine the most obvious cause for the failure,
   and do not ignore failures in multiple voting jobs.
#. If the failure is likely caused by the proposed patch, you should
   try whenever possible to reproduce the failure locally. This will
   allow you to revise the change and re-submit with a higher
   likelihood of subsequently getting a passing run.
#. If the failure appears to be totally unrelated to the patch at
   hand, look for some indication of what went wrong. Only after you
   have done this should you ask Zuul to re-run the tests. To do this,
   comment on the patch the recheck command and a reason. Examples of
   this are:

   ``recheck nova timed out waiting for glance``

   ``recheck glance lost connection to mysql``

   ``recheck cinder failed to detach volume``

#. The gold standard for recheck commands is ``recheck bug #XXXXXXX``,
   which directly references a known problem that is being
   worked. Doing this helps add heat to that bug and enables stats
   tracking so the community knows what bugs are blocking the most
   people in the CI system.
#. In some cases, it may be entirely unclear why something failed. In
   this case, you may need to recheck with a reason of "Not sure what
   failed, rechecking to get another data point."
#. In case of the root cause not being clear for the issue but
   it is also pretty clear that it was not related to the patch, you can also
   add name of the failed test to the recheck command, like for example
   ``recheck - failed test tempest.api.network.test_ports.test_example``
#. If a recheck results in a similar failure on the subsequent run, it
   would be best to reach out (via the mailing list or IRC) to the
   project team responsible for the service you think is failing and
   look for some guidance on whether or not the issue is known and
   being worked, as it may be that a patch for the problem is proposed
   but not merged, which you can ``Depends-On`` to move forward.
#. Especially if the same failure occurs more than once and is not yet
   reported, it is highly recommended that you open a bug against the
   project (or projects) affected and use that for your recheck.

Suggestions For Determining Causes of Failure
---------------------------------------------

This is more art than science, but here are some ideas:

- First examine the ``job-output.txt`` file to see if the job failed
  while running tests, or earlier when setup was running.
- If it looks like a test failure, the ``testr_results.html`` file is
  usually very helpful for looking at individual failures.
- If a test failed, try to identify which services are being used in
  that test. Quickly skim the logs for those services looking for
  **ERROR** lines and especially tracebacks that seem to line up with
  the test failure. For example, if the test is a compute failure to
  attach a volume, it would be good to look at ``n-api``,
  ``n-cpu``, ``c-api``, and ``c-vol`` logs as Nova and Cinder are
  both involved in that process.
- Test failures in tempest-based jobs generally print out resource
  IDs, such as instance or volume UUIDs. Use these to search the
  relevant logs for errors and warnings related to a resource that was
  involved in the test failure.
- Looking at the timestamps of test failures can also help locate
  relevant lines in the service logs.

Checking status of other job results
------------------------------------

Each Zuul CI job results is sent to the Opensearch service.
This service can be very useful specially when the Zuul job status
is Failure. Checking whether a given error has not occured in another
project or it has not appeared regularly recently will allow
for faster problem recognition or notification of the OpenStack community
about the problem.

To check the Opensearch service, you need to login with credentials that are
described below:

* url: https://opensearch.logs.openstack.org/_dashboards/app/discover?security_tenant=global
* username: `openstack`
* password: `openstack`
* tenant: `global`
