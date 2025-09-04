# Networking Configuration

This document outlines the virtual networking components configured within the SkyBridge Intelligence project to ensure secure, performant, and scalable connectivity between Azure services, virtual machines, and cognitive resources.

---

## Virtual Network (VNet)

| **Setting**         | **Value**                     |
|---------------------|-------------------------------|
| Name                | `skybridge-vnet`              |
| Address Space       | `10.0.0.0/16`                  |
| Subnet              | `skybridge-subnet`            |
| Subnet Range        | `10.0.1.0/24`                  |
| Region              | Same as AI Resource Group     |
| DNS Settings        | Default (Azure-provided)      |

---

## Network Security Group (NSG)

| **Setting**         | **Value**                           |
|---------------------|-------------------------------------|
| Name                | `skybridge-nsg`                     |
| Associated Subnet   | `skybridge-subnet`                  |
| Inbound Rules       | Allow SSH (port 22), HTTP (port 80) |
| Outbound Rules      | Allow all                           |

---

## Public IP

| **Setting**         | **Value**               |
|---------------------|-------------------------|
| Name                | `skybridge-ip`          |
| Allocation Method   | Dynamic or Static       |
| Associated With     | Virtual Machine NIC     |

---

## Network Interface Card (NIC)

| **Setting**         | **Value**               |
|---------------------|-------------------------|
| Name                | `skybridge-nic`         |
| Associated With     | Virtual Machine         |
| Subnet              | `skybridge-subnet`      |
| NSG Attached        | `skybridge-nsg`         |

---

## Notes

- The VNet isolates resources from the public internet unless explicitly exposed.
- The NSG enforces traffic rules for basic security.
- Dynamic IP is sufficient for non-critical environments; use static IP for consistent endpoint access.
- Ensure VNet region matches with Cognitive Services for optimal latency and compatibility.
