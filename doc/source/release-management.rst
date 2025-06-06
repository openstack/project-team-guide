====================
 Release Management
====================

OpenStack project teams produce a large variety of code repositories. Some
are services providing infrastructure APIs. Some are libraries being consumed
by those services. Some are supporting cast and tools. Most of those
are formally *released* at given points. We call those *deliverables* and
use git tags to define the release points. Each deliverable may contain one or
more git repositories, which are all tagged with the same version at the same
moment.


Deliverables handled by the Release management team
===================================================

The Release Management project team is generally responsible for handling
release management for all official OpenStack deliverables, as determined by
the Technical Committee. This guarantees a convergence and basic release
management standards across all of "OpenStack" software.

Official OpenStack deliverables produced by each project team are described
in the ``reference/projects.yaml`` file in the ``openstack/governance``
repository. By default, release management for all those deliverables is
handled by the release management team, through an automated process driven
from changes proposed to the ``openstack/releases`` git repository.

Exceptions to this general rule are documented in the deliverable definition
(in the ``reference/projects.yaml`` file) using the ``release-management``
key:

* **release-management: none** means that the deliverable is not released,
  and does not need any release management. Examples include specs
  repositories, cookiecutter repositories, etc.

* **release-management: external** means that the deliverable is published
  on a separate publication platform, and its release is managed by its
  project team directly. This should stay exceptional, and is generally
  limited to corner cases like deployment tools. Examples include Chef
  recipes being published in the Chef supermarket, of OpenStack Charms being
  published on the Charm store.

* **release-management: deprecated** means that, while it is present in
  ``reference/projects.yaml`` for now, the deliverable will soon be removed,
  and should not be released anymore.

The release management team periodically checks that the deliverables
defined in governance are consistent with the deliverable files present
in the ``openstack/releases`` git repository.


Release cycle
=============

OpenStack development follows a common, time-based release cycle. It results
in a coordinated release of all the components that make up an "OpenStack"
release at the end of the development cycle.

You can find the schedule for the most current development cycle by following
the 'schedule' link for the most recent series on the `releases website`_.

How often and when those various components will be released during the cycle
depends on the deliverable type (service, library...) and the release model
chosen. Deliverable types are described in the `deliverable types section`_
of the release management team documentation. Release models are described in
the `release models section`_ of the release management team's documentation.

Some deliverables, like general-purpose libraries that are not specific
to OpenStack, can be released completely outside of the release cycle,
under an `independent`_ release model.

.. _deliverable types section: https://releases.openstack.org/reference/deliverable_types.html

.. _release models section: https://releases.openstack.org/reference/release_models.html


Choosing a release model
========================

For each OpenStack deliverable, you should choose one of the available
release models. Here is a bit of guidance on how to choose, depending
on the type of deliverables considered.

Libraries
---------

Libraries come in three different styles: OpenStack client libraries,
other OpenStack-specific libraries, and generally-useful libraries.

OpenStack client libraries must pick the `cycle-with-intermediary`_ model.
In this model, they will release early and often, to make new features
quickly available to consuming services. To that effect, the release
management team will propose releases for libraries that have not been
released at least once for each development cycle milestone, assuming
there are significant changes.

Other OpenStack-specific libraries and generally-useful libraries can
pick the `cycle-with-intermediary`_ model, but may also pick the
`independent`_ release model. This latter option may make sense if the
library has no real ties to OpenStack at all, or if it's mostly stable
and feature-complete.

Services, Horizon plugins and other deliverables
------------------------------------------------

Other OpenStack components (including services, Horizon plugins and other
deliverables) have to adhere to the release cycle. They can choose between
the `cycle-with-rc`_ model and the `cycle-with-intermediary`_ model.

The `cycle-with-rc`_ model is the historic OpenStack release model. In this
model, a single release is produced at the end of the development cycle for
inclusion in the coordinated OpenStack release. The major release number
(the X in X.Y.Z numbers) is incremented for each release series.

