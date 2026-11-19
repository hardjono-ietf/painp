---

stand_alone: yes
pi:
  rfcedstyle: yes
  toc: yes
  tocindent: yes
  tocdepth: 4
  sortrefs: yes
  symrefs: yes
  strict: yes
  comments: yes
  inline: yes
  text-list-symbols: o-*+
  compact: yes
  subcompact: no

title: Traffic Matrices File Format
abbrev: Traffic Matrices
docname: draft-ietf-kepner-traffic-matrices-latest
category: info

ipr: trust200902
area: "Internet Area (INT)"
workgroup: "Individual"

stream: IETF
keyword: Internet-Draft
consensus: true

venue:
  group: "Network Working Group"
  type: "Working Group"
  mail: "painp@mit.edu"
  arch: "https://mailman.mit.edu/pipermail/painp/"
  github: "irtf-painp/drafts"
  latest: "https://github.com/irtf-painp/drafts/blob/main/draft-kepner-traffic-matrices.md"

author:
  -
    ins: J. Kepner
    name: Jeremy Kepner
    organization: MIT
    email: kepner@ll.mit.edu
  -
    ins: H. Jananthan
    name: Hayden Jananthan
    organization: MIT
    email: haydenj@mit.edu
  -
    ins: A. Pentland
    name: Alexander (Sandy) Pentland
    organization: MIT
    email: sandy@media.mit.edu
  -
    ins: T. Hardjono
    name: Thomas Hardjono
    organization: MIT
    email: hardjono@mit.edu


informative:
  Houle2024:
    author:
    - ins: M. Houle
    - ins: M. Jones
    - ins: D. Wallmeyer
    - ins: R. Brodeur
    - ins: J. Burr
    - ins: H. Jananthan
    - ins: S. Merrell
    - ins: P. Michaleas
    - ins: A. Perez
    - ins: A. Prout
    - ins: J. Kepner
    date: September 2024
    target: https://arxiv.org/abs/2409.12297
    title: Hypersparse traffic matrices from Suricata network flows using GraphBLAS
  Kepner2024:
    author:
    - ins: J. Kepner
    - ins: H. Jananthan
    - ins: M. Jones
    - ins: W. Arcand
    - ins: D. Bestor
    - ins: W. Bergeron
    - ins: D. Burrill
    - ins: A. Buluc
    - ins: C. Byun
    - ins: T. Davis
    - ins: V. Gadepally
    - ins: D. Grant
    - ins: M. Houle
    - ins: M. Hubbell
    - ins: P. Luszczek
    - ins: L. Milechin
    - ins: C. Milner
    - ins: G. Morales
    - ins: A. Morris
    - ins: J. Mullen
    - ins: R. Patel
    - ins: A. Pentland
    - ins: S. Pisharody
    - ins: A. Prout
    - ins: A. Reuther
    - ins: A. Rosa
    - ins: G. Wachman
    - ins: C. Yee
    - ins: P. Michaleas
    date: September 2024
    target: https://arxiv.org/abs/2409.03111
    title: What is Normal? A Big Data Observational Science Model of Anonymized Internet Traffic



normative:
  NIST:
    author:
    - ins: Katerina Megas
    - ins: Barbara Cuthill
    - ins: Marissa Dotter
    - ins: Michael Garris
    - ins: Ishika Khemani
    - ins: Bronwyn Patrick
    - ins: Noah Schiro
    - ins: Julie Nethery Snyder
    - ins: Mohammad Zarei
    date: December 2025
    target: https://nvlpubs.nist.gov/nistpubs/ir/2025/NIST.IR.8596.iprd.pdf
    title: Cybersecurity Framework Profile for Artificial Intelligence (NIST IR 8596 iprd)
  Jananthan2024:
    author:
    - ins: H. Jananthan
    - ins: M. Jones
    - ins: W. Arcand
    - ins: D. Bestor
    - ins: W. Bergeron
    - ins: D. Burrill
    - ins: A. Buluc
    - ins: C. Byun
    - ins: T. Davis
    - ins: V. Gadepally
    - ins: D. Grant
    - ins: M. Houle
    - ins: M. Hubbell
    - ins: P. Luszczek
    - ins: P. Michaleas
    - ins: L. Milechin
    - ins: C. Milner
    - ins: G. Morales
    - ins: A. Morris
    - ins: J. Mullen
    - ins: R. Patel
    - ins: A. Pentland
    - ins: S. Pisharody
    - ins: A. Prout
    - ins: A. Reuther
    - ins: A. Rosa
    - ins: G. Wachman
    - ins: C. Yee
    - ins: J. Kepner
    date: September 2024
    target: https://arxiv.org/abs/2409.08115
    title: Anonymized Network Sensing Graph Challenge
  


