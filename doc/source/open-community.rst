================
 Open Community
================

As one of four core tenets of OpenStack's open mantra, an "Open Community"
is a critical and required component of an OpenStack project. Open communities
enable an open development environment, and are a key component of a successful
open source project. An OpenStack project should follow this model to ensure
its success and growth in the OpenStack ecosystem.


.. _irc-meetings:

Public Meetings on IRC
======================

OpenStack projects are required to have their team meetings (if any) on the
OFTC :term:`IRC` network in one of the publicly logged
channels managed by the OpenStack infrastructure team. As part of
working in an open community, the logging of meetings allows those who
cannot participate at the meeting's designated time to read the logs
and participate asynchronously.

The OpenStack infrastructure team maintains a limited number of channels
dedicated to meetings. While teams can hold meetings on their own team IRC
channels, they are encouraged to use those common meeting channels to give
their meeting some external exposure. The limited number of meeting channels
encourages teams to spread their meetings around and reduce conflicts.

To schedule a team meeting, please go to the eavesdrop_ site and follow the
instructions there. All meeting reservations are managed through gerrit
in the `IRC meetings`_ repository.


.. _irc-channels:

Project IRC Channels
====================

OpenStack projects may have a team IRC channel on OFTC_. These channels
`should be logged`_, in keeping with the open community aspects of OpenStack.
Project teams typically congregate in their IRC channels to discuss the project
and build a sense of community amongst members. If a new project doesn't yet
have critical mass, the channel #openstack-dev on OFTC is available to use
until such time as the project team decides to create their own channel.

Given the fact open source in general, and OpenStack in particular, are global
communities, IRC is a great way for the geographically disparate teams to work
together. Usage of IRC proxies (or bouncers) allow team members to access
messages that were sent while they are disconnected.


Mailing Lists
=============

OpenStack makes use of `mailing lists`_ for communication. Much like IRC,
mailing lists are used to allow geographically distributed teams to communicate
and share information, in this case asynchronously. In addition to team
communication, mailing lists also provide an interaction point for
cross-project communication. If an idea needs to span projects, the mailing
lists is a great place for this to happen. And finally, mailing lists are used
to interact with non-developer community members of OpenStack.

Because mailing lists are a form of asynchronous communication, they are the
best resource the community has for sharing information with, and getting ideas
from, the entire community. People who participate at different times of the
day or less regularly have a greater opportunity to engage. People who read or
write English with different levels of proficiency can be on a more (but not
perfectly) level playing field with email than they might be with synchronous
forms of text-based communication, such as IRC. When in doubt, prefer using a
mailing list to other options.

For the openstack-discuss_ mailing list, where most usage, operations and
development discussions take place, the following tags are recommended to
better categorize topics (add as many to the beginning of the subject line as
are relevant):

* ``[$groupname-wg]`` Discussion within $groupname working group (example:
  `[publiccloud-wg]`)
* ``[$projectname]`` Discussion affects the $project project team (example:
  `[nova]`)
* ``[$signame-sig]`` Discussion within $signame SIG (example:
  `[upgrades-sig]`)
* ``[all]`` Topic is a general community discussion affecting everyone. Use
  with care.
* ``[dev]`` Discussion is specific to development concerns, but otherwise
  affects all devs
* ``[elections]`` Election-related communications
* ``[docs]`` Any kind of documentation discussions that are cross-projects
* ``[goals]`` Affects community goals
* ``[ops]`` Discussion is specific to operators concerns, but otherwise
  affects all ops
* ``[ptl]`` Topics needing the attention of PTLs
* ``[release]`` Affects the upcoming release, all PTLs or release liaisons
  should read
* ``[tc]`` Discussion around Technical Committee activities
* ``[uc]`` Discussion around User Committee activities

There are many mailing lists in the OpenStack ecosystem. Projects should ensure
they have subscribers on all of the lists relevant to their project.

Note that communications on OpenStack mailing-lists follow basic mailing-list
`etiquette rules`_. Getting familiar with those will make sure your messages
will have the desired impact.


Community Support Channels
==========================

As a project in the OpenStack ecosystem, you will inevitably field requests for
support from users of your software. These can come in the following ways:

* Bugs on Launchpad_ or StoryBoard_
* Mailing list requests
* IRC message requests

A project must be prepared to provide best effort support for these types of
requests. Recommended courses of action include:

* Triaging bugs (on Launchpad_ or StoryBoard_) at least weekly.
* Responding to project queries on the various mailing lists.
* Working in-channel on IRC to answer questions.


Planet OpenStack
================

