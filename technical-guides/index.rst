================
Technical Guides
================

This chapter contains basic technical guidelines to help you design and set
up an OpenStack project.

This content is aspirational, and not prescriptive - while working towards a
universally consistent technical design is laudable, it is often impractical,
given technical debt, architectural decisions, and project constraints.

Using Unified Limits
--------------------
.. toctree::
   :maxdepth: 2

   unified-limits

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
.. toctree::
   :maxdepth: 2

   best-practices-for-project-setup
