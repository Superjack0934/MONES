# MONES Architecture

## 1. Architectural Overview

MONES is designed as a multi-layer system that orchestrates multiple isolated operating systems on a single machine.

The system does not aim to replace hypervisors.
Instead, it builds a structured orchestration layer on top of an existing virtualization backend.

High-Level Stack:

Hardware  
↓  
Host Kernel (Linux - Phase 1)  
↓  
Hypervisor (KVM)  
↓  
MONES Orchestration Layer  
↓  
Isolated Guest Operating Systems  

---

## 2. Core Components

### 2.1 Host Layer (Phase 1)

The host layer will initially be a minimal Linux system using KVM for hardware-assisted virtualization.

Responsibilities:
- Provide hardware access
- Run the hypervisor
- Enforce base-level isolation
- Run the MONES orchestration daemon

This choice prioritizes:
- Stability
- Development speed
- Ecosystem maturity

Future versions may explore alternative hypervisors.

---

### 2.2 Hypervisor Layer

The hypervisor is responsible for:
- CPU virtualization
- Memory isolation
- Device virtualization
- VM lifecycle execution

In Phase 1, MONES will use KVM via libvirt.

MONES does not implement CPU virtualization itself.

---

### 2.3 MONES Orchestration Layer

This is the core innovation layer.

It consists of:

- Orchestration Daemon
- CLI Interface
- Configuration Engine
- Policy Engine

Responsibilities:
- Create and manage VMs
- Allocate resources
- Enforce isolation policies
- Handle snapshots
- Manage switching logic
- Track VM state

This layer treats each OS as a first-class environment.

---

### 2.4 Guest OS Environments

Each guest OS runs in its own isolated virtual machine.

Properties:
- Separate virtual disk
- Separate virtual network
- Independent memory allocation
- No implicit resource sharing

Guest OS types may include:
- Linux distributions
- Windows
- BSD
- Custom experimental operating systems

---

## 3. Switching Model (Initial Concept)

MONES switching model aims to feel like workspace switching rather than traditional VM management.

Phase 1:
- VMs run windowed or fullscreen
- Switching handled by window manager

Phase 2:
- Preloaded VMs
- Fast suspend/resume
- Snapshot-assisted resume

Long-term goal:
- Near-instant environment switching

---

## 4. Security Model (Initial Design)

Security assumptions:

- The host layer is trusted.
- Guest OS environments are untrusted by default.
- Guests must not access:
  - Host memory
  - Other guest disks
  - Other guest memory

Isolation mechanisms:
- Hypervisor-enforced memory separation
- Separate virtual networks
- No shared clipboard by default
- Strict device passthrough policies

Future work:
- Formal threat model documentation
- Inter-VM communication policy design

---

## 5. Resource Management Model

Initial policy goals:

- Fixed RAM allocation per VM
- CPU core limits
- Thin-provisioned disk images
- Automatic suspend for idle VMs

Design goal:
MONES must remain usable on consumer hardware.

---

## 6. Architectural Phases

Phase 0 — Research & Design  
Phase 1 — CLI-based orchestration prototype  
Phase 2 — Basic VM lifecycle control  
Phase 3 — Switching UX layer  
Phase 4 — Security hardening  
Phase 5 — Performance optimization  

---

## 7. Non-Goals (For Now)

- Writing a custom hypervisor
- Writing a custom kernel
- Competing with enterprise virtualization platforms
- Supporting every hardware configuration immediately

MONES is a research-driven orchestration system first.