Near the end of the cycle, such deliverables enter a Feature Freeze period.
They are requested to stop merging code adding new features, new dependencies,
new configuration options, database schema changes, changes in strings... all
things that make the work of packagers, translators, documenters or testers
more difficult. Feature Freeze Exceptions ("FFE") may be exceptionally granted
by project PTLs (or release liaison), but every FFE accepted results in more
work, less time spent testing and fixing issues in release candidates,
therefore lowering the quality of the end release.

Once the deliverable is deemed ready, a first release candidate ("RC1") is
created, together with a stable branch on which release-critical issues can
be fixed and further release candidates created. The master branch starts
on the new development cycle and is no longer feature-frozen.

On final release day, the Release Team will take each project's last release
candidate and re-tag it with the final release version. There is no difference
between the last release candidate and the final version, apart from the
version number. The stable branch then passes under stable maintenance team
management, and is open for backports following the stable branch rules.

The alternative model is the `cycle-with-intermediary`_ model. It allows you
to release when you want, as often as you want. The only expectation is that
by the RC1 target week, a stable branch should be created from the
latest-available release. Point releases may be created from this stable
branch as release-critical issues are found. On the coordinated release date,
the most recent release available on the stable branch will be included in the
OpenStack release.

In this model, to make room for stable point releases and avoid version
collisions, we increment at least the minor number (the Y in X.Y.Z) for the
first release in the next development cycle. Numbering otherwise follows
`semantic versioning`_ rules.

The `cycle-with-intermediary`_ model works well if you want to be able to
release early and often, don't need to refine your final release as much,
or don't want as much guidance and process to produce releases. It is still
recommended to focus on bug fixes and hold on major disruptive features as
you get closer to the end of a development cycle, to ensure that the final
release of any given development cycle is as usable and bug-free as it can be.

Trailing the common cycle
-------------------------

Deployment and lifecycle-management tools generally want to follow the
release cycle, but because they rely on the other projects being completed,
they are given more time to publish their final releases.

If they plan to do a single release and want to use RCs, they should choose
the `cycle-with-rc`_ model. If they want more flexibility and are not using
RCs, they should opt for the `cycle-with-intermediary`_ model.

.. _cycle-with-rc: https://releases.openstack.org/reference/release_models.html#cycle-with-rc

.. _cycle-with-intermediary: https://releases.openstack.org/reference/release_models.html#cycle-with-intermediary

.. _independent: https://releases.openstack.org/reference/release_models.html#independent


How to release ?
================

Releases occur as often as weekly (or more), and are typically
scheduled for early in the day and early in the week, based on the
time zone of the library maintainers. This scheduling gives the
maintainers plenty of time to handle issues that arise after a new
release is made to minimize the duration of any outage, without
requiring extra effort outside of a normal work week by overlapping
with the weekend.

Technically, releases are created by pushing a *signed* tag to the git
repositories associated with that deliverable. The CI system recognizes the
new signed tag, and triggers the jobs that build the packages, upload them
to the distribution servers (our tarball site and the Python Package Index),
and send email announcements.

For more details about setting up a repository to support automated
releases, see the `Project Creator's Guide`_ from the
*Infrastructure User Manual*.

.. _Project Creator's Guide: https://docs.opendev.org/opendev/infra-manual/latest/creators.html


The tagging and releasing process is error-prone. In order to properly review
proposed tags and run tests before the tag is actually pushed, we use a
specific repository, ``openstack/releases``, to file release requests.
Releases are requested by the PTL or release liaison for the project, in the
form of a patch to the appropriate "deliverables" file of that repository.
See the `README file in that repository`_ for more details.

Such requests are then automatically tested, reviewed and processed by the
Release Team, generally avoiding weekends when no one would be around to help
triage potential release automation issues.

.. _README file in that repository: https://opendev.org/openstack/releases/src/branch/master/README.rst

.. _semantic versioning: https://docs.openstack.org/pbr/latest/user/semver.html#semantic-versioning-specification-semver


Release Liaisons
================

As with other cross-project teams, the release management team relies
on a liaison from each participating project to help with coordination
and release-related tasks. The liaison is usually the PTL, but the PTL
can also delegate the responsibilities to someone else on the team by
updating the liaison list in the ``data/release_liaisons.txt`` file in the
``openstack/releases`` repository.

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

