---
title: "ARCHER2 software environment "
teaching: 30
exercises: 10
questions:
- "What does the ARCHER2 software environment look like and how do I access software?"
- "How can I find out what software is available?"
- "How can I request, install or get help with software on ARCHER2?"
objectives:
- "Know how to access different software on ARCHER2 using Lmod modules."
- "Know how to find out what is installed and where to get help."
keypoints:
- "Software is available through modules."
- "The CSE service can help with software issues."
---

<!-- TODO: Need to check all material here once Lmod is actually available -->

## Using software modules on ARCHER2

ARCHER2 software modules use the [Lmod](https://lmod.readthedocs.io/en/latest/) system to provide
access to different software and versions on the system. The modules and versions available will
change across the lifetime of the service.

Software modules are provided by both Cray and the ARCHER2 CSE team at [EPCC](https://www.epcc.ed.ac.uk).

## What modules are loaded when you log into ARCHER2?

All users start with a default set of modules loaded into their environment. These include:

   - Cray Compiler Environment (CCE)
   - Cray MPICH2 MPI library
   - Cray LibSci scientific and numerical libraries
   - Cray lightweight performance analysis toolkit
   - System modules to enable use of the ARCHER2 hardware

> ## Do not purge!
> 
> Unlike many other HPC systems you may have used, you should not use the `module purge` 
> command before starting to use the system. Some of the modules loaded by default are
> required for you to be able to use the system correctly and so many things will not
> work if you use `module purge`. If you need to change the setup, you will generally 
> use `module load` instead.
{: .callout}

## Finding out what software is available

You can query which software is provided by modules with the `module avail` command:

```
auser@login01-nmn:~> module avail
```
{: .language-bash}
```
------------------------------------------- /opt/cray/pe/perftools/20.05.0/modulefiles -------------------------------------------
perftools       perftools-lite-events  perftools-lite-hbm    perftools-nwpc     
perftools-lite  perftools-lite-gpu     perftools-lite-loops  perftools-preload  

--------------------------------------------- /opt/cray/pe/craype/2.6.4/modulefiles ----------------------------------------------
craype-hugepages1G  craype-hugepages4M   craype-hugepages32M   craype-hugepages256M  craype-network-slingshot10  
craype-hugepages2G  craype-hugepages8M   craype-hugepages64M   craype-hugepages512M  craype-x86-rome             
craype-hugepages2M  craype-hugepages16M  craype-hugepages128M  craype-network-none   

------------------------------------------------- /usr/local/Modules/modulefiles -------------------------------------------------
dot  module-git  module-info  modules  null  use.own  

---------------------------------------------------- /opt/cray/pe/modulefiles ----------------------------------------------------
atp/3.5.4(default)                    cray-libsci/20.03.1.4(default)             craype-dl-plugin-py3/20.05.1     
cce/10.0.0(default)                   cray-mpich-abi/8.0.10                      craype/2.6.4(default)            
cray-ccdb/4.5.4(default)              cray-mpich/8.0.10(default)                 craypkg-gen/1.3.9(default)       
cray-cti/2.5.6(default)               cray-netcdf-hdf5parallel/4.6.3.2(default)  gdb4hpc/4.5.6(default)           
cray-dsmml/0.1.0(default)             cray-netcdf/4.6.3.2(default)               iobuf/2.0.9(default)             
cray-fftw/3.3.8.4(default)            cray-openshmemx/10.1.0(default)            papi/5.7.0.3(default)            
cray-fftw/3.3.8.5                     cray-parallel-netcdf/1.11.1.1(default)     perftools-base/20.05.0(default)  
cray-ga/5.7.0.3                       cray-pmi-lib/6.0.5(default)                PrgEnv-cray/7.0.0(default)       
cray-hdf5-parallel/1.10.5.2(default)  cray-pmi/6.0.5(default)                    PrgEnv-gnu/7.0.0(default)        
cray-hdf5/1.10.5.2(default)           cray-stat/4.4.5(default)                   valgrind4hpc/2.5.5(default)      

----------------------------------------------------- /opt/cray/modulefiles ------------------------------------------------------
capsules/0.8.3(default)                                                                 
chapel/1.20.1(default)                                                                  
cray-lustre-client/2.12.0.5_cray_166_gf6711cf-7.0.1.0_2.24__gf6711cfb3.shasta(default)  
cray-shasta-mlnx-firmware/1.0.5(default)                                                
dvs/2.12_2.2.259-7.0.1.0_6.1__g6ee74127(default)                                        
libfabric/1.10.0.0.249(default)                                                         
spark/3.0.0(default)                                                                    
xpmem/2.2.35-7.0.1.0_3.9__gfa8d091.shasta(default)                                      

-------------------------------------------------------- /opt/modulefiles --------------------------------------------------------
cray-python/3.8.2.0(default)  cray-R/3.6.3(default)  gcc/8.1.0  gcc/9.3.0(default)  

```
{: .output}

The output lists the available modules and their versions. It also shows you which modules you currently
have loaded: those marked with `(L)`  and which modules are loaded by default (marked with `(D)`) when there
are multiple versions available and you do not specify the version when you load.

> ## Licensed software
> Some of the software installed on ARCHER2 requires the user to have their licence validated before they
> can use it on the service. More information on gaining access to licensed software through the SAFE
> is provided below.
{: .callout}

If you want more information on a particular module, you can use the `module help` command. For example,
to get more info on the `cray-netcdf` module:

```
aturner@login01-nmn:~> module help cray-netcdf
```
{: .language-bash}
```
-------------------------------------------------------------------
Module Specific Help for /opt/cray/pe/modulefiles/cray-netcdf/4.6.3.2:


cray-netcdf
===========

Release Date:
-------------
  November 2019

Purpose:
--------
  * Support for AMD's AOCC compiler.
  * Fixes a broken symlink for the 'bin' directory.

Product and OS Dependencies:
----------------------------
  The NetCDF release is supported on the following Cray systems:
    * Cray XC systems with CLE 6.0 or later

  The NetCDF 4.6.3.2 release requires the following software products:

    Cray HDF5 1.10.5.*
    CrayPE 2.1.2 or later

    One or more compilers:
        CCE 8.7 or later
        GCC 7.3 or later
        Intel 19.0 or later
        PGI 19.0 or later
        Allinea 18.2.0 or later
        AOCC 2.0 or later

Notes and Limitations:
---------------------
    Unidata now packages Netcdf-4 and legacy netcdf-3 separately. Cray has
    decided not to continue supplying the legacy Netcdf-3 package. Due to CCE
    changes a version of netcdf built with "-sreal64" is neither needed nor
    provided.

    NetCDF is supported on the host CPU but not on the accelerator on
    Cray XC systems.

Documentation:
--------------
    http://www.unidata.ucar.edu/software/netcdf/docs

Modulefile:
-----------
    module load cray-netcdf
    OR
    module load cray-netcdf-hdf5parallel

Product description:
--------------------
  NetCDF (network Common Data Form) is a set of interfaces for array-oriented
  data access and a freely-distributed collection of data access libraries for
  C, Fortran, C++, Java, and other languages. The netCDF libraries support a
  machine-independent format for representing scientific data. Together, the
  interfaces, libraries, and format support the creation, access, and sharing
  of scientific data.
```
{: .output}


<!-- Commented out until Lmod is really available and we can test

If you ask for more detailed information on a module, it will also tell you any modules that need to be
loaded as prerequisites to loading your chosen modules (i.e. the `netcdf/4.6.2` module's *dependencies*).
The Lmod tool will load these dependencies for you when you load the module (see below for more on this).

```
[auser@archer2-login1 ~]$ module spider netcdf/4.6.2
```
{: .bash}
```
------------------------------------------------------------------------------------------------------------------------
  netcdf: netcdf/4.6.2
------------------------------------------------------------------------------------------------------------------------
    Description:
      C Libraries for the Unidata network Common Data Form 


    You will need to load all module(s) on any one of the lines below before the "netcdf/4.6.2" module is available to load.

      intel/19.0.3.199  impi/2019.3.199
      intel/19.0.3.199  mpich/3.3
      intel/19.0.3.199  mvapich2/2.3
 
```
{: .output} -->


## Loading and switching modules

Lets look at our environment before we change anything. To see just our loaded modules we use the
`module list` (or `ml`) command:

```
auser@login01-nmn:~> ml
```
{: .language-bash}
```
Currently Loaded Modulefiles:
 1) cce/10.0.0(default)              5) craype/2.6.4(default)             9) cray-dsmml/0.1.0(default)                           
 2) cray-libsci/20.03.1.4(default)   6) craype-x86-rome                  10) perftools-base/20.05.0(default)                     
 3) cray-mpich/8.0.10(default)       7) libfabric/1.10.0.0.249(default)  11) xpmem/2.2.35-7.0.1.0_3.9__gfa8d091.shasta(default)  
 4) PrgEnv-cray/7.0.0(default)       8) craype-network-slingshot10 
```
{: .output}

You load modules with the `module load` (or `ml`) command. For example, to load the `cray-netcdf` module:

```
auser@login01-nmn:~> ml cray-netcdf
```
{: .language-bash}

> ## `ml` has two meanings
> Notice the the convenience command `ml` has a different meaning depending on whether or not you 
> supply an argument. Without an argument it means `module list` and with an argument, it means
> `module load`.
{: .callout}

Now, lets list our loaded modules again with `ml`:

```
auser@login01-nmn:~> ml
```
{: .language-bash}
```
Currently Loaded Modulefiles:
 1) cce/10.0.0(default)              5) craype/2.6.4(default)             9) cray-dsmml/0.1.0(default)                           
 2) cray-libsci/20.03.1.4(default)   6) craype-x86-rome                  10) perftools-base/20.05.0(default)                     
 3) cray-mpich/8.0.10(default)       7) libfabric/1.10.0.0.249(default)  11) xpmem/2.2.35-7.0.1.0_3.9__gfa8d091.shasta(default)  
 4) PrgEnv-cray/7.0.0(default)       8) craype-network-slingshot10       12) cray-netcdf/4.6.3.2(default) 
```
{: .output}

You can see that as well as the default `cray-netcdf` module (`cray-netcdf/4.6.3.2` as we did not specify a version
explicitly).

<!--
  Note: Once Lmod is working, we likely want to choose a module that has dependencies so we 
  can note that Lmod has automatically loaded dependencies. -->

If you want to swap two versions of the same module then you simply load the version that you 
want to swap for the currently loaded version, Lmod recognises that they are the same module 
with different versions and swaps them for you.

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

