================
 Stable Branches
================

The stable branches are intended to be a safe source of fixes for high impact
bugs and security issues which have been fixed on master since a given release.

Stable branches are cut from the last release of a given deliverable, at the
end of the common 6-month development cycle.


Maintenance phases
==================

Project stable branches will be in one of the following states:

.. list-table::
   :header-rows: 1
   :widths: 15 35 50

   - * State
     * Time frame
     * Summary
   - * Maintained
     * Approximately 18 months
     * All bugfixes (that meet the criteria described below) are
       appropriate. Releases produced.
   - * Unmaintained
     * While there is community interest.
     * Only SLURP releases. All bugfixes (that meet the criteria
       described below) are appropriate.  No Releases produced,
       reduced CI commitment.
   - * End of Life (EOL)
     * N/A
     * Branch no longer accepting changes.

It is not required that all projects for a given branch transition between
phases at the same time.  For example it's quite reasonable for the
``stable/$series`` branch of ``openstack/long-life-project`` to still be
in the Maintained phase while all other projects have transitioned to either
`Unmaintained`_ or even `End of Life`_.

.. note::
   How can I tell what state a branch is in?

   Current development branch
     This is the ``master`` branch in a repository.

     The project's core team (for example, ``cinder-core``) is responsible
     for merging changes into this branch.

   Maintained branches
     These are the ``stable/$series`` branches in a repository, for
     example, ``stable/2023.1``.

     The project's stable core team (for example, ``cinder-stable-maint``),
     which may be a subset of the core team, is responsible for merging
     changes into Maintained branches.

     A project's PTL or release team liaison must acknowledge proposed
     releases from a Maintained branch, which are then subject to approval
     by the OpenStack Release Management Team.

     See :ref:`Stable maintenance teams<stable-maint-teams>` for details.

   Unmaintained branches
     These are the ``unmaintained/$series`` branches in a repository,
     for example, ``unmaintained/xena``.

     There is a global ``openstack-unmaintained-core`` team that is
     responsible for merging changes into Unmaintained branches.
     Optionally, an individual project may have a
     ``$project-unmaintained-core`` team (for example,
     ``ironic-unmaintained-core``) that has this responsibility,
     and which may override the global team's approval power on
     Unmaintained branches.

     Releases are not allowed from Unmaintained branches.

     Changes to Unmaintained branches are expected to be
     :ref:`Appropriate Fixes<appropriate-fixes>` as described elsewhere
     in this document.

   End of Life
     These do not exist as branches in the code repository, but can
     be recreated by checking out a ``$series-eol`` tag, for example,
     ``train-eol``.

.. _Maintained:

Maintained
----------

For any project/branch combination that is considered Maintained, OpenStack
Infrastructure, `OpenStack Vulnerability Management`_ and QE tools are expected
to work and be active.  Project teams will produce consumable releases and
upgrades are tested.

During this phase, per Project Stable teams and the Stable Maintainers are
responsible for all projects which asserted they follow the stable branch
policy.


.. _`Unmaintained`:

Unmaintained
------------

.. note::
   Only SLURP branches are eligible for Unmaintained status.

After a branch is no longer in Maintained status, the branch is tagged
``$series-eom``, the ``stable/$series`` branch is deleted, and an
``unmaintained/$series`` branch is created based on the ``$series-eom``
tag.

Releases are not allowed from Unmaintained branches.

By default, only the latest eligible Unmaintained branch is kept open.
To prevent an Unmaintained branch from automatically transitioning to
`End of Life`_ once a newer eligible branch enters the status, the
Unmaintained branch liaison must manually opt-in as described below for each
branch.

.. note::
   An Unmaintained liaison may be appointed by the PTL, but there should not
   be an expectation on the upstream community team to keep the branch open.
   Members of the community interested in a given branch are encouraged to
   engage with the appropriate project team *early* in its life-cycle to ensure
   this process runs well. In the absence of identified maintainers the branch
   will automatically transition to `End of Life`_.

To opt-in to keep the Unmaintained branch open, the PTL or Unmaintained
liaison must -1 the appropriate patch in the ``openstack/releases`` repo to
EOL the branch. Please contact the PTL or Unmaintained liaison to show
interest in helping keep the branch open with backports and reviews.
The patch to EOL the Unmaintained branch will be merged no earlier than one
month after its proposal. Even if a PTL or liaison has voted +1 to EOL the
branch, the patch must remain open for the full grace period to permit other
community members the opportunity to volunteer to take it over.

