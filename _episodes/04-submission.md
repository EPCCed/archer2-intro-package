---
title: "ARCHER2 scheduler: SLURM"
teaching: 20
exercises: 10
questions:
- "How do I write job submission scripts?"
- "How do I control jobs?"
- "How do I find out what resources are available?"
objectives:
- "Understand the use of the basic SLURM commands."
- "Know what components make up and ARCHER2 scheduler."
- "Know where to look for further help on the scheduler."
keypoints:
- "ARCHER2 uses the SLURM scheduler."
- "`srun` is used to launch parallel executables in batch job submission scripts."
- "There are a number of different partitions (queues) available."
---

ARCHER2 uses the SLURM job submission system, or *scheduler*, to manage resources and how they are made
available to users. The main commands you should will use with SLURM on ARCHER2 are:

* `sinfo`: Query the current state of nodes
* `sbatch`: Submit non-interactive (batch) jobs to the scheduler
* `squeue`: List jobs in the queue
* `scancel`: Cancel a job
* `salloc`: Submit interactive jobs to the scheduler
* `srun`: Used within a batch job script or interactive job session to start a parallel program

## Finding out what resources are available: `sinfo`

The `sinfo` command shows the current state of the compute nodes known to the scheduler:

```
[auser@archer2-login1 ~]$ sinfo
```
{: .bash}
```
PARTITION       AVAIL  TIMELIMIT  NODES  STATE NODELIST
standard           up 2-00:00:00      1  fail* cn580
standard           up 2-00:00:00    128  down$ cn[96,579,793,814,1025-1044,1081-1088]
standard           up 2-00:00:00     26  maint cn[27,93-95,206,232,310,492,568,577-578,585-588,813,815-816,818,846,889,921-924,956]
standard           up 2-00:00:00      2   fail cn[274,871]
standard           up 2-00:00:00      4  down* cn[528,614,637,845]
standard           up 2-00:00:00   1034  alloc cn[1-26,28-38,40-58,62-86,88-92,97-174,176-205,207-231,233-273,275-309,311-333,335-341,344-371,373-376,378-413,415-452,454-489,493-513,515-527,529-532,535-539,541-550,554-561,563-567,569,572-576,581-584,589-595,598-601,603-613,615,617-620,623-631,633-636,638-647,651-659,661-678,680-687,690-695,697-716,718-736,738-775,777-790,792,794-812,817,819-844,847-852,854-870,872-888,890-920,925-955,957-977,980-1014,1016-1020,1023-1024,1045,1047-1070,1072-1080,1089-1105,1107-1152]
standard           up 2-00:00:00     26   idle cn[61,490-491,540,551-552,562,570,596,602,621,632,648-650,660,688-689,696,853,978-979,1015,1021-1022,1071]
```
{: .output}

There is a row for each node state and partition combination. The default output shows the following columns:

* `PARTITION` - The system partition (**TODO** confirm partitions on ARCHER2)
* `AVAIL` - The status of the partition - `up` in normal operation
* `TIMELIMIT` - Maximum runtime as `days-hours:minutes:seconds`
* `NODES` - The number of nodes in the partition/state combination
* `STATE` - The state of the listed nodes (more information below)
* `NODELIST` - A list of the nodes in the partition/state combination

The nodes can be in many different states, the most common you will see are:

* `idle` - Nodes that are not currently allocated to jobs
* `alloc` - Nodes currently allocated to jobs
* `draining` - Nodes draining and will not run further jobs until released by the systems team
* `down` - Node unavailable
* `fail` - Node is in fail state and not available for jobs
* `reserved` - Node is in an advanced reservation and is not generally available
* `maint` - Node is in a maintenance reservation and is not generally available

If you prefer to see the state of individual nodes, you can use the `sinfo -N -l` command.

> The `sinfo -N -l` command will produce a lot of output as there are over 5000 individual 
> nodes on ARCHER2!
{: .callout}

```
[auser@archer2-login1 ~]$ sinfo -N -l
```
{: .bash}
```
NODELIST           NODES       PARTITION       STATE  CPUS    S:C:T MEMORY TMP_DISK WEIGHT AVAIL_FE REASON  
    cn1                1        standard    allocated  128   2:64:1 256800        0      5  standard none                
    cn2                1        standard    allocated  128   2:64:1 256800        0      5  standard none                
    cn3                1        standard    allocated  128   2:64:1 256800        0      5  standard none                
    cn4                1        standard    allocated  128   2:64:1 256800        0      5  standard none                
    cn5                1        standard    allocated  128   2:64:1 256800        0      5  standard none                
    cn6                1        standard    allocated  128   2:64:1 256800        0      5  standard none                
    cn7                1        standard    allocated  128   2:64:1 256800        0      5  standard none                
    cn8                1        standard    allocated  128   2:64:1 256800        0      5  standard none                
    cn9                1        standard    allocated  128   2:64:1 256800        0      5  standard none                
    cn10               1        standard    allocated  128   2:64:1 256800        0      5  standard none                
    cn11               1        standard    allocated  128   2:64:1 256800        0      5  standard none                
    cn12               1        standard    allocated  128   2:64:1 256800        0      5  standard none                
    cn13               1        standard    allocated  128   2:64:1 256800        0      5  standard none                
    cn14               1        standard    allocated  128   2:64:1 256800        0      5  standard none                
    cn15               1        standard    allocated  128   2:64:1 256800        0      5  standard none                
    cn16               1        standard    allocated  128   2:64:1 256800        0      5  standard none                
    cn17               1        standard    allocated  128   2:64:1 256800        0      5  standard none                
    cn18               1        standard    allocated  128   2:64:1 256800        0      5  standard none                
    cn19               1        standard    allocated  128   2:64:1 256800        0      5  standard none                
    cn20               1        standard    allocated  128   2:64:1 256800        0      5  standard none                
    cn21               1        standard    allocated  128   2:64:1 256800        0      5  standard none 

...lots of output trimmed...

```
{: .output}

> ## Explore a compute node
> Let’s look at the resources available on the compute nodes where your jobs will actually run. Try running this
> command to see the name, CPUs and memory available on the worker nodes (the instructors will give you the ID of
> the compute node to use):
> ```
> [auser@archer2-login1 ~]$ sinfo -n cn1 -o "%n %c %m"
> ```
> {: .bash}
{: .challenge}

## Writing job submission scripts and `srun`

## Managing jobs: `sbatch`, `squeue`, `scancel`

## Other job types

### Interactive jobs: `salloc` 

**TODO** Add information on how to use solid state storage

{% include links.md %}


