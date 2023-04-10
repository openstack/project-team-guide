===================
Repository Handling
===================

Project Renames
===============

If you rename a project to move out from "openstack" namespace to any
other namespace, follow `this OpenStack TC resolution
<https://governance.openstack.org/tc/resolutions/20190711-mandatory-repository-retirement.html>`_.

For a project being added to existing official OpenStack project:
Create an ``openstack/governance`` change and add a "Depends-On:
project-change-url" of the change you make in the following steps to
the commit message, and add a comment in the
``openstack/project-config`` change that references the
governance change. You will also make sure the PTL has expressed
approval for the addition in some way.

If you rename an existing official OpenStack project, add the
``openstack/governance`` change as dependency on the
``openstack/project-config`` change so that it can merge after the
rename is done.

Do not forget to update the Zuul job definitions. This is important to
keep the Zuul jobs definition correct and utilize the CI/CD infra resources
in better way. You need to find if retiring repository is used in Zuul
job definition on master and stable branches as ``required-projects``
or any other place. Searching in master jobs is easy via
`codesearch <https://codesearch.openstack.org/>`_ but searching in
stable branches is tricky. Make sure you cleanup all the stable branches
including branches in ``Extended Maintenance`` state. Otherwise it will
leads to the Zuul config errors which can be found in the `Zuul config
errors list <https://zuul.opendev.org/t/openstack/config-errors>`_

The detailed steps for renaming a project are documented in the
section `Project Renames
<https://docs.opendev.org/opendev/infra-manual/latest/creators.html#project-renames>`_
of the OpenDev Manual.

Retiring a Repository
=====================

If you need to retire a project and no longer accept patches - for
both master and stable branches -, it is important to communicate that
to both users and contributors. The following steps will help you wind
down a project gracefully.

.. note::

   The following sections are really separate steps. If your project
   has jobs set up, you need to submit *four* different changes as
   explained below and in the OpenDev Manual. We recommend to link
   these changes with "Depends-On:" and "Needed-By:" headers.

Step 1: End Project Gating
--------------------------

Follow the Step 1 about `End Project Gating
<https://docs.opendev.org/opendev/infra-manual/latest/drivers.html#step-1-end-project-gating>`_
in the OpenDev Manual.

Step 2: Remove Project Content
------------------------------

Follow the Step 2 about `Remove Project Content
<https://docs.opendev.org/opendev/infra-manual/latest/drivers.html#step-2-remove-project-content>`_
in the OpenDev Manual.

Step 3: Retire Repository from the Governance Repository
--------------------------------------------------------

This step is to get the agreement in the Technical Committee by
retiring the repository from the ``reference/projects.yaml`` file and
add it to the file ``reference/legacy.yaml`` in the ``openstack/governance``
repository. Note that if the project was recently active, this may have
implications for automatic detection of ATCs.

We need to wait for this patch to merge first before proceeding next. Main
goal here is to get the Technical Committee agreement and governance scripts
to check the proper retirement which will help us to avoid any re-retirement
work.

NOTE: use Depends-On on ``Remove Project Content`` patch submitted in Step 2.

Step 4: Stop requirements syncing (if set up)
---------------------------------------------

Submit a review to the ``openstack/requirements`` project removing the
project from ``projects.txt``.  This needs to happen for stable
branches as well.

NOTE: use Depends-On on ``governance`` patch submitted in Step 3.

Steps 5: Remove Project from Infrastructure Systems
---------------------------------------------------

Follow the steps about `Remove Project from Infrastructure System
<https://docs.opendev.org/opendev/infra-manual/latest/drivers.html#step-3-remove-project-from-infrastructure-systems>`_ in the OpenDev Manual.

NOTE: use Depends-On on ``governance`` patch submitted in Step 3.

Use Depends-On on ``governance`` patch submitted in Step 1.

Step 6: Remove repository references
------------------------------------

Make sure all the reference to the retiring repository has been removed
properly. A few of the places to audit and update are:

#. Check all the projects source code and documentation if there is any
   reference of retiring repository. Use `codesearch
   <https://codesearch.openstack.org/>`_ to check it.

#. Zuul job definitions:

   This is important to keep the Zuul jobs definition correct and utilize
   the CI/CD infra resources in better way. You need to find if retiring
   repository is used in Zuul job definition on master and stable branches
   as ``required-projects`` or any other place. Searching in master jobs
   is easy via `codesearch <https://codesearch.openstack.org/>`_ but searching
   in stable branches is tricky. Make sure you cleanup all the stable branches
   including branches in ``Extended Maintenance`` state. Otherwise it will
   leads to the Zuul config errors which can found in the `Zuul config
   errors list <https://zuul.opendev.org/t/openstack/config-errors>`_


Use Depends-On on ``governance`` patch submitted in Step 1.

Step 7: Remove docs.openstack.org content
-----------------------------------------

