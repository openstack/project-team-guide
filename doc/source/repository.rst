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

Step 1: Stop requirements syncing (if set up)
---------------------------------------------

Submit a review to the ``openstack/requirements`` project removing the
project from ``projects.txt``.  This needs to happen for stable
branches as well.

Steps 2-4: Follow all three steps from OpenDev Manual
-----------------------------------------------------

Follow the steps about `Retiring a Project
<https://docs.opendev.org/opendev/infra-manual/latest/drivers.html#retiring-a-project>`_
in the OpenDev Manual.

In step 1, keep the template ``official-openstack-repo-jobs`` besides
``noop-jobs``, this is needed to sync changes to GitHub. It will be
removed in step 3.

Step 5: Remove docs.openstack.org content
-----------------------------------------

Inform users that reach the ``docs.openstack.org`` page of your
repository that it is retired.

For this, add the name of your repository to the ``_RETIRED_REPOS``
list in the file ``tools/www-generator.py`` in the
``openstack/openstack-manuals`` repository. Once the change merged,
the URL ``https://docs.openstack.org/openstack/<projectname>/latest``
will redirect to the repositories' ``README.rst`` file.

Step 6: Mark the deliverables as retired
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


Step 7: Remove Repository from the Governance Repository
--------------------------------------------------------

Remove the repository from the ``reference/projects.yaml`` file and
add it to the file ``reference/legacy.yaml`` in the
``openstack/governance`` repository. Note that if the project was
recently active, this may have implications for automatic detection of
ATCs.

Use Depends-On on ``project-config`` `final step patch
<https://docs.opendev.org/opendev/infra-manual/latest/drivers.html#step-3-remove-project-from-infrastructure-systems>`_.

Deprecating a Repository
========================

If you only want to stop development of the master branch but keep
stable branches, you need to do a slightly different approach.

Deprecating the project or repository is different than removal.
If the project want to stop the development on master branch but
support the stable branches with bug fixes, then project with
the `stable policy tag <https://governance.openstack.org/tc/reference/tags/stable_follows-policy.html>`
must be marked as deprecated. If project has no stable branch or does not
follow the stable policy tag then you have option to go with removal process
directly.

Step 1: Stop requirements syncing (if set up)
---------------------------------------------

Submit a review to the ``openstack/requirements`` project removing the
project from ``projects.txt``.

Step 2: Retire master branch
----------------------------

Step 2a: Use only noop jobs
~~~~~~~~~~~~~~~~~~~~~~~~~~~

Add ``noop`` jobs for master only in ``project-config`` repository and
remove all templates temporarily with exception of
``official-openstack-repo-jobs``, it should look something like this::

  - project:
    name: openstack/<projectname>
    templates:
       official-openstack-repo-jobs
    check:
      jobs:
        - noop:
            branches: master
    gate:
      jobs:
        - noop:
            branches: master

Adjust the project description. Find the entry for your project in
``gerrit/projects.yaml`` and look for the line which defines the description,
prefix it with ``DEPRECATED,`` like this::

  description: DEPRECATED, existing project description

Step 2b: Remove project content
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Follow step 2 about `Removing project content
<https://docs.opendev.org/opendev/infra-manual/latest/drivers.html#step-2-remove-project-content>`__
in the OpenDev Manual.

Step 2c: Remove noop jobs
~~~~~~~~~~~~~~~~~~~~~~~~~

Once the project content is retired, partially revert the change you merged
earlier for ``project-config`` in step 2a and re-add templates and jobs you
need so that you can merge content on stable branches.
Please ensure you keep the ``DEPRECATED,`` prefix you added to project
description in step 2a.

Step 3: Remove docs.openstack.org content
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

Step 4: Mark the  Repository as Deprecated in the Governance Repository
-----------------------------------------------------------------------

Mark the repository in the ``reference/projects.yaml`` file as
deprecated with adding a line::

  deprecated: <deprecated-cycle-name>
  release-management: deprecated

Use Depends-On on ``project-config`` final step patch done in Step 2c.
