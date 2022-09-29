# Specification of test data for ([NAMESERVER02]).

## Test Case
This document specifies available test data for test case ([NAMESERVER02])..

## Base domain for test zones

The base domain name for the test zones in this document is
`nameserver02.xa.`. All test zones are child zones to the base domain name.

## Messages
The messages are defined in the test case ([NAMESERVER02]).

* "T" = the message _MUST_ be outputted
* "F" = the message must _NOT_ be outputted
* "-" = the presence or non-presence is ignored.

Message \ Scenario group          | A | B | C | D | E | F
:---------------------------------|:--|:--|:--|:--|:--|:--
NO_RESPONSE                       | T | - | - | - | - | F
BREAKS_ON_EDNS                    | - | T | - | - | - | F
NO_EDNS_SUPPORT                   | - | - | T | - | - | F
EDNS_RESPONSE_WITHOUT_EDNS        | - | - | - | T | - | F
EDNS_VERSION_ERROR                | - | - | - | - | T | F
NS_ERROR                          | - | - | - | - | - | T 

A scenario group, e.g. "A", will give the given values of True (message
is present) or False (message is absent) of the messages in the table.
Each scenario group has one or several zone files that matches that
scenario. The zone files are numbered, e.g. `A00`, `A01` etc. In the
table below each zone file is briefly described, and its default
outcome is given. The default outcome is derived from the outcome section
[NAMESERVER02].

Zone file|Default outcome|Description of zone
:--------|:--------------|:----------------------------------------------------------------------------------------------
A00      |Debug          |No response with OPT and no response without OPT
B00      |Error          |No response with OPT and response with OPT
C00      |Warning        |RCODE "FORMERR" and no OPT record
D00      |Error          |RCODE "NOERROR" and no OPT record
E00      |Error          |RCODE "NOERROR" and OPT version != 0
F00      |Warning        |not RCODE "NOERROR"
F01      |Warning        |no SOA record for Child Zone
F02      |Warning        |no OPT record with EDNS version 0

The child zone name is derived from the tables above. In the first table, each combination
defined combination of messages from the test case is given a capital letter, e.g. `A`. In
the second table the zone file name prefix is defined, e.g. `A00`. The prefix is is prepended
the base domain name, e.g. `A00.nameserver02.xa` and then a general prefix
`zone` is added to the final domain name, e.g. `zone.A00.nameserver02.xa`.


[NAMESERVER02]:                  ../../specifications/tests/Nameserver-TP/nameserver02.md
