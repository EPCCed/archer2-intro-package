---
title: "ARCHER2 scheduler: Slurm"
teaching: 30
exercises: 20
questions:
- "How do I write job submission scripts?"
- "How do I control jobs?"
- "How do I find out what resources are available?"
objectives:
- "Understand the use of the basic Slurm commands."
- "Know what components make up and ARCHER2 scheduler."
- "Know where to look for further help on the scheduler."
keypoints:
- "ARCHER2 uses the Slurm scheduler."
- "`srun` is used to launch parallel executables in batch job submission scripts."
- "There are a number of different partitions (queues) available."
---

ARCHER2 uses the Slurm job submission system, or *scheduler*, to manage resources and how they are made
available to users. The main commands you will use with Slurm on ARCHER2 are:

* `sinfo`: Query the current state of nodes
* `sbatch`: Submit non-interactive (batch) jobs to the scheduler
* `squeue`: List jobs in the queue
* `scancel`: Cancel a job
* `salloc`: Submit interactive jobs to the scheduler
* `srun`: Used within a batch job script or interactive job session to start a parallel program

Full documentation on Slurm on ARCHER2 can be found in the [Running Jobs on ARCHER2](https://docs.archer2.ac.uk/user-guide/scheduler/) section of the ARCHER2 documentations.

## Finding out what resources are available: `sinfo`

The `sinfo` command shows the current state of the compute nodes known to the scheduler:

```
auser@uan01:~> sinfo
```
{: .language-bash}
```
PARTITION AVAIL  TIMELIMIT  NODES  STATE NODELIST 
standard     up 1-00:00:00     17 drain* nid[001301,001363,001388,001552,001561-001562,001568,001622,001690,001746,001825,001834,001841,001848,001884,001894-001895] 
standard     up 1-00:00:00      7  drain nid[001391,001840,001842-001843,001892-001893,001955] 
standard     up 1-00:00:00    926  alloc nid[001000-001110,001112-001238,001241-001300,001302-001361,001364-001387,001389-001390,001392-001395,001397-001408,001411-001471,001476-001520,001525-001551,001563-001567,001569-001570,001575,001580-001604,001608-001621,001623-001656,001661-001664,001676-001679,001681-001689,001691-001745,001747-001762,001764-001805,001816-001824,001826-001831,001836-001839,001844,001846-001847,001849-001883,001885-001886,001888-001891,001896-001903,001908-001954,001956-001977,001979-001996,001999-002022] 
standard     up 1-00:00:00     11   resv nid[001111,001396,001409,001672-001675,001832-001833,001835,002023] 
standard     up 1-00:00:00     63   idle nid[001239-001240,001362,001410,001472-001475,001521-001524,001553-001560,001571-001574,001576-001579,001605-001607,001657-001660,001665-001671,001680,001763,001806-001815,001845,001887,001904-001907,001978,001997-001998] 
```
{: .output}

There is a row for each node state and partition combination. The default output shows the following columns:

* `PARTITION` - The system partition
* `AVAIL` - The status of the partition - `up` in normal operation
* `TIMELIMIT` - Maximum runtime as `days-hours:minutes:seconds`: on ARCHER2, these are set using *QoS*
  (Quality of Service) rather than on partitions
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

> ## Lots to look at!
> Warning! The `sinfo -N -l` command will produce a lot of output as there are 1024 individual 
> nodes on the current ARCHER2 system! (It will be even worse on the full system when there will be
> over 5,500 nodes.) 
{: .callout}

```
auser@uan01:~> sinfo -N -l
```
{: .language-bash}
```
Fri Aug 27 12:08:55 2021
NODELIST   NODES PARTITION       STATE CPUS    S:C:T MEMORY TMP_DISK WEIGHT AVAIL_FE REASON               
nid001000      1  standard   allocated 256    2:64:2 256000        0      1   (null) none                 
nid001001      1  standard   allocated 256    2:64:2 256000        0      1   (null) none                 
nid001002      1  standard   allocated 256    2:64:2 256000        0      1   (null) none                 
nid001003      1  standard   allocated 256    2:64:2 256000        0      1   (null) none                 
nid001004      1  standard   allocated 256    2:64:2 256000        0      1   (null) none                 
nid001005      1  standard   allocated 256    2:64:2 256000        0      1   (null) none                 
nid001006      1  standard   allocated 256    2:64:2 256000        0      1   (null) none                 
nid001007      1  standard   allocated 256    2:64:2 256000        0      1   (null) none                 
nid001008      1  standard   allocated 256    2:64:2 256000        0      1   (null) none                 
nid001009      1  standard   allocated 256    2:64:2 256000        0      1   (null) none                 
nid001010      1  standard   allocated 256    2:64:2 256000        0      1   (null) none                 
nid001011      1  standard   allocated 256    2:64:2 256000        0      1   (null) none                 
nid001012      1  standard   allocated 256    2:64:2 256000        0      1   (null) none                 
nid001013      1  standard   allocated 256    2:64:2 256000        0      1   (null) none                 
nid001014      1  standard   allocated 256    2:64:2 256000        0      1   (null) none                 
nid001015      1  standard   allocated 256    2:64:2 256000        0      1   (null) none                 
nid001016      1  standard   allocated 256    2:64:2 256000        0      1   (null) none                 
nid001017      1  standard   allocated 256    2:64:2 256000        0      1   (null) none                 
nid001018      1  standard   allocated 256    2:64:2 256000        0      1   (null) none                 
nid001019      1  standard   allocated 256    2:64:2 256000        0      1   (null) none                 
nid001020      1  standard   allocated 256    2:64:2 256000        0      1   (null) none                           
...lots of output trimmed...

```
{: .output}

> ## Explore a compute node
> Let’s look at the resources available on the compute nodes where your jobs will actually run. Try running this
> command to see the name, CPUs and memory available on the worker nodes (the instructors will give you the ID of
> the compute node to use):
> ```
> [auser@uan01:~> sinfo -n nid001000 -o "%n %c %m"
> ```
> {: .language-bash}
> This should display the resources available for a standard node. Are they what you expect given what we learnt
> earlier about the configuration of a compute node?
> > ## Solution
> > The output should show:
> > ```
> > HOSTNAMES CPUS MEMORY
> > nid001000 256 256000
> > ```
> > You may be surprised that the output shows 256 CPUS per compute node when the earlier description stated
> > that there were 128 cores per node. This is because each physical core can support two hardware threads
> > (often referred to as hyperthreads). Most jobs will only make use of one of these threads as using the
> > other thread can reduce performance.
> {: .solution}
{: .challenge}

## Using batch job submission scripts

### Header section: `#SBATCH`

As for most other scheduler systems, job submission scripts in Slurm consist of a header section with the
shell specification and options to the submission command (`sbatch` in this case) followed by the body of
the script that actually runs the commands you want. In the header section, options to `sbatch` should 
be prepended with `#SBATCH`.

Here is a simple example script that runs the `xthi` program (which shows process and thread placement) across
two nodes.

```
#!/bin/bash
#SBATCH --job-name=my_mpi_job
#SBATCH --nodes=2
#SBATCH --ntasks-per-node=128
#SBATCH --cpus-per-task=1
#SBATCH --time=0:10:0
#SBATCH --account=ta001
#SBATCH --partition=standard
#SBATCH --qos=standard

# This module needs to be loaded in ALL scripts
module load epcc-job-env

# Now load the "xthi" package
module load xthi

export OMP_NUM_THREADS=1

# Load modules, etc.
# srun to launch the executable
srun --hint=nomultithread --distribution=block:block xthi
```
{: .language-bash}

The options shown here are:

* `--job-name=my_mpi_job` - Set the name for the job that will be displayed in Slurm output
* `--nodes=2` - Select two nodes for this job
* `--ntasks-per-node=128` - Set 128 parallel processes per node (usually corresponds to MPI ranks)
* `--cpus-per-task=1` - Number of cores to allocate per parallel process 
* `--time=0:10:0` - Set 10 minutes maximum walltime for this job
* `--account=ta001` - Charge the job to the `t01` budget

We will discuss the `srun` command further below.

### Submitting jobs using `sbatch`

You use the `sbatch` command to submit job submission scripts to the scheduler. For example, if the
above script was saved in a file called `test_job.slurm`, you would submit it with:

```
auser@uan01:~> sbatch test_job.slurm
```
{: .language-bash}
```
Submitted batch job 23996
```
{: .output}

Slurm reports back with the job ID for the job you have submitted

<!-- This exercise is not ideal - it would be good to replace with something more useful -->

> ## What are the default for `sbatch` options?
> If you do not specify job options, what are the defaults for Slurm on ARCHER2? Submit jobs to find out
> what the defaults are for:
> 
> 1. Budget (or Account) the job is charged to?
> 2. Tasks per node?
> 3. Number of nodes?
> 4. Walltime? (This one is hard!)
> 
> > ## Solution
> > 
> > (1) Budget: defaults to your primary group (for accounts in this course, this will be `ta035`)
> >
> > You can get the answers to 2. and 3. this with the following script):
> > 
> > ```
> > #!/bin/bash
> > #SBATCH --job-name=my_mpi_job
> > module load epcc-job-env
> > ...
> > echo "Nodes: $SLURM_JOB_NUM_NODES"
> > echo "Tasks per node: $SLURM_NTASKS_PER_NODE"
> > module load xthi
> > 
> > export OMP_NUM_THREADS=1
> > 
> > srun --cpu-bind=cores xthi
> > ```
> > {: .language-bash}
> > 
> > (2) Tasks per node: 256
> >
> > (3) Number of nodes: 1
> >
> > Getting the default time limit is more difficult - we need to use `sacct` to query the time limit set for
> > the job. For example, if the job ID was "12345", then we could query the time limit with:
> > 
> > ```
> > auser@uan01:~> sacct -o "TimeLimit" -j 12345
> > ```
> > {: .language-bash}
> > ```
> >  Timelimit 
> > ---------- 
> >  01:00:00 
> > ```
> > {: .output}
> >
> > (4) Walltime: 1 hour
> {: .solution}
{: .challenge}