#. Submit or validate release requests. If the request is not
   submitted by the liaison or PTL, one of them must indicate their
   approval.

#. Coordinate feature freeze exceptions (FFEs) at the end of a release
   cycle (for cycle-with-milestones deliverables), and track blocking
   bug fixes and feature work that must be completed before a release.

   The period between feature freeze and release should be used to
   stabilize new features and fix bugs. However, for every release
   there are a few "must have" features that do not quite make the
   deadline for a variety of reasons. It is up to the project team to
   decide which features they will allow after the deadline, and which
   will be delayed until the next release. The liaison is responsible
   for tracking any open exceptions to the feature freeze, and helping
   the project team to focus their energy on completing the work in a
   timely fashion.

#. Be available in the ``#openstack-release`` IRC channel on OFTC
   to answer questions and address issues.

   There are too many projects for the release team to join all of
   their channels. Please join the central release channel when you
   are on IRC.

#. Monitor and participate in mailing list discussions about release
   topics.

   The primary means of communication between the release management
   team and other project teams is the openstack-discuss mailing
   list. Liaisons must be subscribed and ensure that they pay
   attention to threads with the topic "[release]". Watch for
   instructions related to deadlines, release changes that need to be
   made, etc.

#. Keep the list of project deliverables (and associated git repositories)
   in the project team reference list in the ``openstack/governance``
   repository (``reference/projects.yaml``) up to date.


Typical Development Cycle Schedule
==================================

The OpenStack development cycles follow a repeating pattern, which is
described in general terms here. The length of time between milestones
may change from cycle to cycle because of holidays, event scheduling,
and other factors, so consult the actual 'Under development' schedule
on the `releases website`_ for the actual schedule.

Weeks Leading to Milestone 1
----------------------------

*Usually 4-6 weeks*

- Discussing objectives for the cycle
- Completing blueprint and spec discussions
- Foundational work for the rest of the cycle

Weeks Leading to Milestone 2
----------------------------

*Usually 5-6 weeks*

- Normal development work

Weeks Leading to Milestone 3
----------------------------

*Usually 4-6 weeks*

- Pushing back some objectives and refocus on key priorities
- Feature development completion
- Bug fixes
- Stabilization work

Feature Freeze -1
-----------------

The week before the full feature freeze we prepare the final releases
for Oslo and other non-client libraries to give consuming projects
time to stabilize and for the owners to prepare bug fixes if needed.

- Final Oslo and non-client library release

.. note::

  Exceptions may be requested for libraries impacting project releases
  if it is deemed critical to the release and the risk of an update
  causing regressions is low.

  To request an exception for a library release past the freeze, send
  an email to the openstack-discuss mailing list with the following tags
  in the subject line::

    [release][requirements][other-impacted-projects]

  The release and requirements teams will evaluate the risks and provide
  feedback.

  If at all possible, it is best to wait until the freeze is over and do
  a stable release of the library afterwards.

Milestone 3 / Feature Freeze
----------------------------

- Feature development stops for cycle-with-rc deliverables ("feature freeze")
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

Release Candidate Period, Release -3
------------------------------------

The release candidate period spans several weeks, and usually starts
the week after the feature freeze.

- Cycle-with-rc projects issue their first release candidates
- Create release branches for deliverables
- Submit cycle-highlights in the project deliverables yaml file. See
  below for information about cycle-highlights.

  During this period, patches submitted to and being merged into the
  new branch should be managed carefully.

  1. Avoid aggressive backports during this time period, since having
     a lot of pending reviews consumes reviewer resources and makes it
     harder to understand which patches are release blockers.
  2. All code patches should merge into the master branch before being
     approved to merge into the new release branch.
  3. Translation updates should be merged quickly to ensure they make
     it into the final release.
  4. Requirements sync patches should be merged quickly to ensure they
     make it into the final release.

Release -2
----------

- Create the stable branch for the global requirements list and
  testing tools like devstack and grenade
- Remove the freeze for the global requirements list on the master
  branch
