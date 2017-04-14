
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
directly between web-based applications.

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

- Easy to implement <br />
  Complexity can be a barrier to adoption and implementation
- Basic HTTP tooling <br />
  Any system should have easy access to the tools necessary
- "Stateless" <br />
  Simplifying debugging and implementation
- Minimal need for initial coordination <br />
  Beyond what data is to be returned, the external application does not need to understand the transiting packet
- No need for calling application to coordinate changes to own API after initial coordination <br />
  As the calling application is responsible for decoding the information it initially sent and the location it is sent to, major API changes can occur without the need to coordinate with any other resource
- Have the ability to perform operations "remotely" while still logged-in to the calling application, without the need to coordinate cross-site logins <br />
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