--- abstract

This document proposes a standard format for traffic matrices for data corresponding to common layers of the Internet architecture. Traffic matrices are highly adaptable enabling sources/destinations to be any combination of physical, logical, persona, and/or AI agent endpoints.


--- middle

# Introduction

{: #doc-introduction}

Modern networks generate traffic at a scale and speed that make large-scale long-duration packet\-level capture and analysis challenging. While network flow data reduces overall data volume, analyzing large numbers of flows across extended time periods and across an entire network remains computationally expensive. As a result, security teams often struggle to identify subtle but important behaviors, such as distributed scanning, slow data exfiltration, and gradual deviations from normal traffic patterns.

In many domains protection requires observing the domain and analyzing the observations. Effective cyber domain observation and analysis must be done with the highest regard for privacy. In the broader networking community anonymized source-to-destination traffic matrices with standard data sharing agreements have emerged as one approach to meeting many of these requirements. Traffic matrices are highly adaptable and sources/destinations can be any combination of physical, logical, persona, and/or AI agent endpoints. 

With increasing numbers of Artificial Intelligence (AI) systems and AI-based agents coming online, there is an increased usage of AI technology for cyberattacks against many enterprise organizations. As such, there is an urgent need to utilize AI technology also for cyber\-defense against these new forms of cyberattacks [NIST].

A core component to the new cyber-defense AI profile is the speed at which network-related data can be made available to AI cyber-defense systems within an enterprise. A standard format for traffic matrices produces by network devices (e.g., routers), network elements (e.g., VPNs) and higher-layer functions of the enterprise (e.g., AI Agents) enables the enterprise AI systems dedicated to cyber-defense to process the data in the traffic matrices at a higher speed and efficiency.  

# Key Concepts

{: #keyconcepts}

The following are some key concepts used in the current document.

A traffic matrix (or array) denoted by

    A_t(i,j) = v,   i in I, j in J, v in V

indicates that within time window t communication was sent from source i to destination j
and assigned non-zero (non\-empty) value v.  In this context a source is any unique identifier for a source in a network communication.  Similarly, a destination is any unique identifier for a source in a network communication.

For example, in the case of IPv4 packets

    I = J = {0,...,2^32-1}, V = {1,2,3,...}

so that A_t(i,j) indicates the number of packets sent during time window t from IPv4 source address i to IPv4 destination address j.  Alternately, if a dotted quad representation is desired, then

    I = J = {'0.0.0.0',...,'255.255.255.255'}

in the case of IPv6, this could be:

    I = J = {'0:0:0:0:0:0:0:0',...,'ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff'}

More generally, the sets and I and J can be arbitrary strings enabling every level of the OSI stack (see Table 1) to be represented using traffic arrays. Modern sparse math libraries (e.g., GraphBLAS.org, cuSparse, OneSparse, ...) and associative array math libraries (e.g., d4m.mit.edu) can store and operate on these arrays extremely efficiently.


| OSI Layer | Source | Destination | Capture |
| :--- | :--- | :--- | :--- |
| L7 \- Application | Client | URL String | URLs |
| L5 \- Session | Client | DNS | URLs |
| L4 \- Transport | IP\-Port | IP\-Port | TCP |
| L3 \- Network | IP | IP | packets |
| L2 \- Data Link | MAC | MAC | frames |
| L1 \- Physical | Interface | Interface | bits |

TABLE 1. Example of possible sources and destinations across OSI layers along with possible captures from traffic ingestion.

For a list of additional standard mathematical concepts and notations that are assumed for this document, please see Appendix A.


## Data from Network and Transport Layers

{: #network-transport-data}

Analysis of data from the Network Layer typically utilizes IP addresses as explicit endpoint identifiers to determine source and destination hosts within network communications, with packet counts serving as a common quantitative metric for characterizing communication patterns and network activity. This IP-based approach can be extended by incorporating information on the Transport Layer, demonstrating that the combination of IP addresses and port numbers provides a more detailed representation of communication endpoints by identifying both the host and the associated application-level service. Across both layers, packet count can be used as the measurement parameter for analyzing network behavior. Thus, a useful approach is to define source and destination endpoints using IP address pairs at the Network Layer and IP address\-port number combinations at the Transport Layer. Packet counts can be extracted from packet capture (PCAP) files to quantify communication activity.


## Data from the Application Layer

{: #application-data}

Analytics on data at the application layer typically focuses on HTTP-related data because of its ease of identification within PCAP files. For this layer, the source is defined as the device initiating the request, while the destination is represented by the corresponding web address or DNS-resolved endpoint. The counting parameter is defined as the complete transaction between the client device and the server, providing a measure of Application Layer communication activity.


## Data from the AI Layer

{: #AI-layer-data}

With the emergence of Agent-to-Agent interactions protocols (e.g., Model Context Protocol (MCP)), a new layer of data has emerged at (or above) the Application Layer. We refer to this new level as the AI Layer, which includes interactions data (prompts and responses) from Human\-AI interactions as well as Agent\-to\-Agent interactions.


# Mathematical Requirements

{: #math-reqs-sect}

A primary benefit of traffic matrices is the efficient computation of a wide range of network quantities via matrix mathematics. In this section we outline the types of computations that a traffic matrix are assumed to support.

As above, let A_t be a traffic matrix associated with an observation window at time t. A traffic event is an ordered triple of the form (source, destination, timestamp). Let E_t denote the set of traffic events observed during the corresponding observation window.

- Valid events: The total number of events in E_t (often denoted N_V).
- Link events from source i to destination j: The number of events in E_t linking source i to destination j.
- Unique links: The total number of unique source-destination pair connections in E_t.
- Max link events: The maximum number of connections that occur across all source-destination pairs.
- Unique sources: The number of unique sources across all events in E_t.
- Events from source i: The number of events in E_t with source i.
- Max source events: The maximum number of connections from any one source.
- Source fan\-out from source i: The number of destinations which have a traffic event with source i in E_t.
- Max source fan\-out: The maximum source fan\-out among all possible sources.
- Unique destinations: The number of unique destinations across all events in E_t.
- Destination events to destination j: The number of events in E_t with destination j.
- Max destination events: The maximum number of connections to any one destination.
- Destination fan\-in to destination j: The number of sources which have a traffic event with source j in E_t.
- Max destination fan\-in: The maximum destination fan\-in among all possible destinations.

These network quantities are summarized along with their summation-based and matrix-based notations in Table 2 and Figure 1.

| Aggregate Property | Summation Notation | Matrix Notation |
| :--- | :--- | :--- |
| Valid events | sum_i(sum_j(A_t(i, j))) | 1^T @ A_t @ 1 |
| Unique links | sum_i(sum_j(\|A_t\|_0(i, j))) | 1^T @ \|A_t\|_0 @ 1 |
| Link events from i to j | A_t(i, j) | A_t |
| Max link events | max_{i,j}(A_t(i, j)) | max(A_t) |
| Unique sources | sum_i(\|sum_j(A_t(i, j))\|_0) | 1^T @ \|A_t @ 1\|_0 |
| Events from source i | sum_j(A_t(i, j)) | A_t @ 1 |
| Max source events | max_i(sum_j(A_t(i, j))) | max(A_t @ 1) |
| Source fan-out from i | sum_j(\|A_t\|_0(i, j)) | \|A_t\|_0 @ 1 |
| Max source fan-out | max_i(sum_j(\|A_t\|_0(i, j))) | max(\|A_t\|_0 @ 1) |
| Unique destinations | sum_j(\|sum_i(A_t(i, j))\|_0) | \|1^T @ A_t\|_0 @ 1 |
| Destinations events to j | sum_i(A_t(i, j)) | 1^T @ \|A_t\|_0 |
| Max destination events | max_j(sum_i(A_t(i, j))) | max(1^T @ \|A_t\|_0) |
| Destination fan-in to j | sum_i(\|A_t(i, j)\|_0) | 1^T @ A_t |
| Max destination fan-in | max_j(sum_i(\|A_t(i, j)\|_0)) | max(1^T @ A_t) |

TABLE 2. List of network quantities computed from a traffic matrix A_t using both summation-based and matrix-based notations.

~~~
                                      unique links
                              ____________/\____________
                             /                          \
                unique                                     unique
                sources        source fan-out              destinations
                 \                   v                              /
                  \---------------> @\\''''''''''/@ <--------------/
                   \               /  \'\_   _/'/  \              / 
          source    \............./..  \  \_/  /  ..\............/   
          events     \           /  |   \_/ \_/   |  \          /    
                |     \         /   |  _/\   /\_  |   \        /      
.............   v      \       /    v_/   \ /   \_v    \      /           .................
valid source | ---------------|-----@-------------@-----|--------------- |valid destination
event window |           \     \     '\_  / \  _/''    /    /       ^    |event window
'''''''''''''             \     \       \/   \/       /    /        |     '''''''''''''''''
                           \     \      /'\_/ \      /    /         |
                            \     \    / _/ \_ \    /    /          destination
                             \     \  /_/     \_\  /    /           events
                              \---> @//..........\@ <--/
                                                  ^
                                             destination fan-in

                             \____________  ____________/
                                          \/
                                      link events
~~~
{: #matrix-gen-figure}

FIGURE 1. Networks quantities extracted from an event stream E_t.


# Privacy

{: #privacy-sect}

Network data must be handled with care. The Center for Applied Internet Data Analysis (CAIDA) based at the University of California's San Diego Supercomputer Center has pioneered trusted data sharing best practices that combine anonymizing source and destination internet addresses using CryptoPAN with data sharing agreements. These data sharing best practices include the following principles.

- Data is made available in curated repositories.
- Using standard anonymization methods where needed: hashing, sampling, and/or simulation.
- Registration with a repository and demonstration of legitimate research need.
- Recipients legally agree to neither repost a corpus nor deanonymize data.
- Recipients can publish analysis and data examples necessary to review research.
- Recipients agree to cite the repository and provide publications back to the repository.
- Repositories can curate enriched products developed by researchers.

Traffic matrices readily support these practices via the invariance of matrix operations under permutations. In other words, relabeling the sources and destinations of a traffic matrix (anonymizing) does not change the formulas or resulting matrix computations of the type shown in Table 2.






# Security Considerations

{: #security-consider}



# IANA Considerations

{: #iana-consider}

This document has no IANA actions.


--- back

# Appendix A. Explanation of Mathematical Assumptions and Notation

{: #appendix-a}

The following enumerates some foundational mathematical concepts and notations:

- Hypersparse matrix: Any subset M of a Cartesian product of the form A x B x REAL where A and B are finite totally ordered sets and REAL is the set of real numbers, subject to the constraint that if (a, b, r) and (a, b, s) are in M for some a in A and b in B, then r = s. Notationally, given any set a and set b, M(a, b) is defined as the unique real number r such that (a, b, r) is in M, if such a real number r exists, otherwise M(a, b) is defined as 0. Let ind(M) denote the set \{(i, j) \| there exists r in REAL such that (i, j, r) in M\}, called the index set of M.

- Traffic matrix: Given an understood traffic event observation window, the corresponding traffic matrix is the hypersparse matrix A which is the subset of \{possible sources\} x \{possible destinations\} x REAL consisting of triples of the form (i, j, n) where n is the number of traffic events observed with source i and destination j, assuming there is at least one such traffic event. As such, under the convention that A(i, j) = 0 whenever (i, j) is not a member of ind(A), it is the case that A(i, j) = number of traffic events observed with source i and destination j for any possible source i and possible destination j. 

## Matrix Operations

{: ##matrix-operations}

Suppose A and B are arbitrary hypersparse matrices. 

- Number of nonzeros: nnz(A) denotes the number of members of the set \{(i, j) in ind(A) \| A(i, j) nonzero\}.

- Row extraction: Given any set i, A(i,:) denotes the hypersparse matrix C = \{(i, j, A(i, j)) in A \| j such that (i, j) in ind(A)\}.

- Column extraction: Given any set j, A(:,j) denotes the hypersparse matrix C = \{(i, j, A(i, j)) in A \| i such that (i, j) in ind(A)\}.

- Matrix elementwise addition: A \+ B is the hypersparse matrix C defined by C = \{(i, j, A(i, j)) in A \| (i, j) in ind(A)\\ind(B)\} union \{(i, j, B(i, j)) in B \| (i, j) in ind(B)\\ind(A)\} union \{(i, j, A(i, j) + B(i, j)) in (ind(A) intersect ind(B)) x REAL \| (i, j) in ind(A) intersect ind(B)\}. 

- Matrix elementwise multiplication: A \* B is the hypersparse matrix C defined by C = \{(i, j, A(i, j) \* B(i, j)) in (ind(A) intersect ind(B)) x REAL \| (i, j) in ind(A) intersect ind(B)\}.

- Matrix multiplication: A @ B is the hypersparse matrix C consisting of triples of the form (i, j, C(i, j)) where C(i, j) = sum of A(i, k) \* B(k, j) over all k for which (i, k) in ind(A) and (k, j) in ind(B), assuming such k exist. 

- Matrix tranpose: A^T is the hypersparse matrix C defined by C = \{(j, i, A(i, j)) \| (i, j) in ind(A)\}.

- Identity matrix associated with a set S: I_S is the hypersparse matrix C defined by C = \{(i, i, 1) \| i in S\}. 

- Matrix of ones associated with a set S: 1_S is the hypersparse matrix C defined by C = \{(i, 0, 1) \| i in S\}. By 1 we mean 1_S where S = \{possible sources\} union \{possible destinations\}.

- Matrix summation: sum(A,0) (AKA 'summing down columns') is the hypersparse matrix C consisting of triples of the form (0, j, C(0, j)) where C(0, j) = sum of A(i, j) over all i for which (i, j) in ind(A), assuming such i exist. sum(A,1) (AKA 'summing across rows') is the hypersparse matrix C consisting of triples of the form (i, 0, C(i, 0)) where (C(i, 0) = sum of A(i, j) over all j for which (i, j) in ind(A), assuming such j exist. Finally, sum(A) is the real number r such that sum(sum(A,0),1) = \{(0, 0, r)\} or, equivalently, the sum of A(i, j) over all (i, j) in ind(A).

- Matrix maximum: max(A,0) is the hypersparse matrix C consisting of triples of the form (0, j, C(0, j)) where C(0, j) = maximum of A(i, j) over all i for which (i, j) in ind(A), assuming such i exist. max(A,1) is the hypersparse matrix C consisting of triples of the form (i, 0, C(i, 0)) where C(i, 0) = maximum of A(i, j) over all j for which (i, j) in ind(A), assuming such j exist. max(A) is the real number r such that max(max(A,0),1) or, equivalently, the maximum of A(i, j) over all (i, j) in ind(A).

- Zero norm: \|A\|_0 is the hyperspace matric C defined by C = \{(i, j, 1) in ind(A) x REAL \| (i, j) in ind(A)\}. 

## Set Operations

{: ##set-operations}

- Set: A mathematical object formalizing the informal notion of a collection of abstract objects. Sets support a single primitive binary relation between them called membership; the phrase 'A in B' denotes that A and B are both sets and that A is a member of B. The mathematical theory of sets is assumed to be the Zermelo-Fraenkel Set Theory with Choice. Notationally, if A1, A2, ..., An are sets, then \{A1, A2, ..., An\} is the set whose members are exactly A1, A2, ..., An. Additionally, if P(A) denotes a predicate representing whether a set A has some set theoretic property and B is another set, then \{A in B \| P(A)\} is the set whose members are all those members A of B for which P(A) is TRUE.

- Subset relation: A derived binary relation between sets; given two sets A and B, A is said to be a subset of B if every member of A is a member of B. 

- Ordered pair: Given two sets A and B, the ordered pair (A, B) is defined as the set \{{A}, {A, B}\}.

- Ordered triple: Given three sets A, B, and C, the ordered triple (A, B, C) is defined as the ordered pair ((A, B), C). 

- Cartesian product: Given two sets A and B, their Cartesian product X = A x B is the set consisting of all ordered pairs (a, b) where a is a member of A and b is a member of B. Given three sets A, B, and C, their Cartesian product X = A x B x C is the set consisting of all ordered triples (a, b, c) where a is a member of A, b is a member of B, and c is a member of C.

- Union: Given two sets A and B, their union U = A union B is the set consisting precisely of all members of A and all members of B.

- Intersect: Given two sets A and B, their intersection I = A intersect B is the set consisting precisely of all members of A which are also members of B. 

- Set difference: Given two sets A and B, their set difference D = A\\B is the set consisting precisely of all members of A which are not also members of B.

- Real numbers: The set of all real numbers is denoted as REAL.

- Finite totally ordered set: A set A which has only finitely many members and which has a distinguished binary relation \<= on its members such that: (1) \<= is reflexive, meaning a \<= a for all a in A; (2) \<= is antisymmetric, meaning a \<= b and b \<= a for a, b in A implies a = b; (3) \<= is transitive, meaning for all a, b, c in A, a \<= b and b \<= c implies a \<= c; (4) \<= is total, meaning for any a, b in A either a \<= b or b \<= a.


# Acknowledgments
{:numbered="false"}


We thank the following for contributions: (list here).

