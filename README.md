# RFCs
Proposed modifications, additions, and-or subtractions to the PMIx definitions

**This process has been superceded by the PMIx Administrative Steering Commitee procedures**
  
**https://github.com/pmix/pmix-standard#contributing-to-the-pmix-standard**

## Submission Procedure
A "Request For Comment" shall be submitted prior to modifying any existing API, adding new APIs to the PMIx definitions, or substantially modifying the behavior of an existing part of the code. The intent of this procedure is to provide adequate oversight of the PMIx definitions, and to aid in community dialog prior to adopting changes.

The PMIx RFC process borrows liberally from other open source communities, but is streamlined to avoid placing undue burden on community contributors. Again, the intent here is to stimulate discussion, not to inhibit contribution or inventiveness. We want people to experiment with new methods and approaches, while respecting that others are using the code base for production environments.

The procedure has only three elements, which can be executed somewhat in parallel:

* Submit an RFC - an author should start by forking the [PMIx RFC
  repository](https://github.com/pmix/RFCs) into their private Github space, and then creating a branch that will house the new RFC. Creation of the RFC begins by simply copying the provided [Template](https://github.com/pmix/RFCs/tree/master/Template.md), and then editing the file to provide the outlined information. When a draft is ready, the author can generate a pull request against the PMIx RFC repo to submit the RFC for consideration.

  The PR number automatically assigned by Github will serve as the official RFC number for the proposal. This number should also be added to the submitted text in the RFC title section as per the Template.

* Develop a prototype implementation - an author should start by forking
  the [PMIx master repository](https://github.com/pmix/master) into their private Github space, and then creating a branch that will house the new code. When complete, the author can generate a pull request against the PMIx master with the proposed changes. This PR _must_ be referenced in the RFC.

* Send an email to the PMIx mailing list alerting members to the RFC. This
  should include links to the RFC and the accompanying pull request, along with a very brief statement of the RFC's purpose (e.g., a copy of the abstract section). Both the email and the PR shall include a proposed timetable within which the author(s) would like to have the RFC reviewed. This is not intended to form a hard deadline, but rather to inform the community as to the anticipated pace of the proposal.

Upon submission, the PR of the proposed RFC will remain in active development until the author(s) feel it is ready. During this time, interested parties are encouraged to provide comments and otherwise work with the authors to improve the proposal.

Once the author(s) decide that the RFC is ready for official consideration, they will indicate this by setting the "SUBMIT" label on the RFC PR and sending an email to the mailing list requesting review. RFCs will not be formally reviewed for acceptance until at least one person not belonging to any author's organization marks the RFC PR as "SECONDED". This does not indicate any form of approval - the "SECONDED" label is only intended to indicate that the person feels the proposal is ready for formal consideration.

Seconded proposals must be reviewed during a community teleconference two weeks after being seconded. The two week window is provided to allow adequate time for community consideration, and questions (posted to the authors on the RFC PR) are encouraged during that time.

The teleconference review does not require that authors provide a presentation on the proposal, though authors are encouraged to do so. Regardless, a brief description of the proposal is to be provided by the authors, followed by a question and answer session.

RFC's will be considered accepted based on the following criteria:

* Consensus approval by all commentators. We do not require a formal vote be
  held on an RFC as we do not have a large enough community to justify formal meetings and voting procedures. Instead, we operate under the principle that those who care will participate, and those who don't participate do not have a right to complain about the results.

* Sufficient time for comment has passed. This is a judgement call made by the
  RFC administrators based on the magnitude of the proposed change, the availability of the more frequent community contributors, etc. For example, an RFC submitted during typical vacation times, or when a contributor known to be interested in the affected area is away, may be held for a longer period of time. RFC administrators will clearly indicate the comment time, taking into account comments received from the community, and will provide several warnings prior to ending the comment period.

* Urgency of the change. We recognize that not all changes are equal, and that
  our community members may have internal milestones driving their need for accepting the change. Attempts will be made to accommodate these situations, subject to the "do no harm" mandate - i.e., if community members raise reasonable concerns regarding the impact of the change, then the proposal may be delayed regardless of urgency.

Once the RFC is accepted, the RFC administrators will commit the RFC pull request into that repository, and the author is free to commit the accompanying code to the PMIx master. The RFC administrators will subsequently mark the final RFC with:

* date the RFC was accepted
* date the code pull request was committed to the master
* the SHA of that commit

Transfer of the change to a PMIx release series must be done via a separate pull request to the appropriate release branch - however, a new RFC is _not_ required for this move.

## Requirements Language
The following key words are to be interpreted as defined below when used in a PMIx RFC:

* "MUST" - This word, or the terms "REQUIRED" or "SHALL", mean that the
   definition is an absolute requirement of the specification and all implementations are to support it. For example, an API or PMIx-defined key labeled this way must be supported by the host resource manager.
* "MUST NOT" - This phrase, or the phrase "SHALL NOT", mean that the
   labeled item is an absolute prohibition of the specification.
* "SHOULD" - This word, or the term "RECOMMENDED", mean that there
   may exist valid reasons in particular circumstances to ignore the labeled item, but that implementors are requested to support it if at all possible.
* "SHOULD NOT" - This phrase, or the phrase "NOT RECOMMENDED", mean that
   there may exist valid reasons in particular circumstances when the
   labeled item is acceptable or even useful, but that implementors are requested to avoid it if at all possible.
* "OPTIONAL" - This word, or the term "MAY", mean that an item is
   truly optional.  One vendor may choose to include the item because a
   particular marketplace requires it or because the vendor feels that
   it enhances the product while another vendor may omit the same item.
   An implementation which does not include a particular option _MUST_ be
   prepared to interoperate with another implementation which does
   include the option, though perhaps with reduced functionality. In the
   same vein an implementation which does include a particular option
   _MUST_ be prepared to interoperate with another implementation which
   does not include the option (except, of course, for the feature the
   option provides).

These terms must be used with care and sparingly.  In particular, they _MUST_ only be used where it is actually required for interoperation or to limit behavior which has potential for causing harm, and _MUST NOT_ be used to try to impose a particular method on implementors.
