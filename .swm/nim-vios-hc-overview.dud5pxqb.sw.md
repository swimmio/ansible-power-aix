---
title: NIM VIOS HC Overview
---
# Overview

NIM VIOS HC is a health assessment tool designed for VIOS pre-install routines. It checks if a VIO Server or a pair of VIO Servers can be updated.

# Purpose

The primary purpose of NIM VIOS HC is to ensure that VIO Servers are ready for updates. It performs a series of checks to validate the current state of the servers.

# Checks Performed

The tool performs various checks including active client LPARs, vSCSI mapping, NPIV Path for Fibre Channel configuration, SEA configuration, and VNIC configuration.

# Execution

NIM VIOS HC is executed on the NIM master and uses `lsnim` commands to gather necessary information.

# Interaction with HMC

The tool interacts with the HMC REST API using Curl (pycurl) and retrieves HMC login/password from the HMC password file if not specified. A HMC session key is used for Curl requests to ensure secure communication.

# Example Usage

An example usage of NIM VIOS HC is to perform a health check on a pair of VIO Servers using the command: `vioshc [-u id] [-p pwd] -i hmc_ip_addr -m managed_system_uuid -U vios_uuid -U vios_uuid [-v]`.

&nbsp;

*This is an auto-generated document by Swimm 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBYW5zaWJsZS1wb3dlci1haXglM0ElM0Fzd2ltbWlv" repo-name="ansible-power-aix"><sup>Powered by [Swimm](/)</sup></SwmMeta>
