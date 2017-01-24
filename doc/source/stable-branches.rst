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
to reach that stability, we define several support phases.

.. list-table::
   :header-rows: 1
   :widths: 10 20 20 50

   - * Phase
     * Time frame
     * Summary
     * Changes Supported
   - * I
     * First 6 months
     * Latest release
     * All bugfixes (that meet the criteria described below) are
       appropriate
   - * II
     * 6-12 months after release
     * Maintained release
     * Only critical bugfixes and security patches are acceptable
   - * III
     * more than 12 months after release
     * Legacy release
     * Only security patches are acceptable

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

Anyone can propose stable branch backports. See `Proposing Fixes`_ for more
information on how to do that.


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
maintenance team, which will be in charge of reviewing backports for a given
project, following the stable branch policy. Originally that group should be
the project Stable Branch Cross-Project Liaison + the stable maintenance core
team. Those groups are managed by the stable maintenance core team, names are
added after the suggestion of the Stable Branch cross-project liaison.

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
#. Releasing frequently enough to get fixes out without overwhelming the release
   team or consumers. In general, security fixes and other critical bug fixes
   should be released quickly. Otherwise when there are a reasonable amount of
   unreleased fixes committed, teams should be looking at doing a release.
   Milestone boundaries during the master release schedule are also good times
   to be inspecting the list of unreleased changes to see if a stable point
   release should happen.
#. Monitoring and resolving issues in the continuous integration 'gate' system.
   This basically means making sure there aren't things blocking proposed
   backports from passing tests. These could be project-specific or global in
   nature and are usually tracked in the `stable tracker etherpad`_. From time
   to time the Stable Maintenance Core team may also ask for help from
   individual projects in IRC or the openstack-dev mailing list and expect a
   reasonably prompt response.

   .. note::
      Projects with the ``stable:follows-policy`` tag should be running the
      ``periodic-<release>`` jobs as defined in the
      `openstack-infra/project-config repo`_. Here is an example of running
      periodic-kilo and periodic-liberty jobs `on Designate`_.

#. Stable branch cross-project liaisons should be available in the
   #openstack-stable channel on freenode IRC to answer questions or be made
   aware of issues.


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

Proactive backports
===================

To make sure suitable bug fixes that land in *master* branches are delivered to
stable branch consumers in timely manner, and to avoid situations when a high
impact bug fix falls through the cracks and does not get quickly provisioned to
users, projects may adopt a 'proactive' approach towards tracking patches that
are candidates for backports, as described below.

.. note::

   The first project that adopted the described approach is Neutron. Other
   projects are welcome to experiment with similar practices and provide
   feedback and improvements.

.. note::

   Tools mentioned in the guidelines below are currently maintained in
   `openstack-infra/release-tools repository`_. Most of them are implemented as
   Unix filters that can be interconnected into a pipeline to accommodate for
   specific project needs and practices.

.. note::

   Guidelines below assume that there is a group of people behind the effort
   that are willing to help. Tips on how to build the subteam are out of scope
   for the document.

From high level perspective, proactive backporting process consists of the following steps:

#. identify bugs fixed since the previous triage event;
#. of those, pick only those bugs that does not break stable policy policies;
#. distribute identified backport candidates among subteam members;
#. subteam members consider each candidate bug for inclusion into *stable* and
   *oldstable* branches; if applicable, backports are proposed for review and
   tracked until inclusion into appropriate branches;
#. new stable releases are created in due time.

.. note::
    Most of those steps require human intervension (with the prominent
    exception of the first step) because triaging requires specific judgement.
    New release proposals can be automated, but at the moment, this is left out
    of scope for this document.

This is ongoing process, and it's usually executed on weekly basis, or with
other frequency that fits better the subteam and the project in question.

Now, let's cover each step with more details.

Identify new bug fixes
----------------------

The process assumes that the subteam keeps track of the last git hash that was
validated somewhere. For the initial candidate list generation, it's advised to
start on a branch boundary (the latest common git commit between *stable* and
*master* branches).

For every new git commit found in *master* branch, commit message is checked
for bug tags (Closes-Bug, Partial-Bug, Related-Bug, ...) All bugs mentioned are
considered for initial filtering.

For this exact need, use the following release tool:

.. code-block:: bash

    $ ./bugs-fixed-since.py --repo ../neutron --start 1ce8ce9546479c0ce6055c0c205a8885699e3051
    1514424
    1560464
    1546110
    ...

Filter out features and enhancements
------------------------------------

Due to stable policy described above, new features and enhancements are
generally not allowed in stable branches. For example, to filter out bugs that
have importance set to Wishlist in Launchpad, you can use the following tool:

.. code-block:: bash

    $ ./bugs-fixed-since.py [...] | lp-filter-bugs-by-importance.py neutron --importance Wishlist
    1514424
    1560464
    1546110
    ...

The resulting list is expected to contain only actual bug fixes.

In case you also want to filter out bugs of Low importance, append another call to the tool:

.. code-block:: bash

    $ [...] | lp-filter-bugs-by-importance.py neutron --importance Low
    1514424
    1560464
    1546110
    ...

Once you are satisfied with the query result, you should remember the latest
commit checked, and also store the bug list somewhere.

To achieve the latter, multiple directions can be taken.

#. One way is to store it in some external tool like Etherpad. If this
   direction is chosen, the following tool may become handy to make the list
   more consumable:

