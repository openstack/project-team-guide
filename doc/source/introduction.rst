==========================================
 Introduction: A Bit of OpenStack History
==========================================

The origin
==========

OpenStack was created during the first months of 2010. Rackspace wanted to
rewrite the infrastructure code running its Cloud servers offering, and
considered open sourcing the existing Cloud files code. At the same time,
Anso Labs (contracting for NASA) had published Nova.cc, a Python-based
compute infrastructure-as-a-service proof-of-concept.

Both efforts converged and formed the base for OpenStack. The first Design
Summit was held in Austin, TX mid-July 2010, and the project was officially
announced at OSCON July 21st, 2010.


The mission
===========

The OpenStack mission is "to produce the ubiquitous Open Source Cloud Computing
platform that will meet the needs of public and private clouds regardless of
size, by being simple to implement and massively scalable".

It appeared on the wiki on May 24th, 2010 and still captures the long-term goal
of the OpenStack community.


The Four Opens
==============

The best short definition of "the OpenStack Way" is the four opens. The
original version of these appeared June 28th, 2010 on the wiki.

In the following chapters, we'll further elaborate on those basic principles
and explain more precisely what they mean for OpenStack project teams.

Open source
-----------

We do not produce "open core" software.

We are committed to creating truly open source software that is usable and
scalable. Truly open source software is not feature or performance limited
and is not crippled. There will be no "Enterprise Edition".

We use the Apache License, 2.0.

* OSI approved,
* GPLv3 compatible
* DFSG compatible

Open design
-----------

We are committed to an open design process. Every six months the development
community holds a design summit to gather requirements and write specifications
for upcoming release. The design summits, which are open to the public, include
users, developers, and upstream projects. We gather requirements and produce
an approved roadmap used to guide development for the next six months.

The community controls the design process. You can help make this software
meet your needs.

Open development
----------------

We maintain a publicly available source code repository through the entire
development process. We do public code reviews. We have public roadmaps.
This makes participation simpler, allows users to follow the development
process and participate in QA at an early stage.

Open community
--------------

One of our core goals is to maintain a healthy, vibrant developer and user
community. Most decisions are made using a lazy consensus model. All processes
are documented, open and transparent.

The technical governance of the project is a community meritocracy with
contributors electing technical leads and members of the Technical Committee.

All project meetings are held in public IRC channels and recorded.


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

In September 2012, the OpenStack Foundation was launched as an independent
body providing shared resources to protect, empower, and promote OpenStack
software and the community around it.

The reponsibilities of the Project Policy Board were split between two bodies:

* The Foundation board of directors, which defines the objectives of the
  OpenStack Foundation, controls how the Foundation budget is spent, and
  has authority on the OpenStack trademark

* The Technical Committee, which manages the technical matters and has
  authority over the open source upstream OpenStack Project

The Technical Committee was originally formed by all the PTLs + five members
directly elected by all the contributors. In June 2013, to accommodate the
growth in the number of project teams and PTLs, the Technical Committee
decided to switch to 13 directly-elected members instead. Half of those are
renewed every 6 months.