- Freeze all library releases, except independently-released libraries
  (which can still be released, although constraint and requirement changes
  will be held until the end the freeze period)

Release -1
----------

- Final release candidates and releases of components for inclusion in the
  final release.

Release 0
---------

- Emergency last-minute release candidates (unlikely)
- Tag the final release candidates as the official release early on
  Thursday of this week
- All library releases freeze on master ends

.. _releases website: https://releases.openstack.org/

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

.. _adding_the_release_notes_jobs_to_your_ci:

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

#. Edit ``releasenotes/source/conf.py`` and add:

   ::

      # -- Options for Internationalization output ------------------------------
      locale_dirs = ['locale/']

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
         :branch: stable/liberty

#. Edit ``releasenotes/source/index.rst`` to remove most of the
   automatically-generated content and replace it with a title and
   ``toctree`` referring to the branch files you created in the
   previous two steps.

#. Update ``tox.ini`` to add a ``releasenotes`` test environment by
   adding:

   ::

      [testenv:releasenotes]
      commands = sphinx-build -a -W -E -d releasenotes/build/doctrees -b html releasenotes/source releasenotes/build/html

#. Edit the Zuul config (this will typically be a ``.zuul.yaml`` file
   or a set of files in a ``.zuul.d`` subdirectory at the top level of
   the repo). Find the ``project:`` section and in its ``templates:``
   list add an entry for ``release-notes-jobs-python3`` like this::

        - project:
          templates:
            - check-requirements
            - integrated-gate-compute
            ...
            - publish-openstack-docs-pti
            - release-notes-jobs-python3
          check:
            jobs:
              ...

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
<https://docs.openstack.org/reno/latest/#creating-new-release-notes>`__
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

Not every patch is worth a release note. A user may skim through the
release notes for a dozen projects or more after the release, what is
helpful and what may be noise should be considered carefully.

.. _reno: https://docs.openstack.org/reno/latest/

How to Write a Good Release Note
--------------------------------

Release notes should be written from the perspective of the user and
what they should know. Here are a few sample questions to keep in mind
when writing them:

* What is particularly relevant from the end-user/deployer's
  perspective?
* What changes for them?
* Is there anything they need to do in particular?
* Will the change have an impact on their day-to-day use?

Release notes are not meant to be a replacement for git commit
messages. They should focus on the impact for the user and make that
understandable, even for people who don't know the full technical
context for the patch or project.

Updating Stable Branch Release Notes
------------------------------------

Occasionally it is necessary to update release notes for past releases.
Release notes need to be handled differently than normal code backports.

.. note::

   Due to the way reno parses release notes, if a note is updated on
   master instead of its original stable branch, it will then show up
   in the release notes for the later release.

See the reno user documentation for details on the correct way to
`Update stable branch release notes
<https://docs.openstack.org/reno/latest/user/usage.html#updating-stable-branch-release-notes>`__.

How to Preview Release Notes at RC-time
---------------------------------------

OpenStack projects on the common cycle with development milestones will
typically add a release note before each milestone and release candidate
is tagged.  These will appear on the same generated page, but separated
by tag.  After the stable branch is tagged for final release, however,
when the release notes are generated they will all be combined into a
single note.  If you're following the advice above about what to include
in release notes (and including release notes throughout the development
cycle on appropriate patches), then you're likely to have some notes with
a Prelude, some without, and so on for all the sections.  Before the release
is cut, you'll probably want to see exactly what the single generated note
is going to look like so that you can read through the entire note in the
same order that consumers will read it.  Here's one way to do that:

* Clone a new repo from git or make sure your copy is completely up to
  date.

* Suppose you're preparing for the Pike release, which will be tagged
  as '15.0.0' and is being prepared in the branch 'stable/pike'.
  Check out the stable/pike branch and create a tag for the release
  in your local repository: ``git tag 15.0.0``

* Check out master, and generate the release notes the usual way:
  ``tox -e releasenotes``

* Browse to the generated notes in the releasenotes/build/html
  directory

* When you're done proof reading, delete the tag:
  ``git tag -d 15.0.0``

Release Notes for SLURP Releases
--------------------------------

