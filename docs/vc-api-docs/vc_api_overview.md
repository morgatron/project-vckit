
# Motivation

There are many software platforms in development that peform the core VC
services of issuing and verifying. These are to a greater or lesser extent
compatible in terms of the VCs they issue and verify, but have different
interfaces- different programming languages, features supported, different
defaults and response paradigms. This makes attempting to "pick a winner" for
a project like VC-KIT is fraught.

This it the first reason why VC-KIT chooses to put the basic VC functions
behind an API- to act as a unified wrapper layer. Any library can be used so
long as it can be made to confirm to the API.

But what does a uniform API look like? Fortunately, others are already working
hard to answer this question. There are several projects aiming to standardise
the role of various protocols in the VC lifecycle- CHAPI, DIDComm and VC-API
to name a few. But for current purposes the main project it is the last, [VC-
API](https://github.com/w3c-ccg/vc-api), that is the most relevant (formerly
VC-HTTP-API). Many libraries are now implementing elements of the VC-API out
of the box, so that in the future it may become trivial to swap out the
libraries used by VC-KIT.

The other major behefit of wrapping in a standard API is to enable testing
against stnandard test suites. These greatly simplifies the automated end-to-
end testing of one platform against another.

# VC-API and Traceability-interop

VC-API has been developed since a few years as a work item by members of the
W3C-CCG. Beyond basic credential issuance and verification, it includes
mechanisms for processes such as mediating exchanges and requests between
previously unkown parties. For VC-KIT at present these mechanisms aren't used,
although they may become relevant in the future.

VC-KIT aims primarily at the flavour of VC-API developed by [traceability-
interop](https://github.com/w3c-ccg/traceability-interop). This prohect builds
on VC-API to provide a standard interface for participants in the supply-chain
traceability group of the DHS sponsored Silicon Valley Innovation Program. To
a large extent this is an opinionated take on VC-API that is continuously
tried and tested. It also includes additional mechanisms not currently
relevant for VC-KIT, such as "workflows" and dscoverability.

Both projects (traceability-interop and VC-API) strongly influence each other
and have a large number of contributors in common. Both proijects have
associated test suites developed by the community. VC-KIT aims to meet the
conformance tests for traceability-interop associated with the functionality
it uses- in particular VC issuance and verification.

# Base endpoints:

The endpoints needed for basic VC services in VC-API/traceablity-interop are:

  * /credentials/[issue](Issuance-API_301662300.html) <\- issue a credential
  * /credentials/[verify](Verify-API_303041038.html) <\- verify a credential
  * [/credentials/status](Status-API_301662302.html) <\- update the revocation status of a previoulsy issued credential

Each of these is covered in it's own section. The traceability-intero openapi
spec is found [here](https://w3c-ccg.github.io/traceability-interop/) and
should pass relevant tests from the suites (see the project on
[github](https://github.com/w3c-ccg/traceability-interop))

In general, the issuer related endpoints need authorisation, while verify
doesn't.

# Multiple backends

The traceability-interop test suite focuses on a particular feature profile
(sometimes called the "SVIP" or "DHS" profile) which is relevant for most
uses. However VC-KIT also supports the use of OpenAttestation VCs, and in
principle could support other backends in the future (although ideally
projects will converge and make this unnecessary). As the intentions of all
VCs is essentially the same, they are able to map to the same API. The
approach taken by VC-KIT is to have a single endpoint for each purpose, and
map to either backend where appropirate based on parameters.
