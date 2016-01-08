====================
 Release Management
====================

OpenStack project teams produce a large variety of code repositories. Some
are services providing infrastructure APIs. Some are libraries being consumed
by those services. Some are supporting cast and tools. Most of those
are formally "released" at given points. We call those "deliverables", and
use git tags to define the release points.

OpenStack deliverables can be released under three different models. Most
follow a common 6-month development cycle, with some releasing intermediary
releases within that. The release management team manages the release process
for a selection of project teams repositories (indicated by the release:managed
tag) and provide tools for other teams and repositories to do self-service
releases.


Release models
==============

Common cycle with development milestones
----------------------------------------

By default, most OpenStack services opt to follow a common, time-based
release model. It results in a single release at the end of the development
cycle. It is recommended at the earlier stages of development, when
it is difficult to commit to multiple upgrade paths and large features or
architectural refactors are still common.

Projects following that model use a pre-version numbering scheme. If the
final release will be called 5.0.0, intermediary milestones will be called
5.0.0.0b1, 5.0.0.0rc2 etc.

This time-based release model includes 3 development milestones, called
``$SERIES-1``, ``$SERIES-2`` and ``$SERIES-3``. Those make useful reference
points in time to organize the development cycle. Project teams may, for
example, set specific deadlines that match those dates. b1, b2 and b3 tags are
pushed to the repositories to clearly mark those reference points in the git
history.

The dates for the milestones and final release in a given development cycle
are defined by the Release Management team, and communicated before the new
development cycle starts.

The $series-3 milestone coincides with Feature Freeze ("FF"). Managed projects
are requested to stop merging code adding new features, new dependencies, new
configuration options, database schema changes, changes in strings... all
things that make the work of packagers, documenters or testers more difficult.
Feature Freeze Exceptions ("FFE") may be requested by PTLs to the Release
managers. As we get closer to the release date, those are less likely to get
accepted.

At the same time as Feature Freeze, is Soft String Freeze. Translators start
to translate the strings after ``$SERIES-3``. To aid their work, it is
important to avoid changing existing strings, as this will invalidate some of
their translation work. New strings are allowed for things like new log
messages, as in many cases leaving those strings untranslated is better than
not having any message at all.

After the $series-3 milestone, a list of release-critical bugs is built, and
when all those are fixed (or considered not-release-critical after all), a
first release candidate is tagged (rc1). This RC1 will be used as-is as the
final release, unless new release-critical issues are found that warrant a RC
respin.

After RC1 is tagged, a stable/$series branch is cut from that same commit.
That is where further release candidates (and the final release) will be
tagged. The master branch starts on the new development cycle and is no
longer feature-frozen.

After RC1 is tagged, that project hits a Hard String Freeze. At this point the
translation team tries to complete the translation before the final release.
Any string changes after RC1 should be discussed with the translation
team. It is expected that at least 10 working days after RC1 there will be
another milestone tagged that includes the latest translations.

Potential new release critical issues have first to get fixed on the master
branch. Once merged in master, based on their severity and the risk of
regression in the patch, they may or may not trigger the opening of a
new release candidate window. Once a new RC window is opened, a number of
bugs are marked for backport to the stable branch, and the new RC tag is
pushed when all the backports are completed. Until the final release is
produced, the stable/$series branch is under the control of the release
management team, to make sure that merges happen only when a release
candidate window is open.

On final release day, each project's last release candidate is tagged with
the final release version. There is no difference between the last release
candidate and the final version, apart from the version number. The stable
branch passes under stable maintenance team management, and is open for
backports following the stable branch rules.

Common cycle with intermediary releases
---------------------------------------

Projects which want to do a formal release more often, but still want to
coordinate a release at the end of the cycle from which to maintain a stable
branch may opt for this model. This is especially suitable to more stable
projects which add a limited set of new features and don't plan to go through
large architectural changes. Getting the latest and greatest out as often as
possible, while ensuring stability and upgradeability.

Projects following this model do not use intermediary development milestones.
They may tag new versions at any point in time during the development cycle.
They do not use Feature Freeze, they do not go through a RC cycle. They use
a post-version semver-based numbering scheme, where every tag is a X.Y.Z
version.

Those projects must tag a final version for a development cycle (generally
in the last month of the cycle). A stable branch is cut from that proposed
version, and the master branch will from then on produce releases of the
next development cycle. If a critical issue is found in the "final release",
backports can be pushed to the stable branch and a new release be
tagged there. That is why it is important to increment at least the Y component
of the X.Y.Z version when we switch to the next development cycle, so that the
Z component can be used in future tags on the release (or stable) branch.

