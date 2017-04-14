
# LALA (Light Application-Level API)

## Current work and implementations

* https://github.com/geneontology/noctua/issues/283
* https://github.com/geneontology/noctua/issues/316
* https://github.com/pubannotation/pubannotation/issues/3
* https://github.com/geneontology/amigo/issues/422

## Presentations and context

- [BLAH 3](http://blah3.linkedannotation.org/) [presentation](https://docs.google.com/presentation/d/1-3HgoBmnu9p1Alt6myjfLUJIOojIgpskRFh47NjEd5A/edit?usp=sharing) and [work summary](https://docs.google.com/document/d/16DNkAO9YHKZmQjdVxq2B_ULYQDu7jjc0aciVKdTwX8w/edit?usp=sharing)
- [BioHackathon 16](http://2016.biohackathon.org/), [work summary](https://github.com/dbcls/bh16/wiki/Noctua#add-connections-with-external-markuppaper-annotation-resources-partial)

This proposal has its origins in initial work done with Noctua
communicating with Textpresso Central, for the use cases of 1) adding
annotons and 2) to enrich the evidence captured by Noctua with TPC
textual evidence IDs. Later work was done building on that base for
communication between Noctua and PubAnnotation,
at [BioHackathon 16](http://2016.biohackathon.org/), to capture text
spans to add to evidence to models. Full generalization was started in
building up to and during [BLAH 3](http://blah3.linkedannotation.org/).

## Overview

Much thought and paper has been put into sharing information between
resources, from the use of common identifiers to use of common data
stores. We would like to explore an area that has not, to the authors'
knowledge, been as well explored: the sharing of simple information
directly between web-based applications. For example, a curator may wish to
import IDs from one tool to another, curate the same paper using
different tools, or a researcher may wish to analyze the same piece of
genome in different analysis tools.

More concretely, the use case that we wish to look at is how to obtain
small packets of specific information from an external resource that
has its own associated web application; we look to do this by
round-tripping a packet of information (acting as a "black box" or
"piggy bank") through the external application using basic HTTP
methods, allowing an easy high-level "federation". In an environment
where much effort is being spent on the creation of rich web
applications tailored to the habits of specific groups of scientists
and curators, users and engineers would have much to gain by being
able to reuse functionality from external applications in their
workflows and stacks, as well as having the added benefit of driving
traffic to external web applications that implement such a common
specification.

In creating this protocol, we want to captures the following
qualities:

- _Easy to implement_ <br />
  Complexity can be a barrier to adoption and implementation
- _Basic HTTP tooling_ <br />
  Any system should have easy access to the tools necessary
- _"Stateless"_ <br />
  Simplifying debugging and implementation
- _Minimal need for initial coordination_ <br />
  Beyond what data is to be returned, the external application does not need to understand the transiting packet
- _No need for calling application to coordinate changes to own API after initial coordination_ <br />
  As the calling application is responsible for decoding the information it initially sent and the location it is sent to, major API changes can occur without the need to coordinate with any other resource
- _Have the ability to perform operations "remotely" while still logged-in to the calling application, without the need to coordinate cross-site logins_ <br />
  This can be done by placing an authorization token in the transiting packet

Taking inspiration from the methods used
by [Galaxy](http://galaxyproject.org/) to pull data in from external
applications, we'd like to propose (and get feedback) about a general
pattern for passing relatively simple data directly between
applications. Variations of this proposal have been implemented or
explored in applications such
as:
[Noctua](http://noctua.berkeleybop.org/),
[Textpresso Central](), [AmiGO](http://amigo.geneontology.org),
and [PubAnnotation](http://pubannotation.org/).

## Details

The above tickets go a long way for details and discussion, but very
brief summary would be:

- *caller* passes "black box" to *external*
- *external* holds on to "black box" during workflow, adding data to "black box"
- *external* passes "black box" back to *caller*

A more detailed summary, paying a little more attention to detail
would be:

- *caller* links/POSTs to *external* with the additional parameters *endpoint\_url* (a URL of the *caller*'s choice) and *endpoint\_arguments* (an encoded JSON blob)
- in the *external* application, a button (or what have you) is created that will POST the data in *endpoint\_arguments*, plus *external*'s additions into the *requests* field, to *endpoint\_url*, using whatever workflow is made available
- *endpoint\_url* resolves to an endpoint that recreates the calling state from *caller* and runs the additional data from *external* in the context of the passed-through data
- depending on a (TBD) passed parameter, *external* either continues the session or closes and passes control back to *caller* (e.g. forwards the user to the next page in the workflow or returns a raw JSON response to the external application)

A full explanation, with examples, would look an awful lot like the
tickets above. A good place to start might be the later https://github.com/pubannotation/pubannotation/issues/3 .

## Open questions

While we have worked out the details to satisfy our core use cases, we
are interested in federating more generally with the wide range of
biological and curation applications available. We would like input on how that could happen from the community. Ideas include:

- better documentation
- changes to the specification
- a central registry

As well, there are some edge cases, especially around passing control
(or not) at the end of an interaction that we would like to clarify by
implementations in the real world.

## Availability

[GitHub](https://github.com/kltm/lala)

## License 

This work is a proposal for a software protocol, not a yet concrete or
generalized implementation. As such, the text of this work is
available under the
[CC-BY 4.0 license](https://github.com/kltm/lala/blob/master/LICENSE).
