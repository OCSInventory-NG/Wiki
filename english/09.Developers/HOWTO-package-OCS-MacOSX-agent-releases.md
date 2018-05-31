# HOWTO package OCS MacOSX server releases

To package the OCS MacOSX agent release, we will use the OCS Unix Agent release tarball file.

## Uncompress release tarball file

Get the OCS Unix agent release tarball and uncompress it by double clicking on it.

## Build OCSNG.app

Go to the _tools/macosx_ directory and build the appalication

    cd tools/macosx
    sh BUILDME.sh -release

## Build the graphical installer

* Go to the _installer_gui/iceberg_ directory and click on the iceberg_project.packproj to open the
Icerberg project.
* Click on _Build_ menu and _Build_ to build the installer

## Create the zip release file

* Go the the _build_ direcory inside the _iceberg_ directory
* Rename _Ocsinventory_Agent_MacOSX.pkg_ to _MacOcsinventory_Agent_MacOSX-XXX.pkg_
* Compress _MacOcsinventory_Agent_MacOSX-XXX.pkg_ file using a right click on the file