While the release management team will not enforce a formal feature-frozen
period for projects in an independent release model, it is recommended to
focus on bugfixes and hold on major disruptive features as you get closer
to the end of a development cycle, to ensure that the final release of any
given development cycle is as usable and bug-free as it can be.

Independent release model
-------------------------

Deliverables that do not benefit from a coordinated release or from stable
branches may opt to follow a completely independent release model.

Versions are tagged from the master branch without any specific constraint,
although the usage of a post-version numbering scheme based on
`semantic versioning`_ is strongly recommended.

.. _library-release-process-managed:

Libraries release process, managed projects
===========================================

Releases for managed projects are handled by the release team at the
request of the release liaison for the project. Requests should be
submitted in the form of a patch to the appropriate "deliverables"
file in the ``openstack/releases`` git repository. See the README file
in that repository for more details.

Libraries release process, unmanaged projects
=============================================

OpenStack library projects follow the "common cycle with intermediary releases"
model described above, including the use of stable branches. They use a
variation of `semantic versioning`_ (or SemVer) rules to choose
version numbers. Library releases may be managed by the Release
Management team, or by the liaison from the project team.

Releases occur as often as weekly (or more), and are typically
scheduled for early in the day and early in the week, based on the
time zone of the library maintainers. This scheduling gives the
maintainers plenty of time to handle issues that arise after a new
release is made to minimize the duration of any outage, without
requiring extra effort outside of a normal work week by overlapping
with the weekend.

Releases are created by pushing a *signed* tag to the gerrit
repository where the library is managed. The CI system recognizes the
new signed tag, and triggers the jobs that build the packages and
upload them to the distribution servers (our tarball site and the
Python Package Index).

The release scripts also manage milestones within the Launchpad page
for the project, recording the release date and completed work.
Project that track the work that will land in an upcoming release can
use the special milestone name "``next-$SERIES``", which will be
renamed as a release is created. If no ``next-$SERIES`` milestone
exists, a new empty milestone is created and then the fixed bugs and
closed blueprints not already associated with a milestone are moved
into the new milestone.

Announcing each library release is an important part of the process,
because new versions may introduce instability into the test
systems. The release notes for each library version are generated
automatically and sent to the openstack-announce mailing list as part
of the release process.

For more details about setting up a repository to support automated
releases, see the `Repository Creator's Guide`_ from the
*Infrastructure User Manual*.

.. _Repository Creator's Guide: http://docs.openstack.org/infra/manual/creators.html
.. _semantic versioning: http://docs.openstack.org/developer/pbr/semver.html


Release Liaisons
================

As with other cross-project teams, the release management team relies
on a liaison from each participating project to help with coordination
and release-related tasks. The liaison is usually the PTL, but the PTL
can also delegate the responsibilities to someone else on the team by
updating the liaison list on the CrossProjectLiaisons_ wiki page.

.. _CrossProjectLiaisons: https://wiki.openstack.org/wiki/CrossProjectLiaisons

Liaison Responsibilities
------------------------

The liaison does not have to personally do all of these things, but
must ensure they are done by someone on the project team.

#. Monitor the release schedule and remind team members of deadlines.

#. Ensure that release-related patches in the project are reviewed in
   a timely manner.

   From time to time, teams need to merge changes to their projects to
   stay current with release team practices. The release team relies
   on liaisons to help make and review such changes quickly to avoid
   blocking future releases. For example, keeping the requirements
   lists up to date, adding tools, and updating packaging files.

#. Submit release requests and milestone tags. If the request is not
   submitted by the liaison or PTL, one of them must indicate their
   approval.

   See :ref:`library-release-process-managed` above.

#. Coordinate feature freeze exceptions (FFEs) at the end of a
   release, and track blocking bug fixes and feature work that must be
   completed before a release

   The period between feature freeze and release should be used to
   stabilize new features and fix bugs. However, for every release
   there are a few "must have" features that do not quite make the
   deadline for a variety of reasons. It is up to the project team to
   decide which features they will allow after the deadline, and which
   will be delayed until the next release. The liaison is responsible
   for tracking any open exceptions to the feature freeze, and helping
   the project team to focus their energy on completing the work in a
   timely fashion.

#. Be available in the ``#openstack-release`` IRC channel on freenode
   to answer questions and address issues.

   There are too many projects for the release team to join all of
   their channels. Please join the central release channel when you
   are on IRC.

#. Monitor and participate in mailing list discussions about release
   topics.

   The primary means of communication between the release management
   team and other project teams is the openstack-dev mailing
   list. Liaisons must be subscribed and ensure that they pay
   attention to threads with the topic "[release]". Watch for
   instructions related to deadlines, release changes that need to be
   made, etc.