Beginning with the 2023.1 OpenStack release, each "dot-one" release
will be designated a SLURP (Skip Level Upgrade Release Process) release.
Such a release will support upgrades from either the immediate previous
release (that is, following the traditional OpenStack upgrade process) or
from the previous SLURP release.  (See the `Release Cadence Adjustment
<https://governance.openstack.org/tc/resolutions/20220210-release-cadence-adjustment.html>`_
resolution for details.)

An implication of supporting both the SLURP and the traditional release
process is that operators upgrading from one SLURP to the next SLURP may
not see the release notes from the release in between the SLURPs.  At the
same time, operators who follow the traditional release upgrade process
should not have to read the non-SLURP release notes twice.  (This
is because if people are forced to re-read a bunch of stuff, it is more
likely that their eyes will glaze over and they'll miss something new
and important.)

Thus, we need to make the non-SLURP release notes easily discoverable
from the SLURP release notes, both so that they don't clutter up the SLURP
notes and so that they are easily available for operators who haven't
already read them.  Discovery will be enhanced if all projects follow the
same basic structure for doing this, which is outlined below.

The first SLURP release is **2023.1**, the immediately following release
(non-SLURP) is **2023.2**, and the release immediately following that one is
**2024.1** (a SLURP release).  Deployers following the SLURP strategy will
upgrade directly from 2023.1 to 2024.1, skipping 2023.2.  Suppose that
2024.1 has not been released yet, and you are finalizing the 2024.1
(second SLURP) release notes now.

#. Generate a static page of the 2023.2 (SLURP minus 1) release notes.

   * when: shortly following the 2023.2 release (where "shortly" means
     after any release note corrections have been merged to stable/2023.2,
     but before any backports containing a **new** release note is merged
     to stable/2023.2)

     *  Because of the way reno works, corrections to release notes in
        stable branches must be made *directly to the stable branch*.
        Thus if the static file is generated too early, you will miss
        out on any corrections and will have to apply them manually
        to the static file (which isn't that big a deal, really).

   * how: from the root directory of the project repo:

     ::

       $ tox -e releasenotes --notest

       $ .tox/releasenotes/bin/reno report \
            --title "2023.2 Release Notes" \
            --branch "stable/2023.2" | \
           sed 's/^ *$//g' > "releasenotes/source/2023.2-static.rst"

   * a few points to note:

     * The title heading in the "regular" release notes is always
       "X Series Release Notes".  Note that we're using something
       different, namely, "X Release Notes".  This is because reno
       uses the title when generating anchors in the ``.rst`` file,
       and we need the anchors in the static document to be
       unique.

     * Make sure that you include "-static" in the filename
       being written to.  There will already be a ``xena.rst``
       file in the directory and we don't want to overwrite it.
       (That file is used for the "regular" release notes for
       the series, which will continue to be generated in
       the normal way as patches are backported following the
       normal backport process.)

     * Why use this extra static ``.rst`` file?  For three reasons:

       * Any release notes associated with changes since the non-SLURP
         2023.2 coordinated release will automatically be included in
         following SLURP release's notes, because the OpenStack
         backport process dictates that changes must be merged to
         release *n*\+1 before they can be merged to release *n*.
         We don't want these to be duplicated.

       * Having a static ``.rst`` file available will allow you
         to include anchors in the static file so that your SLURP
         release notes can link to specific items in the static page.

       * The static file can also be edited manually, if necessary, to
         emphasize items relevant to SLURP (or delete irrelevant items
         if the notes are very long).

#. Edit (and commit) the static release notes page.

   * when: as soon as you generate it

   * The top of the static ``.rst`` file should look something like this:

     .. code-block:: RST

       ====================
       2023.2 Release Notes
       ====================

       .. _2023.2 Release Notes_23.0.0_stable_2023.2:

       23.0.0
       ======

       .. _2023.2 Release Notes_23.0.0_stable_2023.2_Prelude:

       Prelude
       -------

       .. releasenotes/notes/2023.2-prelude-25dc371d85fb6610.yaml @ b'7ee5824c64b1c3c85d1ce1636bdecd86acb64970'

       Welcome to the 2023.2 (Bobcat) release of the OpenStack Block Storage
       service (cinder). With this release, we added several drivers ...

   * Here is the change that you must make to the static ``.rst`` file.
     We do not want it to be included in the release notes toctree; its
     sole purpose is to be linked to it from the SLURP release notes.
     Thus we need to add an ``:orphan:`` directive at the top of the page
     so that sphinx doesn't generate a warning (which the openstack
     releasenotes job treats as an error, thereby failing your releasenotes
     build).  It should be the first non-comment non-whitespace characters
     in the file:

     .. code-block:: RST

       :orphan:

       ====================
       2023.2 Release Notes
       ====================

       etc.

   * Don't forget to do a 'git add' to the static file, because you
     will need to commit it to the repository.

