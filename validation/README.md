# Validation

## Purpose
This folder contains the validation results for Project 02: a multilayer campus redundancy design using inter-VLAN routing, HSRP, trunking, and STP.

The goal of validation was to confirm that:

- VLAN segmentation was working correctly
- trunk links were carrying the required VLANs
- HSRP provided first-hop gateway redundancy
- STP prevented Layer 2 loops while preserving alternate paths
- end-to-end connectivity was maintained during failure scenarios

## Validation Structure

- `baseline/`  
  Baseline operational checks before any failure was introduced.

- `failover-test-1-hsrp/`  
  Validation of first-hop redundancy by simulating loss of the active distribution switch path and confirming gateway failover.

- `failover-test-2-stp/`  
  Validation of Layer 2 redundancy by shutting down a redundant uplink and confirming traffic recovery through the alternate path.

- `recovery/`  
  Post-test restoration checks to confirm the topology returned to the intended steady state.

## Baseline Summary
The following baseline conditions were verified before failover testing:

- VLANs 10, 20, 30, 99, and 999 existed on all switches
- trunk links were operational and carrying VLANs 10, 20, 30, 99, and 999
- native VLAN was set to 999 across trunk links
- HSRP was operating as intended:
  - VLAN 10 active gateway on DSW1
  - VLAN 20 active gateway on DSW1
  - VLAN 30 active gateway on DSW2
- hosts in each VLAN successfully reached:
  - their default gateway
  - hosts in other VLANs

## HSRP Design Intent
The HSRP configuration was designed for split gateway ownership:

- **DSW1 active**
  - VLAN 10
  - VLAN 20

- **DSW2 active**
  - VLAN 30

This allowed gateway load distribution while preserving first-hop redundancy.

## STP Design Intent
Spanning Tree Protocol was used to prevent Layer 2 loops created by redundant uplinks between the access and distribution layers.

During validation, some trunk ports appeared in a non-forwarding state for specific VLANs, which was expected behavior in a redundant topology.

## Packet Tracer Limitations
This project was built and validated in Cisco Packet Tracer. Some HSRP verification behavior in Packet Tracer is simplified compared to real Cisco IOS platforms.

Because of this, validation relied on:

- supported show commands
- operational trunk and STP checks
- end-to-end ping testing
- observed failover behavior during simulated link or switch-path loss

## Overall Result
Baseline checks passed, and the topology was confirmed to be ready for structured failover validation.

The project successfully demonstrated:

- inter-VLAN routing
- first-hop redundancy using HSRP
- Layer 2 path redundancy using STP
- connectivity continuity during fault scenarios