To be eligible to opt-in, a branch must have functional CI including unit
tests for the published supported python versions for that OpenStack release.
Functional CI means all configured tests pass and do not generate errors in
Zuul.

Additionally, no SLURP branches may be skipped between the oldest unmaintained
branch and the current maintained releases. This makes sure operators have an
upgrade path from one SLURP to the next all the way to maintained releases.

.. note::
   For further details about Unmaintained status, please take a look
   at the OpenStack Technical Committee's resolution `Unmaintained status
   replaces Extended Maintenance
   <https://governance.openstack.org/tc/resolutions/20230724-unmaintained-branches.html>`_
   as amended to define the `openstack-unmaintained-core Gerrit Group
   <https://governance.openstack.org/tc/resolutions/20231114-amend-unmaintained-status.html>`_.

.. warning::
   Tempest and its plugins are branchless and there is no guarantee that
   Unmaintained branches will continue to be supported by the master version
   of tempest or its plugins.

   .. note::
      For more information, see the `Tempest Stable Branch Support Policy
      <https://docs.openstack.org/tempest/latest/stable_branch_support_policy.html>`_.

   If Tempest master starts breaking when testing Unmaintained branches, then
   we need to use the `last compatible version of Tempest and its plugins
   <https://docs.openstack.org/tempest/latest/tempest_and_plugins_compatible_version_policy.html>`_.

   To make the last compatible version of Tempest and its plugins available
   for a particular branch, at the time the branch enters Unmaintained status
   the Tempest maintainers will release a new tag at a master hash with the
   name ``$series-last`` (as well as a new version number).  The maintainers
   of tempest plugins will also release ``$series-last`` tags for each of the
   plugins.

   This makes it easy for Tempest consumers who want to continue CI
   testing of an Unmaintained branch (either upstream or in a production
   cloud) to identify the last compatible versions of Tempest and its plugins
   for that branch.  It is also more reliable than requiring anyone who wishes
   to use Tempest on an Unmaintained branch to manually try and find the
   working version.

   For an example of creating ``$series-last`` tags for Tempest and its
   plugins, take a look at these Gerrit reviews:
   https://review.opendev.org/q/topic:%22tempest-plugin-stein-last%22+(status:open%20OR%20status:merged)

.. _End Of Life:

End of Life
-----------

After a branch is ineligible to remain active as Unmaintained or a team
decides to explicitly end support for a branch, it will become End of Life.
The HEAD of the appropriate branch will be tagged as ``$series-eol`` and the
branch deleted.

To initiate this transition, either the PTL of the given project or other
stable maintainer should:

#. Send an announcement to the openstack-discuss mailing list (in order to give
   some time for others to step up as maintainers if there are volunteers).
#. Remove any related zuul jobs that are defined in ``other repositories`` and
   not needed anymore.
#. Propose a patch against the given project/repository. (For example, see:
   https://review.opendev.org/#/c/677478/)
#. After the branch is tagged with ``$series-eol``, request the Release
   Management team to delete the branch.

.. _appropriate-fixes:

Appropriate Fixes
=================

Only a limited class of changes are appropriate for inclusion on the stable
branch. A number of factors must be weighed when considering a change:

#. The risk of regression: even the tiniest changes carry some risk of breaking
   something and we really want to avoid regressions on the stable branch
#. The user visible benefit: are we fixing something that users might actually
   notice and, if so, how important is it?
#. How self-contained the fix is: if it fixes a significant issue but also
   refactors a lot of code, it's probably worth thinking about what a less
   risky fix might look like
#. Whether the fix is already on master and all consequent stable branches:
   a change **must** be a backport of a change already merged onto master,
   unless the change simply does not make sense on master. Same applies to N-2
   releases, where N is master, in which case both N-1 and N branches should
   have the patch merged and so on.

.. note::
   It's nevertheless allowed to backport fixes for other bugs if their safety
   can be easily proved. For example, documentation fixes, debug log message
   typo corrections, test only changes, patches that enhance test coverage,
   configuration file content fixes can apply to all supported branches. For
   those types of backports, stable maintainers will decide on case by case
   basis.