`Planet OpenStack`_ is a collection of thoughts from developers and other key
players in the OpenStack project ecosystem. If your project contains active
bloggers, it's a good idea to make sure their blogs are listed here. This is a
nice way to provide an avenue of communication for developers in your project
to share their thoughts and ideas around the work they are doing throughout
the cycle.

To list your blog in the Planet OpenStack aggregator, follow the steps outlined
on the wiki for `adding your blog`_.


.. _ptl-duties:

Technical Committee and PTL Elections
=====================================

All OpenStack technical leadership positions are elected. There are two types
of elected technical positions in OpenStack:

* Technical Committee (TC)
* Project Team Lead (PTL)

The *project team* guide naturally focuses on PTLs. More information about the
TC can be found on the `Technical Committee website`_. You can reach out to
TC members using the openstack-discuss_ mailing-list (including the ``[tc]``
"tag" in your subject line will make it more likely for them to see the
message), or on the #openstack-tc IRC channel (especially around
`TC office hours`_).

Each project team in OpenStack needs a PTL. The PTL is an elected leader who
has final say over all things in that specific project team, and all the code
repositories in it. The PTL typically leads the day to day operations of the
project, and acts as a default ambassador of the project team in communications
with other teams. The PTL is expected to have sufficient time available to
dedicate to running the project. Responsibilities of the PTL include the
following tasks:

* Organizing the team participation to events like the Forum or Project Teams
  Gatherings
* Interacting with the release team in the #openstack-release IRC channel
* Engaging with and tracking cross-project initiatives, including
  `OpenStack-wide goals`_.
* Maintaining cycle and development milestone plans. The dates for milestones
  and releases are `posted`_ well in advance, make sure you have sufficient
  free time on those special weeks.
* Targeting and maintaining targeted bugs
* Working with the release team on milestone delivery week, feature freeze,
  release candidate weeks, and final release week
* If an unexpected event occurs that doesn't give you sufficient time to
  dedicate to the items above, it is your responsibility to step down and allow
  someone with more time to take over.

The PTL for each project team is elected on a 6-month term. Thus, the project
will have an election every 6 months to determine the leader of the project
for the upcoming 6-month cycle.

Projects without any nominated PTL candidates during a specified period will be
considered leaderless and default to the technical committee for `decision`_.

The electorate for elections (both PTL and TC) are the active contributors
to a project or projects. If your project is a git repository and all active
contributors submit patches to gerrit, their work will be automatically
acknowledged for elections. Should you have any contributors who support
your project in a way not reflected in gerrit, edit the extra-atcs file
in the openstack/governance repo.

OpenStack uses a Condorcet_ voting system for all Technical elections. This
includes both the TC as well as PTL positions. The elections are run by a
trusted team of election officials from the community who make election
announcements throughout the process, set up the election tooling and oversee
candidate and voter eligibility.

Tie Breaking
------------

Condorcet may result in ties, which should be broken in a fair and reproducible
manner. To this end, OpenStack uses the hash of a string describing the tie
results in a seed in a random generator to determine the tie winners. This way
anyone may verify the fairness of the tie break. For more details, see the
wiki page on `tie breaking`_.

Election Schedule
-----------------

The `Technical Committee charter
<https://governance.openstack.org/tc/reference/charter.html>`__
defines the rules for the election schedule. Dates are generally based on the
release cycle (for PTL elections) and summit dates (for the TC elections).


.. _should be logged: https://governance.openstack.org/tc/reference/irc.html
.. _etiquette rules: https://wiki.openstack.org/wiki/MailingListEtiquette
.. _Launchpad: https://launchpad.net/openstack
.. _StoryBoard: https://storyboard.openstack.org
.. _Technical Committee website: https://governance.openstack.org/tc/
.. _TC office hours: https://governance.openstack.org/tc/#office-hours
.. _Condorcet: https://en.wikipedia.org/wiki/Condorcet_method
.. _tie breaking: https://wiki.openstack.org/wiki/Governance/TieBreaking
.. _eavesdrop: http://eavesdrop.openstack.org/
.. _IRC meetings: http://git.openstack.org/cgit/openstack-infra/irc-meetings/tree/
.. _OFTC: https://www.oftc.net/
.. _mailing lists: http://lists.openstack.org/cgi-bin/mailman/listinfo
.. _Planet OpenStack: http://planet.openstack.org/
.. _posted: http://releases.openstack.org
.. _decision: https://governance.openstack.org/tc/resolutions/20141128-elections-process-for-leaderless-programs.html
.. _adding your blog: https://wiki.openstack.org/wiki/AddingYourBlog
.. _Openstack-wide goals: https://governance.openstack.org/tc/goals/index.html
.. _openstack-discuss: http://lists.openstack.org/cgi-bin/mailman/listinfo/openstack-discuss
