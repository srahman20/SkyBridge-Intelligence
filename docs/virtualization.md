# Virtualization Setup – UTM on macOS

This document outlines how SkyBridge Intelligence was developed and tested using local virtual machines (VMs) on macOS using UTM. This setup simulates enterprise-grade infrastructure on a personal computer without the need for physical servers.

---

## Host Environment

| **Component**      | **Value**                         |
|--------------------|-----------------------------------|
| Host Machine       | Apple Silicon MacBook             |
| Hypervisor         | UTM (QEMU-based)                  |
| macOS Version      | Ventura / Sonoma (Apple Silicon)  |
| Virtualization Mode| ARM64 / x86 emulation             |

---

## Virtual Machines Used

| **VM Name**        | **Purpose**                       | **OS**                |
|--------------------|-----------------------------------|------------------------|
| `skybridge-win11`  | Hybrid client (Azure + On-prem AD)| Windows 11 ARM64       |
| `skybridge-server` | AD DS, File Share, Python backend | Windows Server 2022    |
| `skybridge-ubuntu` | Lightweight client/AI prototype   | Ubuntu 22.04 ARM64     |

---

## VM Configuration Guidelines

### Windows Server 2022

- Architecture: `x86_64 (emulated)`
- CPUs: 2
- RAM: 4 GB
- Disk: 64 GB (Dynamic)
- Network: Shared (NAT)
- Boot ISO: Windows Server Evaluation

### Windows 11 ARM

- Architecture: `ARM64`
- CPUs: 4
- RAM: 4–6 GB
- Disk: 64 GB
- Boot: Microsoft ARM Insider ISO or self-generated

### Ubuntu

- Architecture: `ARM64`
- CPUs: 2
- RAM: 2 GB
- Disk: 30 GB
- Boot: Ubuntu 22.04 ARM ISO

---

## Network Setup in UTM

- All VMs use **Shared (NAT)** for internet access
- Internal lab testing can use **Emulated VLANs** if needed
- IPs are managed via static assignment or DHCP reservation

---

## Enabling Clipboard & Drivers

For Windows VMs:

- Mount **SPICE Guest Tools ISO** for clipboard, resolution, and mouse integration
- Mount **VirtIO Drivers ISO** during Windows setup to enable network and storage drivers

---

## Snapshot Strategy

| **When**                        | **Why**                                 |
|----------------------------------|------------------------------------------|
| After OS installation            | Prevent rework from corrupt setup        |
| After AD / Azure Connect setup  | Stable rollback for identity configuration |
| After Flask API tested           | Restore point for AI functionality       |

---

## Advantages of UTM

- Runs on Apple Silicon with full virtualization or emulation
- Does not require admin/root privileges for basic operation
- Free and open-source
- Supports snapshot, USB passthrough, and advanced networking

---

## Limitations

- Windows VMs may have slower performance under x86 emulation
- No native Azure VNet connection (lab simulated locally)
- ARM Windows requires Insider Preview ISO (not stable for production use)

---

## Notes

- This virtualization stack is suitable for lab prototyping, resume projects, and proof-of-concept IT deployments.
- Future expansion can include Hyper-V, VMware ESXi, or Azure VMs for real-world infrastructure.