.. note::
   Some patches may get exception from rule 4 above. These are patches
   that do not touch production code, like test-only patches, or tox.ini
   changes that fix major gate breakage, etc.; or security patches that
   should not take much time to merge once the patches are published.
   In those cases, stable patches may be pushed into gate without waiting
   for all consequent branches to be fixed.

.. _stable-modifications:
.. warning::
   In case review process reveals issues in the master patch which require
   rework after stable patches are merged, it's expected that additional
   changes are merged into stable branches to avoid unneeded difference
   between branches. So use the exception with due care.

Anyone can propose stable branch backports. See `Proposing Fixes`_ for more
information on how to do that.


.. _stable-maint-teams:

Stable maintenance teams
========================

Each project team should designate a `stable branch cross-project liaison
<https://wiki.openstack.org/wiki/CrossProjectLiaisons#Stable_Branch>`_ as
the main point of contact for all stable branch support issues in the team.
If nobody is specifically designated, the PTL will be assumed to cover that
duty.

Project-specific teams
----------------------

Each project with a stable branch will have a project-specific stable
maintenance Gerrit team called PROJECTNAME-stable-maint. This team
will have CodeReview+2 and Workflow+1 rights over the stable branches,
and be in charge of reviewing backports for a given project, following
the rules of the stable branch policy. That group should be the project
Stable Branch core (not necessary to be master core) + the stable maintenance
core team. That group is managed by the project team like they manage their
master branch core team. To manage that group or stable policies, they can
consult with the stable maintenance core team.

Stable Maintenance Core team
----------------------------

The `stable maintenance core team`_ is responsible for the definition and
enforcement of the Stable Branch policy. It will be granting exceptions for
all questionable backports raised by project-specific stable maintenance
groups, providing backports reviews help everywhere, maintaining the stable
branch policy (and make sure its rules are respected), educating proposed
project-specific team members on those rules and adding them to those
project-specific teams.

Active Maintenance
------------------

Project-specific teams are expected to be actively maintaining their stable
branches which generally includes:

#. Following the `Review guidelines`_. Specifically, not allowing backports of
   new features, new dependencies, or backward incompatible changes.

   * Hint: if a project version has a cap in stable branch global-requirements
     in stable/liberty or later, it means there was a backward incompatible
     change which broke that stable branch. This generally applies to libraries
     and client projects.

#. Proactively identifying and backporting significant bug fixes from master to
   stable branches. This means the team is trying to get high impact bugs fixed
   on stable before anyone hits them and has to report a bug or propose a
   backport after the fact (after they already hit the issue in their
   production cloud). There is no rule about how often or how many bugs found
   and fixed in master should be backported to stable branches. The main idea
   is to get regressions and other high-impact issues resolved on all
   appropriate branches quickly.
#. Monitoring the backlog of open backport reviews and actually reviewing them
   in a timely manner.
#. Releasing frequently enough to get fixes out without overwhelming the
   release team or consumers. In general, security fixes and other critical bug
   fixes should be released quickly. Otherwise when there are a reasonable
   amount of unreleased fixes committed, teams should be looking at doing a
   release. Milestone boundaries during the master release schedule are also
   good times to be inspecting the list of unreleased changes to see if a
   stable point release should happen.
#. Monitoring and resolving issues in the continuous integration 'gate' system.
   This basically means making sure there aren't things blocking proposed
   backports from passing tests. These could be project-specific or global in
   nature and are usually tracked in the `stable tracker etherpad`_. From time
   to time the Stable Maintenance Core team may also ask for help from
   individual projects in IRC or the openstack-discuss mailing list and expect
   a reasonably prompt response.

   .. note::
      Projects which asserted they follow the stable branch policy should be
      running the ``periodic-stable-jobs`` template in their project's
      Zuul configuration file, usually ``.zuul.yaml`` (`example .zuul.yaml`_)
      or ``zuul.d/project.yaml`` (`example zuul.d/project.yaml`_).

      The template is defined in `zuul.d/project-templates.yaml`_ in the
      `openstack/openstack-zuul-jobs repo`_ and is maintained by the
      OpenStack QA team.

#. Stable branch cross-project liaisons should be available in the
   #openstack-stable channel on OFTC IRC to answer questions or be made
   aware of issues.

Unmaintained branch maintenance
-------------------------------