### Checking progress of your job with `squeue`

You use the `squeue` command to show the current state of the queues on ARCHER2. Without any options, it
will show all jobs in the queue:

```
auser@uan01:~> squeue
```
{: .language-bash}
```

JOBID  PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON) 
451729  standard 243d9382        a PD       0:00     64 (AssocMaxCpuMinutesPerJobLimit) 
451750  standard Neg_3.51        b PD       0:00     35 (QOSMaxNodePerUserLimit) 
451752  standard    PosL1        b PD       0:00     35 (QOSMaxNodePerUserLimit) 
451754  standard   Pos_R1        b PD       0:00     35 (QOSMaxNodePerUserLimit) 
451757  standard    PosL2        b PD       0:00     35 (QOSMaxNodePerUserLimit) 
451759  standard    PosL4        b PD       0:00     35 (QOSMaxNodePerUserLimit) 
461382  standard       NO        c PD       0:00      2 (AssocMaxCpuMinutesPerJobLimit) 
461387  standard       ON        c PD       0:00      4 (AssocMaxCpuMinutesPerJobLimit) 
461392  standard       ON        c PD       0:00      2 (AssocMaxCpuMinutesPerJobLimit) 
404533  standard  xml_gen        d PD       0:00      1 (DependencyNeverSatisfied) 
404600  standard  xml_gen        d PD       0:00      1 (DependencyNeverSatisfied) 
406679  standard  xml_gen        d PD       0:00      1 (DependencyNeverSatisfied) 
406696  standard  xml_gen        d PD       0:00      1 (DependencyNeverSatisfied) 
462580_[29-55]  standard   int1.9       e PD       0:00     24 (QOSMaxNodePerUserLimit) 
464006  standard lammps_t        f PD       0:00      4 (QOSMaxJobsPerUserLimit) 
464007  standard lammps_t        f PD       0:00      4 (QOSMaxJobsPerUserLimit) 
463164+1  standard    KrJ50      g PD       0:00     56 (Resources) 
463164+0  standard    KrJ50      g PD       0:00     21 (Resources) 
461842  standard u-cg647.        h PD       0:00     82 (Priority) 
462637+0  standard eO1_medu      i PD       0:00      8 (Resources) 
462637+1  standard eO1_medu      i PD       0:00      1 (Resources) 
462086  standard nemo_tes        j PD       0:00      8 (Priority) 
...
```
{: .output}

