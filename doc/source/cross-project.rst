*************
Cross-Project
*************

Cross-Project Specifications
============================

In order to provide a consistent and desiriable experience for OpenStack
end-users, it's necessary to have some concepts to be agreed across all the
projects within the big tent. In addition, it helps future projects that want
to be part of the big tent to have these guidelines for certain concepts (e.g.
quotas, backwards compatibility).

Cross-Project Specification Liaisons
====================================

The OpenStack project relies on the cross-project spec liaisons from each
participating project to help with coordination and cross-project
spec related tasks. The liaison defaults to the PTL, but the PTL can also
delegate the responsibilites to someone else on the team by updating the
liaison list on the CrossProjectLiaisons_ wiki page.

Liaisons Responsibilities
-------------------------

The liaison does not have to personally do all of these things, but must ensure
they are done by someone on the project team.

* Watching the `cross-project spec repo`_.

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


.. _CrossProjectLiaisons: https://wiki.openstack.org/wiki/CrossProjectLiaisons
.. _cross-project spec repo: https://review.openstack.org/#/q/project:+openstack/openstack-specs+status:+open,n,z
.. _cross-project meeting: https://wiki.openstack.org/wiki/Meetings/CrossProjectMeeting
