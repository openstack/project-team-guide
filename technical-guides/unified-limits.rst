====================
Using Unified Limits
====================

This document contains guidelines and rules for OpenStack projects
regarding the usage of Keystone's unified limits functionality. The
items laid out here are based on existing real-world implementations
and are provided in order to help ensure consistency among other
projects moving to this model. While this document assumes the reader
understands the content of the `keystone unified limits`_
documentation some concepts are covered here again for consistency.

Overview
========

Keystone provides a "limit" functionality which is effectively a
central repository of named numerical resource maximums tied to
projects. Each resource limit is "registered" with keystone and given
a default value which can be overridden per-project with different
values.

Existing in-project quota APIs generally do not have a clean-up
process when projects are deleted from Keystone. Further, some also do
not verify project identifiers when recording new limits which leads
to further cruft. Implementing any sort of complex hierarchy or
inheritance would require a lot of information about project
relationships which are Keystone's responsibility.

Why store this in Keystone?
---------------------------

Keystone owns the definition of a "project" and as such projects are
created and destroyed in Keystone. Since limits (i.e. quotas) are tied
to a project, it can be difficult for other services (i.e. Nova,
Glance, Cinder, Neutron) to avoid stale limit cruft from accumulating
in their databases since they do not get a notification when a
project is deleted.

Storing limit information in Keystone means that limits can be cleaned
up along with projects, but also that rich information about
hierarchies can be applied to the calculation of dynamic limits. It
places the resource limit closer to the "primary key" of what is being
limited.

So Keystone implements quotas?
------------------------------

Keystone does not implement quotas, but implements limits. For the
purposes of this document, consider a quota to be a tuple of "a limit
on a resource" and "the amount of resource currently being
consumed". The quota is enforced by comparing the amount currently
used to the amount allowed at the time that a new resource allocation
attempt is made. For example, a service like Cinder implements a
quota on disk usage when a new allocation is requested by comparing
current allocations to the limit before deciding if the request
should be granted.

Keystone does not store or concern itself with the amount of resource
currently allocated it only stores the limit: the maximum amount
allowed. It is up to the service that owns the resource to do the
checking of consumption against the limit to decide whether a resource
allocation should be allowed. An Oslo library interface is provided to
make this easy.

Is this only relevant for allocate-able or quota-able resources?
----------------------------------------------------------------

An important thing to note on the topic of quotas is that all quotas
involve a limit, but not all limits imply a quota. Limits can be used
for non-quota things such as "the maximum number of metadata items
per instance" or "the maximum number of parallel upload
requests". These cases have typically been implemented with static
configuration values which apply to all users of a service. By
implementing these limits via Keystone it becomes possible to
override specific limits per project.

The players involved
--------------------

In order to implement limits in Keystone, the `oslo.limit`_ library is
provided, and should be used by consumers to do so. This library
provides an interface to efficiently query limits from Keystone and
generally provides a quota-driven workflow that will directly apply
in most cases. Limits queried via the limit library must be registered
before use and thus most projects will need devstack changes to make
sure that registration happens.

Guidelines
==========

Caching
-------

Since looking up limits in Keystone involves making a network request,
load on the Keystone server and network can be increased. This lends
itself to efforts to cache results, but such efforts should be limited
in scope. It is best to avoid caching individual results for longer
than the scope of a single request to avoid breaking the ability of
an administrator to lower or raise a limit and see immediate results.

An example of a longer-lived result cache that is still limited to a
single request is the Glance image upload scenario. When consuming an
unbounded image stream we need to look up the limit once and apply
that limit to the stream (or result) without hitting Keystone once per
block read or anything near as frequent. The lifetime of the cached
value is limited to the single upload request.

Use the Enforcer
----------------

The `oslo.limit`_ library provides an Enforcer class for quota-like
behaviors which should be used whenever possible. This class allows a
batch-driven approach where multiple named resources are presented
with functions that calculate current usage which are then compared
against the limits to determine whether or not a resource allocation
operation should be allowed. It may be tempting to just look up the
limits and perform the enforcement in a different way (and in some
cases there may be no alternative), but the standard enforcement
routines should be used when possible. This helps to ensure consistent
application of "current + requested >= limit" behavior, as well as
standardized error messages.

