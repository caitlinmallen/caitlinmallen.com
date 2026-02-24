---
layout: page 
title: macOS Memory Analysis 
permalink: /resources/macos/osxmemory
---
## Overview

*   First thing you should collect on a system
    *   Once a system restarts after malware is installed, it will be harder to detect the installation method when looking at the memory dump
        *   Also applied to any commands used
*   Tools
    *   [Volatility Framework](https://github.com/volatilityfoundation/volatility3)
        *   Works on OS X, Windows, Linux, and Android
    *   [Rekall Memory Analysis Framework](https://github.com/google/rekall)
        *   Live memory analysis or dump memory to the hard drive
        *   Load kext driver and run volatility commands directly on the system
    *   [OSXPMem](https://code.google.com/archive/p/pmem/wikis/OSXPmem.wiki)

## Artifacts

### Physical Memory

*   Data stored in the physical memory of the system -> memory dumps are referring to this

### Swap Files

**/private/var/vm/swapfile01**

*   Cache when physical memory fills
*   Data in physical memory is pushed to the swapfile and then swapped back into physical memory when needed
    *   Similar to the Windows pagefile
*   More than 1 of these file can exist (swapfile1, swapfile2, etc.)
    *   Size of these files depend on the memory of the computer

**/private/var/vm/sleepimage**

*   When OS X goes into hibernation, data in memory is put into the sleepimage file
    *   When computer is woken, the memory is restored from the sleepimage, allowing the user to pick up where they left off
*   Downside: any type of memory that exists in the hard drive will be encrypted by default on OS X 10.7 and greater
    *   Makes collecting the files a possible waste of hard drive space
        *   Need to adjust the OS settings to not encrypt these files, but not realistic that everybody will do it
        *   Check for encryption by looking at the output of ‘sysctl vm.swapusage.’

### Memory Acquisition

#### OSXPMem

*   Using OSXPMem
    *   Download from Rekall and unzip
        *   Creates an app called osxpmem.app
        *   Mach-O binary to dump memory and KEXT bundle
    *   Loading KEXT
        *   Must be root and in wheel group
            *   _chown -R root:wheel osxpmem.app/MacPmem.kext_
                *   Applies new permissions to .kext and any contents within it
    *   Once loaded, two new device files will be located at /dev/pmem and /_dev/pmem\_info_
        *   /dev/pmem
            *   Raw memory
        *   _/dev/pmem\_info_
            *   Information about the system that was collected
    *   Dump memory using _./osxpmem.app/osxpmem -o memory.aff4_
        *   \-o lets us specify the name of the memory dump
            *   .aff4 extension stands for Advanced Forensic File Format
    *   View contents using _oxpmem -V memory.aff4_
        *   Lists all sorts of data about the contents of the file
        *   Unique identifier ID for archive file
            *   Aff4 URN
                *   Any additional items added to the archive will use the same identification number
    *   Collecting swapfiles
        *   _osxpmem.app/ospmem -i /private/var/vm/swapfile\* -o memory.aff4_

#### Strings and Grep

*   Sometimes, just looking up words that exist can be the best way to find artifacts
    *   Raw memory dump is full of text that is not human readable
        *   Strings should be used on memory to pull human readable text -> save to a new file to save time for future searches
        *   Grep is used to search through the file for different keywords
*   Using Strings and Grep
    *   Example commands
        *   _strings memory.dmp > memory.strings_
            *   Dumps memory into the memory.strings file
        *   _egrep “sudo su” memory.strings_
            *   Searches the file for instances of ‘sudo su’
        *   _egrep -n3 “sudo su” memory.strings_
            *   Shows us three lines above any instance of ‘sudo su’ and three lines below

#### Volatility

*   One of the most widely used memory analysis framework
*   Open source and well documented
*   Checking functionality in Volatility
    *   _python vol.py –info | grep mac\__
*   Before you use, check what version of OS X you are using as it needs a specific profile to be used
    *   _sw\_vers_
        *   Example: If the output is 10.9.5, you need to load the 10.9.5 profile when using Volatility
            *   Search using the -info output for the profile
                *   _python vol.py –info | grep Mac | grep 10\\.9\\.5_
            *   Load profile using the following
                *   _python vol.py –profile <Profile Name> -f <Path to memory dump> <Plugin>_
*   Using Volatility
    *   Example using ifconfig module
        *   _python vol.py –profile <Profile> -f memory.dmp mac\_ifconfig_
            *   Would show the ifconfig info taken directly from the memory dump

##### Processes

*   In memory, multiple different locations store running process information
    *   Can collect with ps aux, but data could be unreliable
        *   Rootkits can modify the ps command or hooked the system to filter the output
*   **mac\_tasks**
    *   Go to command for viewing running processes
        *   mac\_pslist can sometimes skew during memory collection
    *   _python vol.py –profile <Profile> -f memory.dmp mac\_tasks_
*   **mac\_psaux**
    *   Shows the command line argument of each process ran
*   **mac\_dead\_procs**
    *   Pulls up a list of recently killed processes
*   **mac\_psxview**
    *   Search of six different Volatility process plugins and returns a true or false of whether or not each process showed up
    *   Helps find possible rootkits that are hiding their processes on the system
*   **mac\_netstat**
    *   Displays data regarding which processes is making which connections
    *   TCP and UDP connections made over UNIX Sockets can be seen
*   **mac\_network\_conns**
    *   Similar to netstat, but more thorough
        *   Doesn’t display process information, but more comprehensive list and contains connections made by kernel extensions
*   **mac\_check\_syscalls**
    *   Checks for modified system calls
        *   Popular for rootkits to do
            *   Hide data from users by filtering results before displaying them
*   **mac\_recover\_filesystem**
    *   Recover cached portions of the file system directly from memory
    *   Output will contain a number of recovered directories and files
    *   _python vol.py –profile <Profile> -f memory.dmp mac\_recover\_filesystem –dump-dir cleanup_
*   **mac\_arp**
    *   Prints ARP table showing recent systems the host communicated with
*   **mac\_bash**
    *   Recovers commands typed into the bash shell
        *   Can potentially recover cleared bash history
*   **mac\_procdump**
    *   One of Volatility’s most valuable features
    *   Extraction of binaries directly from memory

### Live Memory Analysis

*   Rekall Framework is used for live memory analysis
    *   Saves the output of various Rekall plugins to text files
*   Download, unzip Rekall, assign permissions, and load the KEXT file as root
    *   _sudo su_
    *   _unzip Rekall\_version.zip_
    *   _cd Rekall\_version_
    *   _tar -xvf MacPmem.kext.tgz_
    *   _chown -R root:wheel MacPmem.kext_
    *   _kextload MacPmem.kext_
*   Execute ‘rekall’ in the same directory
    *   _./recall -f /dev/pmem_
*   Rekal is an interactive shell that can have commands ran within it
    *   [Rekall cheat sheet from SANS](https://sansorg.egnyte.com/dl/L4hx0pM9y3)
*   Rekall can also be ran in one liners from the CLI
    *   _./rekall -f /dev/pmem lsmod_
    *   Save the above to a new file via _./rekal -f /dev/pmem lsmod –output rekal\_lsmod.txt_
