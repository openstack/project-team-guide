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

The OpenStack mission is "to produce the ubiquitous Open Source Cloud Computing
platform that will meet the needs of public and private clouds regardless of
size, by being simple to implement and massively scalable".

It appeared on the wiki on May 24th, 2010 and still captures the long-term goal
of the OpenStack community.


The Four Opens
==============

The best short definition of "the OpenStack Way" is the four opens as
defined in the governance document approved by the Technical
Committee.

http://governance.openstack.org/reference/opens.html

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

The reponsibilities of the Project Policy Board were split between two bodies:

* The Foundation `Board of Directors`_, which defines the objectives of the
  OpenStack Foundation, controls how the Foundation budget is spent, and
  has authority on the OpenStack trademark

* The `Technical Committee`_, which manages the technical matters and has
  authority over the open source upstream OpenStack Project

The Technical Committee was originally formed by all the PTLs + five members
directly elected by all the contributors. In June 2013, to accommodate the
growth in the number of project teams and PTLs, the Technical Committee
decided to switch to 13 directly-elected members instead. Half of those are
renewed every 6 months.

.. _OpenStack Foundation: http://www.openstack.org/foundation/
.. _Board of Directors: http://www.openstack.org/foundation/board-of-directors/
.. _Technical Committee: http://governance.openstack.org/
