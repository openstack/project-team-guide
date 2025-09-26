==================================
Becoming a Project Team Lead (PTL)
==================================

Have you been approached about becoming a Project Team Lead (PTL) for an
OpenStack project?
Are you considering running for PTL and are unsure about the workload?
Are you speaking with your management about the benefits of running for
PTL and your potential candidacy?
Or, does the project you have been working on not have a candidate for the
upcoming PTL election?

If any of the above is true, the following document is designed to help you
understand the responsibilities that a PTL of an Openstack project is
expected to undertake.

This document is meant to serve as a general guide for current and future PTLs.
It is not all encompassing, nor are all sections required, it is simply a guide
that you may refer to. Each OpenStack project is different and releases,
plans, and delegates differently.

If you are unfamiliar with some of the concepts, terms, or action items
suggested below, do not hesitate to reach out to the current PTL of your
project. If you are unable to do so, you can reach out to any of the
members of `the current serving Technical Committee (TC) <https://governance.openstack.org/tc/>`_
for further guidance on next steps.

.. important::

   It is important to note that there is a lot of content in this document
   that can be considered an overwhelming amount of work. We (the community)
   recommend that any individual considering running for PTL is fully aware
   of the responsibilities, and is comfortable delegating any tasks if they
   are unable to fulfill the entire spectrum of tasks.

Recurrent tasks
===============

The recurrent tasks outlined below are a minimum expectation that the community
believes the serving PTL should be capable of as the serving team leader.

Keep in mind that the items listed below are *optional* and dependent entirely
on your team's function.

#.  Organize the team meeting agenda. Most teams use a wiki or etherpad.

    .. note::

       Not all teams require a weekly meeting. This is a recommended cadence.

#.  Follow the weekly `[release]` guidelines email to keep track of the
    development cycle tasks (unless you have an appointed release liaison to
    follow it for you). It is also useful to subscribe to the `release
    calendar`_.

#.  The PTL should make sure the team regularly triages incoming bugs. For example,
    to view all `new` bugs for `keystone <https://bugs.launchpad.net/keystone/+bugs?orderby=status&start=0>`_
    on Launchpad or `Ironic <https://storyboard.openstack.org/#!/project/openstack/ironic>`_
    on Storyboard.


Core member maintenance
=======================

This will vary greatly from team to team, but the following provides a general
guide you may consider.

Criteria when considering new cores
-----------------------------------

A successful appointment can be made solely by the PTL, however it is
encouraged to hear the opinions of the existing core team. This can be done
through private messages on IRC or publicly through the openstack-discuss_
mailing list.

#.  Are their review numbers (quantity and disagreements) similar to those of
    existing core members?

#.  Does the person follow up on reviews?

#.  Are the reviews helpful, critical, and nice to authors?

#.  Does the person help in triaging bugs? (For example: reproducing the issue)

#.  Does the person provide bug fixes?

#.  Does the person create and implement new features?

#.  Does the person review specifications?

#.  Does the person participate in weekly meetings?

#.  Does the person participate in the forum discussions?

#.  Does the person participate at the mid-cycle / project-team-gathering?

#.  Does the person know the project direction and priorities?

#.  Does the person help others on IRC?


Actions to perform when adding new cores
----------------------------------------

Once a person is ready, perform the following:

#.  Announce new core member on mailing list

#.  Add them to the project drivers group in launchpad.

#.  `Update the core team <https://docs.opendev.org/opendev/infra-manual/latest/creators.html#update-the-gerrit-group-members>`_ in Gerrit.


Criteria when removing new cores
---------------------------------

#.  Is the core reviewer reviewing enough? Use data (i.e. `stackalytics <https://www.stackalytics.io/>`_
    or `reviewstats <https://github.com/openstack/reviewstats>`_) to
    determine if the core reviewer is among the other core members in terms of
    overall review count.

#.  Does the core reviewer have enough time to review? Job responsibilities
    may have changed.

#.  Does the core reviewer no longer have knowledge of the code base and new
    features?

#.  Does the core reviewer still provide useful feedback, even if infrequently?


At the beginning of a new cycle
===============================

