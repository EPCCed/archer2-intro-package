---
title: "Connecting to ARCHER2 and transferring data"
teaching: 10
exercises: 10
questions:
- "How can I access ARCHER2 interactively and transfer data?"
objectives:
- "Understand how to connect to ARCHER2."
- "Know how to transfer data onto and off of ARCHER2 efficiently."
keypoints:
- "ARCHER2's login address is `login.archer2.ac.uk`."
- "The password policy for ARCHER2 is well documented."
- "There are a number of ways to transfer data to/from ARCHER2."
---

## Connecting using SSH

The ARCHER2 login address is

:: ..

  login.archer2.ac.uk

Access to ARCHER2 is via SSH using either password or SSH key pair.

## Passwords and password policy

When you first get an ARCHER2 account, you will get a single-use password from the 
SAFE which you will be asked to change to a password of your choice. Your chosen 
password must have the required complexity as specified in the
[ARCHER2 Password Policy](https://www.archer2.ac.uk/about/policies/passwords_usernames.html).

The password policy has been chosen to allow users to use both complex, shorter passwords and
long, but comparatively simple passwords. For example both `LA10!£lsty` and `horsebatterystaple`
would be supported.

> ## Picking a good password
> Which of these passwords would be a good, valid choice according to the ARCHER2 Password
> Policy?
> * `rainbowllamajumping`
> * `horsebatterystaple`
> 
{.challenge}

## SSH keys

As well as password access, users can add the public part of an SSH key pair to access ARCHER2.
Once you are able to log in using your password you can append the public part of an SSH key 
pair to the `~/.ssh/authorized_keys` file on ARCHER2 to allow you to access the system using 
SSH keys.

The ARCHER2 User and Best Practice Guide has more information on how to create SSH key pairs
and how to use SSH Agents to make access more convenient while remaining secure. See:

* [Connecting to ARCHER2](https://docs.archer2.ac.uk/user-guide/connecting.html)

## Data transfer services: scp, rsync, Globus Online

ARCHER2 supports a number of different data transfer mechanisms. The one you choose depends
on the amount and structure of the data you want to transfer and where you want to transfer
the data to. The three main options are:

* scp: The standard way to transfer small to medium amounts of data off ARCHER2 to any other location
* rsync: Used if you need to keep small to medium datasets synchronised between two different locations
* Globus Online: Used to transfer large amounts of data to other sites which are Globus Online enabled



## Data transfer best practice (top 5 and point to data transfer documentation)


{% include links.md %}

