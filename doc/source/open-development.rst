==================
 Open Development
==================

Open Development is also part of the four tenets that help OpenStack to move
forward. Just like Open Community and Open Source, an Open Development allows
for engaging with larger communities and it welcomes a broader group of
members. Open Development is the key behind the large developers community and
the enormous collaboration model in the project.

Project Team Lead
==================

.. TODO(flaper87): add a link to the elections section

Every OpenStack project has a Project Team Lead (PTL) who's elected at
the end of every cycle by the active contributors of the project. The
PTL is responsible for taking the management leadership of the project
it serves. It's a volunteer position and it does not directly relate
to other leadership positions in the Open Source world - i.e
BDFL. Projects are driven by their communities and the PTL serves as a
tie-breaker when hard decisions need to be made.

Core Reviewers
==============

A core reviewer is a member of the OpenStack community that has volunteered to
dedicate time to reviews for a specific project. To become a core reviewer, the
interested person is required to have provided enough reviews to the project in
order to demonstrate the understanding of the project structure, goals and
policies.

Technically speaking, a core reviewer is someone that has +2/-2 powers on the
project they are part of. It's possible to be a core reviewer of several
projects and it's completely fine - and recommended - to step down at any time
if your focus and priorities have changed.

Reviews Guidelines
==================

Reviewing is a critical step in OpenStack's development workflow. It requires
reviewers to understand the project in order to be able to provide great
feedback and guarantee better quality for that project. Nonetheless, reviews
are also a great place for people to start from, to get into OpenStack and
start contributing and to learn the project they are interested in.

In order to make the review process easier, OpenStack has developed some
guidelines that aim to help reviewers identify potential issues and that also
guides them in the interaction with the contributor.

#. One easy first step is to verify that the proposed code complies with the
   project's guidelines. Many projects have a file called `HACKING.RST` in
   their repository. This file contains the base guidelines of that project. If
   the file is not there, then following OpenStack's general guidelines is the
   recommend thing to do. Note that these project's guidelines inherit from the
   OpenStack's guidelines anyway.

#. Verify that the commit message is also compliant with the following criterias:

   #. The title is not longer than 50 characters.
   #. Summary lines should be wrapped at 70 characters.
   #. Make sure the title and the summary describe correctly what the goal of
      the patch is. Having a bug or a blueprint referenced is not enough.
      Sometimes the patches are partially fixes for a bigger problem and
      it's extremely important to have all that explicitly stated in the
      commit message.
   #. Make sure the commit message is correctly flagged. If there are API changes
      then the `APIImpact` flag should be used. If there are security
      implications then the `SecurityImpact` flag should be used. If there are
      documentation changes then the `DocImpact` flag should be used.
   #. Make sure blueprints and bugs are correctly referenced in the commit message:

      #. Closes-bug: #12345
      #. Implements-blueprint: my-blueprint-slug

#. Once the commit message has been reviewed, it'll be a good moment to dive
   into the change itself. While doing this, make sure the following points
   apply as well:

   #. Make sure the code is readable and maintainable.
   #. Make sure the code supports both, python2 and python3. Using six for
      compatibility is recommended.

In general, reviewing code takes a lot of time and effort and it's important
for every reviewer to dedicate as much time as needed to each review in order
to provide better comments and quality. However, when it comes to style and
subjective arguments, it's recommended not to be extremely nitpicky. Finding
the right balance and evaluating trade-off is as important as verifying all the
above points.

Every reviewer has the ability to vote on patches (+1, 0, -1) and only core
reviewers have the ability to +2, -2 and approve patches. Each of these votes
have an influence on the patch itself and they communicate agreement,
disagreement or errors in the patch. Therefore, these votes must be used
properly and it's the reviewer's responsibility to do so.

Specifications
==============

Whenever a change that introduces a new feature, breaks compatibility
or requires discussion needs to be introduced, a specification (spec
from now on) is what you'd need. Specs are no more than a
RestructuredText file that contains enough information about the
proposed change and the problems this change tries to solve.

Specs may change in form depending on the project and target
change. However, the workflow to propose these specs is the same for
every project. Specs ought to be committed to the project's spec
repository and they follow the same contribution workflow as every other
patch. This guarantees that the discussion and proposal remains open
and stands behind our open development tenet.

Projects may or may not have deadlines for spec proposals and
approvals. These projects may also have a slightly different process
and the team responsible for reviewing these specs might not be the
same.

Just like OpenStack's documentation, the merged specs are rendered and
published on-line. The url where they can be found is
http://specs.openstack.org .