#.  Appoint `cross project liaisons`_ (Docs, release, QA, Oslo, etc.).

#.  Appoint `First Contact SIG liaisons`_ (By default, the liaison will be the
    PTL).

#.  Check if the meeting time works for most active contributors.

#.  If you keep release liaison responsibilities, join `#openstack-release` and
    be sure to follow `[release]` emails from the mailing list.

#.  If necessary, squash database migrations. This is usually not necessary,
    but will reduce the amount of time necessary for upgrading. A sample
    squash can be seen `by the keystone
    team <https://github.com/openstack/keystone/commit/f5c64718a1c91fdce5c1da3b1043c14c5b0a97fd>`_.

#.  Track removed and deprecated features, for example using
    `deprecated-as-of-<series>` and `removed-as-of-<series>` blueprints.

#.  Organize the specifications page (if any).

#.  If the TC has approved community goals, check the relevance of the goal to
    the project and assess work items for the team.

#.  Propose to add the extra Active Contributors from your project. To
    understand who can be an extra Active Contributor and how to add them,
    refer to the `TC document <https://governance.openstack.org/tc/reference/charter.html#voters-for-tc-seats-ac>`_

During the cycle
================

#.  Help first time contributors, they will be struggling the most.

    .. note::

       It is not your job as a PTL to full-time mentor an individual. If a
       new contributor is significantly struggling, point them in the direction
       of the `First Contact SIG <https://wiki.openstack.org/wiki/First_Contact_SIG>`_
       (#openstack-upstream-institute on IRC) so they can receive appropriate onboarding.

#.  Lack of reviews? Reach out to the core team and remind them.

#.  `Release libraries as
    necessary <https://releases.openstack.org/reference/release_models.html#cycle-with-intermediary>`_,
    but don't wait too long! Some teams will release after 4 weeks even if the
    changes are minor. *More often is better than less often.*

Conference and event tasks
==========================

Project Updates
----------------

#. Decide if your project has news they want to share with the community
   in the form of a 20 or 40 min project update at the upcoming Summit.
#. Keep an eye out for the Project Update Request Survey from OSF staff.
   The survey will come directly to PTLs, not to the openstack-discuss ML.
#. If you requested a Project Update, make sure to register yourself (or
   that whoever else is listed as speaker does) before the deadline noted
   in the email with the survey to secure the update slot.

Before the Forum
----------------

#.  Start an etherpad to brainstorm potential session topics. For example:
    http://lists.openstack.org/pipermail/openstack-dev/2017-March/114123.html

#.  Based on that brainstorming, propose sessions. Create an etherpad for
    every session, prime the content. List these etherpads in the Wiki.

During the Forum
----------------

#.  Reach out to potential new contributors to the project, participate in
    project on-boarding sessions.

#.  Attend as many cross-project sessions as possible.

#.  In the discussion sessions you moderate:

    * Take notes on the etherpad (or delegate a scribe)
    * Act as a moderator rather than actively participate (or delegate a
      moderator)

#.  After the discussion, post a summary of the session outcome to the ML, for
    the benefit of those who could not be present.

#.  Towards the end of the Forum, ensure a summary of all discussions are sent
    to the ML for individuals who did not attend the event.

Before the PTG
--------------

#.  Decide if your team will hold a team meeting at the PTG, and communicate
    with the events organizers. An email is sent out beforehand by OSF staff
    directly to PTLs- keep an eye out.

#.  If your team gathers at the PTG, create an etherpad to dynamically build
    the room agenda, and list it on the event wiki page.

#.  Create a tentative time schedule so that people from other projects who are
    interested in a certain topic know when to join in the discussion.

During the PTG
--------------

#.  Be as flexible as possible, attend inter-project sessions as appropriate.

#.  Keep the event schedule up to date on what the current topics of discussion
    in your team room is.

#.  Towards the end of the PTG, ensure a summary of all discussions are sent to
    the ML for individuals who did not attend the event.

Attending events
----------------

Whilst attending the Summit and PTG as a PTL is preferential, it is not the
end of the world if you are unable to do so for personal or professional
reasons. The community is here to support you, and is available to help plan
team orientated events and tasks if you are unable to make the trip.

