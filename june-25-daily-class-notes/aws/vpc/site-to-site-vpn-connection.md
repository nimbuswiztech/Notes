# Site to site VPN Connection

### Site-to-Site VPN Explained

A site-to-site VPN securely links two private networks—typically a headquarters (HQ) LAN and a remote-branch LAN—so every device at either site can communicate as if they share one extended network. Instead of leasing dedicated circuits, the two sites use the public Internet but protect all traffic with an encrypted IPsec tunnel negotiated between their edge gateways.

<figure><img src="https://user-gen-media-assets.s3.amazonaws.com/gpt4o_images/548fe53e-a31d-4f2d-a542-38d26e4df572.png" alt=""><figcaption></figcaption></figure>

Site-to-Site VPN architecture: HQ and Branch connected with IPSec tunnel

### Core Components

| Component                             | Role in the tunnel                                        | Typical device examples                                           |
| ------------------------------------- | --------------------------------------------------------- | ----------------------------------------------------------------- |
| VPN Gateway                           | Performs encryption/decryption, builds the IPsec tunnel   | Firewall, router, virtual appliance                               |
| Public WAN link                       | Carries the encrypted packets between gateways            | Any ISP connection                                                |
| Encryption & Authentication protocols | Provide confidentiality, integrity, entity authentication | IKEv2 for key exchange, AES-256/SHA-256 in IPsec phase 2          |
| Internal routing                      | Moves decrypted packets inside each LAN                   | Static routes or dynamic (OSPF/BGP) advertised through the tunnel |

### How the Tunnel Forms (IKE/IPsec)

1. IKE Phase 1: gateways authenticate (pre-shared key or certificates) and build a secure control channel.
2. IKE Phase 2: they negotiate IPsec Security Associations (SAs) specifying ciphers, lifetimes, subnets.
3. Data flow: packets matching the crypto ACL are encrypted, encapsulated, sent through the public Internet, then decrypted and forwarded to the destination LAN.

### Real-Time Use Case: HQ ↔ Warehouse Inventory System

Scenario: A retailer’s HQ runs an ERP database (10.0.0.0/24). A new warehouse 500 km away needs real-time stock updates from barcode scanners on its LAN (10.1.0.0/24).

Steps:

1. Each site’s firewall is assigned a public static IP from its ISP.
2. Network engineer defines non-overlapping internal subnets and a shared pre-shared key.
3. Matching IKE/IPsec policies (AES-256, SHA-256, DH Group 14) are configured on both gateways.
4. Crypto ACLs permit traffic between 10.0.0.0/24 and 10.1.0.0/24 only, limiting exposure.
5. Static routes (or OSPF) direct ERP traffic into the tunnel; scanners update the HQ ERP in milliseconds, while all traffic remains encrypted end-to-end.

Benefits for students to note:

* Security: Data is unreadable in transit; only vetted subnets pass through.
* Cost efficiency: Uses inexpensive broadband instead of MPLS.
* Transparency: End-devices need no VPN client; the network itself is extended.
* Scalability: Additional branches can join via hub-and-spoke or full-mesh topologies using the same principles.

Key design tips:

* Avoid overlapping IP ranges across sites.
* Plan redundancy—dual tunnels or secondary gateways—to survive device/ISP failure.
* Monitor SA lifetimes and tunnel health; enable alerts on re-key failures.

With this architecture and example, students can visualize how two distant LANs operate as one secure network without touching individual endpoints, a foundational concept for modern distributed enterprises.
