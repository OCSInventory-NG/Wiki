# Creating a standalone binary

We provide a script to prepare standalone binary of the unified agent. It uses several Perl dependencies :

* PAR::Packer
* XML::SAX::EXPAT
* Net::IP
* Digest::MD5
* Getopt::Long

We suggest you to use cpan to install it.

    #cpan

    cpan shell -- CPAN exploration and modules installation (v1.7602)
    ReadLine support enabled

    cpan> install PAR::Packer
    cpan> intall XML::SAX::Expat
    cpan> install Net::IP
    cpan> install Digest::MD5
    cpan> install Getopt::Long

Then you'll be able to use the script itself from the official archive:

    gunzip Ocsinventory-Agent-XXXXXXX.tar.gz # Not needed with a recent GNU tar
    tar xf Ocsinventory-Agent-XXXXXXX.tar
    cd Ocsinventory-Agent-XXXXXXX/
    $./tools/standalone.sh
    ocsinventory-agent.bin generated!

The binary is **ocsinventory-agent.bin**

# Creating a 'truly' standalone binary

A way to create a truly standalone binary (but _not_ architecture indepent), truly in this case means
that there's no need to install the perl libraries from the agent on the box that will be running the agent.

**NOTE**: The only thing remaining is telling PAR to also include the system libraries needed by
Compress::Zlib, Net::SSLeay among others

## Compiling the agent