If you are unable to attend, see our section on
`How to successfully delegate`_.

At the end of the cycle
=======================

#.  Clean up release notes.

#.  Coordinate with the `release management`_ team for deliverables, unless a
    liaison has been appointed and make sure release-highlights are documented
    in the release files.

#.  Perform a retrospective via an etherpad. Suggested sections include:
    `What went well?`, `What didn't go well`.

#.  Analyze how `complete` each new feature is. Does it have DevStack support?
    Horizon support? Client bindings? CLI support? Documentation? Does the
    install guide need to be updated?

#.  Ensure documentation is up-to-date with any major changes that were
    implemented throughout the cycle.


Collecting feedback
===================

Collecting feedback from users and operators is an essential step for
incrementally improving software. Anyone can collect feedback, but sometimes it
falls on the shoulders of the PTL to facilitate open lines of communication.
The following are a few ways you can do that.

Mailing lists
-------------

Our community has several mailing lists. Most usage, operations and
development discussions take place on the openstack-discuss_ mailing list,
making it a great place to ask for feedback. An advantage of using mailing
lists is that responses are logged making it easy to reference them later. You
also don't have to wait for a specific time or place to use mailing lists,
making it easy to attempt to collect feedback in a pinch or when a formal
setting isn't feasible.

User survey
-----------

The Foundation puts together a survey for users and operators. The Foundation
shares the results with PTLs, who can then disseminate the knowledge to others
who may be interested.

It's worth checking to see if your project is participating in the survey. Make
sure the survey questions for your project are relevant and reflect the current
status of what the team is doing. If you're not sure what's being asked in the
survey or want to update the project-specific survey questions, reach out to
someone from the Foundation.

PTG sessions
------------

Occasionally, you might find operators or users at Project Team Gatherings. You
can set up timeslots on your projects agenda, inviting them to share feedback
with developers. If an official time slot doesn't make its way into the
schedule, hallway discussions are good ways to collect quick feedback.

Forum sessions
--------------

It isn't uncommon to find more operators and users at Summits and Forums than
PTGs. You can use this as an opportunity to collect as much feedback from them
as possible if you're attending. Since everyone usually has a busy schedule,
it's better to plan ahead and socialize those sessions. There are a couple of
specific ways you can collect feedback throughout the week.

First, submit a Forum session proposal to collect feedback for your project.
The Foundation asks the community for session proposals, which are used to
build the schedule for the Forum. Be explicit if you're looking for feedback on
specific things. By having a feedback session on the formal schedule, you're
letting operators and users know your project is open to listening to what they
have to say. It's a great way to meet users face-to-face, exchange contact
information, and discuss issues they might be having.

Second, use your project update to advertise feedback sessions or that the team
is interested in feedback. If you're looking for direction on a new feature,
share a little bit about it and say you'd like to hear what people think. You
don't have to spend the entire project update focusing on this, but it could
result in a follow-up afterward or an interesting hallway discussion.

Stable
======

Alternatively, the responsibilities in this section can be delegated to an
individual to manage local stable maintenance.

#.  Ensure the stable branches gates are not broken.

#.  Co-ordinate with the stable release team to ensure releases are performed
    when a critical fix is backported, or sufficient smaller fixes have
    landed.

One offs
========

When necessary, the following can be performed at unscheduled times.

#.  Bug smashes

#.  API sprints

Tips and tricks
===============

Now that this document has told you everything you could be doing, here are
some community tips on what you can do to help make your experience as an
OpenStack project leader better.

If you can think of anything else that might be helpful, do not hesitate
to clone the `project-team-guide` repo from
`OpenDev <https://opendev.org/openstack/project-team-guide>`_
and submit an addition.

- Ensuring your email filters are setup to catch anything with `[ptl]` or
  your project name in the subject header.

- Join the #openstack-tc IRC channel if you have not already for discussion
  on community goals or anything relevant to your project.

- Join the #openstack-release channel, even if you have a release liaison.

