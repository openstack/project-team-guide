==============================================
 Dependency Management for OpenStack Projects
==============================================

Why do we have a global requirements list?
==========================================

During the Havana release cycle we kept running into coherency issues
with trying to install all the OpenStack components into a single
environment. The issue is that syncing of ``requirements.txt`` between
projects was an eventually consistent problem. Some projects would
update quickly, others would not. We'd never have the same versions
specified as requirements between packages.

Because of the way that python package installation with pip works,
this means that if you get lucky you'll end up with a working
system. If you don't you can easily break all of OpenStack on a
requirements update.

An example of how bad this had gotten is that python-keystoneclient
would typically be installed / uninstalled 6 times during the course
of a DevStack gate run during Havana. If the last version of python
keystoneclient happened to be incompatible with some piece of
OpenStack a very hard to diagnose break occurs.

We also had an issue with projects adding dependencies of python
libraries without thinking through the long term implications of those
libraries. Is the library actively maintained? Is the library of a
compatible license? Does the library duplicate the function of existing
libraries that we already have in requirements? Is the library python
3 compatible? Is the library something that already exists in Linux
Distros that we target (Ubuntu / Fedora). The answer to many of these
questions was no.

Global requirements gives us a single place where we can evaluate
these things so that we can make a global decision for OpenStack on
the suitability of the library.

Since Havana we've also observed significant CI disruption occurring due to
upstream releases of software that are incompatible (whether in small
or large ways) with OpenStack. So Global Requirements also serves as a control
point to determine the precise versions of dependencies that will be used
during CI.

Solution
========

The mechanics of the solution are relatively simple. We maintain a
central list of all the requirements (``global-requirements.txt``)
that are allowed in OpenStack projects. This is enforced for
``requirements.txt``, ``test-requirements.txt``,
``doc/requirements.txt``, and extras defined in
``setup.cfg``. This is maintained by hand, with changes going through CI.

We also maintain a compiled list of the exact versions, including transitive
dependencies, of packages that are known to work in the OpenStack CI system.
This is maintained via an automated process that calculates the list and
proposes a change back to this repository. A consequence of this is that
new releases of OpenStack libraries are not immediately used: they have to
pass through this automated process before we can benefit from (or be harmed
by) them.

Each project team may also optionally maintain a list of "lower
bounds" constraints for the dependencies used to test the project in a
``lower-constraints.txt`` file. If the file exists, the requirements
check job will ensure that the values it contains match the minimum
values specified in the local requirements files, so when the minimums
are changed ``lower-constraints.txt`` will need to be updated at the
same time. Per-project test jobs can be configured to use the file for
unit or functional tests.

Format
------

``global-requirements.txt`` supports a subset of pip requirement file
contents. Distributions may only be referenced by name, not URL. Options
(such as -e or -f) may not be used. Environment markers
and comments are permitted. Version specifiers are only allowed for excluding
(blacklisting) versions, not setting minimum required versions. Minimum
required versions should be specified in ``lower-constraints.txt`` per-project.
A single distribution may be listed more than once if different specifiers are
required with different markers - for instance, if a dependency has dropped
Python 2.7 support.

``upper-constraints.txt`` is machine generated and nothing more or less than
an exact list of versions.


Enforcement for Test Runs
-------------------------

DevStack
++++++++

DevStack uses the pip ``-c`` option to pin all the libraries to known good
versions. ``edit-constraints`` can be used to unpin a single constraint, and
this is done to install libraries from git.

Enforcement in Projects
-----------------------

All projects that have accepted the requirements contract (as listed
in ``projects.txt``) are expected to run a requirements compatibility
job. This job ensures that a project can not change any dependencies to
versions not compatible with ``global-requirements.txt``. It also ensures that
those projects can not add a requirement that is not already in
``global-requirements.txt``. This ``check-requirements`` job should
be merged in infra before proposing the change to ``projects.txt`` in
``openstack/requirements``.

Update Processes
================

Updating dependency settings can be a two-step process.  If you create
both patches at the same time, #2 can use ``Depends-On`` to link it to
#1.

Adding a new dependency
-----------------------

1. Add the dependency to ``global-requirements.txt`` in
   ``openstack/requirements``, including any instructions for
   excluding versions or choosing different versions for python 2
   or 3. Add a recent version to ``upper-constraints.txt`` in the same
   repository at the same time.
