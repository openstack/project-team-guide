=============
 Open Source
=============

One of the "Four Opens" which frame the basis for an OpenStack project is
Open Source. OpenStack projects do not produce "open core" software, they
instead produce purely open source software. In addition, the software is
produced with a community and contributor accepted license.


Fully Functional Open Source
============================

When we say "fully functional open source", what we mean is OpenStack projects
should not be producing something which requires an enterprise version or some
additional proprietary software or hardware to be used at its full potential.
Functionality should not be purposefully limited in any way. We strive to make
things scalable and efficient in a fully open source manner.

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
modules will work and the compatibility of the licenses involved. If there
are doubts or concerns, it is recommended to raise the issue in the
`Technical Committee Meeting`_ to discuss with the Technical Committee how to
proceed. In general, err on the side of caution here.

With regards to dependencies, any third-party libraries or modules need to be
vetted in the `global requirements`_. This ensures the added requirement of
including the third party module goes through review and will not conflict
with broad cross-project efforts, such as the Python 3 porting effort.

.. _Apache License, 2.0: http://www.apache.org/licenses/LICENSE-2.0
.. _Technical Committee Meeting: https://wiki.openstack.org/wiki/Meetings/TechnicalCommittee
.. _global requirements: https://git.openstack.org/cgit/openstack/requirements/plain/README.rst
