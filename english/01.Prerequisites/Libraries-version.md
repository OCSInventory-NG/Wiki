# Libraries and Modules versions

Here is the list of the different libraries and modules and their versions useful for OCS Inventory server as well as the Agents.

## OCS Inventory Server

* Apache version 2.2 or higher.
    * Mod_perl version 1.29 or higher.
* PHP 5.5 or higher, with ZIP and GD support enabled.
    * php_curl
    * php_mbstring
    * php_soap
    * php_xml
* PERL 5.6 or higher.
    * Perl module XML::Simple version 2.12 or higher.
    * Perl module Compress::Zlib version 1.33 or higher.
    * Perl module DBI version 1.40 or higher.
    * Perl module DBD::Mysql version 2.9004 or higher.
    * Perl module Apache::DBI version 0.93 or higher.
    * Perl module Net::IP version 1.21 or higher.
    * Perl module SOAP::Lite version 0.66 or higher (optional)
    * Perl module Mojolicious::Lite
    * Perl module Plack::Handler
    * Perl module Archive::Zip
    * Perl module YAML
    * Perl module XML::Entities
    * Perl module Switch
* MySQL or MariaDB version 4.1.0 or higher with InnoDB engine active. Mysql version upper than 5.5 are not supported but may work.
* Make utility such as GNU make.

## OCS Inventory Agent 2.x on Unix Operating Systems

* PERL 5.8 and higher
    * Perl module XML::Simple
    * Perl module Compress::Zlib
    * Perl module Net::IP
    * Perl module LWP::UserAgent
    * Perl module Digest::MD5
    * Perl module Net::SSLeay
    * Perl module Data::UUID
    * Perl Module Mac::SysProfile is needed on MacOSX
* dmidecode
* lspci on Linux and BSD (pciutils package)
* Make utility
* C/C++ compiler like GNU GCC

Recommended modules:

* Perl module IO::Socket::SSL
* Perl module Crypt::SSLeay
* Perl module LWP::Protocol::https
* Perl module Proc::Daemon
* Perl module Proc::PID::File if Proc::Daemon is installed
* Perl module Net::SNMP
* Perl module Net::Netmask
* Perl module Nmap::Parser
* Perl module Module::Install
* Perl module Net::CUPS
* Perl module Parse::EDID
* Nmap (v3.90 or superior)

## OCS Inventory Agent 2.x on Windows Operating Systems

OCS Inventory NG Agent 2.X does not work on Windows 9X, Windows Millennium Edition or Windows NT4.
You need to use old 1.X agent 4061-1.

On Windows XP and 2003R2 you can only use the Windows agent 2.1.1.1.

## OCS Inventory MacOSX Agent 2.x

OCS MacOSX agent 2.3 and newer agent are fully compatible under MacOSX 10.11 El captain and more.
For older macos system you will have to user older versions of our agent. We don't support Mac OS versions that are not maintained by Apple.

## OCS Inventory Android Agent

Have a device under Android 4.0 or higher.
