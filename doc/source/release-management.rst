====================
 Release Management
====================

OpenStack project teams produce a large variety of code repositories. Some
are services providing infrastructure APIs. Some are libraries being consumed
by those services. Some are supporting cast and tools. Most of those
are formally "released" at given points. We call those "deliverables", and
use git tags to define the release points.

OpenStack deliverables can be released under various models. They all follow
the common 6-month development cycle, but may release more than once during
those cycles. The release management team manages the release process for
a selection of project teams repositories (indicated by the release:managed
tag) and provide tools for other teams and repositories to do self-service
releases.

Common time-based release model
===============================

By default, most OpenStack services opt to follow the common, time-based
release model. It is recommended at the earlier stages of development, when
it is difficult to commit to multiple upgrade paths and large features or
architectural refactors are still common.

Projects following that model use a pre-version numbering scheme. If the
final release will be called 5.0.0, intermediary milestones will be called
5.0.0b1, 5.0.0rc2 etc.

The common 6-month time-based release cycle includes 3 development milestones,
called $series-1, $series-2 and $series-3. Those make useful reference points
in time to organize the development cycle. Project teams may, for example,
set specific deadlines that match those dates. b1, b2 and b3 tags are pushed
to the repositories to clearly mark those reference points in the git
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

After the $series-3 milestone, a list of release-critical bugs is built, and
when all those are fixed (or considered not-release-critical after all), a
first release candidate is tagged (rc1). This RC1 will be used as-is as the
final release, unless new release-critical issues are found that warrant a RC
respin.

After RC1 is tagged, a stable/$series branch is cut from that same commit.
That is where further release candidates (and the final release) will be
tagged. The master branch starts on the new development cycle and is no
longer feature-frozen.

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


Independent release model
=========================

Projects which want to do a formal release more often may opt for an
independent model. This is especially suitable to more stable projects
which add a limited set of new features and don't plan to go through large
architectural changes. Getting the latest and greatest out as often as
possible, while ensuring stability and upgradeability.

Projects following this model do not use intermediary development milestones.
They may tag new versions at any point in time during the development cycle.
They do not use Feature Freeze, they do not go through a RC cycle.

Those projects must tag a final version for a development cycle (generally
in the last month of the cycle). A stable branch is cut from that proposed
version, and the master branch will from then on produce releases of the
new development cycle. If a critical issue is found in the "final release",
backports can be pushed to the stable branch and a new "final release" be
tagged there. That is why it is important to increment at least the Y component
of the X.Y.Z version when we switch to the next development cycle, so that the Z
component can be used in future tags on the stable branch.

While the release management team will not enforce a formal feature-frozen
period for projects in an independent release model, it is recommended to
focus on bugfixes and hold on major disruptive features as you get closer
to the end of a development cycle, to ensure that the final release of any
given development cycle is as usable and bug-free as it can be.


Libraries release model
=======================

OpenStack library projects follow the independent release model
described above, including the use of stable branches. They use a
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

Feature branches
================

...
