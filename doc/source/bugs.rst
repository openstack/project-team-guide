======
 Bugs
======

Launchpad bugs and StoryBoard stories are used to track known issues and
defects in OpenStack software.

Bugs Reference
==============

Here are the different fields available in Launchpad bugs and StoryBoard tasks,
and how we use them within the OpenStack project.

Status
------

**Launchpad:**

.. list-table::
   :widths: 20 100

   - * `New`
     * The bug was just created
   - * `Incomplete`
     * The bug is waiting on input from the reporter
   - * `Confirmed`
     * The bug was reproduced or confirmed as a genuine bug
   - * `Triaged`
     * The bug comments contain a full analysis on how to properly fix the
       issue
   - * `In Progress`
     * Work on the fix is in progress, bug has an assignee
   - * `Fix Committed`
     * Not used
   - * `Fix Released`
     * The fix has been merged into an official branch
   - * `Invalid`
     * This is not a bug
   - * `Opinion`
     * This is a valid issue, but it is the way it should be
   - * `Won't Fix`
     * This is a valid issue, but we don't intend to fix that

**StoryBoard:**

.. list-table::
   :widths: 20 100

   - * `Todo`
     * The task is awaiting an assignee to start work
   - * `In Progress`
     * Work on the fix is in progress, task has an assignee
   - * `Review`
     * A patch to complete this task is in review
   - * `Merged`
     * The patch has been merged into the branch specified in the task
   - * `Invalid`
     * This is not a valid task

StoryBoard conveys story metadata via free-form tags and named worklists, and
displays priority via an item's position in a worklist. The other things on
this page apply to Launchpad only. Only StoryBoard tasks have statuses, not the
stories themselves, as a story describes a goal and its tasks describe the
specific work required to meet that goal. So, when all tasks are marked
`Merged` or `Invalid`, the story is automatically marked `Merged`.

Importance
----------

This should be set when the status reaches `Confirmed` stage. It is a
combination of short-term impact (unavailability of a feature), long-term
impact (data corruption, security breach), number of people affected, and
presence of a documented workaround. Use these as guidelines:

+-------------+------------------------------------------------------------+
| `Critical`  | Data corruption / complete failure affecting most users,   |
|             | no workaround                                              |
+-------------+------------------------------------------------------------+
| `High`      | Data corruption / complete failure affecting most users,   |
|             | with workaround                                            |
|             +------------------------------------------------------------+
|             | Failure of a significant feature, no workaround            |
+-------------+------------------------------------------------------------+
| `Medium`    | Failure of a significant feature, with workaround          |
|             +------------------------------------------------------------+
|             | Failure of a fringe feature, no workaround                 |
+-------------+------------------------------------------------------------+
| `Low`       | Small issue with an easy workaround                        |
|             +------------------------------------------------------------+
|             | Any other insignificant bug                                |
+-------------+------------------------------------------------------------+
| `Wishlist`  | Not really a bug, but a suggested improvement              |
+-------------+------------------------------------------------------------+
| `Undefined` | Impact was not assessed yet                                |
+-------------+------------------------------------------------------------+

Note that presence of `Critical` bugs will delay the release.

Assigned To
-----------

The person currently working to fix this bug. Must be set by `In Progress`
stage.

Milestone
---------

The milestone we need to fix the bug for, or the milestone/version it was fixed
in.

Tags
----

Free-form tags you can use to search bugs with. Here is the list of `official
tags`_.

.. _`official tags`: https://wiki.openstack.org/wiki/BugTags

Bugs lifecycle
==============

Bugs go through multiple stages before final resolution.

Reporting
---------

When you find a bug, you should file it against the proper OpenStack project.
Make sure there is no bug already filed for the same issue, then enter the
details of your report. It should at least include:

* The release, or milestone, or commitid corresponding to the software that
  you are running

When this is done, the bug is created with:

* Status: `New`

Confirming & prioritizing
-------------------------

This stage is about checking that a bug is real and assessing its impact. If
you are lacking information to properly reproduce or assess the importance
of the bug, you should ask the original reporter for more information and
set the bug to:

* Status: `Incomplete`

Once you have reproduced the issue (or are 100% confident that this is
indeed a valid bug), you should set:

* Status: `Confirmed`

If you're a core developer or a member of the project bug supervision team,
you should also prioritize the bug:

* Importance: <Bug impact> (see above)

Debugging (optional)
--------------------

This optional stage is about determining how to fix the bug and posting the
solution in the comments. Sometimes the implementation of the fix will be
straightforward and you would jump directly to bugfixing, but in some other
cases, you would just post your complete debugging analysis and give someone
else the opportunity of fixing the bug. Then you should ask a core
developer or bug supervisor to set:

* Status: `Triaged`

Bugfixing
---------

At this stage, a developer will work on a fix. During that time, in order to
avoid duplicating the work, he should set:

* Status: `In Progress`
* Assignee: <yourself>

When the fix is ready, he will propose the change and get it reviewed.

Note that Gerrit will automatically set the status and assignee when a
change is proposed that mentions the bug number.

After the change is accepted
----------------------------

Once the change is reviewed, accepted, and has landed in master, it will
automatically move to:

* Status: `Fix Released`
