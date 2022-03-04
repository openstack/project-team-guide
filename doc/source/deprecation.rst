===========
Deprecation
===========

OpenStack projects should adhere to the guidelines outlined here
regarding deprecation of features and functionality. This process aims
to ensure that operators and users of OpenStack are able to gracefully
upgrade between releases, as well as migrate away from dependence on
functional aspects that are removed over time. Care must be taken to
avoid breaking compatibility between adjacent releases, as well as
between tick releases as defined in the TC resolution on `release
cadence`_.


Justification for careful deprecation
=====================================

End users of a given service need to know if a feature or an API they are
using and rely on will still be supported by the software tomorrow.
Operators and deployers of a given service want to be able to roll out code
and configuration changes asynchronously, and therefore rely on new code
working correctly with the existing config files.

At the early stages of development it's important to be agile, experiment,
and fail fast. At that point it's not reasonable to commit to support those
early mistakes forever. But as the project matures and gets more users that
rely on existing features, knowing under which conditions the project can
remove features, APIs or alter configuration options in the future becomes
important. It can be a factor in deciding if the project is stable and mature
enough for a specific use case.


Guidelines
==========

Project teams should follow the following process for end-user-visible
or operator-visible features deprecation:

#. Features, APIs or configuration options are marked deprecated in the code.
   Appropriate warnings will be sent to the end user, operator or library user.
   Code will be frozen and only receive minimal maintenance (just so that it
   continues to work as-is).

#. A migration path will be documented for current users of the feature. An
   email thread will be started on openstack-discuss to determine how many
   people are using the deprecated API or feature, and how costly the migration
   plan is to implement. A migration path may be "stop using that feature":
   the cost is then very related to the number of people using the feature
   and how dependent they are to that feature.

#. If the deliverable is part of an Interop Working Group Guideline, the
   project will check if the deprecated feature is part of the exposed
   capabilities. If it is, the obsolescence date (see below) additionally
   needs to take into account Interop WG capabilities deprecation schedule.

#. Based on that data, an obsolescence date will be set. At the very
   minimum the feature (or API, or configuration option) should be
   marked deprecated (and still be supported) in at least one "tick"
   release branch, and for at least 12 months of releases. Consider
   the following examples:

   #. A feature deprecated in the 2023 tick release should still
      appear (deprecated, but supported) in the 2023 tock release, and
      may be removed in the 2024 tick release. Operators deploying
      only tick releases receive one notice and have 12 months before
      the next tick where the feature is removed. Operators deploying
      tick-tock releases receive two notices and still have 12 months
      before the release where the feature is removed.

   #. A feature deprecated in the 2023 tock release must also be
      present and supported the 2024 tick release, and **must** also
      be called out as deprecated (so that operators only deploying
      tick releases receive that warning). The feature may be removed
      in the 2024 tock release. Operators deploying only tick releases
      receive one notice and have 12 months before the next tick where
      the feature is (long-since) removed. Operators deploying
      tick-tock receive two notices and still have 12 months before
      the release where the feature is removed.

Note that this delay is a required minimum. For significant features, it is
recommended that the deprecated feature appears at least in the next *two*
tick release branches. Ideally, features are deprecated and removed
only in tick releases to keep the accounting to a minimum, but there
may be cases where it is advantageous to be more flexible.

Another scenario to consider is experimental features that are
introduced in a tock release, which "don't survive" and need to be
removed quickly. In that case the earliest possible sequence would be
deprecation in the following tick (effectively introducing the feature
as deprecated to tick-only deployers) and then removal in the
following tock release.

In addition, projects should:

* Use automated testing to verify that configuration files and
  database schema are forward-compatible from release to release and
  that this policy is not accidentally broken (for example, a gating
  grenade test).

* Use automated testing to verify the above compatibility *also*
  remains stable between *tick* releases so that operators are able to
  skip tock releases.

* No existing config options will have their meaning changed in such a way
  that it would alter the software behavior or otherwise render an existing
  config file broken.

Note: these guidelines apply to both service and library feature
deprecation policies and procedures.

.. _release cadence: https://governance.openstack.org/tc/resolutions/20220210-release-cadence-adjustment.html
