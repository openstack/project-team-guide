***************************************
How to Review Changes the OpenStack Way
***************************************

In almost all repositories, Gerrit offers 5 options for voting on a patch. The
+2 and -2 options are only available to members of the core review team for a
project. Everyone has access to the -1 and +1 options. You can also leave a
comment without voting (0). This document is a non-exhaustive guide to
techniques you can use when reviewing a change to deliver your feedback while
keeping the process flowing. In all cases use your best judgement, but we offer
these heuristics from the collective experience of the community to guide you.

It's patch submitters who keep OpenStack moving forward. As a reviewer,
remember that you're providing a service to submitters, not the other way
around. As a submitter, remember that everyone is subject to the same rules:
even the core reviewers voting on your patch put their changes through the same
code review process.

.. _code-review-plus-2:

Code Review +2
==============

The +2 vote is only available to core reviewers. In general, two +2 votes are
required before a change can be merged, although some projects relax the
requirements in certain circumstances, such as trivial changes. Confusingly,
two +1s do not equal a +2!

Voting +2 indicates that you're happy for another core reviewer to Approve the
change. If another core reviewer has already voted +2 then you would generally
Approve the change at the same time. However, you might hold off on approval to
give the author or another reviewer the chance to respond to some trivial
feedback if they think it appropriate. If the feedback is sufficiently trivial,
this is preferable to only voting +1.

If another core reviewer had previously voted +2 on an earlier patch set, and
the patch has only changed in trivial ways that you're sure they would be happy
with since then, go ahead and Approve with your single +2 rather than wait for
them to re-review. When you do this, leave a comment explaining your decision.

.. _code-review-plus-1:

Code Review +1
==============

For non-core reviewers, a +1 indicates that you've reviewed the change and are
comfortable with it merging. Leaving just a +1 without a comment is not that
useful unless your review history is well known to the core team (in which case
there's a good chance you'll soon be joining it). If it makes sense to, try to
leave a comment - if you tested the patch, say so; if you had to look anything
up to confirm it was correct, leave a comment with the link to the reference to
help the next reviewer.

Core reviewers can make use of a +1 vote as well. Consider doing so when you
think the patch is OK but you have an open question, or you have minor feedback
that could be fixed in a :ref:`follow-up change <follow-up-changes>` but you
want to give the contributor a chance to fix it themselves - for example,
because you want their opinion too, or because you're trying to help mentor
them. If you're not sure if the contributor is looking for mentoring or would
prefer you to just fix the patch or submit a follow-up change yourself, try
asking them! Many contributors will respond quickly to a +1 from a core
reviewer, because they know it means a +2 just around the corner, and this is
much better at encouraging the contributor and preserving goodwill than a -1.
It will also help you re-review any subsequent patch set more quickly, since
you'll see that you were basically OK with it before and need only to check any
differences.

Even as a core reviewer, you may not be familiar with every part of a project.
If you encounter a change in an area that you're not confident with, you can
vote +1 where you would otherwise have voted +2. As always, it pays to leave a
comment to say why.

.. _code-review-0:

Code Review 0
=============

You might leave a comment without a review if you don't have strong feelings
either way about a change, or if you simply have a question you need answered
in order to form an opinion. Unless you're fairly sure the answer to the
question is going to reveal a problem, this is preferable to voting -1 in the
first instance. If it's been a while without an answer, or new patch sets are
posted without anyone responding to your comments, that might be time to
consider a -1.

.. _code-review-minus-1:

Code Review -1
==============

Use a -1 review to indicate that you have found significant problems with the
patch that you strongly believe should be corrected before the change is
merged. There are many legitimate reasons to do this, ranging from a new bug
being introduced by the change, through to even something as small as a typo in
the commit message - *if* (and only if!) it's in a key word that will make the
patch hard to find in git history. Once again, use your best judgement, but
remember that when you vote -1 you're obstructing someone else's work so make
sure it's for a good reason and not something that can be addressed in another
way (such as a :ref:`follow-up change <follow-up-changes>`).

Remember also that many busy reviewers will not prioritise changes that already
have negative reviews, so by voting -1 you are not only requiring the submitter
to make another revision, you're also potentially cutting them off from more
sources of feedback.

A -1 review should always be accompanied by comments with actionable feedback.

