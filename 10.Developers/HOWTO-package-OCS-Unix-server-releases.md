# HOWTO package OCS Unix server releases

To package the OCS Unix server release, we will use bazaar to get code from Launchpad.

## Create the working directory

Create a working directory first and go in this directory:

    $mkdir OCSNG_UNIX_SERVER-XXX
    $cd OCSNG_UNIX_SERVER-XXX

## Get OcsInventory Server code

Now, get the Ocsinventory Server code from Launchpad using this command :

    $bzr export . lp:ocsinventory-server/stable-xxx

## Get OcsInventory Ocsreports code

Create a directory to put the ocsreports code and go in this directory:

    $mkdir ocsreports
    $cd ocsreports

Now, get the Ocsinventory ocsreports code from Launchpad using this command:

    $bzr export . lp:ocsinventory-ocsreports/stable-XXX

## Create the tarball file

Create the release tarball using this commands:

    $cd ..
    $tar -cvzf OCSNG_UNIX_SERVER-XXX.tar.gz OCSNG_UNIX_SERVER-XXX

You now have the OCSNG_UNIX_SERVER-XXX.tar.gz tarball in the parent directory.