### Cancelling jobs with `scancel`

You can use the `scancel` command to cancel jobs that are queued or running. When used on running jobs
it stops them immediately.

### Running parallel applications using `srun`

Once past the header section your script consists of standard shell commands required to run your
job. These can be simple or complex depending on how you run your jobs but even the simplest job
script usually contains commands to:

* Load the required software modules
* Set appropriate environment variables (you should always set `OMP_NUM_THREADS`, even if you are
  not using OpenMP you should set this to `1`)

After this you will usually launch your parallel program using the `srun` command. At its simplest,
`srun` only needs 2 arguments to specify the correct binding of processes to cores (it will use the
values supplied to `sbatch` to work out how many parallel processes to launch). In the example above,
our `srun` command simply looks like:

```
srun --hint=nomultithread --distribution=block:block xthi
```
{: .language-bash}

> ## Underpopulation of nodes
> You may often want to *underpopulate* nodes on ARCHER2 to access more memory or more memory 
> bandwidth per task. Can you state the `sbatch` options you would use to run `xthi`:
> 
> 1. On 4 nodes with 64 tasks per node?
> 2. On 8 nodes with 2 tasks per node, 1 task per socket?
> 3. On 4 nodes with 32 tasks per node, ensuring an even distribution across the 8 NUMA regions
> on the node?
> 
> Once you have your answers run them in job scripts and check that the binding of tasks to 
> nodes and cores output by `xthi` is what you expect.
> 
> > ## Solution
> > 1. `--nodes=4 --ntasks-per-node=64`
> > 2. `--nodes=8 --ntasks-per-node=2 --ntasks-per-socket=1`
> > 3. `--nodes=4 --ntasks-per-node=32 --ntasks-per-socket=16 --cpus-per-task=4`
> {: .solution}
{: .challenge}

