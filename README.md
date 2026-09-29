Campus Network Build 

A Cisco Packet Tracer implementation of a 3-floor campus network. Floor 1 houses reception; Floor 2 houses the General Manager's office and Admin department; Floor 3 houses Marketing and R&D.

This is a working build with some requirements fully met and others still open — see below. The intent is to invite review, troubleshooting, and pull requests on the open items, not to present a completed solution, 
you can decide to test troubleshooting on any of the met unmtet requirments.

Contents
topology/ — Packet Tracer .pkt file and topology diagram
configs/ — one file per device (Core, Aggregation, Access, ASA, Edge Router)
docs/ — full requirements list, VLAN/IP plan, DHCP pool table, design rationale

Architecture summary
Core layer: two Core switches (CORE1/CORE2), OSPF Area 0, HSRP for gateway redundancy across all Core-terminated VLANs, dedicated Vlan999 transit link between the two Cores.
Aggregation layer: redundant AGG pairs per floor (F1/F2/F3), each with HSRP + tracked uplinks, acting as the L3 boundary for that floor's wired VLANs.
Access layer: access switches per floor (9 on F2 and F3, matching the full 200-terminal wired population; 1 on F1), all Layer 2 only.
Egress: ASA 5506 firewall performing NAT and default-deny inbound, sitting between the Core/Edge router and the ISP.
Wireless: currently autonomous/standalone APs — see Requirement 10 under "Next Steps."

Requirements — Met
Terminal counts and types (wired/wireless) provisioned per floor as specified
Traffic rates: 100 Mbit/s per wired client, 1000 Mbit/s per server
Layer 3 redundancy and failover — OSPF + HSRP live and verified (show standby brief clean on all Core/Agg pairs)
Traffic control — inter-VLAN ACLs, port-security, BPDU Guard/PortFast configured
Static egress IP addressing at the edge
Expansion — access-layer design supports adding floors without replacing existing devices
Wired VLAN plan — Servers, GM Office, Admin, Marketing, R&D, per-floor management, and interconnect VLANs all created
IP addressing / DHCP / gateway placement — DHCP relay and floor-appropriate gateway placement configured and verified
Guest isolation ACL, NAT, and web-server exposure (port 80 only) configured

Requirements — Next Steps (open for contribution)
Req 2 (partial): 2 Mbit/s per-wireless-client rate limit — not yet applied; requires a working AC to configure
Req 8: Per-floor wireless VLAN separation — designed on paper, not functioning end-to-end; wireless client VLANs currently live at the Core and aren't reachable from floor-local APs without further trunk/VLAN extension work
Req 10 — not met: Unified AC-managed WLAN. Two AC/WLC device models were attempted in Packet Tracer and neither could be made reachable/functional for AP registration. Currently running autonomous APs as a workaround, which does not satisfy this requirement. Open for anyone who's solved AC/WLC reachability in Packet Tracer.
Req 12: SNMPv3 configured but never demonstrated against a live NMS/poller
Req 13: Acceptance checklist — OSPF adjacency and inter/intra-network connectivity verified; wireless client detection, guest isolation in practice, and NMS management still unverified
Req 14: O&M handover documentation not yet written
How to open it

Open topology/*.pkt in Cisco Packet Tracer (version noted in the file). Device configs are also provided individually under configs/ for reference without needing to run the simulation.

Contributing

If you can resolve any of the "Next Steps" items above, or spot something wrong in the "Met" list, open an Issue or a PR. Diagnostic output (show command results) is more useful than a guess at the fix.
so happy building with you guys 
