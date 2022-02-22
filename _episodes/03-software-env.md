---
title: "ARCHER2 software environment "
teaching: 30
exercises: 10
questions:
- "What does the ARCHER2 software environment look like and how do I access software?"
- "How can I find out what software is available?"
- "How can I request, install or get help with software on ARCHER2?"
objectives:
- "Know how to access different software on ARCHER2 using software modules."
- "Know how to find out what is installed and where to get help."
keypoints:
- "Software is available through modules."
- "The CSE service can help with software issues."
---

## Using software modules on ARCHER2

ARCHER2 software modules use the
[Lmod](https://lmod.readthedocs.io/) system to provide
access to different software and versions on the system. The modules and versions available will
change across the lifetime of the service.

Software modules are provided by both HPE and the ARCHER2 CSE team at
[EPCC](https://www.epcc.ed.ac.uk).

## What modules are loaded when you log into ARCHER2?

All users start with a default set of modules loaded into their environment. These include:

   - HPE Cray Compiler Environment (CCE)
   - HPE Cray MPICH MPI library
   - HPE Cray LibSci scientific and numerical libraries
   - System modules to enable use of the ARCHER2 hardware

You can see what modules you currently have loaded with the `module list` command:

```
auser@ln01:~> module list
```
{: .language-bash}
```

Currently Loaded Modules:
  1) cce/11.0.4              6) perftools-base/21.02.0                     11) bolt/0.7
  2) craype/2.7.6            7) xpmem/2.2.40-7.0.1.0_2.7__g1d7a24d.shasta  12) epcc-setup-env
  3) craype-x86-rome         8) cray-mpich/8.1.4                           13) load-epcc-module
  4) libfabric/1.11.0.4.71   9) cray-libsci/21.04.1.1
  5) craype-network-ofi     10) PrgEnv-cray/8.0.0

   
```
{: .output}

> ## Getting back if you purge or make a mistake
> 
> Unlike many other HPC systems you may have used, you should not generally use the
> `module purge` command before starting to use the system. Some of the modules
> loaded by default are required for you to be able to use the system correctly and
> so many things will not work if you use `module purge`. If you need to change the
> setup, you will generally use the `module load` command instead.
>
> If you do find yourself with a broken environment you can usually fix things by
> using the `module restore` command or by logging out and logging back in again.
{: .callout}

## Finding out what software is available

You can query which software is provided by modules with the `module avail` command:

```
auser@ln01:~> module avail
```
{: .language-bash}
```

------------------------------- /work/y07/shared/archer2-lmod/python/core -------------------------------
   matplotlib/3.4.3    netcdf4/1.5.7    seaborn/0.11.2

-------------------------------- /work/y07/shared/archer2-lmod/libs/core --------------------------------
   adios/1.13.1                                 gmp/6.2.1            parmetis/4.0.3
   arpack-ng/3.8.0                              gsl/2.7              petsc/3.14.2
   boost/1.72.0                                 hypre/2.18.0         scotch/6.1.0
   eigen/3.4.0                                  libxml2/2.9.7        slepc/3.14.1
   epcc-cray-hdf5-parallel/1.12.0.3      (D)    matio/1.5.18         superlu-dist/6.4.0
   epcc-cray-hdf5-parallel/1.12.0.7             metis/5.1.0          superlu/5.2.2
   epcc-cray-netcdf-hdf5parallel/4.7.4.3 (D)    mkl/19.5-281         trilinos/12.18.1
   epcc-cray-netcdf-hdf5parallel/4.7.4.7        mkl/21.2-2883 (D)
   glm/0.9.9.6                                  mumps/5.3.5

-------------------------------- /work/y07/shared/archer2-lmod/apps/core --------------------------------
   castep/20.11                    gromacs/2021.3     (D)    openfoam/com/v2106
   code_saturne/7.0.1-cce12        lammps/29_Sep_2021        openfoam/org/v8.20200901
   code_saturne/7.0.1-gcc11 (D)    namd/2.14-nosmp           openfoam/org/v9.20210903 (D)
   cp2k/cp2k-8.1            (D)    namd/2.14          (D)    quantum_espresso/6.8
   cp2k/cp2k-8.2                   nektar/5.0.3              vasp/5/5.4.4.pl2-vtst
   elk/elk-7.2.42                  nwchem/7.0.2              vasp/5/5.4.4.pl2         (D)
   gromacs/2021.3+plumed           onetep/6.1.3.7            vasp/6/6.2.1

------------------------------- /work/y07/shared/archer2-lmod/utils/core --------------------------------
   bolt/0.7     (L)    epcc-reframe/0.2         genmaskcpu/1.0    other-software/1.0    visidata/2.1
   cdo/1.9.9rc1        epcc-setup-env    (L)    gnuplot/5.4.2     reframe/3.8.2         vmd/1.9.3-gcc10
   cmake/3.21.3        gct/v6.2.20201212        ncl/6.6.2         usage-analysis/1.1    xthi/1.2

---------------- /opt/cray/pe/lmod/modulefiles/mpi/crayclang/10.0/ofi/1.0/cray-mpich/8.0 ----------------
   cray-hdf5-parallel/1.12.0.3 (D)    cray-parallel-netcdf/1.12.1.3 (D)
   cray-hdf5-parallel/1.12.0.7        cray-parallel-netcdf/1.12.1.7

---------------------------- /opt/cray/pe/lmod/modulefiles/perftools/21.02.0 ----------------------------
   perftools         perftools-lite-events    perftools-lite-hbm      perftools-preload
   perftools-lite    perftools-lite-gpu       perftools-lite-loops

---------------------- /opt/cray/pe/lmod/modulefiles/comnet/crayclang/10.0/ofi/1.0 ----------------------
   cray-mpich-abi/8.1.4 (D)    cray-mpich-abi/8.1.9    cray-mpich/8.1.4 (L,D)    cray-mpich/8.1.9

------------------------------- /opt/cray/pe/lmod/modulefiles/net/ofi/1.0 -------------------------------
   cray-openshmemx/11.2.0 (D)    cray-openshmemx/11.3.3

---------------------------- /opt/cray/pe/lmod/modulefiles/cpu/x86-rome/1.0 -----------------------------
   cray-fftw/3.3.8.9 (D)    cray-fftw/3.3.8.11

------------------------- /opt/cray/pe/lmod/modulefiles/compiler/crayclang/10.0 -------------------------
   cray-hdf5/1.12.0.3 (D)    cray-hdf5/1.12.0.7

--------------------------------- /usr/share/lmod/lmod/modulefiles/Core ---------------------------------
   lmod    settarg

---------------------------------- /opt/cray/pe/lmod/modulefiles/core -----------------------------------
   PrgEnv-aocc/8.0.0 (D)      cray-ccdb/4.11.1      (D)      cray-stat/4.11.5
   PrgEnv-aocc/8.1.0          cray-ccdb/4.12.4               craype/2.7.6           (L,D)
   PrgEnv-cray/8.0.0 (L,D)    cray-cti/2.13.6       (D)      craype/2.7.10
   PrgEnv-cray/8.1.0          cray-cti/2.15.5                craypkg-gen/1.3.14     (D)
   PrgEnv-gnu/8.0.0  (D)      cray-dsmml/0.1.4      (D)      craypkg-gen/1.3.18
   PrgEnv-gnu/8.1.0           cray-dsmml/0.2.1               gcc/9.3.0
   aocc/2.2.0                 cray-jemalloc/5.1.0.4          gcc/10.2.0             (D)
   aocc/2.2.0.1      (D)      cray-libpals/1.0.17            gcc/10.3.0
   aocc/3.0.0                 cray-libsci/21.04.1.1 (L,D)    gcc/11.2.0
   atp/3.13.1        (D)      cray-libsci/21.08.1.2          gdb4hpc/4.12.5         (D)
   atp/3.14.5                 cray-pals/1.0.17               gdb4hpc/4.13.5
   cce/11.0.4        (L,D)    cray-pmi-lib/6.0.10   (D)      iobuf/2.0.10
   cce/12.0.3                 cray-pmi-lib/6.0.13            papi/6.0.0.6           (D)
   cpe-cuda/21.09             cray-pmi/6.0.10       (D)      papi/6.0.0.9
   cpe/21.04         (D)      cray-pmi/6.0.13                perftools-base/21.02.0 (L,D)
   cpe/21.09                  cray-python/3.8.5.0   (D)      perftools-base/21.09.0
   cray-R/4.0.3.0    (D)      cray-python/3.9.4.1            valgrind4hpc/2.11.1    (D)
   cray-R/4.1.1.0             cray-stat/4.10.1      (D)      valgrind4hpc/2.12.4

------------------------- /opt/cray/pe/lmod/modulefiles/craype-targets/default --------------------------
   craype-accel-amd-gfx908    craype-hugepages256M    craype-network-none
   craype-accel-amd-gfx90a    craype-hugepages2G      craype-network-ofi  (L)
   craype-accel-host          craype-hugepages2M      craype-network-ucx
   craype-accel-nvidia70      craype-hugepages32M     craype-x86-milan
   craype-accel-nvidia80      craype-hugepages4M      craype-x86-rome     (L)
   craype-hugepages128M       craype-hugepages512M    craype-x86-trento
   craype-hugepages16M        craype-hugepages64M
   craype-hugepages1G         craype-hugepages8M

------------------------------------- /usr/local/share/modulefiles --------------------------------------
   load-epcc-module (L)

----------------------------------------- /opt/cray/modulefiles -----------------------------------------
   cray-lustre-client/2.12.4.2_cray_63_g79cd827-7.0.1.0_8.1__g79cd827237.shasta
   cray-shasta-mlnx-firmware/1.0.8
   dvs/2.12_4.0.112-7.0.1.0_15.1__ga97f35d9
   libfabric/1.11.0.4.71                                                        (L)
   xpmem/2.2.40-7.0.1.0_2.7__g1d7a24d.shasta                                    (L)

------------------------------------------- /opt/modulefiles --------------------------------------------
   aocc/2.2.0    aocc/2.2.0.1    aocc/3.0.0    cray-R/4.0.3.0    gcc/8.1.0    gcc/9.3.0    gcc/10.2.0

  Where:
   L:  Module is loaded
   D:  Default Module

Use "module spider" to find all possible modules and extensions.
Use "module keyword key1 key2 ..." to search for all possible modules matching any of the "keys".


                                         
```
{: .output}

The output lists the available modules and their versions. It also shows you which modules are
loaded by default (marked with `(D)`) when there are multiple versions available and you do
not specify the version when you load. An `(L)` next to a module marks one that is currently
loaded in your environment. 

> ## Licensed software
> Some of the software installed on ARCHER2 requires the user to have their licence validated before they
> can use it on the service. More information on gaining access to licensed software through the SAFE
> is provided below.
{: .callout}

Not all of the software modules on the system are listed using the `module avail` command when
you are first logged in. Some modules may not be available to load until prerequisite modules are
loaded. For example, you cannot see (using `module avail`) or load the `cray-netcdf` module until
the `cray-hdf5` module has been loaded. If you want to search all modules (including those hidden
by default) then you can use the `module spider` command, e.g.:

```
auser@ln01:~> module spider cray-netcdf
```
{: .language-bash}
```
-----------------------------------------------------------------------------------------------------
  cray-netcdf:
-----------------------------------------------------------------------------------------------------
     Versions:
        cray-netcdf/4.7.4.3
        cray-netcdf/4.7.4.7
     Other possible modules matches:
        cray-netcdf-hdf5parallel  epcc-cray-netcdf-hdf5parallel

-----------------------------------------------------------------------------------------------------
  To find other possible module matches execute:

      $ module -r spider '.*cray-netcdf.*'

-----------------------------------------------------------------------------------------------------
  For detailed information about a specific "cray-netcdf" package (including how to load the modules) use the module's full name.
  Note that names that have a trailing (E) are extensions provided by other modules.
  For example:

     $ module spider cray-netcdf/4.7.4.7
-----------------------------------------------------------------------------------------------------
```
{: .output}

If you specify the paritcular module version number to the `module spider` command then it will
also tell you which modules need to be loaded in order to be able to load the specified module,
e.g.:

```
auser@ln01:~> module spider cray-netcdf/4.7.4.3
```
{: .language-bash}
```

-----------------------------------------------------------------------------------------------------
  cray-netcdf: cray-netcdf/4.7.4.3
-----------------------------------------------------------------------------------------------------

    You will need to load all module(s) on any one of the lines below before the "cray-netcdf/4.7.4.3" module is available to load.

      aocc/2.2.0  cray-hdf5/1.12.0.3
      aocc/2.2.0.1  cray-hdf5/1.12.0.3
      cce/11.0.4  cray-hdf5/1.12.0.3
      cce/11.0.4  cray-hdf5/1.12.0.7
      gcc/10.2.0  cray-hdf5/1.12.0.3
      gcc/10.2.0  cray-hdf5/1.12.0.7
      gcc/10.3.0  cray-hdf5/1.12.0.3
      gcc/10.3.0  cray-hdf5/1.12.0.7
      gcc/11.2.0  cray-hdf5/1.12.0.3
      gcc/11.2.0  cray-hdf5/1.12.0.7
      gcc/9.3.0  cray-hdf5/1.12.0.3
      gcc/9.3.0  cray-hdf5/1.12.0.7
 
    Help:
      Release info:  /opt/cray/pe/netcdf/4.7.4.3/release_info

```
{: .output}

This output states that you need to have one of the compiler modules available (this should
always be the case) and that you need to load `cray-hdf5` before you can load (or see
using `module avail`) the cray-netcdf module.

## Loading and switching modules

Lets look at our environment before we change anything. As you may recall, to
see just our loaded modules we use the `module list` command:

```
auser@ln01:~> module list
```
{: .language-bash}
```

Currently Loaded Modules:
  1) cce/11.0.4              6) perftools-base/21.02.0                     11) bolt/0.7
  2) craype/2.7.6            7) xpmem/2.2.40-7.0.1.0_2.7__g1d7a24d.shasta  12) epcc-setup-env
  3) craype-x86-rome         8) cray-mpich/8.1.4                           13) load-epcc-module
  4) libfabric/1.11.0.4.71   9) cray-libsci/21.04.1.1
  5) craype-network-ofi     10) PrgEnv-cray/8.0.0

```
{: .output}

You load modules with the `module load` command. For example, to load the `gromacs` module:

```
auser@ln01:~> module load gromacs
```
{: .language-bash}
```

Lmod is automatically replacing "cce/11.0.4" with "gcc/10.2.0".


Lmod is automatically replacing "PrgEnv-cray/8.0.0" with "PrgEnv-gnu/8.0.0".

```
{: .output}

Now, lets list our loaded modules again with `module list` (you can also use `ml` as
a shortcut for `module list` or `module load` if it has an argument):

```
auser@ln01:~> ml
```
{: .language-bash}
```

Currently Loaded Modules:
  1) craype-x86-rome         5) epcc-setup-env     9) cray-libsci/21.08.1.2  13) cpe/21.09
  2) libfabric/1.11.0.4.71   6) load-epcc-module  10) cray-mpich/8.1.9       14) gromacs/2021.3
  3) craype-network-ofi      7) PrgEnv-gnu/8.1.0  11) craype/2.7.10
  4) bolt/0.7                8) cray-dsmml/0.2.1  12) gcc/11.2.0

                   
```
{: .output}

You can see that the default `gromacs` module (`gromacs/2021.3`) has been loaded (loading this
module has also swapped some other modules to math the environment that was used to
compile GROMACS, it has swapped the Cray compilers for the Gnu compilers and updated the
versions of some other modules).

## Licensed software

Some of the software installed on ARCHER2 requires a user to have a valid licence agreed with the 
software owners/developers to be able to use it (for example, VASP). Although you will be able to
load this software on ARCHER2 you will be barred from actually using it until your licence has been
verified.

You request access to licensed software through the EPCC SAFE (the web administration tool you used
to apply for your account and retrieve your initial password) by being added to the appropriate
*Package Group*. To request access to licensed software:

1. Log in to [SAFE](https://safe.epcc.ed.ac.uk)
2. Go to the Menu *Login accounts* and select the login account which requires access to the software
3. Click *New Package Group Request*
4. Select the software from the list of available packages and click *Select Package Group*
5. Fill in as much information as possible about your license; at the very least provide the information
   requested at the top of the screen such as the licence holder's name and contact details. If you are
   covered by the license because the licence holder is your supervisor, for example, please state this.
6. Click *Submit*

Your request will then be processed by the ARCHER2 Service Desk who will confirm your license with the
software owners/developers before enabling your access to the software on ARCHER2. This can take several
days (depending on how quickly the software owners/developers take to respond) but you will be advised
once this has been done.

## Getting help with software

You can find more information on the software available on ARCHER2 in the ARCHER2 Documentation at:

* [ARCHER2 Documentation](https://docs.archer2.ac.uk)

This includes information on the software provided by Cray and the software provided by the 
ARCHER2 CSE Service at EPCC.

If the software you require is not currently available or you are having trouble with the installed
software then please contact
[the ARCHER2 Service Desk](https://www.archer2.ac.uk/support-access/servicedesk.html) and they
will be able to assist you.

{% include links.md %}

