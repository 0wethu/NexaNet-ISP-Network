# NexaNet — ISP Network Engineering Portfolio

**Tagline:** Secure Connectivity. Engineered for Everyone.

## 1. Project Overview

NexaNet is a simulated Internet Service Provider (ISP) network designed and implemented in Cisco Packet Tracer. The project demonstrates practical networking, routing, customer access, network segmentation, security controls, and service delivery in a multi-layer network environment.

The objective is to design, configure, test, and document a network that supports residential customers, business customers, public-facing services, and internal network operations.

This project was developed as a technical portfolio to demonstrate practical skills relevant to network engineering, network administration, and cybersecurity roles.

## 2. Project Objectives

- Design a structured ISP network topology.
- Configure routing between the provider edge, core, and customer access networks.
- Simulate connectivity to two upstream providers.
- Implement residential and business customer networks.
- Configure VLANs and DHCP services.
- Deploy DNS and HTTP services.
- Apply access control lists (ACLs) to enforce network security policies.
- Restrict administrative SSH access.
- Test connectivity, routing preference, customer isolation, and service availability.
- Document device configurations, addressing, screenshots, and test results.

## 3. Technologies and Concepts

- Cisco Packet Tracer
- Cisco IOS-style CLI configuration
- IPv4 addressing and subnetting
- Static routing and default routes
- Floating static routes
- VLANs and 802.1Q trunking
- Router subinterfaces
- DHCP
- DNS and HTTP
- Extended and standard ACLs
- SSH version 2
- Network segmentation
- Connectivity testing and troubleshooting

## 4. Network Architecture

The simulated network includes the following components:

| Component | Purpose |
|---|---|
| NX-EDGE1 | Provider edge and upstream connectivity |
| NX-CORE1 / NX-CORE2 | Core routing |
| NX-UP1 / NX-UP2 | Simulated upstream providers |
| NX-BNG-RES | Residential customer access |
| NX-AGG-BIZ | Business customer aggregation |
| NX-SW-DIST | Layer 3 distribution and service gateways |
| NX-SW-RES / NX-SW-BIZ | Residential and business access switching |
| NX-SW-SVC / NX-SW-MGMT | Service and management switching |
| NX-DNSWEB | Public DNS and web services |
| NX-NMS | Internal network operations server |
| NX-EXTWEB / NX-UP2-EXTWEB | External test web servers |

The topology separates customer access, core routing, public services, and network management into distinct logical areas.

## 5. Addressing and VLAN Design

### Business and service networks

| VLAN | Purpose | Network | Gateway |
|---|---|---|---|
| 10 | Business staff | 10.20.10.0/24 | 10.20.10.1 |
| 20 | Business guest | 10.20.20.0/24 | 10.20.20.1 |
| 30 | Business administration | 10.20.30.0/24 | 10.20.30.1 |
| 40 | Network operations | 10.10.40.0/24 | 10.10.40.1 |
| 50 | Public DMZ | 10.10.30.0/24 | 10.10.30.1 |
| 99 | Network management | 10.10.99.0/24 | 10.10.99.1 |

### Residential networks

| Customer network | Network | Gateway |
|---|---|---|
| Residential A | 100.64.1.0/24 | 100.64.1.1 |
| Residential B | 100.64.2.0/24 | 100.64.2.1 |

### Simulated external networks

| Upstream | External test network | Gateway |
|---|---|---|
| NX-UP1 | 198.51.100.0/24 | 198.51.100.1 |
| NX-UP2 | 203.0.113.0/24 | 203.0.113.1 |

The addressing plan uses private addressing for internal business and infrastructure networks, shared address space for residential customers, and documentation-only public IPv4 ranges for simulated external networks.

## 6. Routing and Upstream Connectivity

NX-EDGE1 connects to two simulated upstream providers. Static routes are used to reach internal and external networks.

A primary default route prefers NX-UP1, while a floating static default route provides a higher administrative distance through NX-UP2.

The project tested route preference by temporarily removing the primary default route and verifying that traffic could use the backup route. The primary route was subsequently restored.

