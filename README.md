# RFCs
Proposed modifications, additions, and-or subtractions to the PMIx definitions

## Submission Procedure
A "Request For Comment" shall be submitted prior to modifying any existing API, adding new APIs to the PMIx definitions, or substantially modifying the behavior of an existing part of the code. The intent of this procedure is to provide adequate oversight of the PMIx definitions, and to aid in community dialog prior to adopting changes.

The PMIx RFC process borrows liberally from other open source communities, but is streamlined to avoid placing undue burden on community contributors. Again, the intent here is to stimulate discussion, not to inhibit contribution or inventiveness. We want people to experiment with new methods and approaches, while respecting that others are using the code base for production environments.

The procedure has only three elements, which can be executed somewhat in parallel:

* Submit an RFC - an author should start by forking the PMIx RFC
  repository into their private Github space, and then creating a branch that will house the new RFC. Creation of the RFC begins by simply copying the provided [Template](https://github.com/pmix/RFCs/tree/master/Template.md), and then editing the file to provide the outlined information. When a draft is ready, the author can generate a pull request against the PMIx RFC repo to submit the RFC for consideration.
* Develop a prototype implementation - an author should start by forking
  the [PMIx master repository](https://github.com/pmix/master) into their private Github space, and then creating a branch that will house the new code. When complete, the author can generate a pull request against the PMIx master with the proposed changes. This PR _must_ be referenced in the RFC.
* Send an email to the PMIx mailing list alerting members to the RFC. This
  should include links to the RFC and the accompanying pull request, along with a very brief statement of the RFC's purpose (e.g., a copy of the abstract section).

Once submitted, the RFC repository administrators will assign an RFC number to the proposal. This number should then be added to the submitted text in the RFC title section as per the Template.

RFC's will be considered accepted based on the following criteria:

* Consensus approval by all commentators. We do not require a formal vote be
  held on an RFC as we do not have a large enough community to justify formal meetings and voting procedures. Instead, we operate under the principle that those who care will participate, and those who don't participate do not have a right to complain about the results.
* Sufficient time for comment has passed. This is a judgement call made by the
  RFC administrators based on the magnitude of the proposed change, the availability of the more frequent community contributors, etc. For example, an RFC submitted during typical vacation times, or when a contributor known to be interested in the affected area is away, may be held for a longer period of time. RFC administrators will clearly indicate the comment time, taking into account comments received from the community, and will provide several warnings prior to ending the comment period.
* Urgency of the change. We recognize that not all changes are equal, and that
  our community members may have internal milestones driving their need for accepting the change. Attempts will be made to accommodate these situations, subject to the "do no harm" mandate - i.e., if community members raise reasonable concerns regarding the impact of the change, then the proposal may be delayed regardless of urgency.

Once the RFC is accepted, the RFC administrators will commit the RFC pull request into that repository, and the author is free to commit the accompanying code to the PMIx master. Transfer of the change to a PMIx release series, however, must be done via a separate pull request to the appropriate release branch - a new RFC is _not_ required for this move.

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

These terms must be used with care and sparingly.  In particular, they MUST only be used where it is actually required for interoperation or to limit behavior which has potential for causing harm, and _MUST\_NOT_ be used to try to impose a particular method on implementors.