### Hybrid MPI and OpenMP jobs

When running hybrid MPI (with the individual tasks also known as ranks or processes) and OpenMP
(with multiple threads) jobs you need to leave free cores between the parallel tasks launched
using `srun` for the multiple OpenMP threads that will be associated with each MPI task.

As we saw above, you can use the options to `sbatch` to control how many parallel tasks are
placed on each compute node and can use the `--cpus-per-task` option to set the stride 
between parallel tasks to the right value to accommodate the OpenMP threads - the value
for `--cpus-per-task` should usually be the same as that for `OMP_NUM_THREADS`. To ensure
you get the correct thread pinning, you also need to specify an additional OpenMP environment
variable. Specifically:

   - Set the `OMP_PLACES` environment variable to `cores` with `export OMP_PLACES=cores` in 
     your job submission script

As an example, consider the job script below that runs across 2 nodes with 8 MPI tasks
per node and 16 OpenMP threads per MPI task (so all 256 cores across both nodes are used,
128 cores per node).

```
#!/bin/bash
#SBATCH --job-name=my_hybrid_job
#SBATCH --nodes=2
#SBATCH --ntasks-per-node=8
#SBATCH --cpus-per-task=16
#SBATCH --time=0:10:0
#SBATCH --account=t01
#SBATCH --partition=standard
#SBATCH --qos=standard

# This module needs to be loaded in ALL scripts
module load epcc-job-env

# Now load the "xthi" package
module load xthi

export OMP_NUM_THREADS=16
export OMP_PLACES=cores

# Load modules, etc.
# srun to launch the executable
srun --hint=nomultithread --distribution=block:block xthi
```
{: .language-bash}