Unmaintained branches are not the responsibility of individual project
teams (though, of course, there is no prohibition against individual
project team members working on Unmaintained branches).

Instead, there is a global ``openstack-unmaintained-core`` team that has
access rights to maintain the CI running on Unmaintained branches, and
to merge appropriate changes into Unmaintained branches.

Additionally, each project may have (but is not required to have) a Gerrit
team called PROJECTNAME-unmaintained-core to handle all work on that
project's Unmaintained branches.  This group is managed by the PTL or the
Unmaintained branch liaison if there is one.  The group is created by
proposing an appropriate set of permissions to the project's Gerrit ACLs
in the ``openstack/project-config`` repository.  See
https://review.opendev.org/c/openstack/project-config/+/902796
for an example.


Review guidelines
=================

Each project stable review team need to balance the risk of any given patch
with the value that it will provide to users of the stable branch. A large,
risky patch for a major data corruption issue might make sense. As might a
trivial fix for a fairly obscure error handling case.

Some types of changes are completely forbidden:

* New features
* Changes to the external HTTP APIs
* Changes to Nova's internal AMQP API
* Changes to the notification definitions
* DB schema changes
* Incompatible config file changes

Proposed backports breaking any of the above guidelines can be discussed as
exception requests on the openstack-discuss list (prefix with [stable]) where
the stable maintenance core team will have the final say.

Each backported commit proposed to Gerrit should be reviewed and +2ed by
two project-specific stable maintenance team members before it is approved.
Where a team member has backported a fix, a single other +2 is sufficient for
approval.

If unsure about the technical details of a given fix, project-specific stable
maintenance team members should consult with the appropriate project core
reviewers for a more detailed technical review.

If unsure if a fix is appropriate for the stable branch, project-specific
stable maintenance team members should seek stable maintenance core team
members opinion.

Existing core reviewers are greatly encouraged to join the stable maintenance
teams in order to help with reviewing backports, judging their appropriateness
for the stable branch and approving them.

Fixes for embargoed security issues receive special treatment. See the chapter
on vulnerability management for more information.

Processes
=========

OpenStack development typically has 3 branches active at any point of time,
*master* (the current development release), *stable* (the most recent release)
and *oldstable* (previous release).  There can from time to time exist older
branches but a discussion around that is beyond the scope of this guide.

In order to accept a change into :code:`$release` it must first be accepted
into all releases back to master.

For the sake of discussion assume a hypothetical development milestones:

* The development branch (:code:`master`) will be the Uniform release.
* The :code:`N-1` branch is :code:`stable/tango`
* The :code:`N-2` branch is :code:`stable/sierra`
* The :code:`N-3` branch is :code:`stable/romeo`
* and so on

Backport examples:

* A change for Tango must exist in :code:`master`
* A change for Sierra must exist in :code:`stable/tango` and :code:`master`
* A change for Romeo must exist in :code:`stable/sierra`, :code:`stable/tango`
  and :code:`master`
* and so on

Proposing Fixes
---------------
Anyone can propose a cherry-pick to the stable-maint team.

One way is that if a bug in launchpad looks like a good candidate for
backporting - e.g. if it's a significant bug with the previous release - then
just nominating the bug for a stable series (either *stable* or *oldstable*)
will bring it to the attention of the maintainers e.g. `Nova Kilo nominations`_

If you don't have the appropriate permissions to nominate the bug, then tagging
it with e.g. *$release-backport-potential* is also sufficient e.g.
`Nova Liberty potential`_

The best way to get the patch merged in a timely manner is to send it
backported by yourself. To do so, you may try to use the "Cherry Pick To"
button in the Gerrit UI for the original patch in master. Gerrit will take care
of creating a new review, modifying the commit message to include
'cherry-picked from ...' line etc.

.. note::
   The backport must match the master commit, unless there is a serious need to
   differ e.g gate failure, test framework changed in master, code refactoring
   or some other reason. If you get a suggestion to *enhance* your backport in
   some way that would be contrary to this intent, the reviewer should be
   referred to :ref:`the warning above <stable-modifications>`.

You can use `git-review`_ to propose a change to the hypothetical stable
branch with:

.. code-block:: bash

    $ git checkout -t origin/stable/tango
    $ git cherry-pick -x $master_commit_id
    $ git review stable/tango

.. note::
   cherry-pick -x option includes 'cherry-picked from ...' line in the commit
   message which is required to avoid `Gerrit bug`_

