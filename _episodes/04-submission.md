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
auser@ln01:~> sinfo
```
{: .language-bash}
```
PARTITION AVAIL  TIMELIMIT  NODES  STATE NODELIST
standard     up 1-00:00:00      6 drain$ nid[001693,002059,002546,003583,005123,005343]
standard     up 1-00:00:00     18 drain* nid[001395,001442,002195,002447,003082,003257,003395,003598,004148,004213,004231,004336-004337,004999,005481,005731,005799,006210]
standard     up 1-00:00:00      8  drain nid[001638,002315,002689,005507,005930,006127,006190,006759]
standard     up 1-00:00:00     95   resv nid[001256-001257,001259-001262,001296-001383,002760]
standard     up 1-00:00:00   5067  alloc nid[001000-001255,001258,001263-001295,001384-001394,001396-001416,001419-001441,001443-001574,001579-001582,001593-001637,001639-001692,001694-001799,001812,001817-002051,002053-002058,002060-002121,002123,002126-002194,002196-002200,002202-002229,002234-002239,002251-002256,002259-002279,002300-002303,002311,002334,002355,002368-002369,002391-002400,002408-002409,002412,002414-002415,002418-002421,002423-002429,002431,002433-002434,002437-002446,002448-002502,002507-002514,002525-002534,002536-002545,002547-002688,002690-002759,002761-002950,002952-002958,002982,002991-002995,003002-003003,003015-003036,003038,003041-003042,003048-003081,003083-003256,003258-003362,003364-003369,003371-003394,003396-003431,003461-003463,003465-003467,003478-003500,003521-003525,003527-003532,003539,003542,003544-003552,003555-003582,003584-003597,003599-003943,003953,003956,003959,003965-003971,003977-003978,003980,003984,004002-004003,004005,004010,004012-004013,004015-004017,004021,004025-004026,004028,004038-004057,004060,004063-004068,004072-004147,004149-004212,004214-004230,004232-004335,004338-004583,004620-004623,004628-004629,004632-004633,004636-004638,004647,004653,004672,004679-004680,004695,004701-004702,004705-004715,004718-004744,004747-004748,004758-004761,004763,004765-004807,004811-004818,004821-004835,004837-004974,004983-004998,005000-005122,005124-005342,005344-005348,005351-005398,005404,005408-005480,005482-005506,005508-005526,005528-005558,005560,005565,005569-005576,005578-005587,005589-005591,005593-005595,005598,005604-005606,005608-005635,005638-005659,005661-005689,005691-005695,005697-005705,005707-005711,005717-005729,005735-005747,005749-005758,005784,005807-005808,005810-005814,005824,005828,005845-005848,005850-005861,005864-005929,005931-006037,006054-006126,006128-006189,006191-006209,006211-006387,006389-006390,006392-006756,006758,006760-006859]
standard     up 1-00:00:00    666   idle nid[001417-001418,001575-001578,001583-001592,001800-001811,001813-001816,002052,002122,002124-002125,002201,002230-002233,002240-002250,002257-002258,002280-002299,002304-002310,002312-002314,002316-002333,002335-002354,002356-002367,002370-002390,002401-002407,002410-002411,002413,002416-002417,002422,002430,002432,002435-002436,002503-002506,002515-002524,002535,002951,002959-002981,002983-002990,002996-003001,003004-003014,003037,003039-003040,003043-003047,003363,003370,003432-003460,003464,003468-003477,003501-003520,003526,003533-003538,003540-003541,003543,003553-003554,003944-003952,003954-003955,003957-003958,003960-003964,003972-003976,003979,003981-003983,003985-004001,004004,004006-004009,004011,004014,004018-004020,004022-004024,004027,004029-004037,004058-004059,004061-004062,004069-004071,004584-004619,004624-004627,004630-004631,004634-004635,004639-004646,004648-004652,004654-004671,004673-004678,004681-004694,004696-004700,004703-004704,004716-004717,004745-004746,004749-004757,004762,004764,004808-004810,004819-004820,004836,004975-004982,005349-005350,005399-005403,005405-005407,005527,005559,005561-005564,005566-005568,005577,005588,005592,005596-005597,005599-005603,005607,005636-005637,005660,005690,005696,005706,005712-005716,005730,005732-005734,005748,005759-005783,005785-005798,005800-005806,005809,005815-005823,005825-005827,005829-005844,005849,005862-005863,006038-006053,006388,006391,006757]
highmem      up 1-00:00:00      1   resv nid002760
highmem      up 1-00:00:00    516  alloc nid[002756-002759,002761-002911,002916-002950,002952-002958,002982,002991-002995,003002-003003,003015-003036,003038,003041-003042,006376-006387,006389-006390,006392-006411,006416-006667]
highmem      up 1-00:00:00     59   idle nid[002951,002959-002981,002983-002990,002996-003001,003004-003014,003037,003039-003040,003043-003047,006388,006391]
serial       up 1-00:00:00      2   idle dvn[01-02]
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
> Warning! The `sinfo -N -l` command will produce a lot of output as there are 5860 individual 
> nodes on the current ARCHER2 system! (It will be even worse on the full system when there will be
> over 5,860 nodes.) 
{: .callout}

```
auser@ln01:~> sinfo -N -l
```
{: .language-bash}
```
Mon Nov 29 19:02:37 2021
> > NodeLIST   NODES PARTITION       STATE CPUS    S:C:T MEMORY TMP_DISK WEIGHT AVAIL_FE REASON              
dvn01          1    serial        idle 256    2:64:2 515450        0      1 DVN,AMD_ none                
dvn02          1    serial        idle 256    2:64:2 515450        0      1 DVN,AMD_ none                
nid001000      1  standard   allocated 256    2:64:2 227328        0      1 COMPUTE, none                
nid001001      1  standard   allocated 256    2:64:2 227328        0      1 COMPUTE, none                
nid001002      1  standard   allocated 256    2:64:2 227328        0      1 COMPUTE, none                
nid001003      1  standard   allocated 256    2:64:2 227328        0      1 COMPUTE, none                
nid001004      1  standard   allocated 256    2:64:2 227328        0      1 COMPUTE, none                
nid001005      1  standard   allocated 256    2:64:2 227328        0      1 COMPUTE, none                
nid001006      1  standard   allocated 256    2:64:2 227328        0      1 COMPUTE, none                
nid001007      1  standard   allocated 256    2:64:2 227328        0      1 COMPUTE, none                
nid001008      1  standard   allocated 256    2:64:2 227328        0      1 COMPUTE, none                
nid001009      1  standard   allocated 256    2:64:2 227328        0      1 COMPUTE, none                
nid001010      1  standard   allocated 256    2:64:2 227328        0      1 COMPUTE, none                
nid001011      1  standard   allocated 256    2:64:2 227328        0      1 COMPUTE, none                
nid001012      1  standard   allocated 256    2:64:2 227328        0      1 COMPUTE, none                
nid001013      1  standard   allocated 256    2:64:2 227328        0      1 COMPUTE, none                
nid001014      1  standard   allocated 256    2:64:2 227328        0      1 COMPUTE, none                
nid001015      1  standard   allocated 256    2:64:2 227328        0      1 COMPUTE, none                                   
...lots of output trimmed...

```
{: .output}

> ## Explore a compute node
> Let’s look at the resources available on the compute nodes where your jobs will actually run. Try running this
> command to see the name, CPUs and memory available on the worker nodes (the instructors will give you the ID of
> the compute node to use):
> ```
> [auser@ln01:~> sinfo -n nid001000 -o "%n %c %m"
> ```
> {: .language-bash}
> This should display the resources available for a standard node. Are they what you expect given what we learnt
> earlier about the configuration of a compute node?
> > ## Solution
> > The output should show:
> > ```
> > HOSTNAMES CPUS MEMORY
> > nid001000 256 227328
> > ```
> > You may be surprised that the output shows 256 CPUS per compute node when the earlier description stated
> > that there were 128 cores per node. This is because each physical core can support two hardware threads
> > (often referred to as symmetric multithreading, SMT). Most jobs will only make use of one of these
> > threads as using the other thread can reduce performance. This manifests itself as 256 *logical* cores
> > per node (with two logical cores per physical core).
> > 
> > You may also notice that the amount of memory available does not match the full capacity of the node. This
> > is because some memory is used up with the OS and system processes.
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
#SBATCH --qos=short

# Now load the "xthi" package
module load xthi

export OMP_NUM_THREADS=1

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
* `--partition=standard` - Use nodes from the standard partition on ARCHER2 (the standard partition
  includes all compute nodes).
* `--qos=short` - Use the `short` Quality of Service (QoS). Different QoS define different limits
  for jobs. The short QoS allows small short jobs only but guarantees that they run quickly/straight away.