2. Add the dependency to the appropriate requirements file(s) within
   the project tree, providing a minimum version specifier. Update the
   ``lower-constraints.txt`` file in the project tree at the same
   time.

Removing a dependency
---------------------

1. Remove the dependency from the requirements files in the project
   tree, including the ``lower-constraints.txt`` file.
2. Check for other projects using the dependency. If none do, update
   ``openstack/requirements`` to remove the item from
   ``global-requirements.txt``.

Updating the minimum version of a dependency
--------------------------------------------

1. Check the ``upper-constraints.txt`` file in
   ``openstack/requirements``. If the version there is lower than the
   desired version, prepare a patch to update the setting.
2. Update the minimum version in the relevant requirements file(s) in
   the project tree. Update the ``lower-constraints.txt`` setting in
   the same patch.

Excluding a version of a dependency
-----------------------------------

We need to maintain a consistent set of exclusions across all projects
to ensure that the ``upper-constraints.txt`` list of versions stays
co-installable.

1. Check ``global-requirements.txt`` in ``openstack/requirements``. If it
   does not exclude the version, prepare a patch to update the
   specifiers for the dependency. If the excluded version is currently
   being used in ``upper-constraints.txt``, update that file in the
   same patch.

   .. warning::

      Lowering the value in upper-constraints.txt may result in
      excluding a version that another project depends on. Check for
      this situation before proceeding.

2. Update the relevant requirements files in the project tree to add
   the exclusion. It is not necessary to copy the exclusion to every
   project that uses the dependency.

Review Guidelines
=================

There are a set of questions that every reviewer should ask on any
proposed requirements change. Proposers can make reviewing easier by
including the answers to these questions in the commit message for
their change.

General Review Criteria
-----------------------

- No specifications for library versions should contain version caps

  As a community we value early feedback of broken upstream
  requirements, so version caps should be avoided except when dealing
  with exceptionally unstable libraries.

  If a library is exceptionally unstable, we should also be
  considering whether we want to replace it over time with one that
  *is* stable, or to contribute to the upstream community to help
  stabilize it.

- Library specifications should not contain a minimum version

  Individual projects may want to start with different "lower bound"
  versions of dependencies, so we do not track those explicitly in the
  ``global-requirements.txt`` file.

- Commit message should refer to consuming projects(s)

  Preferably, the comments should also identify which feature or
  blueprint requires the new specification. Ideally, changes should
  already be proposed, so that its use can be seen.

- The blacklist is for handling dependencies that cannot be constrained.
  For instance, linters which each project has at a different release level,
  and which make projects fail on every release (because they add rules) -
  those cannot be globally constrained unless we coordinate updating all of
  OpenStack to the new release at the same time - but given the volunteer
  and loosely coupled nature of the big tent that is infeasible. Dependencies
  that are only used in unconstrained places should not be blacklisted - they
  may be constrained in future, and there's no harm caused by constraining
  them today. Entries in the blacklist should have a comment explaining the
  reason for blacklisting.

- Reviews that only update ``projects.txt`` should be workflow approved
  alongside or before other reviews in order to have the OpenStack Proposal Bot
  propagation be useful as soon as possible for the other projects. For project
  removal or addition, the +1 from the current PTL (or core if the PTL proposed
  the change) should be enough.

For new Requirements
--------------------

- Is the library actively maintained?

  We *really* want some indication that the library is something we
  can get support on if we or our users find a bug, and that we
  don't have to take over and fork the library.

  Pointers to recent activity upstream and a consistent release model
  are appreciated.

- Is the library good code?

  It's expected, before just telling everyone to download arbitrary 3rd
  party code from the internet, that the submitter has taken a deep dive
  into the code to get a feel on whether this code seems solid enough
  to depend on. That includes ensuring the upstream code has some
  reasonable testing baked in.

- Is the library python 3 compatible?

  OpenStack will eventually need to support python 3. At this point
  adding non python 3 compatible libraries should only be done under
  *extreme* need. It should be considered a very big exception.

- Is the library license compatible?

  The library should be licensed as described in `Licensing requirements`_,
  and the license should be described in a comment on the same line as the
  added dependency. If you have doubts over licensing compatibility, like
  for example when adding a GPL test dependency, you can seek advice from
  Robert Collins (lifeless), Monty Taylor (mordred) or Jim Blair (jeblair).