Failing all that, just ping one of the team and mention that you think the
bug/commit is a good candidate.

Conflicts
~~~~~~~~~

If the patch you're proposing will not cherry-pick cleanly, you can help by
resolving the conflicts yourself and proposing the resulting patch. Please keep
"Conflicts" lines in the commit message to help reviewers, for example:
https://review.opendev.org/686292/

.. note::
   If a cherry-picked patch's commit message contains "Conflicts" lines that
   are not valid anymore in the target branch, then remove those lines.

Change-Ids
~~~~~~~~~~
When cherry-picking a commit, keep the original :code:`Change-Id` and gerrit
will show a separate review for the stable branch while still allowing you to
use the Change-Id to see all the reviews associated with it. `See this change
as an example. <https://review.openstack.org/#/q/Ic5082b74a362ded8b35cbc75cf178fe6e0db62d0,n,z>`_

.. warning::
   :code:`Change-Id` line must be in the last paragraph. Conflicts in the
   backport add a new paragraph, creating a new :code:`Change-Id` but you can
   avoid that by moving conflicts above the paragraph with :code:`Change-Id`
   line or removing empty lines to make a single paragraph.

Releases
--------

For stable releases, branches should be released in the same order as of
backporting fixes. For example, to release a branch :code:`N-1`, we should
first release the branch N and continue in the same sequence of releasing
:code:`N`, :code:`N-1`, :code:`N-2` and so on.

It is not required to release all stable branches together but to avoid
conflict, we should only release branch :code:`N-1` with changes already
released with branch :code:`N` release and should avoid having the case
where an older branch release contain fixes that does not exist in a recent
branch release.
There can be exceptions to this case but it is the preferred way of releasing
stable branches.

Email Notifications
-------------------
If you want to be notified of new stable patches you can create a watch on the
gerrit `watched projects`_ screen with the following settings.

.. code-block::

   Project Name: All-Projects
        Only If: branch:stable/liberty

Then check the "Email Notifications - New Changes" checkbox. That will cause
gerrit to send an email whenever a matching change is proposed, and better yet,
the change shows up in your 'watched changes' list in gerrit.

See the docs for `gerrit notify`_ configuration and the `gerrit search`_
syntax.

Bug Tags
--------

Bugs tagged with *$release-backport-potential* are bugs which apply to a
stable release and may be suitable for backporting once fixed. Once the
backport has been proposed, the tag should be removed.

Gerrit tags bugs with *in-stable-$release* when they are merged into the stable
branch. The release manager later removes the tag when the bug is targeted to
the appropriate series.

Gate Status
-----------

Keeping the stable branches in good health in an ongoing effort. To see what
bugs are currently causing gate failures and preventing code from merging into
stable branches, please see the `stable tracker etherpad`_, where we will track
current bugs and in-flight fixes.

Scheduled test runs occur daily for each project's stable branch. If failures
crop up, the bot will email the `openstack-stable-maint mailing list`_. It is
best to react quickly to these and get them resolved ASAP to prevent them from
piling up. Please subscribe if you're interested in helping out.

Project teams which asserted they follow the stable branch policy
=================================================================

This list can be updated if there is any change in their stable branch
policy (project stop or start following the stable branch policy).

#. `Barbican`_ (Key Manager service): barbican
#. `Cinder`_ (Block Storage service): cinder, cinderlib, os-brick,
   python-brick-cinderclient-ext, python-cinderclient
#. `Designate`_ (DNS service): designate, designate-dashboard,
   python-designateclient
#. `Glance`_ (Image service): glance, glance-store, python-glanceclient
#. `Heat`_ (Orchestration service): heat, python-heatclient
#. `Horizon`_ (Dashboard): horizon
#. `Ironic`_ (Bare Metal service): ironic, ironic-inspector, ironic-lib,
   ironic-python-agent, python-ironic-inspector-client, python-ironicclient
#. `Keystone`_ (Identity service): keystone, keystoneauth, keystonemiddleware,
   pycadf, python-keystoneclient
#. `Manila`_ (Shared File Systems service): manila
#. `Murano`_ (Application Catalog service): murano, murano-agent,
   murano-dashboard, python-muranoclient
#. `Neutron`_ (Networking service): neutron-fwaas, neutron,
   neutron-dynamic-routing, neutron-lib, neutron-vpnaas