> ## Partitions and QoS
> There are a small number of different partitions on ARCHER2 (covering different node types) and
> a wider variety of QoS (covering different use cases). You can find a list of the available 
> partitions and QoS in the [ARCHER2 User and Best Practice Guide](https://docs.archer2.ac.uk/user-guide/scheduler/#resource-limits).
{: .callout}

We will discuss the `srun` command further below.

### Submitting jobs using `sbatch`

You use the `sbatch` command to submit job submission scripts to the scheduler. For example, if the
above script was saved in a file called `test_job.slurm`, you would submit it with:

```
auser@ln01:~> sbatch test_job.slurm
```
{: .language-bash}
```
Submitted batch job 23996
```
{: .output}

Slurm reports back with the job ID for the job you have submitted

### Checking progress of your job with `squeue`

You use the `squeue` command to show the current state of the queues on ARCHER2. Without any options, it
will show all jobs in the queue:

```
auser@ln01:~> squeue
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

You can show just your jobs with `squeue -u $USER`.

### Cancelling jobs with `scancel`

You can use the `scancel` command to cancel jobs that are queued or running. When used on running jobs
it stops them immediately. You need to supply options to `scancel` to specify which jobs to kill.
To kill a particular job you supply the job ID, e.g. to kill the job with ID `12345`:

```
scancel 12345
```
{: .language-bash}

To kill all your queued jobs (but not running ones):

```
scancel --user=$USER --state=PENDING
```
{: .language-bash}

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

### STDOUT/STDERR from jobs

STDOUT and STDERR from jobs are, by default, written to a file called `slurm-<jobid>.out` in the
working directory for the job (unless the job script changes this, this will be the directory
where you submitted the job). So for a job with ID `12345` STDOUT and STDERR would be in
`slurm-12345.out`.

If you run into issues with your jobs, the Service Desk will often ask you to send your job
submission script and the contents of this file to help debug the issue.

If you need to change the location of STDOUT and STDERR you can use the `--output=<filename>`
and the `--error=<filename>` options to `sbatch` to split the streams and output to the named
locations.

> ## Is the output what you expect?
>
> Locate and examine the STDOUT from the job you ran above. Does it indicate that each 
> MPI process is bound to a different physical core as expected?
>
> > ## Solution
> > The output should look something like the below showing that each of the 256 MPI processes
> > is bound to a separate physical core.
> >
> > ```
> > Node summary for    2 nodes:
> > Node    0, hostname nid001342, mpi 128, omp   1, executable xthi
> > Node    1, hostname nid001343, mpi 128, omp   1, executable xthi
> > MPI summary: 256 ranks 
> > Node    0, rank    0, thread   0, (affinity =    0) 
> > Node    0, rank    1, thread   0, (affinity =    1) 
> > Node    0, rank    2, thread   0, (affinity =    2) 
> > Node    0, rank    3, thread   0, (affinity =    3) 
> > Node    0, rank    4, thread   0, (affinity =    4) 
> > Node    0, rank    5, thread   0, (affinity =    5) 
> > Node    0, rank    6, thread   0, (affinity =    6) 
> > Node    0, rank    7, thread   0, (affinity =    7) 
> > Node    0, rank    8, thread   0, (affinity =    8) 
> > Node    0, rank    9, thread   0, (affinity =    9) 
> > Node    0, rank   10, thread   0, (affinity =   10) 
> > Node    0, rank   11, thread   0, (affinity =   11) 
> > Node    0, rank   12, thread   0, (affinity =   12) 
> > Node    0, rank   13, thread   0, (affinity =   13) 
> > Node    0, rank   14, thread   0, (affinity =   14) 
> > Node    0, rank   15, thread   0, (affinity =   15) 
> > Node    0, rank   16, thread   0, (affinity =   16) 
> > Node    0, rank   17, thread   0, (affinity =   17) 
> > Node    0, rank   18, thread   0, (affinity =   18) 
> > Node    0, rank   19, thread   0, (affinity =   19) 
> > Node    0, rank   20, thread   0, (affinity =   20) 
> > Node    0, rank   21, thread   0, (affinity =   21) 
> > Node    0, rank   22, thread   0, (affinity =   22) 
> > Node    0, rank   23, thread   0, (affinity =   23) 
> > Node    0, rank   24, thread   0, (affinity =   24) 
> > Node    0, rank   25, thread   0, (affinity =   25) 
> > Node    0, rank   26, thread   0, (affinity =   26) 
> > Node    0, rank   27, thread   0, (affinity =   27) 
> > Node    0, rank   28, thread   0, (affinity =   28) 
> > Node    0, rank   29, thread   0, (affinity =   29) 
> > Node    0, rank   30, thread   0, (affinity =   30) 
> > Node    0, rank   31, thread   0, (affinity =   31) 
> > Node    0, rank   32, thread   0, (affinity =   32) 
> > Node    0, rank   33, thread   0, (affinity =   33) 
> > Node    0, rank   34, thread   0, (affinity =   34) 
> > Node    0, rank   35, thread   0, (affinity =   35) 
> > Node    0, rank   36, thread   0, (affinity =   36) 
> > Node    0, rank   37, thread   0, (affinity =   37) 
> > Node    0, rank   38, thread   0, (affinity =   38) 
> > Node    0, rank   39, thread   0, (affinity =   39) 
> > Node    0, rank   40, thread   0, (affinity =   40) 
> > Node    0, rank   41, thread   0, (affinity =   41) 
> > Node    0, rank   42, thread   0, (affinity =   42) 
> > Node    0, rank   43, thread   0, (affinity =   43) 
> > Node    0, rank   44, thread   0, (affinity =   44) 
> > Node    0, rank   45, thread   0, (affinity =   45) 
> > Node    0, rank   46, thread   0, (affinity =   46) 
> > Node    0, rank   47, thread   0, (affinity =   47) 
> > Node    0, rank   48, thread   0, (affinity =   48) 
> > Node    0, rank   49, thread   0, (affinity =   49) 
> > Node    0, rank   50, thread   0, (affinity =   50) 
> > Node    0, rank   51, thread   0, (affinity =   51) 
> > Node    0, rank   52, thread   0, (affinity =   52) 
> > Node    0, rank   53, thread   0, (affinity =   53) 
> > Node    0, rank   54, thread   0, (affinity =   54) 
> > Node    0, rank   55, thread   0, (affinity =   55) 
> > Node    0, rank   56, thread   0, (affinity =   56) 
> > Node    0, rank   57, thread   0, (affinity =   57) 
> > Node    0, rank   58, thread   0, (affinity =   58) 
> > Node    0, rank   59, thread   0, (affinity =   59) 
> > Node    0, rank   60, thread   0, (affinity =   60) 
> > Node    0, rank   61, thread   0, (affinity =   61) 
> > Node    0, rank   62, thread   0, (affinity =   62) 
> > Node    0, rank   63, thread   0, (affinity =   63) 
> > Node    0, rank   64, thread   0, (affinity =   64) 
> > Node    0, rank   65, thread   0, (affinity =   65) 
> > Node    0, rank   66, thread   0, (affinity =   66) 
> > Node    0, rank   67, thread   0, (affinity =   67) 
> > Node    0, rank   68, thread   0, (affinity =   68) 
> > Node    0, rank   69, thread   0, (affinity =   69) 
> > Node    0, rank   70, thread   0, (affinity =   70) 
> > Node    0, rank   71, thread   0, (affinity =   71) 
> > Node    0, rank   72, thread   0, (affinity =   72) 
> > Node    0, rank   73, thread   0, (affinity =   73) 
> > Node    0, rank   74, thread   0, (affinity =   74) 
> > Node    0, rank   75, thread   0, (affinity =   75) 
> > Node    0, rank   76, thread   0, (affinity =   76) 
> > Node    0, rank   77, thread   0, (affinity =   77) 
> > Node    0, rank   78, thread   0, (affinity =   78) 
> > Node    0, rank   79, thread   0, (affinity =   79) 
> > Node    0, rank   80, thread   0, (affinity =   80) 
> > Node    0, rank   81, thread   0, (affinity =   81) 
> > Node    0, rank   82, thread   0, (affinity =   82) 
> > Node    0, rank   83, thread   0, (affinity =   83) 
> > Node    0, rank   84, thread   0, (affinity =   84) 
> > Node    0, rank   85, thread   0, (affinity =   85) 
> > Node    0, rank   86, thread   0, (affinity =   86) 
> > Node    0, rank   87, thread   0, (affinity =   87) 
> > Node    0, rank   88, thread   0, (affinity =   88) 
> > Node    0, rank   89, thread   0, (affinity =   89) 
> > Node    0, rank   90, thread   0, (affinity =   90) 
> > Node    0, rank   91, thread   0, (affinity =   91) 
> > Node    0, rank   92, thread   0, (affinity =   92) 
> > Node    0, rank   93, thread   0, (affinity =   93) 
> > Node    0, rank   94, thread   0, (affinity =   94) 
> > Node    0, rank   95, thread   0, (affinity =   95) 
> > Node    0, rank   96, thread   0, (affinity =   96) 
> > Node    0, rank   97, thread   0, (affinity =   97) 
> > Node    0, rank   98, thread   0, (affinity =   98) 
> > Node    0, rank   99, thread   0, (affinity =   99) 
> > Node    0, rank  100, thread   0, (affinity =  100) 
> > Node    0, rank  101, thread   0, (affinity =  101) 
> > Node    0, rank  102, thread   0, (affinity =  102) 
> > Node    0, rank  103, thread   0, (affinity =  103) 
> > Node    0, rank  104, thread   0, (affinity =  104) 
> > Node    0, rank  105, thread   0, (affinity =  105) 
> > Node    0, rank  106, thread   0, (affinity =  106) 
> > Node    0, rank  107, thread   0, (affinity =  107) 
> > Node    0, rank  108, thread   0, (affinity =  108) 
> > Node    0, rank  109, thread   0, (affinity =  109) 
> > Node    0, rank  110, thread   0, (affinity =  110) 
> > Node    0, rank  111, thread   0, (affinity =  111) 
> > Node    0, rank  112, thread   0, (affinity =  112) 
> > Node    0, rank  113, thread   0, (affinity =  113) 
> > Node    0, rank  114, thread   0, (affinity =  114) 
> > Node    0, rank  115, thread   0, (affinity =  115) 
> > Node    0, rank  116, thread   0, (affinity =  116) 
> > Node    0, rank  117, thread   0, (affinity =  117) 
> > Node    0, rank  118, thread   0, (affinity =  118) 
> > Node    0, rank  119, thread   0, (affinity =  119) 
> > Node    0, rank  120, thread   0, (affinity =  120) 
> > Node    0, rank  121, thread   0, (affinity =  121) 
> > Node    0, rank  122, thread   0, (affinity =  122) 
> > Node    0, rank  123, thread   0, (affinity =  123) 
> > Node    0, rank  124, thread   0, (affinity =  124) 
> > Node    0, rank  125, thread   0, (affinity =  125) 
> > Node    0, rank  126, thread   0, (affinity =  126) 
> > Node    0, rank  127, thread   0, (affinity =  127) 
> > Node    1, rank  128, thread   0, (affinity =    0) 
> > Node    1, rank  129, thread   0, (affinity =    1) 
> > Node    1, rank  130, thread   0, (affinity =    2) 
> > Node    1, rank  131, thread   0, (affinity =    3) 
> > Node    1, rank  132, thread   0, (affinity =    4) 
> > Node    1, rank  133, thread   0, (affinity =    5) 
> > Node    1, rank  134, thread   0, (affinity =    6) 
> > Node    1, rank  135, thread   0, (affinity =    7) 
> > Node    1, rank  136, thread   0, (affinity =    8) 
> > Node    1, rank  137, thread   0, (affinity =    9) 
> > Node    1, rank  138, thread   0, (affinity =   10) 
> > Node    1, rank  139, thread   0, (affinity =   11) 
> > Node    1, rank  140, thread   0, (affinity =   12) 
> > Node    1, rank  141, thread   0, (affinity =   13) 
> > Node    1, rank  142, thread   0, (affinity =   14) 
> > Node    1, rank  143, thread   0, (affinity =   15) 
> > Node    1, rank  144, thread   0, (affinity =   16) 
> > Node    1, rank  145, thread   0, (affinity =   17) 
> > Node    1, rank  146, thread   0, (affinity =   18) 
> > Node    1, rank  147, thread   0, (affinity =   19) 
> > Node    1, rank  148, thread   0, (affinity =   20) 
> > Node    1, rank  149, thread   0, (affinity =   21) 
> > Node    1, rank  150, thread   0, (affinity =   22) 
> > Node    1, rank  151, thread   0, (affinity =   23) 
> > Node    1, rank  152, thread   0, (affinity =   24) 
> > Node    1, rank  153, thread   0, (affinity =   25) 
> > Node    1, rank  154, thread   0, (affinity =   26) 
> > Node    1, rank  155, thread   0, (affinity =   27) 
> > Node    1, rank  156, thread   0, (affinity =   28) 
> > Node    1, rank  157, thread   0, (affinity =   29) 
> > Node    1, rank  158, thread   0, (affinity =   30) 
> > Node    1, rank  159, thread   0, (affinity =   31) 
> > Node    1, rank  160, thread   0, (affinity =   32) 
> > Node    1, rank  161, thread   0, (affinity =   33) 
> > Node    1, rank  162, thread   0, (affinity =   34) 
> > Node    1, rank  163, thread   0, (affinity =   35) 
> > Node    1, rank  164, thread   0, (affinity =   36) 
> > Node    1, rank  165, thread   0, (affinity =   37) 
> > Node    1, rank  166, thread   0, (affinity =   38) 
> > Node    1, rank  167, thread   0, (affinity =   39) 
> > Node    1, rank  168, thread   0, (affinity =   40) 
> > Node    1, rank  169, thread   0, (affinity =   41) 
> > Node    1, rank  170, thread   0, (affinity =   42) 
> > Node    1, rank  171, thread   0, (affinity =   43) 
> > Node    1, rank  172, thread   0, (affinity =   44) 
> > Node    1, rank  173, thread   0, (affinity =   45) 
> > Node    1, rank  174, thread   0, (affinity =   46) 
> > Node    1, rank  175, thread   0, (affinity =   47) 
> > Node    1, rank  176, thread   0, (affinity =   48) 
> > Node    1, rank  177, thread   0, (affinity =   49) 
> > Node    1, rank  178, thread   0, (affinity =   50) 
> > Node    1, rank  179, thread   0, (affinity =   51) 
> > Node    1, rank  180, thread   0, (affinity =   52) 
> > Node    1, rank  181, thread   0, (affinity =   53) 
> > Node    1, rank  182, thread   0, (affinity =   54) 
> > Node    1, rank  183, thread   0, (affinity =   55) 
> > Node    1, rank  184, thread   0, (affinity =   56) 
> > Node    1, rank  185, thread   0, (affinity =   57) 
> > Node    1, rank  186, thread   0, (affinity =   58) 
> > Node    1, rank  187, thread   0, (affinity =   59) 
> > Node    1, rank  188, thread   0, (affinity =   60) 
> > Node    1, rank  189, thread   0, (affinity =   61) 
> > Node    1, rank  190, thread   0, (affinity =   62) 
> > Node    1, rank  191, thread   0, (affinity =   63) 
> > Node    1, rank  192, thread   0, (affinity =   64) 
> > Node    1, rank  193, thread   0, (affinity =   65) 
> > Node    1, rank  194, thread   0, (affinity =   66) 
> > Node    1, rank  195, thread   0, (affinity =   67) 
> > Node    1, rank  196, thread   0, (affinity =   68) 
> > Node    1, rank  197, thread   0, (affinity =   69) 
> > Node    1, rank  198, thread   0, (affinity =   70) 
> > Node    1, rank  199, thread   0, (affinity =   71) 
> > Node    1, rank  200, thread   0, (affinity =   72) 
> > Node    1, rank  201, thread   0, (affinity =   73) 
> > Node    1, rank  202, thread   0, (affinity =   74) 
> > Node    1, rank  203, thread   0, (affinity =   75) 
> > Node    1, rank  204, thread   0, (affinity =   76) 
> > Node    1, rank  205, thread   0, (affinity =   77) 
> > Node    1, rank  206, thread   0, (affinity =   78) 
> > Node    1, rank  207, thread   0, (affinity =   79) 
> > Node    1, rank  208, thread   0, (affinity =   80) 
> > Node    1, rank  209, thread   0, (affinity =   81) 
> > Node    1, rank  210, thread   0, (affinity =   82) 
> > Node    1, rank  211, thread   0, (affinity =   83) 
> > Node    1, rank  212, thread   0, (affinity =   84) 
> > Node    1, rank  213, thread   0, (affinity =   85) 
> > Node    1, rank  214, thread   0, (affinity =   86) 
> > Node    1, rank  215, thread   0, (affinity =   87) 
> > Node    1, rank  216, thread   0, (affinity =   88) 
> > Node    1, rank  217, thread   0, (affinity =   89) 
> > Node    1, rank  218, thread   0, (affinity =   90) 
> > Node    1, rank  219, thread   0, (affinity =   91) 
> > Node    1, rank  220, thread   0, (affinity =   92) 
> > Node    1, rank  221, thread   0, (affinity =   93) 
> > Node    1, rank  222, thread   0, (affinity =   94) 
> > Node    1, rank  223, thread   0, (affinity =   95) 
> > Node    1, rank  224, thread   0, (affinity =   96) 
> > Node    1, rank  225, thread   0, (affinity =   97) 
> > Node    1, rank  226, thread   0, (affinity =   98) 
> > Node    1, rank  227, thread   0, (affinity =   99) 
> > Node    1, rank  228, thread   0, (affinity =  100) 
> > Node    1, rank  229, thread   0, (affinity =  101) 
> > Node    1, rank  230, thread   0, (affinity =  102) 
> > Node    1, rank  231, thread   0, (affinity =  103) 
> > Node    1, rank  232, thread   0, (affinity =  104) 
> > Node    1, rank  233, thread   0, (affinity =  105) 
> > Node    1, rank  234, thread   0, (affinity =  106) 
> > Node    1, rank  235, thread   0, (affinity =  107) 
> > Node    1, rank  236, thread   0, (affinity =  108) 
> > Node    1, rank  237, thread   0, (affinity =  109) 
> > Node    1, rank  238, thread   0, (affinity =  110) 
> > Node    1, rank  239, thread   0, (affinity =  111) 
> > Node    1, rank  240, thread   0, (affinity =  112) 
> > Node    1, rank  241, thread   0, (affinity =  113) 
> > Node    1, rank  242, thread   0, (affinity =  114) 
> > Node    1, rank  243, thread   0, (affinity =  115) 
> > Node    1, rank  244, thread   0, (affinity =  116) 
> > Node    1, rank  245, thread   0, (affinity =  117) 
> > Node    1, rank  246, thread   0, (affinity =  118) 
> > Node    1, rank  247, thread   0, (affinity =  119) 
> > Node    1, rank  248, thread   0, (affinity =  120) 
> > Node    1, rank  249, thread   0, (affinity =  121) 
> > Node    1, rank  250, thread   0, (affinity =  122) 
> > Node    1, rank  251, thread   0, (affinity =  123) 
> > Node    1, rank  252, thread   0, (affinity =  124) 
> > Node    1, rank  253, thread   0, (affinity =  125) 
> > Node    1, rank  254, thread   0, (affinity =  126) 
> > Node    1, rank  255, thread   0, (affinity =  127)
> > ```
> {: .solution}
{: .challenge}


### Underpopulation of nodes

You may often want to *underpopulate* nodes on ARCHER2 to access more memory or more memory 
bandwidth per process or for other reasons. Underpopulation in this way is achieved by 
altering the Slurm options, typically by setting the `--ntasks-per-node` option. You 
usually also need to set the `--cpus-per-task` option to ensure that the processes are
evenly distributed across NUMA regions on the node.

For example, to half-populate a node with processes (usually MPI processes) to give 64
processes per node, you would set the following options:

 - `--ntasks-per-node=64` - Sets 64 processes per node
 - `--cpus-per-task=2` - Sets a stride of 2 cores between processes to ensure that they
   are evenly distributed across the node

If you did this, then each process will have access to double the amount of memory than
it would if using 128 processes per node.

> ## Tip
> You will usually want the product of `ntasks-per-node` and `cpus-per-task` to be 
> equal to 128 (the number of physical cores on a node). In the case above, 
> 64&times;2 = 128.
{: .callout}

> ## Underpopulation of nodes
> Can you state the `sbatch` options you would use to run `xthi` on 4 
> nodes with 32 processes per node?
>
> Once you have your answer run them in job script and check that the binding of processes to 
> nodes and cores output by `xthi` is what you expect.
> 
> > ## Solution
> > The options you need are: `--nodes=4 --ntasks-per-node=32 --cpus-per-task=4`. The output
> > from running this should look somethign like:
> > ```
> > Node summary for    4 nodes:
> > Node    0, hostname nid001338, mpi  32, omp   1, executable xthi
> > Node    1, hostname nid001339, mpi  32, omp   1, executable xthi
> > Node    2, hostname nid001340, mpi  32, omp   1, executable xthi
> > Node    3, hostname nid001341, mpi  32, omp   1, executable xthi
> > MPI summary: 128 ranks 
> > Node    0, rank    0, thread   0, (affinity =  0-3) 
> > Node    0, rank    1, thread   0, (affinity =  4-7) 
> > Node    0, rank    2, thread   0, (affinity = 8-11) 
> > Node    0, rank    3, thread   0, (affinity = 12-15) 
> > Node    0, rank    4, thread   0, (affinity = 16-19) 
> > Node    0, rank    5, thread   0, (affinity = 20-23) 
> > Node    0, rank    6, thread   0, (affinity = 24-27) 
> > Node    0, rank    7, thread   0, (affinity = 28-31) 
> > Node    0, rank    8, thread   0, (affinity = 32-35) 
> > Node    0, rank    9, thread   0, (affinity = 36-39) 
> > Node    0, rank   10, thread   0, (affinity = 40-43) 
> > Node    0, rank   11, thread   0, (affinity = 44-47) 
> > Node    0, rank   12, thread   0, (affinity = 48-51) 
> > Node    0, rank   13, thread   0, (affinity = 52-55) 
> > Node    0, rank   14, thread   0, (affinity = 56-59) 
> > Node    0, rank   15, thread   0, (affinity = 60-63) 
> > Node    0, rank   16, thread   0, (affinity = 64-67) 
> > Node    0, rank   17, thread   0, (affinity = 68-71) 
> > Node    0, rank   18, thread   0, (affinity = 72-75) 
> > Node    0, rank   19, thread   0, (affinity = 76-79) 
> > Node    0, rank   20, thread   0, (affinity = 80-83) 
> > Node    0, rank   21, thread   0, (affinity = 84-87) 
> > Node    0, rank   22, thread   0, (affinity = 88-91) 
> > Node    0, rank   23, thread   0, (affinity = 92-95) 
> > Node    0, rank   24, thread   0, (affinity = 96-99) 
> > Node    0, rank   25, thread   0, (affinity = 100-103) 
> > Node    0, rank   26, thread   0, (affinity = 104-107) 
> > Node    0, rank   27, thread   0, (affinity = 108-111) 
> > Node    0, rank   28, thread   0, (affinity = 112-115) 
> > Node    0, rank   29, thread   0, (affinity = 116-119) 
> > Node    0, rank   30, thread   0, (affinity = 120-123) 
> > Node    0, rank   31, thread   0, (affinity = 124-127) 
> > Node    1, rank   32, thread   0, (affinity =  0-3) 
> > Node    1, rank   33, thread   0, (affinity =  4-7) 
> > Node    1, rank   34, thread   0, (affinity = 8-11) 
> > Node    1, rank   35, thread   0, (affinity = 12-15) 
> > Node    1, rank   36, thread   0, (affinity = 16-19) 
> > Node    1, rank   37, thread   0, (affinity = 20-23) 
> > Node    1, rank   38, thread   0, (affinity = 24-27) 
> > Node    1, rank   39, thread   0, (affinity = 28-31) 
> > Node    1, rank   40, thread   0, (affinity = 32-35) 
> > Node    1, rank   41, thread   0, (affinity = 36-39) 
> > Node    1, rank   42, thread   0, (affinity = 40-43) 
> > Node    1, rank   43, thread   0, (affinity = 44-47) 
> > Node    1, rank   44, thread   0, (affinity = 48-51) 
> > Node    1, rank   45, thread   0, (affinity = 52-55) 
> > Node    1, rank   46, thread   0, (affinity = 56-59) 
> > Node    1, rank   47, thread   0, (affinity = 60-63) 
> > Node    1, rank   48, thread   0, (affinity = 64-67) 
> > Node    1, rank   49, thread   0, (affinity = 68-71) 
> > Node    1, rank   50, thread   0, (affinity = 72-75) 
> > Node    1, rank   51, thread   0, (affinity = 76-79) 
> > Node    1, rank   52, thread   0, (affinity = 80-83) 
> > Node    1, rank   53, thread   0, (affinity = 84-87) 
> > Node    1, rank   54, thread   0, (affinity = 88-91) 
> > Node    1, rank   55, thread   0, (affinity = 92-95) 
> > Node    1, rank   56, thread   0, (affinity = 96-99) 
> > Node    1, rank   57, thread   0, (affinity = 100-103) 
> > Node    1, rank   58, thread   0, (affinity = 104-107) 
> > Node    1, rank   59, thread   0, (affinity = 108-111) 
> > Node    1, rank   60, thread   0, (affinity = 112-115) 
> > Node    1, rank   61, thread   0, (affinity = 116-119) 
> > Node    1, rank   62, thread   0, (affinity = 120-123) 
> > Node    1, rank   63, thread   0, (affinity = 124-127) 
> > Node    2, rank   64, thread   0, (affinity =  0-3) 
> > Node    2, rank   65, thread   0, (affinity =  4-7) 
> > Node    2, rank   66, thread   0, (affinity = 8-11) 
> > Node    2, rank   67, thread   0, (affinity = 12-15) 
> > Node    2, rank   68, thread   0, (affinity = 16-19) 
> > Node    2, rank   69, thread   0, (affinity = 20-23) 
> > Node    2, rank   70, thread   0, (affinity = 24-27) 
> > Node    2, rank   71, thread   0, (affinity = 28-31) 
> > Node    2, rank   72, thread   0, (affinity = 32-35) 
> > Node    2, rank   73, thread   0, (affinity = 36-39) 
> > Node    2, rank   74, thread   0, (affinity = 40-43) 
> > Node    2, rank   75, thread   0, (affinity = 44-47) 
> > Node    2, rank   76, thread   0, (affinity = 48-51) 
> > Node    2, rank   77, thread   0, (affinity = 52-55) 
> > Node    2, rank   78, thread   0, (affinity = 56-59) 
> > Node    2, rank   79, thread   0, (affinity = 60-63) 
> > Node    2, rank   80, thread   0, (affinity = 64-67) 
> > Node    2, rank   81, thread   0, (affinity = 68-71) 
> > Node    2, rank   82, thread   0, (affinity = 72-75) 
> > Node    2, rank   83, thread   0, (affinity = 76-79) 
> > Node    2, rank   84, thread   0, (affinity = 80-83) 
> > Node    2, rank   85, thread   0, (affinity = 84-87) 
> > Node    2, rank   86, thread   0, (affinity = 88-91) 
> > Node    2, rank   87, thread   0, (affinity = 92-95) 
> > Node    2, rank   88, thread   0, (affinity = 96-99) 
> > Node    2, rank   89, thread   0, (affinity = 100-103) 
> > Node    2, rank   90, thread   0, (affinity = 104-107) 
> > Node    2, rank   91, thread   0, (affinity = 108-111) 
> > Node    2, rank   92, thread   0, (affinity = 112-115) 
> > Node    2, rank   93, thread   0, (affinity = 116-119) 
> > Node    2, rank   94, thread   0, (affinity = 120-123) 
> > Node    2, rank   95, thread   0, (affinity = 124-127) 
> > Node    3, rank   96, thread   0, (affinity =  0-3) 
> > Node    3, rank   97, thread   0, (affinity =  4-7) 
> > Node    3, rank   98, thread   0, (affinity = 8-11) 
> > Node    3, rank   99, thread   0, (affinity = 12-15) 
> > Node    3, rank  100, thread   0, (affinity = 16-19) 
> > Node    3, rank  101, thread   0, (affinity = 20-23) 
> > Node    3, rank  102, thread   0, (affinity = 24-27) 
> > Node    3, rank  103, thread   0, (affinity = 28-31) 
> > Node    3, rank  104, thread   0, (affinity = 32-35) 
> > Node    3, rank  105, thread   0, (affinity = 36-39) 
> > Node    3, rank  106, thread   0, (affinity = 40-43) 
> > Node    3, rank  107, thread   0, (affinity = 44-47) 
> > Node    3, rank  108, thread   0, (affinity = 48-51) 
> > Node    3, rank  109, thread   0, (affinity = 52-55) 
> > Node    3, rank  110, thread   0, (affinity = 56-59) 
> > Node    3, rank  111, thread   0, (affinity = 60-63) 
> > Node    3, rank  112, thread   0, (affinity = 64-67) 
> > Node    3, rank  113, thread   0, (affinity = 68-71) 
> > Node    3, rank  114, thread   0, (affinity = 72-75) 
> > Node    3, rank  115, thread   0, (affinity = 76-79) 
> > Node    3, rank  116, thread   0, (affinity = 80-83) 
> > Node    3, rank  117, thread   0, (affinity = 84-87) 
> > Node    3, rank  118, thread   0, (affinity = 88-91) 
> > Node    3, rank  119, thread   0, (affinity = 92-95) 
> > Node    3, rank  120, thread   0, (affinity = 96-99) 
> > Node    3, rank  121, thread   0, (affinity = 100-103) 
> > Node    3, rank  122, thread   0, (affinity = 104-107) 
> > Node    3, rank  123, thread   0, (affinity = 108-111) 
> > Node    3, rank  124, thread   0, (affinity = 112-115) 
> > Node    3, rank  125, thread   0, (affinity = 116-119) 
> > Node    3, rank  126, thread   0, (affinity = 120-123) 
> > Node    3, rank  127, thread   0, (affinity = 124-127) 
> > ```
> {: .solution}
{: .challenge}

### Hybrid MPI and OpenMP jobs

When running hybrid MPI (with multiple processes) and OpenMP
(with multiple threads) jobs you need to leave free cores between the parallel processes launched
using `srun` for the multiple OpenMP threads that will be associated with each MPI process.

As we saw above, you can use the options to `sbatch` to control how many parallel processes are
placed on each compute node and the `--cpus-per-task` option to set the stride 
between parallel processes. The `--cpus-per-task` option is also used to accommodate the OpenMP
threads that are launched for each MPI process - the value
for `--cpus-per-task` should usually be the same as that for `OMP_NUM_THREADS`. To ensure
you get the correct thread pinning, you also need to specify an additional OpenMP environment
variable. Specifically:

   - Set the `OMP_PLACES` environment variable to `cores` with `export OMP_PLACES=cores` in 
     your job submission script

As an example, consider the job script below that runs across 2 nodes with 8 MPI processes
per node and 16 OpenMP threads per MPI process (so all 128 physical cores on both nodes are used,
256 physical cores in total).

```
#!/bin/bash
#SBATCH --job-name=my_hybrid_job
#SBATCH --nodes=2
#SBATCH --ntasks-per-node=8
#SBATCH --cpus-per-task=16
#SBATCH --time=0:10:0
#SBATCH --account=t01
#SBATCH --partition=standard
#SBATCH --qos=short

# Now load the "xthi" package
module load xthi

export OMP_NUM_THREADS=16
export OMP_PLACES=cores

# srun to launch the executable
srun --hint=nomultithread --distribution=block:block xthi
```
{: .language-bash}

If you submit this job script, you should see output that looks like:

```
Node summary for    4 nodes:
Node    0, hostname nid001256, mpi   8, omp  16, executable xthi
Node    1, hostname nid001257, mpi   8, omp  16, executable xthi
Node    2, hostname nid001259, mpi   8, omp  16, executable xthi
Node    3, hostname nid001260, mpi   8, omp  16, executable xthi
MPI summary: 32 ranks 
Node    0, rank    0, thread   0, (affinity =    0) 
Node    0, rank    0, thread   1, (affinity =    1) 
Node    0, rank    0, thread   2, (affinity =    2) 
Node    0, rank    0, thread   3, (affinity =    3) 
Node    0, rank    0, thread   4, (affinity =    4) 
Node    0, rank    0, thread   5, (affinity =    5) 
Node    0, rank    0, thread   6, (affinity =    6) 
Node    0, rank    0, thread   7, (affinity =    7) 
Node    0, rank    0, thread   8, (affinity =    8) 
Node    0, rank    0, thread   9, (affinity =    9) 
Node    0, rank    0, thread  10, (affinity =   10) 
Node    0, rank    0, thread  11, (affinity =   11) 
Node    0, rank    0, thread  12, (affinity =   12) 
Node    0, rank    0, thread  13, (affinity =   13) 
Node    0, rank    0, thread  14, (affinity =   14) 
Node    0, rank    0, thread  15, (affinity =   15) 
Node    0, rank    1, thread   0, (affinity =   16) 
Node    0, rank    1, thread   1, (affinity =   17) 
Node    0, rank    1, thread   2, (affinity =   18) 
Node    0, rank    1, thread   3, (affinity =   19) 
Node    0, rank    1, thread   4, (affinity =   20) 
Node    0, rank    1, thread   5, (affinity =   21) 
Node    0, rank    1, thread   6, (affinity =   22) 
Node    0, rank    1, thread   7, (affinity =   23) 
Node    0, rank    1, thread   8, (affinity =   24) 
Node    0, rank    1, thread   9, (affinity =   25) 
Node    0, rank    1, thread  10, (affinity =   26) 
Node    0, rank    1, thread  11, (affinity =   27) 
Node    0, rank    1, thread  12, (affinity =   28) 
Node    0, rank    1, thread  13, (affinity =   29) 
Node    0, rank    1, thread  14, (affinity =   30) 
Node    0, rank    1, thread  15, (affinity =   31) 
Node    0, rank    2, thread   0, (affinity =   32) 
Node    0, rank    2, thread   1, (affinity =   33) 
Node    0, rank    2, thread   2, (affinity =   34) 
Node    0, rank    2, thread   3, (affinity =   35) 
Node    0, rank    2, thread   4, (affinity =   36) 
Node    0, rank    2, thread   5, (affinity =   37) 
Node    0, rank    2, thread   6, (affinity =   38) 
Node    0, rank    2, thread   7, (affinity =   39) 
Node    0, rank    2, thread   8, (affinity =   40) 
Node    0, rank    2, thread   9, (affinity =   41) 
Node    0, rank    2, thread  10, (affinity =   42) 
Node    0, rank    2, thread  11, (affinity =   43) 
Node    0, rank    2, thread  12, (affinity =   44) 
Node    0, rank    2, thread  13, (affinity =   45) 
Node    0, rank    2, thread  14, (affinity =   46) 
Node    0, rank    2, thread  15, (affinity =   47) 
Node    0, rank    3, thread   0, (affinity =   48) 
Node    0, rank    3, thread   1, (affinity =   49) 
Node    0, rank    3, thread   2, (affinity =   50) 
Node    0, rank    3, thread   3, (affinity =   51) 
Node    0, rank    3, thread   4, (affinity =   52) 
Node    0, rank    3, thread   5, (affinity =   53) 
Node    0, rank    3, thread   6, (affinity =   54) 
Node    0, rank    3, thread   7, (affinity =   55) 
Node    0, rank    3, thread   8, (affinity =   56) 
Node    0, rank    3, thread   9, (affinity =   57) 
Node    0, rank    3, thread  10, (affinity =   58) 
Node    0, rank    3, thread  11, (affinity =   59) 
Node    0, rank    3, thread  12, (affinity =   60) 
Node    0, rank    3, thread  13, (affinity =   61) 
Node    0, rank    3, thread  14, (affinity =   62) 
Node    0, rank    3, thread  15, (affinity =   63) 
Node    0, rank    4, thread   0, (affinity =   64) 
Node    0, rank    4, thread   1, (affinity =   65) 
Node    0, rank    4, thread   2, (affinity =   66) 
Node    0, rank    4, thread   3, (affinity =   67) 
Node    0, rank    4, thread   4, (affinity =   68) 
Node    0, rank    4, thread   5, (affinity =   69) 
Node    0, rank    4, thread   6, (affinity =   70) 
Node    0, rank    4, thread   7, (affinity =   71) 
Node    0, rank    4, thread   8, (affinity =   72) 
Node    0, rank    4, thread   9, (affinity =   73) 
Node    0, rank    4, thread  10, (affinity =   74) 
Node    0, rank    4, thread  11, (affinity =   75) 
Node    0, rank    4, thread  12, (affinity =   76) 
Node    0, rank    4, thread  13, (affinity =   77) 
Node    0, rank    4, thread  14, (affinity =   78) 
Node    0, rank    4, thread  15, (affinity =   79) 
Node    0, rank    5, thread   0, (affinity =   80) 
Node    0, rank    5, thread   1, (affinity =   81) 
Node    0, rank    5, thread   2, (affinity =   82) 
Node    0, rank    5, thread   3, (affinity =   83) 
Node    0, rank    5, thread   4, (affinity =   84) 
Node    0, rank    5, thread   5, (affinity =   85) 
Node    0, rank    5, thread   6, (affinity =   86) 
Node    0, rank    5, thread   7, (affinity =   87) 
Node    0, rank    5, thread   8, (affinity =   88) 
Node    0, rank    5, thread   9, (affinity =   89) 
Node    0, rank    5, thread  10, (affinity =   90) 
Node    0, rank    5, thread  11, (affinity =   91) 
Node    0, rank    5, thread  12, (affinity =   92) 
Node    0, rank    5, thread  13, (affinity =   93) 
Node    0, rank    5, thread  14, (affinity =   94) 
Node    0, rank    5, thread  15, (affinity =   95) 
Node    0, rank    6, thread   0, (affinity =   96) 
Node    0, rank    6, thread   1, (affinity =   97) 
Node    0, rank    6, thread   2, (affinity =   98) 
Node    0, rank    6, thread   3, (affinity =   99) 
Node    0, rank    6, thread   4, (affinity =  100) 
Node    0, rank    6, thread   5, (affinity =  101) 
Node    0, rank    6, thread   6, (affinity =  102) 
Node    0, rank    6, thread   7, (affinity =  103) 
Node    0, rank    6, thread   8, (affinity =  104) 
Node    0, rank    6, thread   9, (affinity =  105) 
Node    0, rank    6, thread  10, (affinity =  106) 
Node    0, rank    6, thread  11, (affinity =  107) 
Node    0, rank    6, thread  12, (affinity =  108) 
Node    0, rank    6, thread  13, (affinity =  109) 
Node    0, rank    6, thread  14, (affinity =  110) 
Node    0, rank    6, thread  15, (affinity =  111) 
Node    0, rank    7, thread   0, (affinity =  112) 
Node    0, rank    7, thread   1, (affinity =  113) 
Node    0, rank    7, thread   2, (affinity =  114) 
Node    0, rank    7, thread   3, (affinity =  115) 
Node    0, rank    7, thread   4, (affinity =  116) 
Node    0, rank    7, thread   5, (affinity =  117) 
Node    0, rank    7, thread   6, (affinity =  118) 
Node    0, rank    7, thread   7, (affinity =  119) 
Node    0, rank    7, thread   8, (affinity =  120) 
Node    0, rank    7, thread   9, (affinity =  121) 
Node    0, rank    7, thread  10, (affinity =  122) 
Node    0, rank    7, thread  11, (affinity =  123) 
Node    0, rank    7, thread  12, (affinity =  124) 
Node    0, rank    7, thread  13, (affinity =  125) 
Node    0, rank    7, thread  14, (affinity =  126) 
Node    0, rank    7, thread  15, (affinity =  127) 
Node    1, rank    8, thread   0, (affinity =    0) 
Node    1, rank    8, thread   1, (affinity =    1) 
Node    1, rank    8, thread   2, (affinity =    2) 
Node    1, rank    8, thread   3, (affinity =    3) 
Node    1, rank    8, thread   4, (affinity =    4) 
Node    1, rank    8, thread   5, (affinity =    5) 
Node    1, rank    8, thread   6, (affinity =    6) 
Node    1, rank    8, thread   7, (affinity =    7) 
Node    1, rank    8, thread   8, (affinity =    8) 
Node    1, rank    8, thread   9, (affinity =    9) 
Node    1, rank    8, thread  10, (affinity =   10) 
Node    1, rank    8, thread  11, (affinity =   11) 
Node    1, rank    8, thread  12, (affinity =   12) 
Node    1, rank    8, thread  13, (affinity =   13) 
Node    1, rank    8, thread  14, (affinity =   14) 
Node    1, rank    8, thread  15, (affinity =   15) 
Node    1, rank    9, thread   0, (affinity =   16) 
Node    1, rank    9, thread   1, (affinity =   17) 
Node    1, rank    9, thread   2, (affinity =   18) 
Node    1, rank    9, thread   3, (affinity =   19) 
Node    1, rank    9, thread   4, (affinity =   20) 
Node    1, rank    9, thread   5, (affinity =   21) 
Node    1, rank    9, thread   6, (affinity =   22) 
Node    1, rank    9, thread   7, (affinity =   23) 
Node    1, rank    9, thread   8, (affinity =   24) 
Node    1, rank    9, thread   9, (affinity =   25) 
Node    1, rank    9, thread  10, (affinity =   26) 
Node    1, rank    9, thread  11, (affinity =   27) 
Node    1, rank    9, thread  12, (affinity =   28) 
Node    1, rank    9, thread  13, (affinity =   29) 
Node    1, rank    9, thread  14, (affinity =   30) 
Node    1, rank    9, thread  15, (affinity =   31) 
Node    1, rank   10, thread   0, (affinity =   32) 
Node    1, rank   10, thread   1, (affinity =   33) 
Node    1, rank   10, thread   2, (affinity =   34) 
Node    1, rank   10, thread   3, (affinity =   35) 
Node    1, rank   10, thread   4, (affinity =   36) 
Node    1, rank   10, thread   5, (affinity =   37) 
Node    1, rank   10, thread   6, (affinity =   38) 
Node    1, rank   10, thread   7, (affinity =   39) 
Node    1, rank   10, thread   8, (affinity =   40) 
Node    1, rank   10, thread   9, (affinity =   41) 
Node    1, rank   10, thread  10, (affinity =   42) 
Node    1, rank   10, thread  11, (affinity =   43) 
Node    1, rank   10, thread  12, (affinity =   44) 
Node    1, rank   10, thread  13, (affinity =   45) 
Node    1, rank   10, thread  14, (affinity =   46) 
Node    1, rank   10, thread  15, (affinity =   47) 
Node    1, rank   11, thread   0, (affinity =   48) 
Node    1, rank   11, thread   1, (affinity =   49) 
Node    1, rank   11, thread   2, (affinity =   50) 
Node    1, rank   11, thread   3, (affinity =   51) 
Node    1, rank   11, thread   4, (affinity =   52) 
Node    1, rank   11, thread   5, (affinity =   53) 
Node    1, rank   11, thread   6, (affinity =   54) 
Node    1, rank   11, thread   7, (affinity =   55) 
Node    1, rank   11, thread   8, (affinity =   56) 
Node    1, rank   11, thread   9, (affinity =   57) 
Node    1, rank   11, thread  10, (affinity =   58) 
Node    1, rank   11, thread  11, (affinity =   59) 
Node    1, rank   11, thread  12, (affinity =   60) 
Node    1, rank   11, thread  13, (affinity =   61) 
Node    1, rank   11, thread  14, (affinity =   62) 
Node    1, rank   11, thread  15, (affinity =   63) 
Node    1, rank   12, thread   0, (affinity =   64) 
Node    1, rank   12, thread   1, (affinity =   65) 
Node    1, rank   12, thread   2, (affinity =   66) 
Node    1, rank   12, thread   3, (affinity =   67) 
Node    1, rank   12, thread   4, (affinity =   68) 
Node    1, rank   12, thread   5, (affinity =   69) 
Node    1, rank   12, thread   6, (affinity =   70) 
Node    1, rank   12, thread   7, (affinity =   71) 
Node    1, rank   12, thread   8, (affinity =   72) 
Node    1, rank   12, thread   9, (affinity =   73) 
Node    1, rank   12, thread  10, (affinity =   74) 
Node    1, rank   12, thread  11, (affinity =   75) 
Node    1, rank   12, thread  12, (affinity =   76) 
Node    1, rank   12, thread  13, (affinity =   77) 
Node    1, rank   12, thread  14, (affinity =   78) 
Node    1, rank   12, thread  15, (affinity =   79) 
Node    1, rank   13, thread   0, (affinity =   80) 
Node    1, rank   13, thread   1, (affinity =   81) 
Node    1, rank   13, thread   2, (affinity =   82) 
Node    1, rank   13, thread   3, (affinity =   83) 
Node    1, rank   13, thread   4, (affinity =   84) 
Node    1, rank   13, thread   5, (affinity =   85) 
Node    1, rank   13, thread   6, (affinity =   86) 
Node    1, rank   13, thread   7, (affinity =   87) 
Node    1, rank   13, thread   8, (affinity =   88) 
Node    1, rank   13, thread   9, (affinity =   89) 
Node    1, rank   13, thread  10, (affinity =   90) 
Node    1, rank   13, thread  11, (affinity =   91) 
Node    1, rank   13, thread  12, (affinity =   92) 
Node    1, rank   13, thread  13, (affinity =   93) 
Node    1, rank   13, thread  14, (affinity =   94) 
Node    1, rank   13, thread  15, (affinity =   95) 
Node    1, rank   14, thread   0, (affinity =   96) 
Node    1, rank   14, thread   1, (affinity =   97) 
Node    1, rank   14, thread   2, (affinity =   98) 
Node    1, rank   14, thread   3, (affinity =   99) 
Node    1, rank   14, thread   4, (affinity =  100) 
Node    1, rank   14, thread   5, (affinity =  101) 
Node    1, rank   14, thread   6, (affinity =  102) 
Node    1, rank   14, thread   7, (affinity =  103) 
Node    1, rank   14, thread   8, (affinity =  104) 
Node    1, rank   14, thread   9, (affinity =  105) 
Node    1, rank   14, thread  10, (affinity =  106) 
Node    1, rank   14, thread  11, (affinity =  107) 
Node    1, rank   14, thread  12, (affinity =  108) 
Node    1, rank   14, thread  13, (affinity =  109) 
Node    1, rank   14, thread  14, (affinity =  110) 
Node    1, rank   14, thread  15, (affinity =  111) 
Node    1, rank   15, thread   0, (affinity =  112) 
Node    1, rank   15, thread   1, (affinity =  113) 
Node    1, rank   15, thread   2, (affinity =  114) 
Node    1, rank   15, thread   3, (affinity =  115) 
Node    1, rank   15, thread   4, (affinity =  116) 
Node    1, rank   15, thread   5, (affinity =  117) 
Node    1, rank   15, thread   6, (affinity =  118) 
Node    1, rank   15, thread   7, (affinity =  119) 
Node    1, rank   15, thread   8, (affinity =  120) 
Node    1, rank   15, thread   9, (affinity =  121) 
Node    1, rank   15, thread  10, (affinity =  122) 
Node    1, rank   15, thread  11, (affinity =  123) 
Node    1, rank   15, thread  12, (affinity =  124) 
Node    1, rank   15, thread  13, (affinity =  125) 
Node    1, rank   15, thread  14, (affinity =  126) 
Node    1, rank   15, thread  15, (affinity =  127) 
Node    2, rank   16, thread   0, (affinity =    0) 
Node    2, rank   16, thread   1, (affinity =    1) 
Node    2, rank   16, thread   2, (affinity =    2) 
Node    2, rank   16, thread   3, (affinity =    3) 
Node    2, rank   16, thread   4, (affinity =    4) 
Node    2, rank   16, thread   5, (affinity =    5) 
Node    2, rank   16, thread   6, (affinity =    6) 
Node    2, rank   16, thread   7, (affinity =    7) 
Node    2, rank   16, thread   8, (affinity =    8) 
Node    2, rank   16, thread   9, (affinity =    9) 
Node    2, rank   16, thread  10, (affinity =   10) 
Node    2, rank   16, thread  11, (affinity =   11) 
Node    2, rank   16, thread  12, (affinity =   12) 
Node    2, rank   16, thread  13, (affinity =   13) 
Node    2, rank   16, thread  14, (affinity =   14) 
Node    2, rank   16, thread  15, (affinity =   15) 
Node    2, rank   17, thread   0, (affinity =   16) 
Node    2, rank   17, thread   1, (affinity =   17) 
Node    2, rank   17, thread   2, (affinity =   18) 
Node    2, rank   17, thread   3, (affinity =   19) 
Node    2, rank   17, thread   4, (affinity =   20) 
Node    2, rank   17, thread   5, (affinity =   21) 
Node    2, rank   17, thread   6, (affinity =   22) 
Node    2, rank   17, thread   7, (affinity =   23) 
Node    2, rank   17, thread   8, (affinity =   24) 
Node    2, rank   17, thread   9, (affinity =   25) 
Node    2, rank   17, thread  10, (affinity =   26) 
Node    2, rank   17, thread  11, (affinity =   27) 
Node    2, rank   17, thread  12, (affinity =   28) 
Node    2, rank   17, thread  13, (affinity =   29) 
Node    2, rank   17, thread  14, (affinity =   30) 
Node    2, rank   17, thread  15, (affinity =   31) 
Node    2, rank   18, thread   0, (affinity =   32) 
Node    2, rank   18, thread   1, (affinity =   33) 
Node    2, rank   18, thread   2, (affinity =   34) 
Node    2, rank   18, thread   3, (affinity =   35) 
Node    2, rank   18, thread   4, (affinity =   36) 
Node    2, rank   18, thread   5, (affinity =   37) 
Node    2, rank   18, thread   6, (affinity =   38) 
Node    2, rank   18, thread   7, (affinity =   39) 
Node    2, rank   18, thread   8, (affinity =   40) 
Node    2, rank   18, thread   9, (affinity =   41) 
Node    2, rank   18, thread  10, (affinity =   42) 
Node    2, rank   18, thread  11, (affinity =   43) 
Node    2, rank   18, thread  12, (affinity =   44) 
Node    2, rank   18, thread  13, (affinity =   45) 
Node    2, rank   18, thread  14, (affinity =   46) 
Node    2, rank   18, thread  15, (affinity =   47) 
Node    2, rank   19, thread   0, (affinity =   48) 
Node    2, rank   19, thread   1, (affinity =   49) 
Node    2, rank   19, thread   2, (affinity =   50) 
Node    2, rank   19, thread   3, (affinity =   51) 
Node    2, rank   19, thread   4, (affinity =   52) 
Node    2, rank   19, thread   5, (affinity =   53) 
Node    2, rank   19, thread   6, (affinity =   54) 
Node    2, rank   19, thread   7, (affinity =   55) 
Node    2, rank   19, thread   8, (affinity =   56) 
Node    2, rank   19, thread   9, (affinity =   57) 
Node    2, rank   19, thread  10, (affinity =   58) 
Node    2, rank   19, thread  11, (affinity =   59) 
Node    2, rank   19, thread  12, (affinity =   60) 
Node    2, rank   19, thread  13, (affinity =   61) 
Node    2, rank   19, thread  14, (affinity =   62) 
Node    2, rank   19, thread  15, (affinity =   63) 
Node    2, rank   20, thread   0, (affinity =   64) 
Node    2, rank   20, thread   1, (affinity =   65) 
Node    2, rank   20, thread   2, (affinity =   66) 
Node    2, rank   20, thread   3, (affinity =   67) 
Node    2, rank   20, thread   4, (affinity =   68) 
Node    2, rank   20, thread   5, (affinity =   69) 
Node    2, rank   20, thread   6, (affinity =   70) 
Node    2, rank   20, thread   7, (affinity =   71) 
Node    2, rank   20, thread   8, (affinity =   72) 
Node    2, rank   20, thread   9, (affinity =   73) 
Node    2, rank   20, thread  10, (affinity =   74) 
Node    2, rank   20, thread  11, (affinity =   75) 
Node    2, rank   20, thread  12, (affinity =   76) 
Node    2, rank   20, thread  13, (affinity =   77) 
Node    2, rank   20, thread  14, (affinity =   78) 
Node    2, rank   20, thread  15, (affinity =   79) 
Node    2, rank   21, thread   0, (affinity =   80) 
Node    2, rank   21, thread   1, (affinity =   81) 
Node    2, rank   21, thread   2, (affinity =   82) 
Node    2, rank   21, thread   3, (affinity =   83) 
Node    2, rank   21, thread   4, (affinity =   84) 
Node    2, rank   21, thread   5, (affinity =   85) 
Node    2, rank   21, thread   6, (affinity =   86) 
Node    2, rank   21, thread   7, (affinity =   87) 
Node    2, rank   21, thread   8, (affinity =   88) 
Node    2, rank   21, thread   9, (affinity =   89) 
Node    2, rank   21, thread  10, (affinity =   90) 
Node    2, rank   21, thread  11, (affinity =   91) 
Node    2, rank   21, thread  12, (affinity =   92) 
Node    2, rank   21, thread  13, (affinity =   93) 
Node    2, rank   21, thread  14, (affinity =   94) 
Node    2, rank   21, thread  15, (affinity =   95) 
Node    2, rank   22, thread   0, (affinity =   96) 
Node    2, rank   22, thread   1, (affinity =   97) 
Node    2, rank   22, thread   2, (affinity =   98) 
Node    2, rank   22, thread   3, (affinity =   99) 
Node    2, rank   22, thread   4, (affinity =  100) 
Node    2, rank   22, thread   5, (affinity =  101) 
Node    2, rank   22, thread   6, (affinity =  102) 
Node    2, rank   22, thread   7, (affinity =  103) 
Node    2, rank   22, thread   8, (affinity =  104) 
Node    2, rank   22, thread   9, (affinity =  105) 
Node    2, rank   22, thread  10, (affinity =  106) 
Node    2, rank   22, thread  11, (affinity =  107) 
Node    2, rank   22, thread  12, (affinity =  108) 
Node    2, rank   22, thread  13, (affinity =  109) 
Node    2, rank   22, thread  14, (affinity =  110) 
Node    2, rank   22, thread  15, (affinity =  111) 
Node    2, rank   23, thread   0, (affinity =  112) 
Node    2, rank   23, thread   1, (affinity =  113) 
Node    2, rank   23, thread   2, (affinity =  114) 
Node    2, rank   23, thread   3, (affinity =  115) 
Node    2, rank   23, thread   4, (affinity =  116) 
Node    2, rank   23, thread   5, (affinity =  117) 
Node    2, rank   23, thread   6, (affinity =  118) 
Node    2, rank   23, thread   7, (affinity =  119) 
Node    2, rank   23, thread   8, (affinity =  120) 
Node    2, rank   23, thread   9, (affinity =  121) 
Node    2, rank   23, thread  10, (affinity =  122) 
Node    2, rank   23, thread  11, (affinity =  123) 
Node    2, rank   23, thread  12, (affinity =  124) 
Node    2, rank   23, thread  13, (affinity =  125) 
Node    2, rank   23, thread  14, (affinity =  126) 
Node    2, rank   23, thread  15, (affinity =  127) 
Node    3, rank   24, thread   0, (affinity =    0) 
Node    3, rank   24, thread   1, (affinity =    1) 
Node    3, rank   24, thread   2, (affinity =    2) 
Node    3, rank   24, thread   3, (affinity =    3) 
Node    3, rank   24, thread   4, (affinity =    4) 
Node    3, rank   24, thread   5, (affinity =    5) 
Node    3, rank   24, thread   6, (affinity =    6) 
Node    3, rank   24, thread   7, (affinity =    7) 
Node    3, rank   24, thread   8, (affinity =    8) 
Node    3, rank   24, thread   9, (affinity =    9) 
Node    3, rank   24, thread  10, (affinity =   10) 
Node    3, rank   24, thread  11, (affinity =   11) 
Node    3, rank   24, thread  12, (affinity =   12) 
Node    3, rank   24, thread  13, (affinity =   13) 
Node    3, rank   24, thread  14, (affinity =   14) 
Node    3, rank   24, thread  15, (affinity =   15) 
Node    3, rank   25, thread   0, (affinity =   16) 
Node    3, rank   25, thread   1, (affinity =   17) 
Node    3, rank   25, thread   2, (affinity =   18) 
Node    3, rank   25, thread   3, (affinity =   19) 
Node    3, rank   25, thread   4, (affinity =   20) 
Node    3, rank   25, thread   5, (affinity =   21) 
Node    3, rank   25, thread   6, (affinity =   22) 
Node    3, rank   25, thread   7, (affinity =   23) 
Node    3, rank   25, thread   8, (affinity =   24) 
Node    3, rank   25, thread   9, (affinity =   25) 
Node    3, rank   25, thread  10, (affinity =   26) 
Node    3, rank   25, thread  11, (affinity =   27) 
Node    3, rank   25, thread  12, (affinity =   28) 
Node    3, rank   25, thread  13, (affinity =   29) 
Node    3, rank   25, thread  14, (affinity =   30) 
Node    3, rank   25, thread  15, (affinity =   31) 
Node    3, rank   26, thread   0, (affinity =   32) 
Node    3, rank   26, thread   1, (affinity =   33) 
Node    3, rank   26, thread   2, (affinity =   34) 
Node    3, rank   26, thread   3, (affinity =   35) 
Node    3, rank   26, thread   4, (affinity =   36) 
Node    3, rank   26, thread   5, (affinity =   37) 
Node    3, rank   26, thread   6, (affinity =   38) 
Node    3, rank   26, thread   7, (affinity =   39) 
Node    3, rank   26, thread   8, (affinity =   40) 
Node    3, rank   26, thread   9, (affinity =   41) 
Node    3, rank   26, thread  10, (affinity =   42) 
Node    3, rank   26, thread  11, (affinity =   43) 
Node    3, rank   26, thread  12, (affinity =   44) 
Node    3, rank   26, thread  13, (affinity =   45) 
Node    3, rank   26, thread  14, (affinity =   46) 
Node    3, rank   26, thread  15, (affinity =   47) 
Node    3, rank   27, thread   0, (affinity =   48) 
Node    3, rank   27, thread   1, (affinity =   49) 
Node    3, rank   27, thread   2, (affinity =   50) 
Node    3, rank   27, thread   3, (affinity =   51) 
Node    3, rank   27, thread   4, (affinity =   52) 
Node    3, rank   27, thread   5, (affinity =   53) 
Node    3, rank   27, thread   6, (affinity =   54) 
Node    3, rank   27, thread   7, (affinity =   55) 
Node    3, rank   27, thread   8, (affinity =   56) 
Node    3, rank   27, thread   9, (affinity =   57) 
Node    3, rank   27, thread  10, (affinity =   58) 
Node    3, rank   27, thread  11, (affinity =   59) 
Node    3, rank   27, thread  12, (affinity =   60) 
Node    3, rank   27, thread  13, (affinity =   61) 
Node    3, rank   27, thread  14, (affinity =   62) 
Node    3, rank   27, thread  15, (affinity =   63) 
Node    3, rank   28, thread   0, (affinity =   64) 
Node    3, rank   28, thread   1, (affinity =   65) 
Node    3, rank   28, thread   2, (affinity =   66) 
Node    3, rank   28, thread   3, (affinity =   67) 
Node    3, rank   28, thread   4, (affinity =   68) 
Node    3, rank   28, thread   5, (affinity =   69) 
Node    3, rank   28, thread   6, (affinity =   70) 
Node    3, rank   28, thread   7, (affinity =   71) 
Node    3, rank   28, thread   8, (affinity =   72) 
Node    3, rank   28, thread   9, (affinity =   73) 
Node    3, rank   28, thread  10, (affinity =   74) 
Node    3, rank   28, thread  11, (affinity =   75) 
Node    3, rank   28, thread  12, (affinity =   76) 
Node    3, rank   28, thread  13, (affinity =   77) 
Node    3, rank   28, thread  14, (affinity =   78) 
Node    3, rank   28, thread  15, (affinity =   79) 
Node    3, rank   29, thread   0, (affinity =   80) 
Node    3, rank   29, thread   1, (affinity =   81) 
Node    3, rank   29, thread   2, (affinity =   82) 
Node    3, rank   29, thread   3, (affinity =   83) 
Node    3, rank   29, thread   4, (affinity =   84) 
Node    3, rank   29, thread   5, (affinity =   85) 
Node    3, rank   29, thread   6, (affinity =   86) 
Node    3, rank   29, thread   7, (affinity =   87) 
Node    3, rank   29, thread   8, (affinity =   88) 
Node    3, rank   29, thread   9, (affinity =   89) 
Node    3, rank   29, thread  10, (affinity =   90) 
Node    3, rank   29, thread  11, (affinity =   91) 
Node    3, rank   29, thread  12, (affinity =   92) 
Node    3, rank   29, thread  13, (affinity =   93) 
Node    3, rank   29, thread  14, (affinity =   94) 
Node    3, rank   29, thread  15, (affinity =   95) 
Node    3, rank   30, thread   0, (affinity =   96) 
Node    3, rank   30, thread   1, (affinity =   97) 
Node    3, rank   30, thread   2, (affinity =   98) 
Node    3, rank   30, thread   3, (affinity =   99) 
Node    3, rank   30, thread   4, (affinity =  100) 
Node    3, rank   30, thread   5, (affinity =  101) 
Node    3, rank   30, thread   6, (affinity =  102) 
Node    3, rank   30, thread   7, (affinity =  103) 
Node    3, rank   30, thread   8, (affinity =  104) 
Node    3, rank   30, thread   9, (affinity =  105) 
Node    3, rank   30, thread  10, (affinity =  106) 
Node    3, rank   30, thread  11, (affinity =  107) 
Node    3, rank   30, thread  12, (affinity =  108) 
Node    3, rank   30, thread  13, (affinity =  109) 
Node    3, rank   30, thread  14, (affinity =  110) 
Node    3, rank   30, thread  15, (affinity =  111) 
Node    3, rank   31, thread   0, (affinity =  112) 
Node    3, rank   31, thread   1, (affinity =  113) 
Node    3, rank   31, thread   2, (affinity =  114) 
Node    3, rank   31, thread   3, (affinity =  115) 
Node    3, rank   31, thread   4, (affinity =  116) 
Node    3, rank   31, thread   5, (affinity =  117) 
Node    3, rank   31, thread   6, (affinity =  118) 
Node    3, rank   31, thread   7, (affinity =  119) 
Node    3, rank   31, thread   8, (affinity =  120) 
Node    3, rank   31, thread   9, (affinity =  121) 
Node    3, rank   31, thread  10, (affinity =  122) 
Node    3, rank   31, thread  11, (affinity =  123) 
Node    3, rank   31, thread  12, (affinity =  124) 
Node    3, rank   31, thread  13, (affinity =  125) 
Node    3, rank   31, thread  14, (affinity =  126) 
Node    3, rank   31, thread  15, (affinity =  127) 
```
{: .output}

Each ARCHER2 compute node is made up of 8 NUMA (*Non Uniform Memory Access*) regions (4 per socket) 
with 16 cores in each region. Programs where the threads of a process span multiple NUMA regions
are likely to be *much less* efficient so we recommend using thread counts that fit well into the
ARCHER2 compute node layout. Effectively, this means one of the following options for hybrid jobs
on nodes where all cores are used:

* 8 MPI processes per node and 16 OpenMP threads per process: equivalent to 1 MPI process per NUMA region
* 16 MPI processes per node and 8 OpenMP threads per process: equivalent to 2 MPI processes per NUMA region
* 32 MPI processes per node and 4 OpenMP threads per process: equivalent to 4 MPI processes per NUMA region
* 64 MPI processes per node and 2 OpenMP threads per process: equivalent to 8 MPI processes per NUMA region 

## Other useful information

In this section we briefly introduce other scheduler topics that may be useful to users. We
provide links to more information on these areas for people who may want to explore these 
areas more. 

### Interactive jobs: direct `srun` 

Similar to the batch jobs covered above, users can also run interactive jobs using the `srun`
command directly. `srun` used in this way takes the same arguments as `sbatch` but, obviously, these are 
specified on the command line rather than in a job submission script. As for `srun` within 
a batch job, you should also provide the name of the executable you want to run.

For example, to execute `xthi` across all cores on two nodes (1 MPI process per core and no
OpenMP threading) within an interactive job you would issue the following commands:

```
auser@ln01:~> export OMP_NUM_THREADS=1
auser@ln01:~> module load xthi
auser@ln01:~> srun --partition=standard --qos=short --nodes=2 --ntasks-per-node=128 --cpus-per-task=1 --time=0:10:0 --account=ta001 xthi
```
{: .language-bash}
```
srun: job 851983 queued and waiting for resources
srun: job 851983 has been allocated resources
Node summary for    2 nodes:
Node    0, hostname nid001340, mpi 128, omp   1, executable xthi
Node    1, hostname nid001341, mpi 128, omp   1, executable xthi
MPI summary: 256 ranks 
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
...long output trimmed...
```
{: .output}


<!-- Need to add information on the solid state storage and Slurm once it is in place

### Using the ARCHER2 solid state storage

-->


{% include links.md %}


