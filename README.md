# Testenvironment

## Installation

Clone this repo to your test server.

Run the script add-ip-to-lo.sh to add secondary addresses to your loopback interface.

Make sure that coredns is installed. (https://github.com/coredns/coredns.io)

Set upp each test case with the command listed below. You can only have one test case active for the moment.


## nameserver05: 

Zone                  |Default outcome|Description of zone                                 | Note
:---------------------|:--------------|:---------------------------------------------------|:-------------------------------------------
NO-RESPONSE           |Debug          |No response                                         | Delegated to black hole (correct?)
A-UNEXPECTED-RCODE    |Warning        |Answer with RCODE=NOTIMP                            | Ok
AAAA-QUERY-DROPPED    |Error          |No response                                         | Tested with empty answer, not detected.
AAAA-UNEXPECTED-RCODE |Error          |Answer with RCODE=NOTIMP                            | Not detected!
AAAA-BAD-RDATA        |Error          |Answer with v4 address                              | Not allowed with template
AAAA-WELL-PROCESSED   |Info           |Normal answer                                       | Ok


coredns --conf config-nameserver05.cfg

zonemaster-cli NO-RESPONSE.nameserver05.xa  --raw --level info --test Nameserver/nameserver05
   0.00 INFO      GLOBAL_VERSION   version=v4.5.1
   
zonemaster-cli A-UNEXPECTED-RCODE.nameserver05.xa  --raw --level info --test Nameserver/nameserver05
   0.00 INFO      GLOBAL_VERSION   version=v4.5.1
   0.07 WARNING   A_UNEXPECTED_RCODE   ns=ns1.a-unexpected-rcode.nameserver05.xa/2001:db8::31; rcode=NOTIMPL
   0.07 WARNING   A_UNEXPECTED_RCODE   ns=ns2.a-unexpected-rcode.nameserver05.xa/2001:db8::32; rcode=NOTIMPL

zonemaster-cli AAAA-QUERY-DROPPED.nameserver05.xa  --raw --level info --test Nameserver/nameserver05
   0.00 INFO      GLOBAL_VERSION   version=v4.5.1

zonemaster-cli AAAA-UNEXPECTED-RCODE.nameserver05.xa  --raw --level info --test Nameserver/nameserver05
   0.00 INFO      GLOBAL_VERSION   version=v4.5.1

zonemaster-cli AAAA-BAD-RDATA.nameserver05.xa  --raw --level info --test Nameserver/nameserver05
   0.00 INFO      GLOBAL_VERSION   version=v4.5.1

zonemaster-cli AAAA-WELL-PROCESSED.nameserver05.xa  --raw --level info --test Nameserver/nameserver05
   0.00 INFO      GLOBAL_VERSION   version=v4.5.1
   0.07 INFO      AAAA_WELL_PROCESSED   ns_list=ns2.aaaa-well-processed.nameserver05.xa/2001:db8::32;ns1.aaaa-well-processed.nameserver05.xa/192.0.2.31;ns1.aaaa-well-processed.nameserver05.xa/2001:db8::31;ns2.aaaa-well-processed.nameserver05.xa/192.0.2.32
   


## nameserver07:

Zone                    |Default outcome|Description of zone                                 | Note
:-----------------------|:--------------|:---------------------------------------------------|:-------------------------------------------
UPWARD_REFERRAL         |Warning        | ns2 answers with upward referral                   | 


coredns --conf config-nameserver07.cfg

zonemaster-cli UPWARD-REFERRAL.nameserver07.xa  --raw --level info --test Nameserver/nameserver07
   0.00 INFO      GLOBAL_VERSION   version=v4.5.1
   0.08 WARNING   UPWARD_REFERRAL   ns=ns2.upward-referral.nameserver07.xa/192.0.2.32
   0.08 WARNING   UPWARD_REFERRAL   ns=ns2.upward-referral.nameserver07.xa/2001:db8::32


## Nameserver11:

Zone                    |Default outcome|Description of zone                                 | Note
:-----------------------|:--------------|:---------------------------------------------------|:-------------------------------------------
N11-UNKNOWN-OPTION-CODE |Warning        | ns3 answers with option-code=99                    | 


coredns --conf config-nameserver11.cfg

zonemaster-cli N11-UNKNOWN-OPTION-CODE.nameserver11.xa  --raw --level info --test Nameserver/nameserver11
   0.00 INFO      GLOBAL_VERSION   version=v4.5.1


