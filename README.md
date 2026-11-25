# Enterprise Network Design (Cisco Packet Tracer)

A small enterprise network for 80 hosts, designed and configured in **Cisco Packet Tracer**: two routers, eight access switches, one VLAN per department, subnetting, OSPF routing and basic device hardening.

Built as coursework for the Fundamentals of Networks and Security module (BSc Computer Science, University of Derby).

![Logical topology](docs/topology.png)

## Topology

- **2 routers** (Cisco 2911), R1 and R2, joined by a point-to-point link
- **8 access switches** (Cisco Catalyst 2960), four under each router
- **80 PCs**, ten per switch
- Each switch carries one departmental VLAN (VLAN 10 to VLAN 80), trunked to its router with 802.1Q

## IP addressing

The network uses the `172.16.64.0/20` block, split into nine subnets:

| Subnet | Size | Use |
| --- | --- | --- |
| Eight `/28` subnets | 14 usable hosts each | One per VLAN: gateway, switch management and 10 PCs, with room to grow |
| `172.16.79.252/30` | 2 usable hosts | R1 to R2 link |

In every VLAN the gateway is the first usable address, switch management the second, and PCs use the rest. This keeps broadcast domains small and makes troubleshooting predictable.

## Routing and inter-VLAN traffic

- **Router-on-a-stick:** each router has one 802.1Q subinterface per VLAN, acting as that VLAN's gateway.
- **OSPF (area 0):** R1 and R2 advertise their LAN subnets and the transit link, so every VLAN can reach every other VLAN without static routes.

## Security hardening

- Encrypted enable secret and `service password-encryption`
- SSH-only remote access with local user accounts
- Console password and login banner

## Verification

Connectivity was tested with:

- `show ip route` (OSPF routes to all eight LANs)
- `show ip ospf neighbor` (FULL adjacency between R1 and R2)
- `show vlan brief` and `show interfaces trunk` on the switches
- `ping` and `traceroute` between PCs in different VLANs on opposite routers

## How to open

Open `network-topology.pkt` in **Cisco Packet Tracer 9.0** or later (free through Cisco Networking Academy).