Follow the above steps to create the binary (but don't install install it). Once it is build there's
some editing to be done:

### **Logging**

Modify the _Ocsinventory-Agent-0.0.10beta2/lib/Ocsinventory/Logger.pm_ file to always include it's
logging backends:

    package Ocsinventory::Logger;
    # TODO use Log::Log4perl instead.

    ### Include all Logger Backends so PAR will know about them
    use Ocsinventory::LoggerBackend::Stderr;
    use Ocsinventory::LoggerBackend::Syslog;
    use Ocsinventory::LoggerBackend::File;
    use Carp;


### **Network**

Modify the Ocsinventory-Agent-0.0.10beta2/lib/Ocsinventory/Agent/Network.pm file to always include modules needed

    package Ocsinventory::Agent::Network;
    # TODO:
    #  - set the correct deviceID and olddeviceID
    use strict;
    use warnings;

    use LWP::UserAgent;

    use Ocsinventory::Compress;


### **Backend**

Modify the Ocsinventory-Agent-0.0.10beta2/lib/Ocsinventory/Agent/Backend.pm file to always include all backends

    package Ocsinventory::Agent::Backend;

    use strict;
    no strict 'refs';
    use warnings;

    use ExtUtils::Installed;
    ### Include required modules
    use Net::SSLeay;
    use XML::SAX::Expat;
    use Proc::Daemon;
    ### Include available XML Responses
    use Ocsinventory::Agent::XML::Response::Inventory;
    use Ocsinventory::Agent::XML::Response::Prolog;
    ### Include available backends
    use Ocsinventory::Agent::Backend::DeviceID;
    use Ocsinventory::Agent::Backend::OS::Linux;
    use Ocsinventory::Agent::Backend::OS::Generic::Screen;
    use Ocsinventory::Agent::Backend::OS::Generic::Dmidecode::Memory;
    use Ocsinventory::Agent::Backend::OS::Generic::Dmidecode::Ports;
    use Ocsinventory::Agent::Backend::OS::Generic::Dmidecode::Slots;
    use Ocsinventory::Agent::Backend::OS::Generic::Dmidecode::Bios;
    use Ocsinventory::Agent::Backend::OS::Generic::Dmidecode;
    use Ocsinventory::Agent::Backend::OS::Generic::Hostname;
    use Ocsinventory::Agent::Backend::OS::Generic::Lspci::Modems;
    use Ocsinventory::Agent::Backend::OS::Generic::Lspci::Sounds;
    use Ocsinventory::Agent::Backend::OS::Generic::Lspci::Controllers;
    use Ocsinventory::Agent::Backend::OS::Generic::Lspci::Videos;
    use Ocsinventory::Agent::Backend::OS::Generic::Users;
    use Ocsinventory::Agent::Backend::OS::Generic::Packaging;
    use Ocsinventory::Agent::Backend::OS::Generic::Ipmi;
    use Ocsinventory::Agent::Backend::OS::Generic::Lspci;
    use Ocsinventory::Agent::Backend::OS::Generic::Packaging::Deb;
    use Ocsinventory::Agent::Backend::OS::Generic::Packaging::Gentoo;
    use Ocsinventory::Agent::Backend::OS::Generic::Packaging::RPM;
    use Ocsinventory::Agent::Backend::OS::Linux::Domains;
    use Ocsinventory::Agent::Backend::OS::Linux::Archs::PowerPC;
    use Ocsinventory::Agent::Backend::OS::Linux::Storages;
    use Ocsinventory::Agent::Backend::OS::Linux::Uptime;
    use Ocsinventory::Agent::Backend::OS::Linux::Network::IPv4;
    use Ocsinventory::Agent::Backend::OS::Linux::Network::Networks;
    use Ocsinventory::Agent::Backend::OS::Linux::CPU;
    use Ocsinventory::Agent::Backend::OS::Linux::Storages::HP;
    use Ocsinventory::Agent::Backend::OS::Linux::Storages::3ware;
    use Ocsinventory::Agent::Backend::OS::Linux::Storages::Lsilogic;
    use Ocsinventory::Agent::Backend::OS::Linux::Storages::Adaptec;
    use Ocsinventory::Agent::Backend::OS::Linux::Sounds;
    use Ocsinventory::Agent::Backend::OS::Linux::Drives;
    use Ocsinventory::Agent::Backend::OS::Linux::Controllers;
    use Ocsinventory::Agent::Backend::OS::Linux::Mem;
    use Ocsinventory::Agent::Backend::OS::Linux::Sys;
    use Ocsinventory::Agent::Backend::OS::Linux::Distro::NonLSB;
    use Ocsinventory::Agent::Backend::OS::Linux::Distro::NonLSB::Redhat;
    use Ocsinventory::Agent::Backend::OS::Linux::Distro::NonLSB::Ubuntu;
    use Ocsinventory::Agent::Backend::OS::Linux::Distro::NonLSB::Fedora;
    use Ocsinventory::Agent::Backend::OS::Linux::Distro::NonLSB::Slackware;
    use Ocsinventory::Agent::Backend::OS::Linux::Distro::NonLSB::Gentoo;
    use Ocsinventory::Agent::Backend::OS::Linux::Distro::NonLSB::Debian;
    use Ocsinventory::Agent::Backend::OS::Linux::Distro::NonLSB::Mandriva;
    use Ocsinventory::Agent::Backend::OS::Linux::Distro::NonLSB::Mandrake;
    use Ocsinventory::Agent::Backend::OS::Linux::Distro::NonLSB::Knoppix;
    use Ocsinventory::Agent::Backend::OS::Linux::Distro::NonLSB::Trustix;
    use Ocsinventory::Agent::Backend::OS::Linux::Distro::NonLSB::SuSE;
    use Ocsinventory::Agent::Backend::OS::Linux::Distro::LSB;
    use Ocsinventory::Agent::Backend::OS::Generic;
    use Ocsinventory::Agent::Backend::IpDiscover::Nmap;
    use Ocsinventory::Agent::Backend::IpDiscover::IpDiscover;
    use Ocsinventory::Agent::Backend::AccessLog;
    use Ocsinventory::Agent::Backend::IpDiscover;
    use Ocsinventory::Agent::Backend::OS::Linux;

## Create standalone binary

Now you can compile the standalone binary again, only this time it has all the required modules included:

    pp -B -c -o ocsinventory-agent.bin ocsinventory-agent

#TODO

Include libraries (.so) required by some modules.