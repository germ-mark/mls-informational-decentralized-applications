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
title: "MLS Use Without Centralization"
abbrev: "DMLS"
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
    fullname: Your Name Here
    organization: Your Organization Here
    email: your.email@example.com

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
among alternatives.

--- middle

# Introduction

TODO Introduction


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

## DeMLS (Konrad)

### Overhead
DeMLS maintains the logarithmic overhead of updates in MLS. 

DS overhead and is likewise 
reliant on DS design decisions for DS overhead. 

### DS
### Resiliency

## DMLS (Mark)
### Overhead
### DS
### Resiliency



# Security Considerations

TODO Security


# IANA Considerations

This document has no IANA actions.


--- back

# Acknowledgments
{:numbered="false"}

TODO acknowledge.