Additionally, the Enforcer makes an attempt to coerce the user into a
batch-oriented approach to save on round trips to Keystone. The
consumer of the library should attempt to conform to this behavior
whenever possible.

Counting current usage
----------------------

In some OpenStack projects, early quota implementations focused on a
separate accounting record for quota limits and current usage. These
tables were intended to exactly track actual usage, as well as
temporary reservations to provide air-tight quota enforcement. In
practice, they usually drifted out of sync with reality and required
manual cleanup and adjustment. A number of projects have moved towards
a goal of orienting data structures in a way that they can be easily
and efficiently counted at runtime to determine current usage. The
tradeoff between being off-by-one in quota enforcement and potentially
wildly out of sync with actual consumption has generally preferred the
former. Indeed, the Enforcer pattern in `oslo.limit`_ receives
functions, which are called to calculate current usage, and it is
recommended that the implementations of these do that calculation live
instead of mere lookups in accounting tables.

Preserve standardized error messages
------------------------------------

The `oslo.limit`_ library provides an Enforcer class and standardized
error messages for cases where limits are exceeded. These take a
common form of explaining which (named) limit would be exceeded, what
the limit is, as well as the current usage and allocation amount
attempted. Projects that currently implement quotas will be tempted to
preserve their own error messages for compatibility reasons, but it is
recommended instead to move towards using the standardized
messages. This will help consistency across projects and lead to
better understanding of errors by users and operators.

All or none
-----------

It is recommended that if any unified limits are to be applied, then
all of them should be applied. Meaning, it is okay to allow for
enabling or disabling quota/limit checking entirely, or to enable
"internal limits" vs. "limits in keystone" for projects
transitioning. However, it is recommended that if any limits in
Keystone are used, then all of them are. Disabling individual quotas,
or allowing some quotas to be enforced by limits in keystone and other
from internal/legacy sources is confusing. Resource limits in keystone
can be set to "unlimited" and it is recommended to use such a limit
instead of a boolean enable/disable flag for individual items.

Usage APIs
----------

Projects with existing quota systems likely already expose APIs for
their users to examine their limits and usage for resources. These
APIs should be maintained for exposing usage and limit information but
pull the limit information from Keystone if unified limits are
used. They should *not* attempt to maintain full compatibility with
quota (limit) management by proxying back to Keystone. These APIs
should, in most cases, become read-only (at least with respect to
limits) when unified limits are in use. Projects that do not have
legacy APIs for this purpose should implement them as they are
necessary for proper user behavior.

Keystone does not know about resource consumption and thus cannot
provide users information about it. Further, resource limit
information is not necessarily something users are permitted to see by
talking to Keystone directly. Even if they could, requiring them to
look up their limits in one place and their consumption on another is
not very friendly. Thus, it is recommended that projects implement
quota/usage APIs that provide limits and consumption information in
one place.

For an example of a very simple usage API, check the `Glance
implementation`_. In the absence of existing APIs or a compelling
reason to do something different, this API should serve as a template
for new usage APIs for consistency.

Migration to unified limits
---------------------------

Projects with existing internal quota systems that are migrating
should provide a configuration flag to switch between "internal" and
"unified" limits. Projects that are growing new quota functionality
with only unified limits from the start may also want a configuration
flag to enable or disable quota checking entirely to provide a
graceful on-ramp.

The following general set of steps may be helpful in planning the
work:

#. Add a configuration control to allow switching between existing
   quota systems and unified limits (or enabling unified limits if
   none exists).
#. Refactor existing or implement new quota checking code to pull
   limits from Keystone via oslo.limit if enabled during resource
   allocation requests.
#. Refactor existing or implement new quota usage APIs to expose
   information about current resource consumption and limits.
#. Extend devstack to support defining resource limits and sane
   defaults for test jobs for your project.
#. Add or extend existing tempest tests to validate quota enforcement
#. Document the functionality for your users, including information
   about if/when existing quota functionality will be removed, as well
   as instructions about migrating existing limits into Keystone.


.. _keystone unified limits: https://docs.openstack.org/keystone/latest/admin/unified-limits.html
.. _oslo.limit: https://docs.openstack.org/oslo.limit/latest/
.. _Glance implementation: https://docs.openstack.org/api-ref/image/v2/index.html#quota-usage
