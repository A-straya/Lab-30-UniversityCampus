# Deployment and Validation

## Before configuration

- Confirm R1 has an available `GigabitEthernet0/1` for the new ISP link.
- Confirm the serial modules and interface names match the Packet Tracer routers.
- Confirm MLS1 is a 3560 with `Gi1/0/1–9` and MLS2 has `Gi1/0/1–3`.
- Confirm access switches use `Fa0/1–Fa0/24`.
- If the DCE end of a serial cable requires clocking, apply `clock rate 64000` on that DCE interface only. Verify with `show controllers serial`.

## Server-PT

Configure static IPv4:
- IP: `192.168.1.10`
- Mask: `255.255.255.0`
- Gateway: `192.168.1.1`
- DNS: `192.168.1.10` (only if DNS service is enabled on this server)

Enable DNS service and create records if name resolution is part of the lab. The router DHCP pools advertise this DNS address.

## Endpoint setup

Set PCs to DHCP. The DHCP server on R2 excludes `.1` through `.20` in every user subnet. The access switch management address is `.2`; end-user leases therefore begin at `.21`.

Printers can use DHCP or static/reserved addresses. If static, choose an unused address outside the excluded range and document it.

## Verification commands

### All routers and multilayer switches
```cisco
show ip interface brief
show ip route
show ip ospf neighbor
show ip protocols
```

### R2 DHCP
```cisco
show ip dhcp pool
show ip dhcp binding
show ip dhcp conflict
```

### R1 PAT
```cisco
show ip nat translations
show ip nat statistics
show access-lists
```

### Switches
```cisco
show vlan brief
show interfaces trunk
show spanning-tree
show port-security
show port-security interface fastEthernet0/2
```

## Expected tests

1. Every routed link is `up/up`.
2. OSPF adjacencies form on R1–R2, R1–R3, R1–MLS1 and R3–MLS2.
3. PCs receive correct subnet, gateway and DNS via DHCP.
4. Campus clients can reach their gateway.
5. A permitted client can ping `198.51.100.1`; then inspect PAT translations on R1.
6. A VLAN 100 client can query DNS at `192.168.1.10`, but cannot access other private campus/server addresses.
7. SSH to network devices is accepted from the IT VLAN and rejected from other VLANs.

## Troubleshooting notes

- Serial links down: verify `no shutdown`, correct cable/port, DCE clock rate and matching encapsulation.
- OSPF neighbor missing: verify IP/mask, area 0, interface state and that the interface is not passive.
- DHCP failure: verify pools on R2, `ip helper-address` on every SVI, OSPF route to R2, and ACL DHCP permit.
- PAT failure: verify R1 inside/outside markings, NAT ACL, default route, and that the client traffic reaches R1.
- Trunk failure: verify matching VLAN, trunk mode and allowed VLAN on both ends.
