*************
Cross-Project
*************

Cross-Project Specifications
============================

In order to provide OpenStack end-users with a consistent and desirable
experience, it can be necessary for some concepts (e.g. quotas, backwards
compatibility) and new feature requests to be agreed across all projects
involved with the problem.

What are Specifications?
------------------------

These documents are for problems or functionality that involves multiple
projects. Not everything that meets this criteria is required to go through
this process.  For example, if two projects are trying to reassess their API
interactions with each other, they should keep the specs with their own spec
repositories. If multiple projects have a need for some similar concept (e.g.
quotas), or to coordinate with another project that is a dependency to
a capability, they can propose a spec here.

Since these specifications are cross-project, they should consist mainly of:

* General concept of the problem.
* Use cases.
* General solutions to the problem. (e.g. shared/Oslo libraries) or best
  practices.
* Links to related specs and blueprints in the involved projects, as they
  become available.

Who Is Involved?
^^^^^^^^^^^^^^^^

A specification will have a list of projects it involves, and their linked
blueprint. A project may choose to write a more detailed specification within
their own project specification repository if necessary.

Proposing A Specification
-------------------------

Create
^^^^^^

#. If you haven't already, read our `developer guide`_.
#. git clone https://git.openstack.org/openstack/openstack-specs
#. Use the provided `template`_.
#. Push to gerrit for review.
#. Propose `agenda items`_ to the Cross-Project meeting, and make sure someone
   who understands the subject can attend the meeting to answer questions.

Agreement
^^^^^^^^^

Once the `Cross-Project Spec Liaisons`_ from the list of involved
projects agree on a cross-project specification, the Technical Committee will
review the agreements by the individual teams and merge the specification.

Rolling Out the Change
^^^^^^^^^^^^^^^^^^^^^^

As mentioned in the `Cross-Project Spec Liaisons`_ responsibilities, it's
up to them to communicate with their own project team about these initiatives.
Depending on availability of resources and the project's priorities will
determine when these changes will be rolled out.

Cross-Project Specification Liaisons
====================================

OpenStack relies on the cross-project spec liaisons from each participating
project to help with coordination and cross-project spec related tasks. The
liaison defaults to the PTL, but the PTL can also delegate the responsibilities
to someone else on the team by updating the liaison list on the
`Cross-Project Spec Liaisons`_ wiki page.

Liaison Responsibilities
------------------------

The liaison does not have to personally do all of these things, but must ensure
they are done by someone on the project team.

* Watch the `cross-project spec repo`_.

  * Comment on specs that involve your project. +1 to carry forward for TC
    approval.

    * If you're not able to provide technical guidance on certain specs for
       your project, it's up to you to get the right people involved.
    *  Assuming you get someone else involved, it's up to you to make sure they
       keep up with communication.

  * Communicate back to your project's meeting on certain cross-project specs
    when necessary. This is also good for the previous bullet point of sourcing
    who would have technical knowledge for certain specs.

* Attend the `cross-project meeting`_ when it's called for.
* Work with the PTL of your project to begin prioritizing implementations of
  agreed specs from the community.

Meetings
========

General
-------

The cross-project meeting occurs if there are `agenda items`_ proposed by the
community that involve the attention of the `Cross-Project Spec Liaisons`_.

Sub Cross-Project Meetings
--------------------------

Some cross-project initiatives don't involve all projects under the big tent.
Some may also take more than a couple of meetings to discuss. For these
initiatives it's recommended to break out into a group with all involved
projects' `Cross-Project Spec Liaisons`_, and iterate together through
a specification.

To facilitate scheduling such temporary cross-project meetings, a specific
meeting channel is available and should be used: #openstack-meeting-cp.

Just like what is an applicable cross-project spec, these meetings should not
just be project to project interaction. These meetings should have a specific
and concrete goal. When that goal is reached, the meetings should stop.

Propose A Sub Cross-Project Meeting
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

#. Clone https://git.openstack.org/openstack-infra/irc-meetings
#. In the meetings directory add a file in the format of <name>_wg.rst, where
   <name> should be something to identify your groups initiative. Use this
   example for the file content of service-catalog-wg.rst::

    project:  Service Catalog TNG Working Group Virtual Standup
    meeting_id:  service_catalog_tng
    agenda_url:  https://wiki.openstack.org/wiki/Meetings/ServiceCatalogTNG
    schedule:
      - time:       '1500'
        day:        Thursday
        irc:        openstack-meeting-cp
        frequency:  weekly
        start_date: 20160107
        duration:   60
    chair:  Sean Dague (sdague), Anne Gentle (annegentle)
    description:  >
      This is a weekly 30 minute virtual standup to see where our progress
      is for the overall effort. It is assumed most folks won't attend the
      meeting but will instead update our virtual standup etherpad before
      the meeting. This will just be a checkpoint and point at the top
      issue that needs to be focussed on for the next week.

.. _developer guide: http://docs.openstack.org/infra/manual/developers.html
.. _template: http://git.openstack.org/cgit/openstack/openstack-specs/plain/template.rst
.. _Cross-Project Spec Liaisons: https://wiki.openstack.org/wiki/CrossProjectLiaisons#Cross-Project_Spec_Liaisons
.. _cross-project spec repo: https://review.openstack.org/#/q/project:+openstack/openstack-specs+status:+open,n,z
.. _cross-project meeting: https://wiki.openstack.org/wiki/Meetings/CrossProjectMeeting
.. _agenda items: https://wiki.openstack.org/wiki/Meetings/CrossProjectMeeting#Proposed_agenda