.. code-block:: bash

    $ [...] | ./annotate-lp-bugs.py neutron
    https://bugs.launchpad.net/bugs/1514424 "neutron metadata ns proxy does not support ssl" (Medium,Fix Released) [in-stable-mitaka,l3-ipam-dhcp] [vuntz]
    https://bugs.launchpad.net/bugs/1560464 "ovsdb monitor doesn't return ofport" (High,Fix Released) [in-stable-liberty,in-stable-mitaka] [kevinbenton]
    https://bugs.launchpad.net/bugs/1546110 "DB error causes router rescheduling loop to fail" (Medium,Fix Released) [in-stable-kilo,in-stable-liberty,in-stable-mitaka,l3-ipam-dhcp,liberty-backport-potential] [brian-haley]
    ...

#. Another alternative is to tag backport candidates in Launchpad. For that,
   it's advised to avoid using *$release-backport-potential* tags, and instead
   introduce a new tag per project team (f.e. *neutron-proactive-backport-potential*
   for Neutron). This is to avoid conflicts in the tag usage by multiple teams
   running independent backporting processes when bug fixes spanning multiple
   projects are considered.

.. code-block:: bash

    $ [...] | ./lp-tag.py neutron-proactive-backport-potential

This command will tag all identified backport candidates with the project
specific tag. For example, check the `neutron proactive-backport-potential
dashboard`_.

With that, you get access to all filtering features available in Launchpad.

Distribute the work
-------------------

Once you have a list of candidate bug fixes to consider for backporting, it's
time to distribute it among subteam members. Depending on which method is
chosen above to track candidate bug fixes, you may utilize Launchpad search
queries, or other filtering technique to identify bugs of specific topics of
interest, to distribute the work to folks who are experts in those topics.

.. note::

    Exact search queries and filters are project specific and largely depend on
    existing bug tracking practices adopted by projects. Hence they are out of
    scope for the document.

Candidate triage
----------------

Each candidate bug should be assessed on its applicability to *stable* and
*oldstable* branches, as per corresponding support phase definitions. For
example, for *stable* branch, all bug fixes of user value can be considered to
backport; while for *oldstable* branch, only critical bugs are allowed to be
backported.

For every applicable stable branch, a backport is proposed in Gerrit. The
backporter is expected to follow the progress of the backport to make sure it's
not lost in reviews.

Once all applicable backports are proposed to Gerrit and are on their path
towards stable inclusion, *<project>-proactive-backport-potential* tag can be
removed from the bug.

.. note::
    If possible, consider keeping the order of backports in a way that would
    reduce the number of git conflicts.

Release often
-------------

Proactive backporting process is expected to trigger higher volume of changes
in stable branches. To make releases more granular, it's advised participating
projects create new stable releases often. It may be done on a bi-weekly basis,
or any other schedule that fits better the project and its actual backports
volume.

.. _Nova Kilo nominations: https://bugs.launchpad.net/nova/kilo/+nominations
.. _Nova Liberty potential: https://bugs.launchpad.net/nova/+bugs?field.tag=liberty-backport-potential
.. _Oslo policies: http://specs.openstack.org/openstack/oslo-specs/specs/policy/incubator.html#stable-branches
.. _git-review: https://github.com/openstack-infra/git-review
.. _Gerrit bug: https://code.google.com/p/gerrit/issues/detail?id=1107
.. _watched projects: https://review.openstack.org/#/settings/projects
.. _gerrit notify: https://gerrit-review.googlesource.com/Documentation/user-notify.html#user
.. _gerrit search: https://review.openstack.org/#/settings/projects
.. _stable tracker etherpad: https://etherpad.openstack.org/p/stable-tracker
.. _openstack-stable-maint mailing list: http://lists.openstack.org/cgi-bin/mailman/listinfo/openstack-stable-maint
.. _stable maintenance core team: https://review.openstack.org/#/admin/groups/530,members
.. _openstack-infra/project-config repo: http://git.openstack.org/cgit/openstack-infra/project-config/
.. _on Designate: https://review.openstack.org/#/c/292617/
.. _openstack-infra/release-tools repository: http://git.openstack.org/cgit/openstack-infra/release-tools/
.. _neutron proactive-backport-potential dashboard: https://bugs.launchpad.net/neutron/+bugs?field.searchtext=&orderby=-importance&search=Search&field.status%3Alist=NEW&field.status%3Alist=CONFIRMED&field.status%3Alist=TRIAGED&field.status%3Alist=INPROGRESS&field.status%3Alist=FIXCOMMITTED&field.status%3Alist=FIXRELEASED&field.status%3Alist=INCOMPLETE_WITH_RESPONSE&field.status%3Alist=INCOMPLETE_WITHOUT_RESPONSE&assignee_option=any&field.assignee=&field.bug_reporter=&field.bug_commenter=&field.subscriber=&field.structural_subscriber=&field.tag=neutron-proactive-backport-potential&field.tags_combinator=ANY&field.has_cve.used=&field.omit_dupes.used=&field.omit_dupes=on&field.affects_me.used=&field.has_patch.used=&field.has_branches.used=&field.has_branches=on&field.has_no_branches.used=&field.has_no_branches=on&field.has_blueprints.used=&field.has_blueprints=on&field.has_no_blueprints.used=&field.has_no_blueprints=on
