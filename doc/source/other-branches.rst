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
to be merged back into master.

New feature branches can be requested using the same mechanism as stable
branch creation. Submit a patch to the releases repository with a new
``feature/feature-name`` branch defined. Set the location value to the
repository and commit hash from which to create the feature branch::

    ---
    branches:
      - name: feature/example-feature-work
        location:
          openstack/oslo.config: 02a86d2eefeda5144ea8c39657aed24b8b0c9a39

For more details, refer to the openstack/releases
`README.rst <http://git.openstack.org/cgit/openstack/releases/tree/README.rst>`_
file.

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


Driverfixes Branches
====================

Some projects include drivers for external devices or other vendor
supported plugins. It was found to be a common issue with these
projects that once the stable branches reach `Phase II
<https://docs.openstack.org/project-team-guide/stable-branches.html#support-phases>`_
or later, fixes for drivers are no longer accepted. This resulted
in many vendors maintaining their own stable trees, with some fixes
included in one vendor's repo and other fixes in another's repo.

In order to provided a common place for distributions to pull in
driver fixes past what the normal stable policy allows, these projects
can create a driverfixes branch.

.. warning::
   There is explicitly no expectation that any driverfixes branches
   will be kept in a deployable state. They are solely intended as
   a central collection point for driver fixes only and will not be
   updated with security or other critical updates that will go to
   the normal stable/* branches

Creating the Branch
-------------------

Driverfixes branches are typically created when a stable release
transitions into Phase II support. It is at the discretion of the
project team when, and whether, they want to create a driverfixes
branch. The branch itself is created from the current head of the
stable/* branch it is supporting.

Naming the Branch
-----------------

The driverfixes branches should be named for the release they are
branching from and the code they are targeting. For example, for
driver fixes for the Ocata release, the branch should be named
``driverfixes/ocata``, for Pike, ``driverfixes/pike``, and so on.

Appropriate Commits
-------------------

Only fixes for driver code can be included. No changes to any
non-driver code should be allowed. All patches to the bug branch must
be treated as a backport, and merged into the master branch before
the bug branch.

Pep8 and unit tests should continue to pass for any change introduced.

Maintenance teams
-----------------

Unlike stable branches, driverfixes branches are not managed by a
separate team. They are managed by the core review team for the
project.