Inform users that reach the ``docs.openstack.org`` page of your
repository that it is retired.

For this, add the name of your repository to the ``_RETIRED_REPOS``
list in the file ``tools/www-generator.py`` in the
``openstack/openstack-manuals`` repository. Once the change merged,
the URL ``https://docs.openstack.org/openstack/<projectname>/latest``
will redirect to the repositories' ``README.rst`` file.

NOTE: use Depends-On on ``governance`` patch submitted in Step 3.

Step 8: Mark the deliverables as retired
----------------------------------------

For maintained openstack series, on ``openstack/releases``, amend the related
deliverable files to tag the project with the ``retired`` flag.

Example with ``deliverables/train/puppet-panko.yaml``::

    ---
    launchpad: puppet-panko
    release-model: cycle-trailing
    team: Puppet OpenStack
    type: other
    repository-settings:
      openstack/puppet-panko:
        flags:
          - retired
    ...

Even if a project is retired, stable branches will continue to follow the
existing series life cycle and this flag will allow us to ignore this
deliverable in some specific cases.

NOTE: use Depends-On on ``governance`` patch submitted in Step 3.

Step 9: Update openstack-map to remove the retired project
----------------------------------------------------------

If the retired repository/project is listed in `openstack-map
<https://opendev.org/openinfra/openstack-map>`_ , you need to remove
it from there.

For Example: https://review.opendev.org/c/openinfra/openstack-map/+/764544

Use Depends-On on ``governance`` patch submitted in Step 1.

Deprecating a Repository
========================

If you only want to stop development of the master branch but keep
stable branches, you need to do a slightly different approach.

Deprecating the project or repository is different than removal.
If the project want to stop the development on master branch but
support the stable branches with bug fixes, then project must be marked as
deprecated. If project has no stable branch then you have option to go with
removal process directly.

Step 1: Mark the  Repository as Deprecated in the Governance Repository
-----------------------------------------------------------------------

Mark the repository in the ``reference/projects.yaml`` file of
the ``openstack/governance`` repository as deprecated with adding a line::

  deprecated: <deprecated-cycle-name>
  release-management: deprecated

Step 2: Stop requirements syncing (if set up)
---------------------------------------------

Submit a review to the ``openstack/requirements`` project removing the
project from ``projects.txt``.

NOTE: use Depends-On on ``governance`` patch submitted in Step 1.

Step 3: Retire master branch
----------------------------

If the repository is branchless (for example, Tempest and its plugins) and
its master branch content needs to support the other deliverables stable branch
until they are retired or reach EOL, then you can skip this Step 3 and update
only README.rst file to reflect the deprecation notes.

Step 3a: Use only noop jobs
~~~~~~~~~~~~~~~~~~~~~~~~~~~

Add ``noop-jobs`` template to ``zuul.d/projects.yaml`` for master only
in ``project-config`` repository and remove all other templates temporarily
with exception of ``official-openstack-repo-jobs`` and pypi release template
if any. If your project has ``publish-to-pypi`` template present, then change
it to ``publish-to-pypi-stable-only``. It should look something like
this::

  - project:
    name: openstack/<projectname>
    templates:
      - official-openstack-repo-jobs
      - publish-to-pypi-stable-only
      - noop-jobs

Adjust the project description. Find the entry for your project in
``gerrit/projects.yaml`` and look for the line which defines the description,
prefix it with ``DEPRECATED,`` like this::

  description: DEPRECATED, existing project description

Step 3b: Remove project content
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Follow step 2 about `Removing project content
<https://docs.opendev.org/opendev/infra-manual/latest/drivers.html#step-2-remove-project-content>`__
in the OpenDev Manual.

Step 3c: Remove noop jobs
~~~~~~~~~~~~~~~~~~~~~~~~~

Once the project content is retired, partially revert the change you merged
earlier for ``project-config`` in step 3a and re-add templates and jobs you
need so that you can merge content on stable branches.
Please ensure you keep the ``DEPRECATED,`` prefix you added to project
description in step 3a.

NOTE: In all the patches, use Depends-On on ``governance`` patch submitted in
Step 1.

Step 4: Remove docs.openstack.org content
-----------------------------------------

Inform users that reach the ``docs.openstack.org`` page of your
repository that it is deprecated.

For this, add the name of your repository to the ``_RETIRED_REPOS``
list in the file ``tools/www-generator.py`` in the
``openstack/openstack-manuals`` repository. Once the change merged,
the URL ``https://docs.openstack.org/openstack/<projectname>/latest``
will redirect to the repositories' ``README.rst`` file.

Also, remove the project from the list in the ``www/project-data/latest.yaml``
in the ``openstack/openstack-manuals`` repository if present. That will remove
the project from the list of new releases.

NOTE: use Depends-On on ``governance`` patch submitted in Step 1.