#. `Nova`_ (Compute service): nova, python-novaclient
#. `Octavia`_ (Load-balancer service): octavia, octavia-dashboard,
   python-octaviaclient, octavia-lib
#. `Oslo`_ (Common libraries): automaton, castellan, oslo.cache, oslo.config,
   oslo.context, oslo.db, oslo.messaging, oslo.middleware, oslo.policy,
   oslo.privsep, oslo.serialization, oslo.service, oslo.upgradecheck,
   oslo.utils, oslo.versionedobjects, oslo.vmware, stevedore
#. `Sahara`_ (Data Processing service): python-saharaclient, sahara,
   sahara-dashboard, sahara-extra, sahara-image-elements, sahara-plugin-ambari,
   sahara-plugin-cdh, sahara-plugin-mapr, sahara-plugin-spark,
   sahara-plugin-storm, sahara-plugin-vanilla
#. `Swift`_ (Object Storage service): python-swiftclient, swift
#. `Zaqar`_ (Message service): zaqar

.. _Nova Kilo nominations: https://bugs.launchpad.net/nova/kilo/+nominations
.. _Nova Liberty potential: https://bugs.launchpad.net/nova/+bugs?field.tag=liberty-backport-potential
.. _git-review: https://github.com/openstack-infra/git-review
.. _Gerrit bug: https://code.google.com/p/gerrit/issues/detail?id=1107
.. _watched projects: https://review.openstack.org/#/settings/projects
.. _gerrit notify: https://gerrit-review.googlesource.com/Documentation/user-notify.html#user
.. _gerrit search: https://review.openstack.org/#/settings/projects
.. _stable tracker etherpad: https://etherpad.openstack.org/p/stable-tracker
.. _openstack-stable-maint mailing list: http://lists.openstack.org/cgi-bin/mailman/listinfo/openstack-stable-maint
.. _stable maintenance core team: https://review.openstack.org/#/admin/groups/530,members
.. _example .zuul.yaml: https://opendev.org/openstack/cinder/src/commit/7086157de07b77e8b67bbb767bc2ce25e86c2f51/.zuul.yaml#L6
.. _example zuul.d/project.yaml: https://opendev.org/openstack/manila/src/commit/cbf142912d5333c08f139d2d9be67adea81ed2fd/zuul.d/project.yaml#L9
.. _zuul.d/project-templates.yaml: https://opendev.org/openstack/openstack-zuul-jobs/src/commit/370e620ebe5ac03bf9bc13a88002e7b56291140c/zuul.d/project-templates.yaml#L2201
.. _openstack/openstack-zuul-jobs repo: https://opendev.org/openstack/openstack-zuul-jobs
.. _OpenStack Vulnerability Management: https://security.openstack.org/vmt-process.html
.. _Zuul Drivers: https://zuul-ci.org/docs/zuul/latest/drivers/index.html
.. _Barbican: https://governance.openstack.org/tc/reference/projects/barbican.html
.. _Cinder: https://governance.openstack.org/tc/reference/projects/cinder.html
.. _Designate: https://governance.openstack.org/tc/reference/projects/designate.html
.. _Glance: https://governance.openstack.org/tc/reference/projects/glance.html
.. _Heat: https://governance.openstack.org/tc/reference/projects/heat.html
.. _Horizon: https://governance.openstack.org/tc/reference/projects/horizon.html
.. _Ironic: https://governance.openstack.org/tc/reference/projects/ironic.html
.. _Keystone: https://governance.openstack.org/tc/reference/projects/keystone.html
.. _Manila: https://governance.openstack.org/tc/reference/projects/manila.html
.. _Murano: https://governance.openstack.org/tc/reference/projects/murano.html
.. _Neutron: https://governance.openstack.org/tc/reference/projects/neutron.html
.. _Nova: https://governance.openstack.org/tc/reference/projects/nova.html
.. _Octavia: https://governance.openstack.org/tc/reference/projects/octavia.html
.. _Oslo: https://governance.openstack.org/tc/reference/projects/oslo.html
.. _Sahara: https://governance.openstack.org/tc/reference/projects/sahara.html
.. _Swift: https://governance.openstack.org/tc/reference/projects/swift.html
.. _Zaqar: https://governance.openstack.org/tc/reference/projects/zaqar.html

