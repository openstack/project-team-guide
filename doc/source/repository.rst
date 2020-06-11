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

Steps 2-4: Remove Project from OpenDev and Retire it
----------------------------------------------------

Follow the steps about `Retiring a Project
<https://docs.opendev.org/opendev/infra-manual/latest/drivers.html#retiring-a-project>`_
in the OpenDev Manual.

In step 1, keep the template ``official-openstack-repo-jobs`` besides
``noop-jobs``, this is needed to sync changes to GitHub. It will be
removed in step 3.


Step 5: Remove Repository from the Governance Repository
--------------------------------------------------------

Remove the repository from the ``reference/projects.yaml`` file and
add it to the file ``reference/legacy.yaml`` in the
``openstack/governance`` repository. Note that if the project was
recently active, this may have implications for automatic detection of
ATCs.

Deprecating a Repository
========================

If you only want to stop development of the master branch but keep
stable branches, you need to do a slightly different approach.

Step 1: Stop requirements syncing (if set up)
---------------------------------------------

Submit a review to the ``openstack/requirements`` project removing the
project from ``projects.txt``.

Step 2: Retire master branch
----------------------------

Add ``noop`` jobs for master only in ``project-config`` repository, it
should look something like this::

  - project:
    name: openstack/<projectname>
    templates:
       .. # You can keep the templates for stable branches
    check:
      jobs:
        - noop:
            branches: master
    gate:
      jobs:
        - noop:
            branches: master

Follow step 2 about `Removing project content
<https://docs.opendev.org/opendev/infra-manual/latest/drivers.html#step-2-remove-project-content>`__
in the OpenDev Manual.


Step 3: Mark the  Repository as Deprecated in the Governance Repository
-----------------------------------------------------------------------

Mark the repository in the ``reference/projects.yaml`` file as
deprecated with adding a line::

  release-management: deprecated
