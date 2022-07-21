================
Legal Issues FAQ
================

The `legal-discuss@lists.openstack.org
<http://lists.openstack.org/cgi-bin/mailman/listinfo/legal-discuss>`_
mailing list is a forum for questions that have a legal aspect to them. These
questions may concern (for example) licensing, third party packages,
contributor agreement questions and trademark issues. The list will be used to
build into an ad-hoc knowledge base in the form of this FAQ about those thorny
legal issues that most commonly affect the OpenStack project.

Opinions documented here do not constitute legal advice from the OpenStack
Foundation or anyone else.

Frequently Asked Questions
==========================

.. contents::
   :depth: 1
   :local:
   :backlinks: none

NOTICE Files
------------

Q: Should we include NOTICE files in OpenStack projects?

A: If a NOTICE file exists in a project, the Apache License requires that
derivative works include the attribution notices from the file. This could be
helpful for a number of purposes - (a) ensuring an attribution to the OpenStack
project gets included in derivative works and (b) helping distributors of
derivative works to include any required attribution notices for third party
code included in the project. However, neither of these issues are deemed
significant or important enough to warrant the cost of maintaining the files or
requiring distributors to include an OpenStack project attribution notice.

For the full background, `see this thread
<http://lists.openstack.org/pipermail/legal-discuss/2013-April/thread.html#0>`_.

.. _incorporating:

Incorporating BSD/MIT Licensed Code
-----------------------------------

Q: If we include BSD or MIT licensed code in an OpenStack project, how best
should we comply with the terms of the license?

A: The `2 core clauses in the BSD license
<http://en.wikipedia.org/wiki/BSD_licenses#2-clause_license_.28.22Simplified_BSD_License.22_or_.22FreeBSD_License.22.29>`_
make some demands about the retention of copyright notices, the license and
disclaimers in both source and binary distributions of the code. The
`MIT license
<http://opensource.org/licenses/MIT>`_
contains similar requirements. Probably the easiest thing to do when
incorporating BSD or MIT licensed code is to copy the copyright/license header
from the source file into the destination file, as well as copying the
copyright notice, license and disclaimer into the toplevel LICENSE file with a
brief explanation of which code is under that license.

For the full background, `see this email
<http://lists.openstack.org/pipermail/legal-discuss/2013-April/000002.html>`_.

Copyright Headers
-----------------

Q: Generally speaking, what's the idea with copyright headers in source files
and what should be included in them?

A: Copyright notices are not required in order to create or protect your
copyright rights, but they may nonetheless be useful. (Also, copyright notices
from code taken from outside of OpenStack will typically
:ref:`need to be preserved <incorporating>`
as a requirement of the license of that external code.) We have some overall
copyright guidance on this page, and discussions about copyright headers in
source files continue on mailing lists. In general,

* All references to "OpenStack LLC" can be changed to "OpenStack Foundation"
  because copyrights held by OpenStack LLC were transferred to the Foundation
  when the new entity was created. (However, note that in some cases "OpenStack
  LLC" or "OpenStack Foundation" appear to have been included in copyright
  notices `in error
  <http://lists.openstack.org/pipermail/legal-discuss/2013-April/000010.html>`_
  by contributors not employed by Rackspace or the Foundation at the relevant
  time period.)

* If the content has been substantially updated in 2013, add the year to the
  change.

* Always keep the license in the header.

* We do not yet have guidance for when to add or remove a copyright header in
  source files.

* Reviews of copyright headers may vary across projects.

* For documentation, refer to `Documentation/Copyright
  <https://wiki.openstack.org/wiki/Documentation/Copyright>`_.

Note that the combination of our use of a CLA and per-file copyright notices
from individuals or companies may be creating a confusing
`duplicative licensing situation
<http://lists.openstack.org/pipermail/legal-discuss/2014-January/000150.html>`_.

Q: Should **"All rights reserved"** follow a copyright notice?

A: It is not necessary to follow a copyright notice with the words "All rights
reserved". While it is harmless, some people regard "All rights reserved" as
being inappropriate in conjunction with an open source license grant. Therefore
it is recommended that developers not include "All rights reserved" in
copyright headers.

Copyright Notices in Blueprints
-------------------------------

Q: Should I include a copyright notice in a blueprint?

A: `No
<http://lists.openstack.org/pipermail/legal-discuss/2013-May/000039.html>`_.

OpenStack Foundation Copyright Headers
--------------------------------------

Q: Should I Include an OpenStack Foundation copyright header in my code?

A: **No**, unless you are an employee or contractor of the OpenStack
Foundation. Most existing OpenStack Foundation copyright headers you see in
OpenStack code are likely associated with code from Rackspace developers before
the OpenStack Foundation existed when OpenStack LLC was a wholly owned
subsidiary of Rackspace. Once the foundation was formed, all OpenStack LLC
assets (including copyrights) were transferred to the foundation and the
copyright headers were updated. It's likely the only valid reason for OpenStack
Foundation copyright notices on new code these days is where the code was
authored by an employee or contractor of the foundation.

For the full background, `consult this thread
<http://lists.openstack.org/pipermail/legal-discuss/2013-April/thread.html#9>`_.

New Project Names
-----------------

Q: What sort of things should I bear in mind when choosing a new project name?

A: Some of the non-Apache-specific material in the Apache Software Foundation's
guidelines `Choosing names for ASF projects
<http://www.apache.org/dev/project-names.html>`_
may be helpful. See also the last
paragraph of `section 5.1
<https://softwarefreedom.org/resources/2008/foss-primer.html#x1-600005>`_
of SFLC's *A Legal Issues Primer for Open Source and Free Software Projects*.

A few categories that are best avoided when coming up with a non-generic
project name are: surnames, ubiquitous words, famous trademarks in other
fields, and references to famous things (e.g., superheroes, car names, movie
characters, famous people).

Legal Concerns Over Project Names
---------------------------------

Q: I am concerned that there may be legal issues around an existing OpenStack
project name. What should I do?

A: FIXME

Licensing of library dependencies
---------------------------------

Q: Is it OK for OpenStack Projects to use GPL or AGPL libraries?

A: No, see the `Licensing requirements
<http://governance.openstack.org/reference/licensing.html>`_
page.

.. note::

   This question is about GPL libraries. LGPL libraries would not require
   such discussion.

Licensing of non-library dependencies
-------------------------------------

Q: Is it OK for OpenStack Projects to require AGPLv3 licensed technologies for
production deployment?

A: This issue has come up in the context of Ceilometer and Marconi requiring
MongoDB. The concern has been raised that some users will be unwilling to
deploy any AGPLv3 technologies, but we're still trying to understand those
concerns in detail. See `this thread
<http://lists.openstack.org/pipermail/legal-discuss/2014-March/thread.html#174>`_.
