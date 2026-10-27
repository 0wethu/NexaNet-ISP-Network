# NexaNet — ISP Network Engineering Portfolio

### Secure Connectivity. Engineered for Everyone.

A simulated Internet Service Provider (ISP) network built with **Cisco Packet Tracer**, demonstrating network engineering, routing, customer access, network segmentation, security controls, and service delivery.

![Cisco Packet Tracer](https://img.shields.io/badge/Platform-Cisco%20Packet%20Tracer-1686D9?style=flat-square)
![Networking](https://img.shields.io/badge/Focus-Network%20Engineering-0A7B83?style=flat-square)
![Security](https://img.shields.io/badge/Focus-Network%20Security-6B5BD2?style=flat-square)

---

## Table of Contents

- [Project Overview](#project-overview)
- [Network Topology](#network-topology)
- [Architecture and Devices](#architecture-and-devices)
- [IP Addressing and VLANs](#ip-addressing-and-vlans)
- [Routing and Upstream Connectivity](#routing-and-upstream-connectivity)
- [Network Services](#network-services)
- [Security Implementation](#security-implementation)
- [Testing and Validation](#testing-and-validation)
- [Screenshots and Evidence](#screenshots-and-evidence)
- [Repository Structure](#repository-structure)
- [How to Open the Project](#how-to-open-the-project)
- [Known Limitations and Future Improvements](#known-limitations-and-future-improvements)

---

## Project Overview

NexaNet is a simulated ISP environment designed to demonstrate the planning, configuration, testing, and documentation of a segmented network.

The topology includes a provider edge, core routers, two simulated upstream providers, residential and business access networks, public-facing services, and a dedicated management network.

The project was developed as a technical portfolio to demonstrate practical networking skills relevant to network engineering, network administration, and cybersecurity roles.

### Project Objectives

- Design and implement a multi-layer ISP-style network.
- Configure IPv4 addressing and static routing.
- Simulate connectivity through two upstream providers.
- Implement residential and business customer access.
- Configure VLANs, trunk links, and DHCP services.
- Deploy DNS and HTTP services.
- Apply access control lists to restrict unauthorised traffic.
- Secure network administration using SSH version 2.
- Test connectivity, routing preference, customer isolation, and service access.
- Document the configuration and test evidence.

## Network Topology

The topology separates the provider edge and core, customer access networks, public services, and network management.
<img width="1920" height="1080" alt="TOPOLOGY" src="https://github.com/user-attachments/assets/736e4d01-6106-4b36-8a88-926ee79f93b3" />

*Figure 1: NexaNet logical and physical topology.*

## Architecture and Devices

| Device | Role |
|---|---|
| NX-EDGE1 | Provider edge and upstream connectivity |
| NX-CORE1 | Core routing |
| NX-CORE2 | Core routing and distribution connectivity |
| NX-UP1 | Primary simulated upstream provider |
| NX-UP2 | Secondary simulated upstream provider |
| NX-BNG-RES | Residential customer access and isolation |
| NX-AGG-BIZ | Business VLAN gateways and DHCP |
| NX-SW-RES | Residential access switching |
| NX-SW-BIZ | Business access switching |
| NX-SW-SVC | Public services switching |
| NX-SW-MGMT | Management network switching |
| NX-SW-DIST | Layer 3 distribution and service gateways |
| NX-DNSWEB | Public DNS and web server |
| NX-NMS | Internal network operations server |
| NX-EXTWEB | External test server on UP1 |
| NX-UP2-EXTWEB | External test server on UP2 |

The Packet Tracer environment also includes end-user computers, laptops, and wireless access points for residential, business, guest, and administrative scenarios.

## IP Addressing and VLANs

### Business and service networks

| VLAN | Purpose | Network | Default gateway |
|---|---|---|---|
| 10 | Business staff | `10.20.10.0/24` | `10.20.10.1` |
| 20 | Business guest | `10.20.20.0/24` | `10.20.20.1` |
| 30 | Business administration | `10.20.30.0/24` | `10.20.30.1` |
| 40 | Network operations | `10.10.40.0/24` | `10.10.40.1` |
| 50 | Public DMZ | `10.10.30.0/24` | `10.10.30.1` |
| 99 | Network management | `10.10.99.0/24` | `10.10.99.1` |

### Residential customer networks

| Customer network | IPv4 network | Default gateway |
|---|---|---|
| Residential A | `100.64.1.0/24` | `100.64.1.1` |
| Residential B | `100.64.2.0/24` | `100.64.2.1` |

### Simulated external networks

| Upstream | External test network | Gateway |
|---|---|---|
| NX-UP1 | `198.51.100.0/24` | `198.51.100.1` |
| NX-UP2 | `203.0.113.0/24` | `203.0.113.1` |

The design uses private addressing for internal networks, shared address space for residential customers, and documentation-only IPv4 ranges for simulated external networks.

## Routing and Upstream Connectivity

NX-EDGE1 connects to two simulated upstream providers. Static routes provide reachability between internal networks and the external test networks.

A primary default route prefers NX-UP1. A floating static default route through NX-UP2 uses a higher administrative distance.

The failover test involved temporarily removing the primary default route, checking the routing table, and verifying connectivity through the secondary upstream. The primary route was subsequently restored.
<img width="1920" height="1080" alt="Backup 10 255 0 6 10" src="https://github.com/user-attachments/assets/3ae423c1-e0e7-4b27-b0a3-8396e1e9d7c4" />

<img width="1920" height="1080" alt="Primary 10 255 0 2" src="https://github.com/user-attachments/assets/edd1fce8-1300-4b39-9bc6-50c389d16f34" />



*Figure 2: Default route selection and upstream routing evidence.*

**Important distinction:** this validates floating static route selection when the primary route is removed from the routing table. It does not prove automatic detection of every upstream failure or full end-to-end network redundancy.

## Network Services

### DHCP

DHCP pools provide dynamic IPv4 configuration to residential and business customer networks, including DNS information.

<img width="1920" height="1080" alt="Residential DHCP" src="https://github.com/user-attachments/assets/a1893be6-daa6-4887-9f4f-635f47373158" />


*Figure 3: Residential DHCP lease evidence.*

<img width="1920" height="1080" alt="Business DHCP" src="https://github.com/user-attachments/assets/2d91e08a-257c-442c-9b7c-c9f0b24e754f" />


*Figure 4: Business DHCP lease evidence.*

### DNS and HTTP

NX-DNSWEB provides the simulated public DNS and web services.

| Setting | Value |
|---|---|
| Server address | `10.10.30.10` |
| DNS record | `www.nexanet.test` |
| Record type | A |
| Destination address | `10.10.30.10` |

The domain is intended for the simulated lab and is not a public internet domain.

<img width="1920" height="1080" alt="DNS Record" src="https://github.com/user-attachments/assets/9fd34de7-89a4-44c7-9528-6d3eb9d19b37" />


*Figure 5: DNS record for the NexaNet website.*

The website was tested from business staff and guest networks.

<img width="1920" height="1080" alt="Guest Web Access" src="https://github.com/user-attachments/assets/3a75a87c-2bac-49e5-b51b-6a903c52ab70" />


*Figure 6: Website access from the business guest network.*

## Security Implementation

### Business guest isolation

An extended ACL restricts guest access to internal networks while permitting intended gateway, DNS, and web traffic.

<img width="1920" height="1080" alt="Business guest isolation ACL" src="https://github.com/user-attachments/assets/743f148b-a358-4e98-be50-da2b668b9d94" />


*Figure 7: Business guest traffic restrictions.*

### Residential customer isolation

An ACL on NX-BNG-RES blocks direct traffic between Residential A and Residential B while allowing other traffic permitted by the policy.

<img width="1920" height="1080" alt="Residential Isolation ACLs" src="https://github.com/user-attachments/assets/11fb703a-4c7a-4ce8-ad2d-6c8ca5ada9b2" />


*Figure 8: Residential customer isolation policy.*

### SSH management security

SSH version 2 is configured for administrative access to the distribution switch. A standard ACL restricts access to the authorised management host.

<img width="1920" height="1080" alt="SSH Admin success" src="https://github.com/user-attachments/assets/42690fde-14d8-4aec-8435-40bba13811b5" />


*Figure 9: Successful administrative SSH access.*

<img width="1920" height="1080" alt="SSH-Unauthorized-Denied" src="https://github.com/user-attachments/assets/dccdedc4-a32b-408f-9075-3e7d00ab2548" />


*Figure 10: Unauthorised SSH access rejected.*

Credentials and sensitive configuration values must be changed or removed before publishing the project.

## Testing and Validation

The following outcomes were observed during implementation.

| Test | Expected result | Observed result |
|---|---|---|
| Configured router interfaces | Operational interfaces | Verified on configured links |
| Static routing | Routes use intended next hops | Verified |
| Primary default route | NX-UP1 preferred | Verified |
| Floating static route | NX-UP2 selected when primary route is removed | Verified |
| Residential DHCP | Clients receive addresses | Verified |
| Business DHCP | Staff and guest clients receive addresses | Verified |
| Residential isolation | Residential A and B are isolated | Verified |
| Guest ACL | Restricted internal access | Verified |
| Website access | Website loads from staff and guest networks | Verified |
| SSH access control | Administrator permitted, unauthorised host denied | Verified |
| DNS configuration | Lab domain maps to the web server address | Configured and screenshot evidence collected |

Results should be rechecked against the final saved `.pkt` file before publication. The screenshots provide supporting evidence for the tests performed.

## Screenshots and Evidence

The `screenshots/` directory is intended to contain the following evidence:

| File | Evidence |
|---|---|
| `01-topology.png` | Overall network topology |
| `02-routing.png` | Upstream default routes |
| `03-business-vlans.png` | Business VLAN configuration |
| `04-business-trunk.png` | Business trunk configuration |
| `05-distribution-interfaces.png` | Distribution switch interfaces |
| `06-distribution-routing.png` | Distribution routing table |
| `07-residential-dhcp.png` | Residential DHCP leases |
| `08-business-dhcp.png` | Business DHCP leases |
| `09-guest-web-access.png` | Guest website access |
| `10-business-guest-acl.png` | Business guest ACL |
| `11-residential-isolation-acl.png` | Residential isolation ACL |
| `12-ssh-admin-success.png` | Authorised SSH access |
| `13-ssh-unauthorized-denied.png` | Unauthorised SSH rejection |
| `14-dns-record.png` | DNS A record |

Ensure that each image is actually uploaded under the matching filename. GitHub image links will not work if the path or filename differs.

## Repository Structure

The intended repository layout is:

```text
NexaNet-ISP-Network-Portfolio/
├── README.md
├── NexaNet_ISP_Network_Final.pkt
├── NexaNet_Portfolio_Documentation.docx
├── configs/
│   ├── NX-EDGE1.txt
│   ├── NX-CORE1.txt
│   ├── NX-CORE2.txt
│   ├── NX-UP1.txt
│   ├── NX-UP2.txt
│   ├── NX-BNG-RES.txt
│   ├── NX-AGG-BIZ.txt
│   ├── NX-SW-RES.txt
│   ├── NX-SW-BIZ.txt
│   ├── NX-SW-SVC.txt
│   ├── NX-SW-MGMT.txt
│   └── NX-SW-DIST.txt
└── screenshots/
    ├── 01-topology.png
    ├── 02-routing.png
    ├── 03-business-vlans.png
    ├── 04-business-trunk.png
    ├── 05-distribution-interfaces.png
    ├── 06-distribution-routing.png
    ├── 07-residential-dhcp.png
    ├── 08-business-dhcp.png
    ├── 09-guest-web-access.png
    ├── 10-business-guest-acl.png
    ├── 11-residential-isolation-acl.png
    ├── 12-ssh-admin-success.png
    ├── 13-ssh-unauthorized-denied.png
    └── 14-dns-record.png
```

This is the intended structure. Update the names if your saved files use different filenames.

## How to Open the Project

1. Install a compatible version of Cisco Packet Tracer.
2. Clone or download this repository.
3. Open `NexaNet_ISP_Network_Final.pkt` in Packet Tracer.
4. Inspect the network topology and device configurations.
5. Review the `configs/` directory for exported router and switch configurations.
6. Review the screenshots and supporting documentation for evidence of implementation and testing.

## Known Limitations and Future Improvements

- The upstream providers are simulated in Packet Tracer.
- The PT-Cloud object is visual-only; simulated upstream connectivity is provided by NX-UP1 and NX-UP2.
- Floating static route preference was tested, but automatic failure detection for all upstream failures was not established.
- The topology does not provide full redundant paths for every internal device and customer segment.
- Future improvements could include dynamic routing, more comprehensive failure detection, redundant access paths, centralised logging, network monitoring, and expanded security testing.

## Project Outcome

NexaNet demonstrates a practical approach to designing, configuring, securing, and validating an ISP-style network in a simulation environment.

The project combines routing, customer access, DHCP, DNS and HTTP services, VLAN segmentation, ACL-based traffic restrictions, SSH management controls, testing, and technical documentation.

It is intended to support discussion of network design decisions, troubleshooting, security policies, configuration practices, and future improvements during technical interviews.

---

**Project:** NexaNet ISP Network Engineering Portfolio  
**Tagline:** Secure Connectivity. Engineered for Everyone.  
**Platform:** Cisco Packet Tracer  
**Focus:** Network Engineering, Routing, Segmentation, and Security

**Kwanele Khumalo**

