---
title: "Overview of the ARCHER2 system"
teaching: 20
exercises: 0
questions:
- "What hardware and software is available on ARCHER2?"
- "How does the hardware fit together?"
- "How does this impact me as a user?"
objectives:
- "Gain an overview of the technology available on the ARCHER2 service."
- "Understand the impact of the technology on my use of the service."
keypoints:
- "ARCHER2 consists of different components."
- "The system is based on standard Linux with command line access."
---

The main ARCHER2 service is provided by a Cray Shasta system.

## Architecture

The Cray Shasta system consists of a number of different node types. The ones visible
to users are:

* Login nodes
* Compute nodes
* Data analysis (pre-/post- processing) nodes

All of the node types have the same processors: AMD EPYC Zen2 7742, 2.25GHz, 64-cores. All nodes are dual socket nodes so there are 128 cores per node.

## Compute nodes

There are 5,585 compute nodes in total giving 748,856 compute cores on ARCHER2. There
are 5,300 standard compute nodes with 256 GiB memory per node and 300 high memory 
compute nodes with 512 GiB of memory per node. All of the compute nodes are linked
together using the high-performance Cray Slingshot interconnect.

Access to the compute nodes is controlled by the SLURM scheduling system which supports
both batch jobs and interactive jobs.

## Storage

There are three different storage systems available on ARCHER2:

* Home
* Work
* Solid State

### Home

The home file systems are available on the login nodes only and are designed for the storage
of critical source code and data for ARCHER2 users. They are backed-up regularly offsite for
disaster recovery purposes - restoration of accidentally deleted files is not supported. There is a
total of 1 PB usable space available on the home file systems.

All users have their own directory on the home file systems at:

```
/home/<projectID>/<subprojectID>/<userID>
```

For example, if your username is `auser` and you are in the project `t01` then your main home
directory will be at:

```
/home/t01/t01/auser
```

> ## Subprojects?
> Some large projects may choose to split their resources into multiple subprojects. These 
> subprojects will have identifiers prepended with the main project ID. For example, the
> `rse` subgroup of the `t01` project would have the ID `t01-rse`. If the main project has
> allocated storage quotas to the subproject the directories for this storage will be 
> found at, for example:
> ```
> /home/t01/t01-rse/auser
> ```
> Your default home directory will generally not be changed when you are made a member of 
> a subproject so you must change diectories manually (or change the ownership of files)
> to make use of this different storage quota allocation.

### Work

The work file systems are available on the home, compute and data analysis nodes, are
designed for high performance parallel access and are the primary location that jobs running on
the compute nodes will read data from and write data to. They are based on the Lustre parallel
file system technology. The work file systems are not backed up in any way. There is a total of 
14.5 PB usable space available on the work file systems.

### Solid State

The solid state storage system is available on the compute nodes and is designed for
the highest read and write performance to improve performance of workloads that are I/O bound in
some way. Access to solid state storage resources is controlled through the SLURM scheduling 
system. The solid state storage is not backed up in any way. There is a total of 1.1 PB usable
space available on the solid state storage system.

**TODO** Add RDF details.

## System software

OS

Compilers

Supported shells

> ## What about your research?
>
> Speak to your neighbour about your planned use of ARCHER2. Given what you now know about the system,
> what do you think the biggest opportunities are for your research in using ARCHER2? What do you think
> the largest challenges are going to be for you?
{: .challenge}

{% include links.md %}

