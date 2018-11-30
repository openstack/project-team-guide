==========================================
 Project Team Lead (PTL) Responsibilities
==========================================

So you want to be a PTL? Here is a list of items that you can expect to perform.
This document is meant to serve as a general guide for current and future PTLs.
It is not all encompassing, nor are all sections required, it is simply a guide
that you may refer to. Each OpenStack project is different and will release,
plan, and delegate differently, too.


Weekly Tasks
============

#.  Organize the team meeting agenda. Most teams will use a wiki or etherpad.

#.  Follow the weekly `[release]` guidelines email to keep track of the
    development cycle tasks (unless you have an appointed release liaison to
    follow it for you).  It is also very useful to subscribe into the `release
    calendar`_.

#.  Attend Technical Committee and Cross-Project meetings when possible. For
    more information see `meetings`_.

#.  If the team does not have an appointed bug czar, then perform a bug triage.
    Ensure all `new` bugs that have been reported are triaged. The below is a
    URL to view all `new` bugs for keystone.

    ::

      https://bugs.launchpad.net/keystone/+bugs?orderby=status&start=0

#.  If the team does not have an appointed bug czar, then remember to also
    tag bugs accordingly. The below is a URL to view all untagged bugs for
    keystone.

    ::

      https://bugs.launchpad.net/keystone/+bugs?field.tag=-*


Core member maintenance
=======================

This will vary greatly from team to team, but here is a general guide you may
consider.


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

#.  Give them voice on IRC (if applicable)::

      /msg chanserv flags <room> <irc_handle> +V

#.  Add them to the project drivers group in launchpad.

#.  Update the core team in Gerrit.


Criteria when removing new cores
---------------------------------

#.  Is the core reviewer reviewing enough? Use data (i.e. stackalytics) to
    determine if the core reviewer is among the other core members in terms of
    overall review count.

#.  Does the core reviewer have enough time to review? Job responsibilities
    may have changed.

#.  Does the core reviewer no longer have knowledge of the code base and new
    features?

#.  Does the core reviewer still provide useful feedback, even if infrequently?


At the beginning of a new cycle
===============================

#.  Add placeholder migrations, these should be the first things to merge once
    the n-1 stable branch has been created. *Do not merge any migrations before
    this is done*

#.  Appoint `cross project liaisons`_ (Docs, release, QA, Oslo, etc.).

#.  Appoint `FirstContact SIG liaisons`_ (By default, the liaison will be the PTL).

#.  Check if the meeting time works for most active contributors.

#.  If you keep release liaison responsibilities, join `#openstack-release` and
    be sure to follow `[release]` emails from the mailing list.

#.  If necessary, squash database migrations. This is usually not necessary,
    but will reduce the amount of time necessary for upgrading. A sample
    squash can be seen `by the keystone team <https://github.com/openstack/keystone/commit/f5c64718a1c91fdce5c1da3b1043c14c5b0a97fd>`_.

#.  Track removed and deprecated features, for example using
    `deprecated-as-of-<series>` and `removed-as-of-<series>` blueprints.

#.  Organize the specifications page.


During the cycle
================

#.  Help first time contributors, they will be struggling the most.

#.  Lack of reviews? Reach out to the core team and remind them.

#.  Release libraries as necessary but don't wait too long! Some teams will
    release after 4 weeks even if the changes are minor. *More often is
    better than less often.*

Before the Forum
================

#.  Start an etherpad to brainstorm potential session topics. For example:
    http://lists.openstack.org/pipermail/openstack-dev/2017-March/114123.html

#.  Based on that brainstorming, propose sessions. Create an etherpad for
    every session, prime the content. List these etherpads in the Wiki.

During the Forum
================

#.  Reach out to potential new contributors to the project, participate in
    project on-boarding sessions.

#.  Attend as many cross-project sessions as possible.

#.  In the discussion sessions you moderate:

    * Take notes on the etherpad (or delegate a scribe)
    * Act as a moderator rather than actively participate (or delegate a moderator)

#.  After the discussion, post a summary of the session outcome to the ML, for the
    benefit of those who could not be present.


At the end of the cycle
=======================

#.  Clean up release notes.

#.  Coordinate with the `release management`_ team for deliverables, unless a
    liaison has been appointed

#.  Expect queries from the release marketing staff to name release highlights
    and major features

#.  Perform a retrospective via an etherpad. Suggested sections include:
    `What went well?`, `What didn't go well`.

#.  Analyze how `complete` each new feature is. Does it have DevStack support?
    Horizon support? Client bindings? CLI support? Documentation? Does the
    install guide need to be updated?


Before the PTG
==============

#.  Decide if your team will hold a team meeting at the PTG, and communicate
    with the events organizers

#.  If your team gathers at the PTG, create an etherpad to dynamically build
    the room agenda, and list it on the event wiki page.


During the PTG
==============

#.  Be flexible, attend inter-project sessions as appropriate.

#.  Keep the event schedule up to date on what the current topics of discussion
    in your team room is.


Collecting Feedback
===================

Collecting feedback from users and operators is an essential step for
incrementally improving software. Anyone can collect feedback, but sometimes it
falls on the shoulders of the PTL to facilitate open lines of communication.
The following are a few ways you can do that.

Mailing Lists
-------------

Our community has several mailing lists, though most usage, operations and
development discussions take place on the openstack-discuss_ mailing list
making it a great place to ask for feedback. An advantage of using mailing
lists is that responses are logged making it easy to reference them later. You
also don't have to wait for a specific time or place to use mailing lists,
making it easy to attempt to collect feedback in a pinch or when a formal
setting isn't feasible.

User Surveys
------------

The Foundation puts together a survey for users and operators. The Foundation
shares the results with PTLs, who can then disseminate the knowledge to others
who may be interested.

It's worth checking to see if your project is participating in the survey. Make
sure the survey questions for your project are relevant and reflect the current
status of what the team is doing. If you're not sure what's being asked in the
survey or want to update the project-specific survey questions, reach out to
someone from the Foundation.

User Committee
--------------

The User Committee is an elected body within the community that helps
facilitate communication between users and developers. If there are specific
things your project wants feedback on, but you're not sure how or where to
start, the User Committee can help. They hold `weekly meetings`_ on IRC, and
they can help you come up with a plan for collecting feedback.

PTG Sessions
------------

Occasionally, you might find operators or users at Project Team Gatherings. You
can set up timeslots on your projects agenda, inviting them to share feedback
with developers. If an official time slot doesn't make its way into the
schedule, hallway discussions are good ways to collect quick feedback.

Forum Sessions
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

Alternatively, the responsibilities in this section can be delegated to a
local stable maintenance czar.

#.  Ensure the stable branches gates are not broken.

#.  Co-ordinate with the stable release team to ensure releases are performed
    when a critical fix is backported, or sufficient smaller fixes have
    landed.


One offs
========

When necessary, the following can be performed at unscheduled times.

#.  Bug smashes

#.  API sprints


.. _meetings: http://docs.openstack.org/project-team-guide/cross-project.html#meetings
.. _release calendar: https://releases.openstack.org/schedule.ics
.. _cross project liaisons: https://wiki.openstack.org/wiki/CrossProjectLiaisons
.. _release management: http://docs.openstack.org/project-team-guide/release-management.html
.. _FirstContact SIG liaisons: https://wiki.openstack.org/wiki/First_Contact_SIG#Project_Liaisons
.. _weekly meetings: http://eavesdrop.openstack.org/#User_Committee_Meeting
.. _openstack-discuss: http://lists.openstack.org/cgi-bin/mailman/listinfo/openstack-discuss
