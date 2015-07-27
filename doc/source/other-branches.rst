================
 Other branches
================

In previous chapters we mention usage of the master branch (which
represents current active development) and :doc:`stable-branches`
(where backports following the stable branch policy are posted).
Under specific conditions, it is possible to set up other types of
branches, and this chapter will detail them.


Feature branches
================

Some complex features or significant refactors may take a long time
to develop to the point where they are ready for merging on the
``master`` branch. Maintaining those features as a series of patchsets
can be very painful. To handle those exceptional cases, projects may
create *feature branches*, so that iterating on this large feature
can be done as commits in a temporary git branch.

Creating the Branch
-------------------

Feature branches should be used infrequently, because they bypass regular
CI testing and usually result in extra pain and effort when the work needs
to be merged back into master. New feature branches will be created by the
release team at the request of the project team, after evaluating the need for
the branch. A branch should only be created if the change that would be
developed on it is deemed complex enough to justify the extra pain.

Naming the Branch
-----------------

Feature branches are named using a "``feature/``" prefix. What goes after
that prefix is left to the choice of the project, but should generally
describe the feature that will be developed on the branch.

Maintenance teams
-----------------

Unlike stable branches, feature branches are not managed by a separate
team. They are directly managed by the core review team for the project,
like the ``master`` branch is.


Bug Branches
============

Projects that follow the independent or intermediate release models,
especially library projects, may find that they need to release bug
fixes in the middle of a cycle. Normally this should happen from the
master branch of the project, even if there are new features. Fixes
for stable branches are also supported (see
:doc:`stable-branches`). However, occasionally there is a need to
release a fix into the master testing environment without releasing
the current master branch of a project, because there are other
breaking changes that are not yet ready to be released. For these
cases, a branch can be made from the previous release tag using the
prefix "``bug/``", and the fix can be released from that branch.

Creating the Branch
-------------------

Bug branches should be used extremely infrequently, because they
represent a mechanism for releasing versions out of the normal
development stream. New bug branches will be created by the release
team at the request of the project team, after evaluating the need for
the branch. A branch should only be created if there are breaking
changes on the master branch of the project that cannot be released.

Naming the Branch
-----------------

Releases from bug branches must only include fixes, and no features,
so they can be given SemVer patch release versions, incrementing only
the third part of the version number. The branch must be created from
a previous release point, and the first two parts of that release's
version number will be used as the branch name. After the branch is
created, patch releases for that major/minor version of the library
may only be created from that branch, and the next release from master
must increment at least the minor version of the library.

Example branch names::

  1.12.0 -> bug/1.12
  2.4.1 -> bug/2.4

Appropriate Commits
-------------------

Only critical fixes, for gate breaks or security issues, may be
included in a bug branch. All patches to the bug branch must be
treated as a backport, and merged into the master branch before the
bug branch.

Maintenance teams
-----------------

Unlike stable branches, bug branches are not managed by a separate
team. They are managed by the core review team for the project, in
conjunction with the release management team.