#. Add a link to the static page from the SLURP release notes.

   * when: when you prepare the final notes for the SLURP release
     (2024.1 in this example)

   * what: add an item to the "Prelude" section of the SLURP release
     that contains a link to the static page.  You do this
     in a regular reno ``.yaml`` file.  For example,

     .. code-block:: RST

        ---
        prelude: |
            Welcome to the 2024.1 release of the OpenStack Block Storage
            service (cinder) ...

            * Something important about this release.

            * Another important thing to mention.

            * **This is a SLURP release.**  If you are upgrading directly
              from the previous SLURP release (2023.1), we recommend that
              you read through the :doc:`release notes from the
              intermediate release <2023.2-static>` (2023.2).

There may be items of such importance from an intermediate non-SLURP
release that a project team may wish to reemphasize them in the SLURP
release notes.  That's perfectly OK.  We just ask that you do this
judiciously so that deployers don't think that they can completely ignore
the other notes from the intermediate release (which must be important, or
why did you write them in the first place?).  Since the intermediate
non-SLURP release notes are being kept in a static ``.rst`` document, you
don't have to repeat entire notes.  Instead, you can add an anchor to the
particular item you want to highlight, and then link to it from the SLURP
release notes.

Finally, as of this writing, we are in the development cycle of the first
SLURP-era non-SLURP release (i.e., 2023.2) and haven't actually had a SLURP
release yet that can be upgraded to from a previous SLURP release.
So we fully expect the release note process to evolve as both project
teams and deployers adapt to it.  But hopefully this simple approach is a
good start.


Cycle Highlights
================

Cycle highlights give a high-level, user-focused summary of what has
changed in the latest release. This is not necessarily the most
technically complex work you accomplished in the release, but is the
work that will have the largest impact on users. Cycle highlights
auto-populate the Release Highlights page at
``releases.openstack.org/$RELEASE/highlights.html``.

Adding Cycle Highlights
-----------------------

Cycle highlights should be submitted with RC1. This is done by adding
information to ``deliverables/$RELEASE/$PROJECT.yaml`` in the
``openstack/releases`` repo. You should include 3-5 cycle-highlight bullets.

.. code-block:: yaml

  cycle-highlights:
    - Introduced new service to use unused host to mine bitcoin
    - Merged code from shade, os-client-config and openstacksdk into
      a single library to create a unified and simpler our client-side library
    - Added Rescue Mode to let users recover from lost SSH keys and
      misconfigurations


You can check on the formatting of the output by either running locally:

.. code-block:: console

  tox -e docs

And checking the resulting file under
``doc/build/html/$RELEASE/highlights.html``, or you can view the output of the
``build-openstack-sphinx-docs`` job under ``html/$RELEASE/highlights.html``.

Writing a Good Cycle Highlight
------------------------------

Unlike commit messages for developers or reno release notes for operators,
cycle highlights are intended to give product managers, press, marketers,
users not responsible for operations, etc a snapshot of what will change
for them in this release. You submit 3-5 cycle-highlights bullets, with
a format of:

- What was changed/introduced, what it does for the user/benefit

Highlights should stay fairly brief--aim for less than 2 lines in length.

By submitting your highlights at RC1 or as close as possible, the
Release Management Team will be able to offer edits and help you write
cycle highlights that show off your work.
