# OCS Inventory NG Agent 2.x on Unix Operating Systems

OCS Inventory NG agent for Linux can only be set up locally. You cannot deploy the agent
through the network currently as this is possible for Windows agent. However, you can choose during
setup to activate auto-update of the agent if you’ve chosen HTTP inventory method.

An automated deployment is now possible.

You can use the [Packager for unix](https://github.com/OCSInventory-NG/Packager-for-Unix) to create a standalone package

Automated deployment is possible using our [ansible role](https://github.com/OCSInventory-NG/Ansible-Role-For-Unix-Packager)

**`Warning: You must have root privileges to set Ocsinventory-agent up.`**

## Requirements

Required modules and commands:

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
* lspci on Linux and *BSD (pciutils package)
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


**`Note: it’s better for system integrity to use the precompiled packages for your distribution
if they are available.`**

**On Fedora/Redhat/Centos7 like Linux**, you can use “yum” tool to set required modules up like following:

    $ sudo yum install perl-XML-Simple perl-devel perl-Compress-Zlib perl-Net-IP perl-LWP perl-Digest-MD5 perl-Net-SSLeay perl-Data-UUID

Optional modules: these modules are available on [`EPEL`](https://fedoraproject.org/wiki/EPEL/FAQ#howtouse) repository. Don't forget to add this repository to your system or download each package individually from the repository

    $ sudo yum install perl-Crypt-SSLeay perl-Net-SNMP perl-Proc-Daemon perl-Proc-PID-File perl-Sys-Syslog pciutils smartmontools monitor-edid

**On Debian Stretch like Linux**, you can use “apt” tool to set required modules up:

    $ sudo apt install libmodule-install-perl dmidecode libxml-simple-perl libcompress-zlib-perl libnet-ip-perl libwww-perl libdigest-md5-perl libdata-uuid-perl

Optional modules: but highly recommended

    $ sudo apt install libcrypt-ssleay-perl libnet-snmp-perl libproc-pid-file-perl libproc-daemon-perl net-tools libsys-syslog-perl pciutils smartmontools read-edid nmap libnet-netmask-perl

**Unix agent 2.x is now installed without script “setup.sh”. During compilation, information about
configuration and dependencies are returned. However, it will never upgrade an installed module.
If one module has version lower than required once, you must upgrade it yourself.**

**`Warning`**`: The installer does not install ancestor dependencies. For example, Net::SSLeay requires
openssl to be installed. If not installed, setup of Net::SSLeay will fail and OCS Inventory NG agent
setup will also fail.`

**Also, a log file is generated. If you encounter any error while installing OCS Inventory NG agent,
please refer to this file to have detailed error messages.**

## Installing UNIX Agent with RPM

**On Fedora/Redhat/Centos 7** you can install the unix agent with RPM

You need to have "wget" to download the repo of EPEL and OCS

   $ sudo wget https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
   $ sudo wget https://rpm.ocsinventory-ng.org/ocsinventory-release-latest.el7.ocs.noarch.rpm

You can install the repo with "yum"

   $ sudo  yum install ocsinventory-release-latest.el7.ocs.noarch.rpm epel-release-latest-7.noarch.rpm

To install the unix agent and requierement use this command :

   $ sudo yum install ocsinventory-agent

**`Note : The unix agent gonna be installed with default settings.`**


## Installing the agent non-interactively

Download “Ocsinventory-Agent-2.x.y.tar.gz” from [OCS Inventory Web Site](http://www.ocsinventory-ng.org/en/#download-en).


1. Unpack it.

        $ sudo tar –xvzf Ocsinventory-Agent-2.x.y.tar.gz
        $ sudo cd Ocsinventory-Agent-2.x.y

2. Check perl configuration with the script Makefile.PL. Its looks at the configuration of Perl, machine,
libraries ... and it generates the Makefile. During this step, a temporary environment variable is created to install agent non-interactively.

        $ sudo env PERL_AUTOINSTALL=1 perl Makefile.PL

    Exemple :

       Please install Crypt::SSLeay if you want to use SSL.
       Please install nmap or ipdiscover if you want to use the network discover feature.
       Please install Proc::Daemon and Proc::PID::File if you want to use the daemon mode.

3. Compilation

        $ sudo make
        $ sudo make install

**`Note: Installer writes a log file “ocs_agent_setup.log” in the same directory. If you encounter any errors,
please refer to this log for the detailed error message.`**

A check for PERL interpreter binary, C/C++ compiler and make utility is made during installation.
If one of these components is not found, setup will stop.

Setup will check for:

* dmidecode binary.
* Compress::Zlib PERL module
* XML::Simple PERL module
* Net::IP PERL module
* LWP::UserAgent PERL module
* Digest::MD5 PERL module
* Net::SSLeay PERL module

If not found, it will ask you if you wish to install it. Enter “y” or validate to enable install
of required component. You need to have access to Internet or local repositories. If you enter “n”, setup will stop here.

## Installing the agent interactively

Download “Ocsinventory-Unix-Agent-2.x.y.tar.gz” from [OCS Inventory Web Site](http://www.ocsinventory-ng.org/en/#download-en).


1. Unpack it.

        $ sudo tar –xvzf Ocsinventory-Unix-Agent-2.x.y.tar.gz
        $ sudo cd Ocsinventory-Unix-Agent-2.x.y

2. Check perl configuration with the script Makefile.PL. Its looks at the configuration of Perl, machine,
libraries ... and it generates the Makefile.

        $sudo perl Makefile.PL

    Exemple :

       Please install Crypt::SSLeay if you want to use SSL.
       Please install nmap or ipdiscover if you want to use the network discover feature.
       Please install Proc::Daemon and Proc::PID::File if you want to use the daemon monde.

3. Compilation

        $ sudo make
        $ sudo make install

**`Note: Installer writes a log file “ocs_agent_setup.log” in the same directory. If you encounter any errors,
please refer to this log for the detailed error message.`**

A check for PERL interpreter binary, C/C++ compiler and make utility is made during installation.
If one of these components is not found, setup will stop.

Setup will check for:

* dmidecode binary.
* Compress::Zlib PERL module
* XML::Simple PERL module
* Net::IP PERL module
* LWP::UserAgent PERL module
* Digest::MD5 PERL module
* Net::SSLeay PERL module

If not found, it will ask you if you wish to install it. Enter “y” or validate to enable install
of required component. You need to have access to Internet or local repositories. If you enter “n”, setup will stop here.


**Configuration begins. You answer with y for yes n for no or specify link or location. Letter in brackets [] is chosen if you press enter.**

    Do you want to configure the agent
    Please enter 'y' or 'n'?> [y] y

    Where do you want to write the configuration file?
    0 -> /etc/ocsinventory
    1 -> /usr/local/etc/ocsinventory
    2 -> /etc/ocsinventory-agent
    ?>  2

    Do you want to create the directory /etc/ocsinventory-agent?
    Please enter 'y' or 'n'?> [y] y

    Should the old unix_agent settings be imported ?
    Please enter 'y' or 'n'?> [y] y

    [info] The config file will be written in /etc/ocsinventory-agent/ocsinventory-agent.cfg,

    What is the address of your ocs server?>  https://ocs/ocsinventory

    Do you need credential for the server? (You probably don't)
    Please enter 'y' or 'n'?> [n]

    Do you want to apply an administrative tag on this machine
    Please enter 'y' or 'n'?> [y]
    tag?>  Server

    Do yo want to install the cron task in /etc/cron.d
    Please enter 'y' or 'n'?> [y]

    Where do you want the agent to store its files? (You probably don't need to change it)?> [/var/lib/ocsinventory-agent]

    Should I remove the old unix_agent
    Please enter 'y' or 'n'?> [n]

    Do you want to activate debug configuration option ?
    Please enter 'y' or 'n'?> [y] n

    Do you want to use OCS Inventory NG UNix Unified agent log file ?
    Please enter 'y' or 'n'?> [y]

    Specify log file path you want to use?>  /var/log/ocs_agent.log

    Do you want disable SSL CA verification configuration option (not recommended) ?
    Please enter 'y' or 'n'?> [n]

    Do you want to set CA certificate chain file path ?
    Please enter 'y' or 'n'?> [y] y

    Specify CA certificate chain file path?>  /etc/ocsinventory-agent/cacert.pem

    Do you want to use OCS-Inventory software deployment feature?
    Please enter 'y' or 'n'?> [y]

    Do you want to use OCS-Inventory SNMP scans feature?
    Please enter 'y' or 'n'?> [y]

    Do you want to send an inventory of this machine?
    Please enter 'y' or 'n'?> [y]


Here is a sample configuration file for OCS Inventory NG Linux agent.

    <CONF>
        <DEVICEID>computer.domain.tld-2006-02-27-13-59-47</DEVICEID>
        <DMIVERSION>2.2</DMIVERSION>
        <IPDISCOVER_VERSION>3</IPDISCOVER_VERSION>
        <OCSFSERVER>my_ocs_com_server.domain.tld:80</OCSFSERVER>
    </CONF>
You can choose between 3 methods for sending inventory:

1. http: computer is connected to the network and is able to reach the Communication server with
HTTP protocol *USED BY DEFAULT*.

2. https: computer is connected to the network and is able to reach the Communication server with
HTTPS protocol. You have to configure SSL on your OCS Server and copy the SSL certificate
on the agent directory to use this method.

3. local: computer is not connected to the network and inventory will be generated in a file manually sent to OCS Inventory NG by the operator. This option must be set manually in ocsinventory-agent.conf like this :

        local=/tmp

For two others methods :

    Syntax : http[s]://ocsinventory-ng-server[:port]/ocsinventory

Examples :

    ocsserver.domains.local
    https://w.x.y.z
    ocsserver.domains.local:1234
    https://ocsserver.domains.local

**Figure 5 : Sample agent’s configuration file ocsinv.conf for a network connected computer.**

# Deploying agent through scripted installation without user interaction

Actually not possible in version 2.0. This feature will be integrated in 2.1.

**`Warning: This feature will be only available in OCS 2.1. Don't try this options using OCS 2.0
Unix agent...it won't work !!`**

Since OCS Unix Unified agent 2.1, you are able to launch _postinst.pl_ script in non-interactive mode.
A set of launch arguments has been added to this script to allow to set all configuration options
as you can do in interactive mode. This is a list of all available _postinst.pl_ script arguments:

* **--nowizard** : launch this script without interaction
* **--server** : set OCS Inventory NG server address (e.g: http://ocsinventory-ng/ocsinventory)
* **--basevardir** : set OCS Inventory NG Unix Unified agent variables directory (e.g: /var/lib/ocsinventory-agent)
* **--configdir** : set OCS Inventory NG Unix Unified configuration directory (e.g: /etc/ocsinventory-agent)
* **--user** : set username for OCS Inventory server Apache authentication (if needed)
* **--password** : set password for OCS Inventory NG server Apache authentication (if needed)
* **--realm**: set realm name for OCS Inventory NG server Apache authentication (if needed)
* **--crontab** : set a crontab while installing OCS Inventory NG Unix Unified agent
* **--get-old-linux-agent-config** : retrieve old OCS Inventory NG Linux agent configuration (if needed)
* **--remove-old-linux-agent** : remove old OCS Inventory NG Linux agent from system (if needed)
* **--debug** : activate debug mode configuration option while installing OCS Inventory NG Unix Unified agent
* **--logfile** : set OCS Inventory NG Unix Unified agent log file path (if needed)
* **--nossl** : disable SSL CA verification configuration option while installing OCS Inventory NG Unix Unified agent (not recommended)
* **--ca** : set OCS Inventory NG Unix Unified agent CA certificate chain file path
* **--download** : activate package deployment feature while installing OCS Inventory NG Unix Unified agent
* **--snmp** : activate SNMP scans feature while installing OCS Inventory NG Unix Unified agent
* **--now** : launch OCS Inventory NG Unix Unified agent after installation
* **-h** or **--help** : display help menu

For example, if you want to install OCS Unix Unified agent in non-interactive mode and set server address,
create a crontab, set a custom tag, enable debug mode, set a log file path and activate download feature,
you have to run this command:

   $ sudo env PERL_AUTOINSTALL=1 perl Makefile.PL && make && make install && perl postinstl.pl --nowizard --server=http://yourserver/ocsinventory --crontab

# Agent’s command line switches

If you encounter errors, agent's produce a log file in directory “/var/log/ocsinventory-agent”.

However, agent also supports some command line switches. You can use them while launching the
agent manually using “ocsinventory-agent” command:

Agent’s command line switch | Meaning
----------- | ------------
**--local** | Runs the agent in local mode, without any connection to communication server. You will be prompted for a target directory where agent will put inventory results in XML compressed file with “.ocs” extension.
**--xml** | Agent will create a non-compressed XML file with “.ocs” extension, containing inventory results. You will be prompted for a target directory where agent will put the file. If not used in conjunction with -local, agent tries to connect to communication server.
**--tag=”my tag value”** | Set agent setting TAG value to “my TAG value”.
**--force** | Force agent to always send inventory results, independently of the FREQUENCY parameter.
**--debug** | Force agent to produce a more detailed log file, showing XML exchange with communication server.
**--nosoftware** | Do not search for installed software.
**--info** | Show a detailed output of agent runs.
**--proxy=”url_proxy:[port]”** | Specify the url of a proxy.
**--lazy** | Do not contact the server more than one time during the PROLOG_FREQ and do an exit if there is nothing to do.
**--daemon** | Launch ocsinventory-agent in background. Proc::Daemon is needed.
**--basevardir=”PATH”** | Used to specify the place where the agent should store its files.
**--logfile=”PATH”** | Used to record log message in FILE and turn off STDERR.
**--user=”PASSWORD”** | Used to specify a user for server authentification.
**--password=”PASSWORD”** | Used to specify a password for server authentification.

For more informations, use

    $ man ocsinventory-agent

# Compatibility

## FreeBSD

**OS Version** | **Compatibility** | **Works with agent** | **Comments**
--------|--------|--------|--------
5.x | YES | Unix Unified Agent 1.01 | 32 & 64 bits
6.x | YES | Unix Unified Agent 1.01 | 32 & 64 bits
7.x | YES | Unix Unified Agent 1.01 | 32 & 64 bits
8.x | to be tested | |
9.x | to be tested | |
10.x | to be tested | |
11.x | YES | Unix Unified Agent 2.4 | 32 & 64 bits

## OpenBSD

**OS Version** | **Compatibility** | **Works with agent** | **Comments**
--------|--------|--------|--------
4.5 | YES | Unix Unified Agent 1.01 | 32 & 64 bits
4.6 | YES | Unix Unified Agent 1.01 | 32 & 64 bits
5.1 | YES | Unix Unified Agent 2.4 | 32 & 64 bits
6.3 | YES | Unix Unified Agent 2.4 | 32 & 64 bits

### **Installation Procedure**

    export PKG_PATH="http://ftp.arcane-networks.com/pub/OpenBSD/4.5/packages/i386/"
    export OCS_VERSION="1.1.2"
    mkdir /var/lib
    pkg_add nmap
    pkg_add dmidecode
    pkg_add pciutils
    pkg_add p5-libwww
    pkg_add p5-XML-Simple
    pkg_add p5-Net-IP
    pkg_add p5-Proc-Daemon
    cd
    mkdir ocs
    wget http://launchpad.net/ocsinventory-unix-agent/1.1.x/ocsinventory-unix-agent-$OCS_VERSION/+download/Ocsinventory-Agent-$OCS_VERSION.tar.gz
    tar xvzf Ocsinventory-Agent-$OCS_VERSION.tar.gz
    cd Ocsinventory-Agent-$OCS_VERSION
    perl Makefile.PL
    make
    make install

## AIX

**OS Version** | **Compatibility** | **Works with agent** | **Comments**
--------|--------|--------|--------
4.x | seems to work| Unix Unified Agent 1.1.2 |
5.x | YES | Unix Unified Agent 2.4 |
6 | YES | Unix Unified Agent 2.4 |
7 | YES | Unix Unified Agent 2.4 |

## Solaris

**OS Version** | **Compatibility** | **Works with agent** | **Comments**
--------|--------|--------|--------
>9 | YES | Unix Unified Agent 2.4 |
