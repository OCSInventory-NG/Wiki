# HOWTO package OCS Unix agent releases

To package the OCS Unix server release, we will use git to get code from GitHub.

## Get OcsInventory Unix agent code

Now, get the Ocsinventory Unix agent code from Launchpad using this command:

    $ git clone https://github.com/OCSInventory-NG/UnixAgent
    $ cd UnixAgent

## Create the tarball file

Create the release tarball using this commands:

    $ rm MANIFEST
    $ perl Makefile.PL

**`Warning`**`: If you are under Linux, you have to delete the ipdiscover binary before launching the next
commands. Delete the ipdiscover binary using this command: rm ipdiscover.`**

    $ make manifest
    $ make dist

The last command creates automatically the release tarball file named Ocsinventory-Unix-Agent-XXX.tar.gz
in the current directory.