If you are arriving late to a change with a large number of patch set
revisions, don't forget to look back at the previous history if you see
something strange. It may have been requested by an earlier reviewer. If you
are a core reviewer and you find yourself needing to give the opposite advice
to that given by another core reviewer, it is *your* responsibility to come to
an agreement with the other reviewer and ideally for you both to document it.
Preferably before you vote -1, unless the change actually breaks something (in
which case, leave a comment indicating that you understand there is conflicting
advice and you are working to resolve it as soon as possible). It is never the
patch submitter's responsibility to deal with a disagreement between core
reviewers.

.. _code-review-minus-2:

Code Review -2
==============

The -2 vote is only available to core reviewers. Unlike other votes, this one
is 'sticky' - a -2 vote stays with the change even if the submitter pushes new
(and substantively different) patch sets. That means that to remove a -2 vote
requires action from the same core reviewer, so be careful.

The purpose of the -2 vote is to indicate to the submitter that any further
time they spend on the change will almost certainly be wasted. If you receive a
-2 review on a change you submit, don't feel bad! The reviewer is trying to
redirect your valuable time and energy toward changes that have a chance of
being merged. Communicating clearly means less wasted time on your part.

A -2 review should always be accompanied by a comment explaining the reason
that the change does not fit with the project goals, so that the submitter can
understand the reasons and refocus their future contributions more
productively.

Other than the ':ref:`procedural -2 <procedural-minus-2>`' mentioned below,
there are no other legitimate uses of a -2 vote.

.. _procedural-minus-2:

Procedural Code Review -2
-------------------------

Some projects will put a -2 vote on feature changes after Feature Freeze and
before branching for the next release, to ensure that no features are
unintentionally merged during the freeze. The person who added these -2s will
then remove them again once the master branch is open for new features. They
should leave a comment explaining exactly what is happening. Submitters can
continue to revise the change during the freeze.

.. _workflow-minus-1:

Workflow -1
===========

A Workflow -1 vote indicates that the change is not currently ready for a
comprehensive review. Only core reviewers and the original change owner can
vote Workflow -1. Any workflow votes are cleared when a new patch set is
submitted for the change. This is a better way to get feedback on ongoing work
than the legacy method of a Draft change (which is hidden from reviewers not
specifically added to it).

Core reviewers may also use the Workflow -1 vote to prevent a change from being
merged during some temporary condition, without interrupting the code-review
process.

.. _follow-up-changes:

Follow-up Changes
=================

When possible, submitting follow-up changes is a great way to address minor
issues without stalling the review process by requiring another patch set (thus
wiping out existing reviews). Simply `check out`_ the existing change (using
either the commands Gerrit provides in the Downloads drop-down; the ``git
review -d`` command; or the `git-nit`_ tool), add another commit on top, and
start a new review.

This is usually preferable to modifying the original change yourself, provided
that the change doesn't actually break anything.

.. _modifying-a-change:

Modifying a Change
==================

It is possible for anyone to push a new patch set to an existing review, and
sometimes this is the best way to resolve an issue. However, be aware that this
may be surprising to some contributors, and some may even feel you're trying to
take credit for their patch. This is not the case - all of the statistics
gathering tools give credit to the owner of the Change (i.e. the initial
submitter). If you don't know the submitter, it pays to leave a comment letting
them know what you're doing (you can link to this section of the project team
guide as part of the explanation). Make sure you edit using the `Gerrit UI`_ or
`check out`_ the existing patch using either the commands Gerrit provides in
the Downloads drop-down or the ``git review -d`` command before incorporating
your modifications using ``git commit --amend``, so that the patch author field
remains unchanged and you are listed only as the committer in Git. If your
modifications are substantial, you can add a Co-Authored-By credit in the
commit message.

Some examples of times you might want to modify an existing change:

* When the submitter specifically invites you to
* When the patch needs rebasing
* When the submitter hasn't responded to feedback in some time
* When you plan to merge the patch immediately after an obvious trivial tweak
* When you just need to amend the commit message (commit messages are immutable
  and cannot be fixed in a :ref:`follow-up change <follow-up-changes>`)

Be aware that if the change is not the last (or only) one in a series, the
remainder of the series will also need to be rebased. In such circumstances,
it's usually better to leave the modification to the original author if
possible, because the process of replacing local branch with the latest from
Gerrit may require fairly robust knowledge of Git and Gerrit.


.. _git-nit: https://pypi.org/project/git-nit/
.. _check out: https://docs.openstack.org/contributors/code-and-documentation/using-gerrit.html#checking-out-others-changes
.. _Gerrit UI: https://docs.openstack.org/contributors/code-and-documentation/using-gerrit.html#gerrit-web-editor