- Project update emails: An optional extra for when you're getting into the
  swing of things. Providing an occasional project team update email to the
  openstack-discuss mailing list is a great way to keep part-time contributors
  informed of the changes occurring within the project. For example, the
  Keystone team provides updates weekly:
  http://lists.openstack.org/pipermail/openstack-discuss/2019-June/006799.html

- Make sure the IRC weekly meeting information and agenda is up to date:
  http://meetings.opendev.org

- Set aside time during the weekly meeting to look at the oldest outstanding
  review in the project. The resulting action should be one of the following:
  the patch is merged, -1'd, or someone is assigned to follow up if the review
  cannot be completed in real time. This is a great way to reduce significant
  backlogs and potential technical debt.

- Courtesy pings in IRC meetings: Everyone lives busy lives outside of the
  community. Coming up with a way to ping team members who are interested in
  attending team meetings is a helpful addition.

  Another way to do this is to encourage team members to configure their
  IRC client to highlight on a specific keyword. For example
  `#startmeeting <PROJECT>` or `foo-team`.

- Manage priority reviews. This can be done by adding a review priority column
  in Gerrit or maintaining the priority
  `blueprint <https://blueprints.launchpad.net/>`_ in a spec repo.
  Here is an example from the Cinder team implemented the review priority
  column: https://review.opendev.org/#/c/620664/

How to successfully delegate
----------------------------

Delegating is a large part of your role as an OpenStack PTL. There are numerous
tasks and we know how difficult it can be to keep up with it all. Some projects
are more fortunate than others, having multiple people around to delegate to,
however this is not always the case - no matter the size of the project.

The following are some tips and tricks derived from community members to help
you delegate:

- Reach out to team members on IRC or the mailing lists - everyone communicates
  differently.

- Detail your ask. Vague requests tend to go ignored because people have their
  own workloads. But if you need someone to host a team meeting, summarize the
  forum or PTG, or even fix a bug, details are key to getting results.

- Don't wait until the last minute to ask for help. If you've got a big project
  on horizon, find someone to help at the beginning - even if that person is to
  be your backup if things fall through.

If you can't find a delegate, it is okay to let things go. It is not the upmost
importance to have a team meeting, or plan everything perfectly. Here are some
tips to help you deal with being unable to delegate tasks:

- Do not be afraid to reach out to other project teams, the TC, or the UC for
  help. The TC and the UC are designed to provide guidance and support where
  possible.

- Don't be a hero. Ensure people are aware that you are having troubles and
  some deliverables might not be met. We care about our community members, and
  it's important that you feel supported, and not crushed.

Handing over PTL duties
=======================

Are you thinking of moving on? Hoping to encouraging healthy rotation in the
role? Perhaps you've decided you've had enough and you're burnt out. Or perhaps
you're moving to a new role or company and OpenStack is no longer your work
priority. There are a myriad of reasons why someone would need or want to move
on from the PTL position. While the community would be sad to see you step
down, it is part of the lifecycle of the position and it's often a positive
change to see new people and new ideas into leadership positions.

Handing over the PTL position is not easy, it's not as simple as pinging
someone who is an active contributor and asking if they're interested or not.
The main thing is to get the individual up to speed on the content covered in
this document, as it may be things they have not encountered yet.

.. note::

   There are some important bits of information to pass on, but you're never
   going to have a complete knowledge transfer. This is okay!

To make that process a little bit easier for you, and for them, offer PTL
mentoring before stepping down. If you know that your situation is going to
change in advance, why not reach out to the whole team and ask who is
interested and if you could mentor them in the last few months?

If there are no takers, reach out to the OpenStack TC before stepping down so
they are aware of the current situation and can step in to help.

.. _release calendar: https://releases.openstack.org/schedule.ics
.. _cross project liaisons: https://wiki.openstack.org/wiki/CrossProjectLiaisons
.. _release management: http://docs.openstack.org/project-team-guide/release-management.html
.. _First Contact SIG liaisons: https://wiki.openstack.org/wiki/First_Contact_SIG#Project_Liaisons
.. _weekly meetings: http://eavesdrop.openstack.org/#User_Committee_Meeting
.. _openstack-discuss: http://lists.openstack.org/cgi-bin/mailman/listinfo/openstack-discuss
