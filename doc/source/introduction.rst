==========================================
 Introduction: A Bit of OpenStack History
==========================================

The origin
==========

OpenStack was created during the first months of 2010. Rackspace wanted to
rewrite the infrastructure code running its Cloud servers offering, and
considered open sourcing the existing Cloud files code. At the same time,
Anso Labs (contracting for NASA) had published beta code for `Nova`_, a
Python-based "cloud computing fabric controller".

Both efforts converged and formed the base for OpenStack. The first Design
Summit was held in Austin, TX on July 13-14, 2010, and the project was
officially announced at OSCON in Portland, OR, on July 21st, 2010.

.. _Nova: https://web.archive.org/web/20100620230941/http://novacc.org/


The mission
===========

The OpenStack mission is "to produce a ubiquitous Open Source Cloud Computing
platform that is easy to use, simple to implement, interoperable between
deployments, works well at all scales, and meets the needs of users and
operators of both public and private clouds".

It was updated in February of 2016 to include interoperability and better
serving end users.

The original mission was "to produce the ubiquitous Open Source Cloud Computing
platform that will meet the needs of public and private clouds regardless of
size, by being simple to implement and massively scalable". It originally
appeared on the wiki on May 24th, 2010.


The Four Opens
==============

The best short definition of "the OpenStack Way" is the four opens as
defined in the governance document approved by the Technical
Committee:

https://governance.openstack.org/tc/reference/opens.html

These were further refined in a set of guiding principles that apply to all
OpenStack projects:

https://governance.openstack.org/tc/reference/principles.html

In the following chapters, we'll further elaborate on those basic principles
and explain more precisely what they mean for OpenStack project teams.


A quick history of OpenStack governance
=======================================

Original governance
-------------------

The original project governance defined three main bodies: the Advisory
Board, the Architecture Board and Technical Committees for each sub-project.

This was quickly replaced early 2011 by the Project Oversight Committee,
which consisted of a mix of elected and Rackspace-appointed members. PTLs
were appointed by Rackspace too.

The governance model was once again tweaked in March 2011. The Project
Oversight Committee was renamed to Project Policy Board (still a mix of
appointed and elected members), and PTLs were elected by the contributors
to their project for the first time.

The OpenStack Foundation
------------------------

In September 2012, the `OpenStack Foundation`_ was launched as an independent
body providing shared resources to protect, empower, and promote OpenStack
software and the community around it.

The responsibilities of the Project Policy Board were split between two bodies:

* The Foundation `Board of Directors`_, which defines the objectives of the
  OpenStack Foundation, controls how the Foundation budget is spent, and
  has authority on the OpenStack trademark

* The `Technical Committee`_, which manages the technical matters and has
  authority over the open source upstream OpenStack Project

The Foundation bylaws also established a third body, the `User Committee`_,
to more accurately reflect the views and needs of the users of OpenStack.
Since June, 2020 `User Committee`_ has been `merged
<https://review.opendev.org/c/openstack/governance/+/734074>`_
into the `Technical Committee`_ and is not a separate body anymore.

The Technical Committee was originally formed by all the PTLs + five members
directly elected by all the contributors. In June 2013, to accommodate the
growth in the number of project teams and PTLs, the Technical Committee
decided to switch to 13 directly-elected members instead. Half of those are
renewed every 6 months.

The Project structure reform (a.k.a. the 'big tent')
----------------------------------------------------

One of the prerogatives of The Technical Committee (and its predecessors) is
to define what is "an OpenStack project" from an upstream, open source project
perspective. OpenStack started with two projects, but as their functionality
was refactored and as our community grew, new projects were added.

Requirements for new projects evolved over time. End of 2012 we introduced
the concept of incubation, to be able to grow new projects for inclusion in
"OpenStack". However, requirements based on maturity created a catch-22, as
projects had trouble attracting enough contributors until they were
recognized as official. Concerns around the size of the "integrated
release" also resulted in artificially excluding a lot of people from
the OpenStack community.

In December 2014, the Technical Committee introduced a
`Project structure reform`_ (dubbed the 'big tent') that moved to a
community-centric definition of 'OpenStack'. Its premise was that teams
that follow the OpenStack principles, use our development model and have
a scope compatible with the OpenStack mission should not be excluded from the
OpenStack community. They can apply to become official OpenStack project
teams: if approved they are placing themselves under the OpenStack governance
rules, and their deliverables are considered OpenStack projects.

.. _OpenStack Foundation: http://www.openstack.org/foundation/
.. _Board of Directors: http://www.openstack.org/foundation/board-of-directors/
.. _Technical Committee: https://governance.openstack.org/tc/
.. _User Committee: https://governance.openstack.org/uc/
.. _Project structure reform: https://governance.openstack.org/tc/resolutions/20141202-project-structure-reform-spec.html
