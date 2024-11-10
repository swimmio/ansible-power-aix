---
title: Overview of NIM Master Migration
---
# Overview of NIM Master Migration

NIM Master Migration refers to the process of automating the migration of a NIM (Network Installation Manager) master machine. This is achieved using the `nim_master_migration` role.

# Purpose of NIM Master Migration

The role `nim_master_migration` specifies the NIM Master machine that is supposed to be migrated. It allows for the configuration and execution of the migration process.

# How to Use NIM Master Migration

The role includes various tasks and variables that define the migration process, such as the source and destination NIM masters, the alternate disk to be used, and the database backup file.

# Usage in Playbooks

The role is used in playbooks to automate the migration process, ensuring that all necessary steps are performed in the correct order. This example playbook demonstrates how to use the `nim_master_migration` role to automate the migration process.

# Main Functions

There are several main functions in this folder. Some of them are creating a database backup file, unconfiguring `nim_master_migration_master_b`, setting up `nim_master_migration_master_b` as a client of `nim_master_migration_master_a`, performing NIM Alt disk migration operation, rebooting the machine, copying and installing NIM master fileset, transferring and restoring the database backup. We will dive a little into creating a database backup file and performing NIM Alt disk migration operation.

## Creating Database Backup File

The function `Creating database backup file` is responsible for creating a backup of the database before the migration process begins. This ensures that all data is preserved and can be restored if needed.

## Perform NIM Alt Disk Migration Operation

The function `Perform Nim Alt disk migration operation` handles the actual migration of the NIM master machine to an alternate disk. This step is crucial as it moves the system to a new disk while keeping the original disk intact for fallback.

&nbsp;

*This is an auto-generated document by Swimm 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBYW5zaWJsZS1wb3dlci1haXglM0ElM0Fzd2ltbWlv" repo-name="ansible-power-aix"><sup>Powered by [Swimm](/)</sup></SwmMeta>
