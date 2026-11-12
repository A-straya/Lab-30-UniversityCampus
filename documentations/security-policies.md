# Security Policies

## Student Lab isolation

An extended ACL is applied inbound on MLS2 VLAN 100. It:
1. Permits DHCP client-to-server relay packets.
2. Permits DNS to Server-PT at 192.168.1.10.
3. Denies access to other 172.16.0.0/16 and 192.168.1.0/24 destinations.
4. Permits other destinations, which can be translated by PAT at R1.

If Server-PT is not running DNS, configure its DNS service or change the DHCP DNS option and ACL to match the actual DNS server.

## Device management

SSH only is enabled on VTY lines. The VTY access list permits management from `172.16.70.0/24` (IT VLAN). Replace the sample password before publishing.

## Layer 2 protections

Rapid-PVST+, PortFast and BPDU Guard are enabled. Port Security is enabled on PC and printer ports with sticky learning and a maximum of two MAC addresses. Unused FastEthernet ports 4–24 are shut down.

## Scope

These controls are a baseline lab demonstration, not a complete production security architecture. Add AAA, centralized logging, NTP, dedicated management VLANs, DHCP snooping and Dynamic ARP Inspection in a later iteration.
