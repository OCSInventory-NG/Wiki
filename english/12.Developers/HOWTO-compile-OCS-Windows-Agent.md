# HOWTO compile OCS Windows Agent

To compile OCS Windows Agent, we will use git to get code from GitHub.

## Requirements

- Microsoft Visual C++ 2017 or higher
- Perl 5.8 or newer for building dependencies (you can use XAMPP perl addon)
- Source code exported from GitHub.
  git clone https://github.com/OCSInventory-NG/WindowsAgent.git

## Building dependencies

- zlib 1.2.8 or newer (www.zlib.net)
- openssl 1.0.2r or newer (www.openssl.org)
- curl 7.64.1 or newer (http://curl.haxx.se/)
- tinyXML 2.6.2 or newer (http://www.sourceforge.net/projects/tinyxml/)
- net-snmp 5.7.3 or newer (http://www.net-snmp.org/)
- ZipArchive GPL Edition 4.0.1 or newer (http://www.artpol-software.com)

**`Note : ZipArchive GPL Edition doesn't support 64-bit archive. To compile the Windows Agent with support for the 64-bit archive you can buy the commercial version of ZipArchive or ask us to integrate your code for the next release`**

Uncompress sources of these libraries into directory "External_Deps" to create
the following directory structures.
- External_Deps\Zlib-X.X.X
- External_Deps\openssl-X.X.X
- External_Deps\curl-X.X.X
- External_Deps\tinyxml
- External_Deps\net-snmp-X.X
- External_Deps\ZipArchive

### Building ZipArchive

- Open "ZipArchive.sln" Visual C++ 2013 solution in folder "External_Deps\ZipArchive".
- Select configuration "Release Unicode STL MD DLL"
for platform x64.
- Edit project properties and add
_BIND_TO_CURRENT_VCLIBS_VERSION preprocessor define to _Configuration
properties > C/C++ > Preprocessor_ section to automatically bind DLL to Visual
C++ 2017 and MFC versions.
- Save and build to create unicode DLL for ZipArchive Library.

### Building other libraries

Edit script _External_Deps\OCS_Make_Required_Libs.bat_ to meet your need, especially :

- Set path to MS Visual C++ 2017, for example
  set C:\Program Files (x86)\Microsoft Visual Studio\2017\Community\VC\Auxiliary\Build
- Set path to MS Windows SDK, needed to build cURL, for example with VC++ 2017
  set WINDOWS_SDK_PATH="C:\Program Files\Microsoft SDKs\Windows\vX.X"
- Set path to Perl 5.8 or higher binary, for example
  set PERL_PATH=C:\xampp\perl\bin
- Set path to Zlib sources, for example
  set ZLIB_PATH=D:\Developp\OCS Inventory NG\Bazaar\ocsinventory-windows-agent\External_Deps\zlib-1.2.8
- Set path to OpenSSL sources, for example
  set OPENSSL_PATH=D:\Developp\OCS Inventory NG\Bazaar\ocsinventory-windows-agent\External_Deps\openssl-1.0.2r
- Set path to cURL sources, for example
  set CURL_PATH=D:\Developp\OCS Inventory NG\Bazaar\ocsinventory-windows-agent\External_Deps\curl-7.64.1
- Set path to tinyXML sources, for example
  set XML_PATH=D:\Developp\OCS Inventory NG\Bazaar\ocsinventory-windows-agent\External_Deps\tinyxml
- Set path to ZipArchive sources, for example
  set ZIP_PATH=D:\Developp\OCS Inventory NG\Bazaar\ocsinventory-windows-agent\External_Deps\ZipArchive

Then, open _x64 Native Tools Command Prompt for VS 2017_ and launch script "OCS_Make_Required_Libs.bat" to create all libs and prepare for building OCS agent.

## Building Agent

Open solution "OCSInventory.sln", select configuration "Release" for platform x64, select project "Agent" and build it !

See Options.txt for agent's command line switches.

We hope it will works for you !