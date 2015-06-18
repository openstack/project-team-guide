======
 Oslo
======

Oslo brings together generalist code reviewers and specialist API
maintainers who share a common interest in tackling technical debt
across the OpenStack project, and in making deployment and development
experiences consistent across OpenStack projects.

OpenStack projects share many common design patterns and
implementation details. Early in the history of OpenStack this
resulted in a lot of code being copied out of one project into
another. The Oslo project was created to address this situation, and
to provide a home for common code used by multiple other OpenStack
projects.  Adopting oslo libraries makes a project more similar to the
rest of OpenStack, and that consistency in turn improves the
Operator/Deployer experience.

Libraries
=========

The Oslo team manages a `growing set of libraries of Python code`_,
either created from scratch, adopted from outside of the community, or
extracted from other OpenStack projects. Each library has its own core
review team, combining the efforts of specialists in the problem
domain and the overall Oslo core review team.

Here are a few examples of the types of libraries managed by the Oslo
team:

`oslo.config`_
  The ``oslo.config`` library provides tools for managing
  configuration option definitions, validation, configuration file
  parsing, and command line option processing.

`oslo.messaging`_
  The ``oslo.messaging`` library implements common inter-process
  communication patterns such as notifications and RPC. It includes
  drivers for different backends such as RabbitMQ, AMQP 1.0, and
  ZMQ. This pluggable backend pattern is common across OpenStack as a
  way to provide options for deployers familiar with different tool
  stacks.

`oslo.log`_
  The ``oslo.log`` library is a wrapper around Python's standard
  logging tools, coupling them with ``oslo.config`` and applying
  OpenStack-specific requirements. Projects that use ``oslo.log`` make
  it easier for deployers to set up their logging in a consistent way,
  which in turn makes it easier to post-process the logs with tools
  like Logstash.

.. _oslo.config: http://docs.openstack.org/developer/oslo.config/
.. _oslo.messaging: http://docs.openstack.org/developer/oslo.messaging/
.. _oslo.log: http://docs.openstack.org/developer/oslo.log/

Brief History
=============

The Oslo project started during the Bexar release time-frame, and grew
slowly until being granted a PTL position just before the Grizzly
cycle. During Grizzly, the team adopted the name "Oslo" (based on the
`Oslo Peace Accords`_ and "bringing peace" to the OpenStack project,
the name is not an acronym). The mission statement solidified and a
core team was built during this time, and the ``oslo.config`` library
was the first officially released Oslo library. ``oslo.messaging`` was
released next, during the Havana cycle.  After Grizzly, the amount of
incubated code continued to grow while the team developed the tools
needed to manage it. A major graduation push during the Juno and Kilo
cycles resulted in a total of 26 libraries by early in the Liberty
cycle.

.. _Oslo Peace Accords: https://en.wikipedia.org/wiki/Oslo_I_Accord

The Incubator
=============

Although the team prefers to develop new libraries from scratch as
needs arise, one important source of code for Oslo libraries is code
in an existing project that is identified as useful to another
project. The existing implementation may be tightly coupled to the
application, and need to be modified to make it more general. The Oslo
team often uses a special incubator repository to evolve the API of
the code as it is extracted from the original project, relying on
"managed copy and paste" to allow breaking changes in the API by
actually copying the results back into a small number of early
consuming projects. As the API stabilizes, the code graduates to be a
proper library.

For more details, refer to `The Oslo Incubator`_ in the Oslo team
specs repository.

Liaisons
========

There are more projects consuming code from the Oslo incubator than we
have Oslo contributors. Therefore, the Oslo team relies on "liaisons"
in each consuming project to coordinate work as new code goes through
incubation or new libraries are released. The current liaisons are
listed on the CrossProjectLiaisons_ page in the wiki, and there are
more details in the `Oslo Liaison program spec`_.

Naming Libraries
================

There are currently three naming schemes used for Oslo libraries, each
meant to signal where the library is expected to be useful.

1. Libraries used for production runtime dependencies of OpenStack
   projects follow the naming pattern ``oslo.something`` for the
   library and dist, but use ``oslo_something`` for the top level
   package name to avoid using the ``oslo.`` namespace package.

   Examples of production runtime dependencies include ``oslo.config``
   and ``oslo.messaging``.

2. Libraries used for non-production or non-runtime dependencies of
   OpenStack projects should follow the naming convention
   ``oslosomething`` (leaving out the ``.`` between "oslo" and
   "something") for the library, dist, and top level package.

   Examples of non-production dependencies include ``oslosphinx`` and
   ``oslotest``.

3. Libraries that may be generally useful outside of OpenStack, no
   matter how they are used within OpenStack, should be given a
   descriptive and unique name, without the "oslo" prefix in any form.

   Other examples of Oslo names include ``pbr`` and ``taskflow``.

For more details, refer to the Oslo `naming policy spec`_.

.. seealso::

   * `Published Oslo Specs`_
   * `Oslo in the OpenStack wiki`_
   * `Taking the Long View: How the Oslo Program Reduces Technical
     Debt`_, a presentation at the Kilo summit

.. _`Taking the Long View: How the Oslo Program Reduces Technical Debt`: https://www.openstack.org/summit/openstack-paris-summit-2014/session-videos/presentation/taking-the-long-view-how-the-oslo-program-reduces-technical-debt
.. _Published Oslo Specs: http://specs.openstack.org/openstack/oslo-specs/
.. _Oslo in the OpenStack wiki: https://wiki.openstack.org/wiki/Oslo
.. _growing set of libraries of Python code: http://governance.openstack.org/reference/projects/oslo.html
.. _The Oslo Incubator: http://specs.openstack.org/openstack/oslo-specs/specs/policy/incubator.html
.. _Oslo Liaison program spec: http://specs.openstack.org/openstack/oslo-specs/specs/policy/liaisons.html
.. _CrossProjectLiaisons: https://wiki.openstack.org/wiki/CrossProjectLiaisons
.. _naming policy spec: http://specs.openstack.org/openstack/oslo-specs/specs/policy/naming-libraries.html
