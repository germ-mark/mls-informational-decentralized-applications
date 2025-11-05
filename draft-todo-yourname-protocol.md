---
###
# Internet-Draft Markdown Template
#
# Rename this file from draft-todo-yourname-protocol.md to get started.
# Draft name format is "draft-<yourname>-<workgroup>-<name>.md".
#
# For initial setup, you only need to edit the first block of fields.
# Only "title" needs to be changed; delete "abbrev" if your title is short.
# Any other content can be edited, but be careful not to introduce errors.
# Some fields will be set automatically during setup if they are unchanged.
#
# Don't include "-00" or "-latest" in the filename.
# Labels in the form draft-<yourname>-<workgroup>-<name>-latest are used by
# the tools to refer to the current version; see "docname" for example.
#
# This template uses kramdown-rfc: https://github.com/cabo/kramdown-rfc
# You can replace the entire file if you prefer a different format.
# Change the file extension to match the format (.xml for XML, etc...)
#
###
title: "Distributed and Decentralized Uses of MLS"
abbrev: "DDMLS"
category: info

docname: draft-todo-yourname-protocol-latest
submissiontype: IETF  # also: "independent", "editorial", "IAB", or "IRTF"
number:
date:
consensus: true
v: 3
area: SEC
workgroup: MLS
keyword:
 - next generation
 - unicorn
 - sparkling distributed ledger
venue:
  group: WG
  type: Working Group
  mail: WG@example.com
  arch: https://example.com/WG
  github: USER/REPO
  latest: https://example.com/LATEST

author:
 -
    fullname: Britta Hale Your Name Here
    organization: Naval Postgraduate School
    email: britta.hale@nps.edu

    fullname: 
    organization: 
    email:

normative:

informative:

...

--- abstract

The Messaging Layer Security (MLS) protocol provides continuous key agreement, 
and offers additional benefits in multi-device, a.k.a. group use cases. MLS 
is reliant on a Delivery Service (DS) for message ordering. In highly 
centralized uses, where the DS is a single server, configuration is 
straightforward. However, MLS also lends itself to use cases that are 
decentralized (e.g., federated networks) and even distributed (e.g., mesh 
networks). This informational document lays out uses of MLS and its variants 
and extensions across various topologies and provides guidance on selection 
among alternatives under both functionality and security considerations.

--- middle

# Introduction

While the Messaging Layer Security (MLS) protocol had initial uses in the 
construct of simple secure messaging deployments, developments on use cases 
have extended into decentralized and distributed environments. This includes 
decentralized uses such as federation, under the MIMI working group [cite] 
and distributed uses such as across mesh networks. In such cases, 
identification of an adequate delivery service (DS) to fit various topologies 
and ensure precise commit ordering can be difficult, and the DS can incur 
significant overhead in itself. This has driven consideration of how to 
account for potential forking of the group state [cite] for re-ordered commit 
messages or avoid the risk of out-of-order commits altogether [cite]. This 
informational document provides an outline of uses of MLS and its extensions 
across various network topology considerations, with specific considerations 
to network overhead, storage, and security from state forking. 


# Definitions

Members:       Protocol participants. 
Centralized:   A centralized network has a single server or entity perform 
               the responsibilites of a DS. An entity may also be a member.
Decentralized: A decentralized network relies on federation of servers or 
               select entites performing the responsibilities of a DS. For 
               example, assigned members may coordinate DS responsibilites 
               among themselves.            
Distributed:   A distributed network relies on many entities performing 
               the responsibilities of a DS. This may include cases of 
               many members or even all members participating in DS 
               responsibilities, such as in mesh networks.


# Trade-off Considerations

## MLS 
The MLS protocol is defined in [RFC9420]. 

### Overhead
MLS offers logarthimic overhead for groups. 

Additional overhead from the DS must also be accounted for. 

### DS
The centralized case is straightforward in MLS. For decentralized use cases 
and distributed use cases, care must be taken to identify a suitable DS to 
ensure that there is group consensus on commit ordering. As a use case 
increases in complexity to the decentralized setting and thence to the 
distributed setting, DS design decisions have increasing implications on 
overhead and potentially also security.

In a decentralized setting, one example solution is to assign one server 
(among a set of federated servers) as the decision holder for such ordering, 
thereby creating a pseudo-centralized environment out of a decentralized 
environment. In a distributed use case, the challenge increases. Similar 
temporary role assignment to members, where in one is dedicated as a the 
"lead" entity for deciding commit ordering may be possible in well-connected 
use cases. 

Another approach is hierarchical ordering of commits, e.g., where each 
member is assigned an order in which to commit a PCS update. This eases the 
complexity of the DS, but can create considerations on length of PCS windows, 
especially if a member is offline. Handling of Adds and Removes must also be 
accounted for. 

Yet a further approach is that of using a consensus algorithm to reach 
agreement on commit ordering. This offloads overhead to the DS as such 
consensus algorithms can vary wideline in incurred overhead and delay for 
processing, especially if a member is unreachable. 

Thus maintaining consensus on commit ordering tends to incur increasing DS 
overhead has network topology spreads. 

### Resiliency

MLS is heavily dependent on commit ordering being processed in the correct 
sequence. Out-of-order commits can lead to forks in the group state.


## DeMLS (Konrad)
DeMLS is defined in....

### Overhead
DeMLS maintains the logarithmic overhead of updates in MLS. 
DS overhead must also be accounted for as in MLS for various topologies, 
but imposes slightly less ordering strictness than in MLS, as the mechanism 
enables out-of-order commit processing. 
As a trade-off, DeMLS requires more storage on the end device. The additional 
storage requirements are generally correlated to ordering strictness; the less 
delay or out of order commits occur, the less state and storage expansion is 
required. 

### DS

### Resiliency
DeMLS provides fork resilience and a cost to memory. 


## DiMLS (Mark)
DiMLS is defined in....

### Overhead
DiMLS overhead has linear update overhead. However, it is not dependent on the 
DS for commit ordering, reducing the DS overhead requirements. For example, a 
consensus on commit ordering is not required for the DS unlike in MLS. Thus 
the trade-off in DiMLS is among overhead incurred by the security protocol 
itself and its architectural requirements in DS overhead. 

### DS  
In DiMLS there are fewer requirements on the DS for exact ordering. Messages 
must get to the respective destinations but in-order delivery is not expected 
for security purposes of maintaining a consistent state. 

### Resiliency
DeMLS is highly resilient to out-of-order commits and, in the case of honest 
members, forking of the group state is not possible. This is because each 
member has full control of commit ordering as it relates to the keying state 
protecting the messages that member sends. 


# Use Cases
The basic MLS protocol functions well in environments where reliable in-order 
delivery can be assured. That includes both centralized environment as well as 
those in decentralized and distributed uses where the DS overhead for e.g., 
consensus is deemed supportable. 

For cases of minor network topology spread, e.g., decentralized uses such as 
federated servers, DeMLS can provide improvements to fork resiliency at minor 
costs to storage. 

For distributed environments where significant changes in network topologies are 
expected, e.g., mesh networks, or where memory storage is a consideration, 
DiMLS offers advantages. 


# Security Considerations

This document covers security considerations for various applications of other 
documents to and including MLS. This document is not an exhaustive security 
reference for use of MLS in decentralized or distributed environments, but 
focuses on the issue of state forking in such use cases.


# IANA Considerations

This document has no IANA actions.


--- back

# Acknowledgments
{:numbered="false"}

TODO acknowledge.
