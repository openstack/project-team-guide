=================
 Stable Branches
=================

The stable branches are intended to be a safe source of fixes for high impact
bugs and security issues which have been fixed on master since a given release.

Stable branches are cut from the last release of a given deliverable, at the
end of the common 6-month development cycle.


Support phases
==============

Stability is always a trade-off between "bug-free" and "slow-moving". In order
to reach that stability, we define several support phases:

* Phase I (first 6 months): All bugfixes (which meet the criteria described
  below) are appropriate
* Phase II (6-12 months): Only critical bugfixes and security patches are
  acceptable
* Phase III (more than 12 months): Only security patches are acceptable

.. note::
   It's nevertheless allowed to backport fixes for other bugs if their safety
   can be easily proved. For example, documentation fixes, debug log message
   typo corrections, test only changes, patches that enhance test coverage,
   configuration file content fixes can apply to all supported branches. For
   those types of backports, stable maintainers will decide on case by case
   basis.

Given that stable branches are created every 6 months, that means that at any
given time, only one branch is in Phase I support and only one branch is in
Phase II support. Depending on how long each branch is supported, there may be
one or more releases in Phase III support.

The exact length of any given stable branch life support is discussed amongst
stable branch maintainers and QA/infrastructure teams at every Design Summit.
It is generally between 9 and 15 months, at which point the value of the
stable branch is clearly outweighed by the cost in maintaining it in our
continuous integration systems.


Appropriate Fixes
=================

Only a limited class of changes are appropriate for inclusion on the stable
branch. A number of factors must be weighed when considering a change:

* The risk of regression: even the tiniest changes carry some risk of breaking
  something and we really want to avoid regressions on the stable branch
* The user visible benefit: are we fixing something that users might actually
  notice and, if so, how important is it?
* How self-contained the fix is: if it fixes a significant issue but also
  refactors a lot of code, it's probably worth thinking about what a less
  risky fix might look like
* Whether the fix is already on master and all consequent stable branches:
  a change **must** be a backport of a change already merged onto master,
  unless the change simply does not make sense on master. Same applies to N-2
  releases, where N is master, in which case both N-1 and N branches should
  have the patch merged.

.. note::
   Some patches may get exception from this last rule. These are patches
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

Anyone can propose stable branch backports. See the Infra manual for more
information on how to do that.


Stable maintenance teams
========================

Each project team should designate a stable branch cross-project liaison as
the main point of contact for all stable branch support issues in the team.
If nobody is specifically designated, the PTL will be assumed to cover that
duty.

Project-specific teams
----------------------

Each project with a stable branch will have a project-specific stable
maintenance team, which will be in charge of reviewing backports for a given
project, following the stable branch policy. Originally that group should be
the project Stable Branch Cross-Project Liaison + the stable maintenance core
team. Those groups are managed by the stable maintenance core team, names are
added after the suggestion of the Stable Branch cross-project liaison.

Stable Maintenance Core team
----------------------------

The stable maintenance core team is responsible for the definition and
enforcement of the Stable Branch policy. It will be granting exceptions for
all questionable backports raised by project-specific stable maintenance
groups, providing backports reviews help everywhere, maintaining the stable
branch policy (and make sure its rules are respected), educating proposed
project-specific team members on those rules and adding them to those
project-specific teams.


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
exception requests on the openstack-dev list (prefix with [stable]) where
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

Existing core developers are greatly encouraged to join the stable maintenance
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

* A change for *stable* must exist in master
* A change for *oldstable* must exist in *stable* and *master*

For the sake of discussion assume a hypothetical development milestones:

* The current development branch (:code:`master`) will be the Uniform release.
* The current *stable* branch (:code:`stable/tango`) was Tango and is now in
  **Phase I** support.
* The current *oldstable* branch :code:`stable/sierra` was Sierra and is now in
  **Phase II** support.

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

The best way to get the patch merged in a timely manner is to send it backported
by yourself. To do so, you may try to use the "Cherry Pick To" button in the
Gerrit UI for the original patch in master. Gerrit will take care of creating a
new review, modifying the commit message to include 'cherry-picked from ...'
line etc.

.. note::
   The backport must match the master commit, unless there is a serious need to
   differ e.g gate failure, test framework changed in master, code refactoring
   or some other reason. If you get a suggestion to *enhance* your backport in
   some way that would be contrary to this intent, the reviewer should be
   referred to :ref:`the warning above <stable-modifications>`.

.. note::
   For code that touches code from oslo-incubator, special backporting rules
   apply. More details in `Oslo policies`_

If the patch you're proposing will not cherry-pick cleanly, you can help by
resolving the conflicts yourself and proposing the resulting patch. Please keep
Conflicts lines in the commit message to help reviewers! You can use
`git-review`_ to propose a change to the hypothetical stable branch with:

.. code-block:: bash

    $ git checkout -t origin/stable/tango
    $ git cherry-pick -x $master_commit_id
    $ git review stable/tango

.. note::
   cherry-pick -x option includes 'cherry-picked from ...' line in the commit
   message which is required to avoid `Gerrit bug`_

Failing all that, just ping one of the team and mention that you think the
bug/commit is a good candidate.

Change-Ids
----------
When cherry-picking a commit, keep the original :code:`Change-Id` and gerrit
will show a separate review for the stable branch while still allowing you to
use the Change-Id to see all the reviews associated with it. `See this change
as an example. <https://review.openstack.org/#/q/Ic5082b74a362ded8b35cbc75cf178fe6e0db62d0,n,z>`_

.. warning::
   :code:`Change-Id` line must be in the last paragraph. Conflicts in the
   backport add a new paragraph, creating a new :code:`Change-Id` but you can
   avoid that by moving conflicts above the paragraph with :code:`Change-Id`
   line or removing empty lines to make a single paragraph.

Email Notifications
-------------------
If you want to be notified of new stable patches you can create a watch on the
gerrit `watched projects`_ screen with the following settings.

.. code-block:: none

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

.. _Nova Kilo nominations: https://bugs.launchpad.net/nova/kilo/+nominations
.. _Nova Liberty potential: https://bugs.launchpad.net/nova/+bugs?field.tag=liberty-backport-potential
.. _Oslo policies: http://specs.openstack.org/openstack/oslo-specs/specs/policy/incubator.html#stable-branches
.. _git-review: https://github.com/openstack-infra/git-review
.. _Gerrit bug: https://code.google.com/p/gerrit/issues/detail?id=1107
.. _watched projects: https://review.openstack.org/#/settings/projects
.. _gerrit notify: https://gerrit-review.googlesource.com/Documentation/user-notify.html#user
.. _gerrit search: https://review.openstack.org/#/settings/projects
