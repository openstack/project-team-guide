=========================================
Technical Guidance for OpenStack projects
=========================================

This chapter contains basic technical guidelines to help you design and set
up an OpenStack project.

This content is aspirational, and not prescriptive - while working towards a
universally consistent technical design is laudable, it is often impractical,
given technical debt, architectural decisions, and project constraints.


Base services
-------------

OpenStack services rely on external services to deliver their functionality.
Base services are external services that OpenStack components can assume
will be present in any OpenStack deployment. OpenStack components may
therefore leverage advanced features exposed by those base services without
needing to reimplement the wheel locally.

The Technical Committee maintains the `list of base services`_.

The Technical Committee constantly considers new additions to the list,
conservatively weighing the benefit of that service against the additional
operational cost for OpenStack deployers.

.. _list of base services: https://governance.openstack.org/tc/reference/base-services.html


API guidelines
--------------

We strive to improve the experience of API end users by converging the
OpenStack API to a consistent and pragmatic RESTful design. The
`API SIG`_ maintains a `set of API guidelines`_ to that effect.

.. _API SIG: http://specs.openstack.org/openstack/api-wg

.. _set of API guidelines: http://specs.openstack.org/openstack/api-wg/#guidelines


Best practices for project setup
--------------------------------

OpenStack has many projects, so many that contributor usability is a
serious concern. The harder it is to get from a fresh source tree to a
contribution, the more difficult it becomes to develop a complex,
multi-service feature. A consistent project setup reduces this friction, both
for new contributors and for seasoned ones.

This section addresses common tools, libraries, and approaches, used in the
OpenStack ecosystem. It begins where the `Consistent Testing Interface`_
ends, by explaining guidelines, best practices, lessons learned, and answers
to frequently asked questions.


Avoid multi-language projects
=============================
Every programming language has established solid, reliable tooling for
itself, based on assumptions that are relevant only to itself. These
assumptions rarely translate well between different languages, and thus any
attempt to manage both in the same repository inevitably leads to one set of
tools winning out over the other.

The result of this is that the 'primary' language tooling is often extended to
provide features - such as dependency resolution, testing harnesses, and
resource management - not usually available. This presents two problems:
Firstly, a maintenance requirement is imposed, as these tools must be
maintained. Secondly, an educational requirement is imposed, as engineers
seeking to do work on the secondary language must become familiar with an
entirely unfamiliar set of tooling.

It is far easier, and less fragile, to treat each language as its own
project, while providing documentation that explains how these projects are
integrated.

Subscribe to Global Dependencies
================================
OpenStack maintains a list of global dependencies, and their version
constraints, in order to ensure that peer libraries operate as expected. This
is done in an as-needed basis: If your project declares that it is dependent
on a library, it will automatically receive patch requests whenever that
particular library is updated. This also permits us to globally freeze our
dependencies in anticipation of a release.

Follow language-specific Guides
===============================

For a guide for a specific language, please choose the appropriate article
below.

.. toctree::
   :maxdepth: 1

   Python<project-setup/python>
   JavaScript<project-setup/javascript>

.. _Consistent Testing Interface: https://governance.openstack.org/tc/reference/project-testing-interface.html