Each ARCHER2 compute node is made up of 8 NUMA (*Non Uniform Memory Access*) regions (4 per socket) 
with 16 cores in each region. Programs where the threads of a task span multiple NUMA regions
are likely to be *much less* efficient so we recommend using thread counts that fit well into the
ARCHER2 compute node layout. Effectively, this means one of the following options for hybrid jobs
on nodes where all cores are used:

* 8 MPI tasks per node and 16 OpenMP threads per task: equivalent to 1 MPI task per NUMA region
* 16 MPI tasks per node and 8 OpenMP threads per task: equivalent to 2 MPI tasks per NUMA region
* 32 MPI tasks per node and 4 OpenMP threads per task: equivalent to 4 MPI tasks per NUMA region
* 64 MPI tasks per node and 2 OpenMP threads per task: equivalent to 8 MPI tasks per NUMA region 

## STDOUT/STDERR from jobs

STDOUT and STDERR from jobs are, by default, written to a file called `slurm-<jobid>.out` in the
working directory for the job (unless the job script changes this, this will be the directory
where you submitted the job). So for a job with ID `12345` STDOUT and STDERR would be in
`slurm-12345.out`.

If you run into issues with your jobs, the Service Desk will often ask you to send your job
submission script and the contents of this file to help debug the issue.

If you need to change the location of STDOUT and STDERR you can use the `--output=<filename>`
and the `--error=<filename>` options to `sbatch` to split the streams and output to the named
locations.

## Other useful information

In this section we briefly introduce other scheduler topics that may be useful to users. We
provide links to more information on these areas for people who may want to explore these 
areas more. 

### Interactive jobs: direct `srun` 

Similar to the batch jobs covered above, users can also run interactive jobs using the `srun`
command directly. `srun` used in this way takes the same arguments as `sbatch` but, obviously, these are 
specified on the command line rather than in a job submission script. As for `srun` within 
a batch job, you should also provide the name of the executable you want to run.

For example, to execute `xthi` across all cores on two nodes (1 MPI task per core and no
OpenMP threading) within an interactive job you would issue the following commands:

```
auser@uan01:~> srun --partition=standard --qos=standard --nodes=2 --ntasks-per-node=128 --cpus-per-task=1 --time=0:10:0 --account=ta001 xthi
```
{: .language-bash}
```
Node    0, hostname nid001030
Node    1, hostname nid001031
Node    0, rank    0, thread   0, (affinity = 0,128)
Node    0, rank    1, thread   0, (affinity = 16,144)
Node    0, rank    2, thread   0, (affinity = 32,160)
Node    0, rank    3, thread   0, (affinity = 48,176)
Node    0, rank    4, thread   0, (affinity = 64,192)
Node    0, rank    5, thread   0, (affinity = 80,208)
Node    0, rank    6, thread   0, (affinity = 96,224)
Node    0, rank    7, thread   0, (affinity = 112,240)
Node    0, rank    8, thread   0, (affinity = 1,129)
Node    0, rank    9, thread   0, (affinity = 17,145)
Node    0, rank   10, thread   0, (affinity = 33,161)
Node    0, rank   11, thread   0, (affinity = 49,177)
Node    0, rank   12, thread   0, (affinity = 65,193)
Node    0, rank   13, thread   0, (affinity = 81,209)
Node    0, rank   14, thread   0, (affinity = 97,225)
...long output trimmed...
```
{: .output}


<!-- Need to add information on the solid state storage and Slurm once it is in place

### Using the ARCHER2 solid state storage

-->


{% include links.md %}


