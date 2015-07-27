=============
 Open Design
=============

Common development cycle
========================

OpenStack deliverables are released in various ways and on different
time-based or feature-based schedules (see the
:doc:`Release Management </release-management>` chapter). What binds them
all, however, is our common 6-month development cycle.

OpenStack development cycles are named alphabetically (Austin, Bexar, Cactus,
Diablo...) and may result in one or more releases. Stable branches (see the
chapter on :doc:`Stable Branches </stable-branches>`) are only cut once though, from the last release of a given deliverable for that development cycle.

.. _design-summit:

Design Summit
=============

Design Summits are events held at the beginning of a development cycle, every
6 months. They are a key part of the "Open Design" promise: they are open to
the public, and recent contributors are sponsored by the OpenStack Foundation
to attend free of charge. They were modeled after Ubuntu Developers Summits,
which are now discontinued.

Goals
-----

OpenStack Design Summits have several goals:

* For features at the early design stages, get feedback on early direction
  and quick convergence across a broad range of attendees
* For features that are being implemented, get it through the final
  implementation stages and engage with potential contributors and testers
* Get alignment on project team priorities for the upcoming development cycle
* Make quick progress on issues that are difficult to solve otherwise, by
  getting the right set of people working on it together at the same time
* Meet in person with fellow contributors, socialize in the hallway track or
  evening events, reset relationships after heated online discussions

The OpenStack Design Summits are not for traditional speaker-to-audience
presentations. They should all be open discussions about a specific technical
topic. Therefore, usage of slides or microphones is discouraged, to reduce
the distance between the session proposer/moderator and the rest of the
audience.

Event structure
---------------

The event lasts for 4 days and is organized in tracks.

* The first day is usually dedicated to the "cross-project workshops" track.
  This track is meant to provide a forum to discuss and address issues that
  span more than just one project team. Sessions are generally 40-min long
  or 90-min long.

* The second and third days are usually dedicated to project team tracks (one
  per project team). Sessions are generally 40-min long.

* The "Ops" track is spread across those first three days. Originally a
  separate event (the "Ops Summit"), the operators feedback and discussions
  are now an integral part of our design process. Sessions are generally 40-min
  or 90-min long.

* The last day is generally organized as "contributors meetups": half a day or
  a full day of open discussion between project team members. The agenda is
  generally decided on the spot based on the result of the previous sessions,
  and the amount of energy left at that stage.

Session types
-------------

There are two types of rooms available for sessions, which condition the type
of session you can hold in them.

*Fishbowl* sessions happen in large rooms that can hold between 100 and 300
people. Those rooms are equipped with a projector and whiteboards, and are
organized in fishbowl style (concentric circles of chairs, people who want
to engage in the discussion should move to the center of the fishbowl)

Those sessions are for open discussions where a lot of participation and
feedback is desirable. They generally have catchy titles and appear prominently
on the Design Summit schedule.

*Working* sessions happen in smaller rooms that can hold between 20 and 40
people. Those rooms are equipped with a monitor and whiteboards, and are
organized in boardroom style (everyone around a large table).

Those sessions are for a smaller group of contributors to get specific
work done or prioritized. They generally have a blanket title (like "infra
team working session") and redirect to an etherpad for more precise and
current information about content, in an attempt to limit out-of-team
participation and avoid overcrowding the room.

Pre-summit organization
-----------------------

A few months before the event, every PTL is contacted by the event organizers
to get an idea of the session needs for their project team track. After
conferring with their teams, they should answer with a number of fishbowl
sessions and working sessions desired.

The organizing staff at the Foundation will gather all requirements and
allocate slots based on project requests and availability in the venue,
potentially arbitrating using metrics showing team development activity over
the past cycle.

With that allocation in mind, each project team should come up with a list of
topics to cover in fishbowl and working sessions. This is usually done through
collaborative tools like etherpad.openstack.org and multiple team meetings.

Approximately a month before summit, the draft slot / room allocation will be
proposed. It tries to minimize block conflicts (a project being prevented from
attending any session from another project), but comments are welcome before
it is made final.

A few weeks before summit, PTLs will be asked to push the proposed schedule
through tooling that will connect to the scheduling app used at the Summit.

During the summit
-----------------

We use etherpad.openstack.org to take notes during sessions. It is generally
a good idea to prepare those etherpads in advance and list them on the
common list of Design Summit etherpads.

Each session should have a moderator to keep the discussion on track, and try
to get to actionable outcomes before the end of the session. It is generally
the person who suggested the session in the first place.

As a courtesy to attendees, make sure to start your session on time and end
your session on time, so that they can easily jump to another room. Vacate the
room at the end of the session, and continue the discussion in the hallway if
necessary.

.. _midcycles:

Midcycle sprints
================

Midcycle sprints (also named *midcycle meetups*) are optional project team
gatherings that happen between Design Summits. They should be announced on the
openstack-dev mailing-list and listed on:
https://wiki.openstack.org/wiki/Sprints

Those are freely organized by project teams, usually using space donated by
Foundation member companies. Travel costs are the responsibility of the
individual attending (their employer most likely). Those are especially
useful in the early days of joining or forming a team, when social bonds
and trust need to be established.

Multiple sprints may be co-located, especially when there are good
cross-pollination opportunities between the involved teams. However,
productivity also lies in the small, quiet environment, and social bonding
is easier in smaller groups.

Note that the Design Summit should be the first choice for gathering the
whole team for decisions and roadmap alignment. Attendance to the midcycle
sprints should never be mandatory. It is therefore better to have a specific
objective for the sprint and use it to get things done. Any feeling of
"required" attendance (social or actual) may cause hard feelings, especially
marginalizing contributors without a corporate sponsor, family caretakers,
and people who need Visas to travel.

To enable people to focus on the same topic at the same time, without
factoring in the monetary and life cost of travel, we also support Virtual
Sprints held on IRC. See https://wiki.openstack.org/wiki/VirtualSprints for
details.
