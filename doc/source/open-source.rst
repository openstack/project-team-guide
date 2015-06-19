=============
 Open Source
=============

One of the "Four Opens" which frame the basis for an OpenStack project is
Open Source. OpenStack projects do not produce "open core" software, they
instead produce open source software. There is a subtle but important
difference between the two, but the difference is part of the foundation of
OpenStack. In addition, the software is produced with a community and
contributor accepted license.

Fully Functional Open Source
============================

When we say "Fully Functional Open Source", what we mean is OpenStack projects
should not be producing something which requires an enterprise version
or some additional proprietary software. OpenStack is producing pure open
source projects through it's software. Functionality should not be limited
in any way. We strive to make things scalable and efficient in an open source
manner.

In a similar manner, projects may choose to enable pluggable functionality
which allows third party projects and vendors to integrate. This type of
architecture is a workable solution. An open source option, whether that is
a built in solution or a third party pluggable component, is required to
ensure the "fully functional open source" principle is satisfied.

Acceptable Licensing
====================

OpenStack projects must utilize the `Apache License, 2.0`_ for the source code
they produce. This is the license used by all existing OpenStack software.
New projects are required to utilize this license as well, as it's specifically
called out in the OpenStack Foundation bylaws. The Apache License, 2.0,
provides the following benefits:

* OSI approved
* GPLv3 compatible
* DFSG compatible

Dependencies and Optional Modules
=================================

When utilizing third party modules or libraries which are not Apache 2.0
licensed, contributors need to understand how the interaction between the
modules will work. If there are doubts or concerns, it is recommended to put
an item on the `Technical Committee Meeting`_ agenda to discuss with the
Technical Committee how to proceed. In general, err on the side of caution
here.

With regards to dependencies, any third party libraries or modules need to be
vetted through the `requirements team`_. This ensures the added requirement of
including the third party module goes through review and will not conflict
with broad cross-project efforts, such as the Python 3 porting effort.

.. _Apache License, 2.0: http://www.apache.org/licenses/LICENSE-2.0
.. _Technical Committee Meeting: https://wiki.openstack.org/wiki/Meetings/TechnicalCommittee
.. _requirements team: https://git.openstack.org/cgit/openstack/requirements
