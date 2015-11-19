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
