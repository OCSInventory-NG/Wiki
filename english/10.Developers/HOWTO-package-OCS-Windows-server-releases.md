## REQUIREMENTS

* NSIS 2.10 or higher
([http://nsis.sourceforge.net](http://nsis.sourceforge.net/))
* File functions standard NSIS plugin
([http://nsis.sourceforge.net/Docs/AppendixE.html](http://nsis.sourceforge.net/Docs/AppendixE.html))
* TextReplace plugin
([http://nsis.sourceforge.net/TextReplace_plugin](http://nsis.sourceforge.net/TextReplace_plugin))
* Logic Library plugin
([http://nsis.sourceforge.net/LogicLib](http://nsis.sourceforge.net/LogicLib))
* Registry plugin
([http://nsis.sourceforge.net/Registry_plug-in](http://nsis.sourceforge.net/Registry_plug-in))
* ZipDLL plugin
([http://nsis.sourceforge.net/ZipDLL_plug-in](http://nsis.sourceforge.net/ZipDLL_plug-in))
* Services plugin
([http://nsis.sourceforge.net/Services_plug-in](http://nsis.sourceforge.net/Services_plug-in))

## BUILDING DEPENDANCIES

OCS Inventory NG Server for Windows uses following packages:

* XAMPP for Windows, ZIP version package xampp-win32-1.7.3.zip
([http://www.apachefriends.org/en/xampp-windows.html](http://www.apachefriends.org/en/xampp-windows.html))

    **`Note`**`: XAMPP Zip version 1.7.3 include Perl and mod_perl, so no need of additionnal XAMPP Perl Addon
    as with previous version`

* Perl XML::Simple module sources, XML-Simple-2.18.tar.gz (http://search.cpan.org/dist/XML-Simple/)

OCS Inventory NG Server for Windows needs the following directory structure:

    +---+--- ocsinventory-server (Sources of Communication Server)
        |
        +--- ocsinventory-reports (Sources of Administration Console)
        |
        +--+ ocsinventory-windows-server (Sources of Windows Server setup)
           |
           +--- XML-Simple-2.18

## BUIDLING NSIS INSTALLER

1) Get OCS Inventory NG Communication Server sources into folder ocsinventory-server

    bzr branch https://code.launchpad.net/ocsinventory-server/stable-1.3.1


2) Get OCS Inventory NG Administration Console sources into folder ocsinventory-ocsreports

    bzr branch https://code.launchpad.net/ocsinventory-ocsreports/stable-1.3.1


3) Get OCS Inventory NG Server for Windows sources into folder ocsinventory-windows-server

    bzr branch https://code.launchpad.net/ocsinventory-windows-server/stable-1.3.1


4) Put XAMPP Zip xampp-win32-1.7.3.zip file into folder ocsinventory-windows-server,
WITHOUT uncompressing it. It's the NSIS installer which will uncompress it.


5) Extract XML::Simple sources into folder ocsinventory-windows-server/XML-Simple-2.18


6) Compile NSIS script ocsinventory-windows-server/OCSNG-Windows-Server-Setup.nsi to create installer.


## INFORMATION ABOUT INSTALLER CODE

_OnInit_ function:

* Ensure only one instance is running
* Checks "HKLM\Software\OCS Inventory NG" for previous version to get previous setup folder
* Checks "HKLM\Software\xampp" for already installed XAMPP (no more filled with XAMPP 1.7.3) to get setup folder
* Checks "$INSTDIR\xammp-control.exe" to see if XAMPP is really installed
* Checks "$INSTDIR\Perl\bin\perl.exe" to see if Perl is really installed
* Checks "$INSTDIR\apache\modules\mod_perl.so" to see if mod_perl is really installed

_Section SEC01 "XAMPP Web Server"_ is used to setup XAMPP:

* Extract XAMPP ZIP File into TEMP directory
* Uncompress ZIP to user selected installation folder
* Copy XML::Simple files into XAMPP Perl site library ($INSTDIR\perl\site\lib)
* Launch XAMPP Setup batch command
* Register MySQL as a Windows service
* Register Apache as a Windows service

_Section SEC02 "OCS Inventory NG Server"_ is used to setup OCS Inventory NG Server:

* Checks "$INSTDIR\xammp-control.exe" to see if XAMPP is really installed
* Checks "$INSTDIR\Perl\bin\perl.exe" to see if Perl is really installed
* Checks "$INSTDIR\apache\modules\mod_perl.so" to see if mod_perl is really installed
* Stop Apache service
* Stop MySQL service
* Remove old OCS config (1.0 RC)
* Copy Communication Server files to Perl site library ($INSTDIR\perl\site\lib). Some files Like DTD and docs are copied to INSTDIR\OCS Inventory NG, with uninstaller.
* Copy Apache Communication Server configuration file to $INSTDIR\Apache\conf\extra
* Copy Administration Console files to $INSTDIR\htdocs\ocsreports
* Create package deployement directory into INSTDIR\htdocs\download
* Update Apache Communication Server configuration file
* Update MySQL configuration file $INSTDIR\mysql\bin\my.cnf to enable InnoDB and set max_allowed_packet to 4 MB
* Update MySQL configuration file $WINDIR\my.ini (if exist) to enable InnoDB and set max_allowed_packet to 4 MB
* Update HP configuration file $WINDIR\php.ini and $INSTDIR\Apache\bin\php.ini (if exist) to set memory_limit, post_max_size, upload_max_filesize to 128 MB, enable file_uploads, php_zip and php_openssl extensions
* Start MySQL service
* Start Apache service
* Launch Web browser to configure Administration Console.

_Section "-AdditionalIcons"_ creates icons for OCS Inventory NG is start menu and desktop.

_Section "-Post"_ creates uninstaller.

_Section "Uninstall"_ is used to remove OCS Inventory NG Server files, but not XAMPP Web Server.