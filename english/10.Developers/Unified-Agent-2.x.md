# Presentation

This is a draft about the futur development of the Perl agent (AKA OCS Inventory Unified Agent)
(AKA OCS Inventory UNIX Agent).

# Current issue with the 1.x series

* Memory usage. Currently the agent uses 40MB of memory. It's even worst with daemon mode because
this memory is not given back to the system when there is no activity.
* Limited module mechanism. It was designed for inventory and don't feet weel with the new needs
(software deployment, SNMP, WoL, ...)
* Only one OCS Inventory server per agent.

# Vocabulary

* Task: it's a feature of the agent (inventory, network scan, wol)

# Suggestions

## Module namespaces

    Ocsinventory::Agent # the agent itself

Create a module herarchy per task

    Ocsinventory::Agent::Task::$TaskName

Eg:

    Ocsinventory::Agent::Task::Inventory # Main Inventory module
    Ocsinventory::Agent::Task::Inventory::OS::Linux # Get inventory information from Linux
    ...
    Ocsinventory::Agent::Task::Deploy # Software deployment job
    ...
    Ocsinventory::Agent::Task::SNMP # SNMP feature module
    Ocsinventory::Agent::Task::SNMP::XXXXXX # The private modules for SNMP

## Split in subprocesses

In order to free the memory, every “task” has it own process. A minimalist main script will be used to launch them (ocsinventory-agent).

### **Minimalist main script**

* Very light
* A minimalist HTTP server listen on the 62354 port

### **Task processes**

* Limited execution time (Max 10 minutes)
* Can contact the server themself

## HTTP server

For now we will use HTTP::Daemon::SSL.

These HTTP error code are used:

* 200
* 403
* 500

### **Download**

Purpose: Download a file. The file have to be in the $VARDIR/shared

URI parameters:

* action: download
* file: filename


    http://client:62354/?action=download?file=/124567890/1234567890-1

### **Log**

Purpose: Get the last log file of the agent

URI parameters:

* action: log
* deviceid: $DEVICEID


    http://client:62354/?action=wakeup&deviceid=$DEVICEID

Returns an err 403 if $DEVICEID != machine deviceid

## Log

Ocsinventory::Agent::Logger will be used. We need to extend it to record the $TaskName too.

    [core][ERROR]This is an example from ocsinventory-agent
    [SNMP][ERROR]This is another example from the SNMP task
    [Inventory][ERROR]Yet another example from Inventory task
