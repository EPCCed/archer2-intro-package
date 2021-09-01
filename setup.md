---
title: Setup
---

Please try to complete the following setup tasks ahead of the lesson. 

## SSH client

All attendees should have an SSH client installed.
SSH is a tool that allows us to connect to and use a remote computer, within a terminal window, as our own.
Please follow the directions below to install an SSH client for your system.

**Windows**

Modern versions of Windows have SSH available in Powershell. You can test if it is available by typing `ssh --help` in Powershell. If it is
installed, you should see some useful output. If it is not installed, you will get an error. If SSH is not available in Powershell, then
you should install MobaXterm as described below.

An alternative is to install MobaXterm from [http://mobaxterm.mobatek.net](http://mobaxterm.mobatek.net). You will want to get the Home edition (Installer edition). However, if Git Bash works, you do not need this.

**macOS**

macOS comes with SSH pre-installed, so you should not need to install anything.

**Linux**

Linux users do not need to install anything, you should be set!

## Account on ARCHER2

Please sign up for your account on our HPC machine, [ARCHER2](https://www.archer2.ac.uk/), which will be available to
you for the duration of the course and for a few days afterwards, to allow you to
complete the practical exercises and put some of what you have learned into practice.

### Sign up for a SAFE account

To sign up, you must first register for an account on [SAFE](https://safe.epcc.ed.ac.uk/) (our service administration web application):

If you are already registered on the ARCHER or Tier-2 SAFE you do not need to re-register. Please proceed to the next step.

1. Go to the [SAFE New User Signup Form](https://safe.epcc.ed.ac.uk/signup.jsp)
2. Fill in your personal details. You can come back later and change them if you wish. _**Note:** you should register using your institutional or company email address - email domains such as gmail.com, outlook.com, etc. are not allowed to be used for access to ARCHER2_
3. Click “Submit”
4. You are now registered. A single use login link will be emailed to the email address you provided. You can use this link to login and set your password.

### Sign up for an account on ARCHER2 through SAFE

In addition to your password, you will need an SSH key pair to access ARCHER2. There is useful guidance on how
to generate SSH key pairs in [the ARCHER2 documentation](https://docs.archer2.ac.uk/user-guide/connecting/#ssh-key-pairs).
It is useful to have your SSH key pair generated before you request an account on ARCHER2 as you can add it when 
you request the account

1. [Login to SAFE](https://safe.epcc.ed.ac.uk)
2. Go to the Menu "Login accounts" and select "Request login account"
3. Choose the `ta001` project “Choose Project for Machine Account” box and click "Next"
4.  Select the *ARCHER2* machine in the list of available machines
5.  Click *Next*
6.  Enter a username for the account and an SSH public
    key
    1.  If you do not specify an SSH key at this stage, your default
        key will be used (if you have one). For users who had an ARCHER
        account, the default key will be your ARCHER SSH key.
    2.  You can always add an SSH key (or additional SSH keys) using
        the process described below.
7.  Click *Request*

Now you have to wait for the course organiser to accept your request to register. When this has happened, your account will be created on ARCHER2.
Once this has been done, you should be sent an email. _If you have not received an email but believe that your account should have been activated, check your account status in SAFE which will also show when the account has been activated._ You can then pick up your one shot initial password for ARCHER2 from your SAFE account.

### Log into ARCHER2

You should now be able to log into ARCHER2 by following the [login instructions in the ARCHER2 documentation](https://docs.archer2.ac.uk/user-guide/connecting/#ssh-clients).



{% include links.md %}