#. Manage the release-related tags on project deliverables in the
   project list in the ``openstack/governance`` repository.

   Ensure that as new repositories are added to the list managed by
   each project team, the release model and project type tags are
   accurate.

Typical Development Cycle Schedule
==================================

The development cycles follow a repeating pattern, which is described
in general terms here. The length of time between milestones may
change from cycle to cycle because of holidays, summit scheduling, and
other factors, so consult the wiki for the actual schedule for the
current cycle.  The cycles follow a repeating pattern, which is
described more generally here.

Weeks with negative numbers are counting down leading to the event
("Summit -2" is 2 weeks before the summit). Weeks with positive
numbers are counting up following an event ("Feature Freeze +1" is the
week following the feature freeze).

.. note::

  Dates for elections are specified in the Technical Committee charter
  relative to the design summits, while most other dates are based on
  community consensus and expressed in terms of the release date.
  Because the summit may move around in the cycle, the two scheduling
  systems may overlap differently in different cycles.

Weeks Leading to Milestone 1
----------------------------

*Usually 4-6 weeks*

- Finishing work left over from previous cycle
- Completing blueprint and spec discussions
- Foundational work for the rest of the cycle

Weeks Leading to Milestone 2
----------------------------

*Usually 5-6 weeks*

Normal development work

Weeks Leading to Milestone 3
----------------------------

*Usually 4-6 weeks*

- Feature development completion
- Bug fixes
- Stabilization work

Feature Freeze -1
-----------------

The week before the full feature freeze we prepare the final releases
for Oslo and other non-client libraries to give consuming projects
time to stablize and for the owners to prepare bug fixes if needed.

- Final Oslo and non-client library release

Milestone 3 / Feature Freeze
----------------------------

- Feature development stops ("feature freeze")
- Message strings stop changing ("string freeze") to give the
  translation team time to finish their work
- Dependency specifications stop changing ("requirements freeze") to
  give packagers time to prepare packages
- Final releases for client libraries for all projects. Note that new
  features that block other projects need to be released earlier in
  the cycle than this, since the projects will not be able to adopt
  them while the feature freeze and requirements freeze are in effect.

Feature Freeze +1
-----------------

- Final Feature Freeze Exceptions merged
- Create stable branches for all libraries

Release -3
----------

The release candidate period spans several weeks, and usually starts
the week after the feature freeze.

- All projects issue their first release candidates
- Create branches for all services to use for release candidates, and
  eventually stable maintenance work

Release -2
----------

- Create the stable branch for the global requirements list and
  testing tools like devstack and grenade
- Remove the freeze for the global requirements list on the master
  branch
- All library releases freeze

Release -1
----------

Final release candidates, with translations

Release 0
---------

- Emergency last-minute release candidates (unlikely)
- Tag the final release candidates as the official release early on
  Thursday of this week
- All library releases freeze on master ends

Summit -2
---------

Final summit planning and design session preparation.

Summit
------

The semi-annual Design Summit and Conference where contributors,
operators, and users meet in person to discuss the state of the
project and future work.

.. _Mitaka Release Schedule: https://wiki.openstack.org/wiki/Mitaka_Release_Schedule

Managing Release Notes
======================

Release notes for OpenStack deliverables are managed in the source
repository for the project using reno_. The reno documentation
explains how the tool works in general, and the instructions below
explain how to set it up for use in your project.

Directory Structure
-------------------

Most projects have a ``doc/source`` directory with Sphinx configured
to build developer-focused documentation that is eventually published
under ``https://docs.openstack.org/developer/$PROJECT``. Release notes
are not developer-focused, so they need to be published separately,
and that means a separate Sphinx project in the source tree. The jobs
that run the release note builds expect to find that project in
``releasenotes/source``.

The release note files read by reno should be kept in
``releasenotes/notes``. *Only* release notes YAML files should be
placed in this directory.

Setting up the Release Note Tool Within Your Project
----------------------------------------------------

The release notes are built from the configuration in the master
branch, and pull notes from all of the stable branches for which notes
should be published. Start by following these steps to configure the
master branch build, and then backporting necessary changes to the
stable branches where you wish to use reno.

#. Set up a new Sphinx project using ``sphinx-quickstart``. The
   interactive prompts will ask where to put the new files. If you run
   the tool from the root of your git repository, answering
   ``releasenotes/source`` will produce the correct results.

#. Edit ``releasenotes/source/conf.py`` to change the ``extensions``
   list to include ``'reno.sphinxext'``.

#. Edit ``test-requirements.txt`` to add ``reno``. Make sure to use
   the current entry from the global requirements list to avoid
   version conflicts.

#. Create a directory ``releasenotes/notes`` and add an empty
   ``.placeholder`` file to ensure git tracks the directory.