**Scope limitation:** this demonstrates floating static route selection when the primary route is removed from the routing table. It does not establish automatic detection of every upstream failure or complete end-to-end redundancy.

## 7. Network Services

### DHCP

DHCP pools provide dynamic IPv4 configuration for residential and business customer networks. The configured pools supply addressing information and the DNS server address.

### DNS and HTTP

NX-DNSWEB hosts the simulated public website and DNS service.

- DNS name: `www.nexanet.test`
- Web/DNS server address: `10.10.30.10`

The website was tested from business staff and guest networks.

### Internal operations

NX-NMS represents an internal network operations service, separated from the public-facing service network.

## 8. Security Implementation

### Business guest isolation

An extended ACL restricts business guest traffic from accessing internal infrastructure, business networks, and residential customer networks, while allowing the intended gateway, DNS, and web traffic.

### Residential customer isolation

The residential access router applies an ACL to prevent communication between Residential A and Residential B while permitting other traffic allowed by the configured policy.

### SSH management access

SSH version 2 is configured for secure administrative access on the distribution switch. A standard ACL restricts management access to the authorised management host.

Testing confirmed that authorised administration was permitted and an unauthorised staff host was denied.

**Security note:** Credentials must be rotated and sensitive values removed from configuration exports before the project is shared publicly.

## 9. Testing and Validation

The following tests were performed during implementation:

| Test | Expected outcome | Result |
|---|---|---|
| Router interface status | Configured links operational | Verified on configured links |
| Static routing | Routes resolve through intended next hops | Verified |
| Primary default route | NX-UP1 preferred | Verified |
| Floating default route | NX-UP2 selected after primary route removal | Verified |
| Residential DHCP | Customer clients receive addresses | Verified |
| Business DHCP | Staff and guest clients receive addresses | Verified |
| Residential isolation | Residential A and B cannot communicate directly | Verified |
| Guest access controls | Internal destinations blocked by policy | Verified |
| Public website | Website accessible from staff and guest networks | Verified |
| SSH management | Authorised host accepted; unauthorised host denied | Verified |
| DNS record | `www.nexanet.test` points to `10.10.30.10` | Configured; see screenshot evidence |

Results describe the tests performed during implementation and should be rechecked against the final saved Packet Tracer file before publication.

## 10. Repository Structure

```text
NexaNet-Portfolio/
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
```

The folder names and filenames above describe the intended layout. Ensure the files are present and named consistently before publishing.

## 11. How to Open the Project

1. Install a compatible version of Cisco Packet Tracer.
2. Download or clone the portfolio repository.
3. Open `NexaNet_ISP_Network_Final.pkt` in Packet Tracer.
4. Inspect the topology and device configurations.
5. Review the `configs/` folder for exported device configurations.
6. Review the `screenshots/` folder and supporting documentation for implementation evidence.

The `www.nexanet.test` domain is a simulated lab name and is not intended to represent a public internet website.

## 12. Known Limitations and Future Improvements

- The upstream providers are simulated within Packet Tracer.
- The PT-Cloud object is visual-only; simulated upstream connectivity is provided by NX-UP1 and NX-UP2.
- Floating static route preference was tested, but automatic failure detection for all upstream failures was not established.
- The internal topology does not provide complete redundant paths for every device or customer segment.
- Further development could include dynamic routing, improved failure detection, redundant access paths, centralised monitoring, logging, and more extensive security testing.

## 13. Project Outcome

NexaNet demonstrates a practical approach to building and validating a segmented ISP-style network in a network simulation environment. It brings together routing, customer access, address allocation, DNS and web services, traffic restrictions, administrative access control, and technical documentation.

The project is intended to support discussion of network design decisions, configuration practices, troubleshooting methods, security controls, and potential improvements during technical interviews.

---

**Project:** NexaNet ISP Network Engineering Portfolio  
**Tagline:** Secure Connectivity. Engineered for Everyone.  
**Environment:** Cisco Packet Tracer  
**Purpose:** Networking and cybersecurity skills demonstration
**Kwanele Khumalo**