- Is the library already packaged in the distros we target (Ubuntu
  latest / Fedora latest)?

  By adding something to OpenStack ``global-requirements.txt`` we are
  basically demanding that Linux Distros package this for the next
  release of OpenStack. If they already have, great. If not, we should
  be cautious of adding it. :ref:`finding-distro-status`

- Is the function of this library already covered by other libraries
  in ``global-requirements.txt``?

  Everyone has their own pet libraries that they like to use, but we
  do not need three different request mocking libraries in OpenStack.

  If this new requirement is about replacing an existing library with
  one that's better suited for our needs, then we also need the
  transition plan to drop the old library in a reasonable amount of
  time.

- Is the library required for OpenStack project or related dev or
  infrastructure setup? (Answer to this should be Yes, of course)
  Which?

  Please provide details such as gerrit change request or launchpad
  bug/blueprint specifying the need for adding this library.

- If the library release is managed by the Openstack release process does
  it use the `cycle-with-intermediary` release type?

  This is needed to ensure that updated releases that consume requirements
  updates are available for integration/coninstallability tests with other
  projects.

- Do I need to update anything else?

  When new library is added, initial version of release needs to be added
  to ``upper-constraints.txt``. After that, OpenStack Proposal Bot will
  propose updates.

.. _Licensing requirements: https://governance.openstack.org/tc/reference/licensing.html

.. _finding-distro-status:

Finding Distro Status
---------------------

From the OpenStack distro support policy:

OpenStack will target its development efforts to latest Ubuntu/Fedora,
but will not introduce any changes that would make it impossible to
run on the latest Ubuntu LTS or latest RHEL.

As such we really need to know what the current state of packaging is
on these platforms (and ideally Debian, Gentoo, and SUSE as well).

For people unfamiliar with Linux Distro packaging you can use the
following tools to search for packages:

- Ubuntu - http://packages.ubuntu.com/
- Fedora - https://apps.fedoraproject.org/packages/
- Gentoo - https://packages.gentoo.org/
- SUSE - https://build.opensuse.org/project/show/devel:languages:python

For ``upper-constraints.txt`` changes
-------------------------------------

If the change was proposed by the OpenStack CI bot, then if the change has
passed CI, only one reviewer is needed and they should +2 +A without thinking
about things.

If the change was not proposed by the OpenStack CI bot, and only
changes the ``upper-constraints.txt`` entry for a new library release,
then the change should be approved if it passes the tests. See the
README.rst in openstack/releases for more details of the release
process.

If the change was not proposed by the OpenStack CI bot, and is not
related to releasing one of our libraries, and does not include a
``global-requirements.txt`` change, then it should be rejected: the CI
bot will generate an appropriate change itself. Ask in
#openstack-infra if the bot needs to be run more quickly.

Otherwise the change may be the result of recalculating the constraints which
changed when a ``global-requirements.txt`` change is proposed. In this case, ignore
the changes to ``upper-constraints.txt`` and review the
``global-requirements.txt`` component of the change.

stable-branch maintenance
-------------------------

Upper-constraints
+++++++++++++++++

Most of the work is done by stable-maint in the releases project.  The releases
project ensures valid stable releases (little to no API level changes, bugfix
only, etc).  Once released, the new version is requested to be updated in
requirements.  The following restrictions are in place to help ensure stable
branches do not break.

- In stable branches, we usually only update constraints for projects managed
  within the OpenStack community. Exceptions are made for other projects when
  there are gate issues. Those updates must be proposed by hand.

- The requirements team also verifies the new version's requirements changes
  line up with the requirements in the stable branch (GR and UC).

Global-requirements
+++++++++++++++++++

These should be few and far between on stable branches, mainly masking known
bad versions or in extreme adding a maximum version allowable for a package.
We work to remove these caps as well.  Raising effective minimums is only
acceptable during `Phase I`, and only due to security issues.

.. _Phase I: https://docs.openstack.org/project-team-guide/stable-branches.html#support-phases

New requirements
++++++++++++++++

In nearly all cases this is not allowed.  An example where this is allowed
would be:  A dependency of a dependency has an issue that impacts OpenStack.
It wasn't listed in global-requirements.txt but it is required.  In order to
block the affected releases and still be able to keep requirements in sync, we
list the library in global-requirements.txt and update all projects that
require it.

Resources
=========

- Documentation: https://docs.openstack.org/requirements/latest/
- Wiki: https://wiki.openstack.org/wiki/Requirements
- Bugs: https://launchpad.net/openstack-requirements