#. Create a file to hold the release notes from the "current" branch
   by using a ``release-notes`` directive without specifying an
   explicit branch. This file is used by the test jobs to ensure that
   patches on a stable branch cannot introduce release notes that
   break the real release notes build job on the master branch. For
   example, Glance uses ``releasenotes/source/unreleased.rst``
   containing:

   ::

      ==============================
       Current Series Release Notes
      ==============================

      .. release-notes::

#. Create a separate file for each stable branch for which you plan to
   use reno to manage release notes. Use the ``release-notes``
   directive to generate the correct release notes for each
   series. For example, the liberty release is represented in a file
   called ``releasenotes/source/liberty.rst`` containing:

   ::

      ==============================
       Liberty Series Release Notes
      ==============================

      .. release-notes::
         :branch: origin/stable/liberty

#. Edit ``releasenotes/source/index.rst`` to remove most of the
   automatically-generated content and replace it with a title and
   ``toctree`` referring to the branch files you created in the
   previous two steps.

#. Update ``tox.ini`` to add a ``releasenotes`` test environment by
   adding:

   ::

      [testenv:releasenotes]
      commands = sphinx-build -a -W -E -d releasenotes/build/doctrees -b html releasenotes/source releasenotes/build/html

#. Submit all of the above changes together as one patch. For example,
   see https://review.openstack.org/241323 and
   https://review.openstack.org/243302 (Glance was set up using 2
   separate patches).

.. note::

   Repeat this process for any existing stable branches for which reno
   is being used for release notes, back through
   stable/liberty. Although we do not run reno in the branches to
   publish the notes, we *do* run it in test jobs to ensure that
   release note changes in stable branches do not break the release
   note build in master.

Adding the Release Notes Jobs to Your CI
----------------------------------------

After your project has the necessary change to enable reno to build
the release notes, the next step is to modify the CI system to add the
necessary jobs. All of these changes are made to the
``openstack-infra/project-config`` repository.

#. Modify the section of ``jenkins/jobs/projects.yaml`` related to
   your repository to add the ``openstack-releasenotes-jobs`` job
   group to the list of jobs for your project.

#. Modify the section of ``zuul/layout.yaml`` related to your
   repository to add ``release-notes-jobs`` to the list of job
   templates for your project.

#. Submit all of the changes as one patch. You may want to set the
   ``Depends-On`` tag in the commit message to point to the Change-Id
   of the commit from the previous section, to avoid adding jobs that
   will fail until that patch lands. For example, see
   https://review.openstack.org/241344.

How to Add New Release Notes
----------------------------

reno scans the git history to find release notes files and tags to
determine which notes are part of each release. That means you need to
put the notes for a release into the branch where the release will be
generated *before* the release is tagged. The note files can be edited
later, but they will always appear under the first release in the
series where they were introduced.

In general, release notes should be added with fixes that go into the
master branch, and then included in the backport for the fix as it
goes into older stable branches. Because the release notes for each
series are generated separately, the same note may appear in the
output for multiple versions.

If a note does not apply to the master branch for some reason, it can
be added directly to the stable branch.

Use ``reno new`` to generate a new release note file with a unique
suffix value. The unique filename created by reno ensures that there
will be no merge conflicts as the fix is backported. For example:

.. code-block:: bash

  $ tox -e venv -- reno new bug-XXX

After the new file is created, edit it to remove any sections that are
not relevant and to add notes under the appropriate sections. Refer to
the `Editing a Release Note
<http://docs.openstack.org/developer/reno/usage.html#creating-new-release-notes>`__
section of the reno documentation for details about what should go in
each section of the YAML file and for tips on formatting notes.

To see the rendered version of the new release note, you need to
commit the change so reno can find the note file in the git log, and
then build the release notes documentation.

.. code-block:: bash

  $ git commit  # Commit the change because reno scans git log.

  $ tox -e releasenotes

Then look at the generated release notes files in
``releasenotes/build/html`` in a web browser.

When to Add Release Notes
-------------------------

The release notes for a patch should be included in the patch. If not, the
release notes should be in a follow-on review.

If the patch meets any of the following criteria, a release note is
recommended.

* Upgrades

  * The deployer needs to take an action when upgrading
  * A new configuration option is added that the deployer should
    consider changing from the default
  * A configuration option is deprecated
  * A configuration option is removed

* Features

  * A new feature is implemented
  * A feature is marked for deprecation
  * A feature is removed
  * Default behavior is changed

* Bugs

  * A security bug is fixed
  * A long-standing or important bug is fixed

* APIs

  * A driver interface or other plugin API changes
  * A REST API changes

.. _reno: http://docs.openstack.org/developer/reno/
