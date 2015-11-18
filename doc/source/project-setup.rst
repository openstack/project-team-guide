=============
Project Setup
=============

OpenStack has many projects, so many that contributor usability is a
serious concern. The harder it is to get from a fresh source tree to a
contribution, the more difficult it becomes to develop a complex,
multi-service feature. A consistent project setup reduces this friction, both
for new contributors and for seasoned ones.

This chapter addresses common tools, libraries, and approaches, used in the
OpenStack ecosystem. It begins where the `Consistent Testing Interface`_
ends, by explaining guidelines, best practices, lessons learned, and answers
to frequently asked questions.

This content is aspirational, and not prescriptive - while working towards a
universally consistent project setup is laudable, it is often impractical,
given technical debt, architectural decisions, and project constraints.

Best Practices
--------------

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

Language-specific Guides
------------------------

For a guide for a specific language, please choose the appropriate article
below.

.. toctree::
   :maxdepth: 1

   Python<project-setup/python>
   JavaScript<project-setup/javascript>

.. _Consistent Testing Interface: http://governance.openstack.org/reference/project-testing-interface.html
