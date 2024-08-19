## Introduction

OCS Inventory has now his own repository for Debian based and Redhat based distributions.
You will find below how to install OCS Inventory agent unix using our repository

## Installing UNIX Agent with APT

**On Debian-based distributions** you can install the agent with APT

You need to add our repository using the following command

    $ curl -sS https://deb.ocsinventory-ng.org/pubkey.gpg | sudo apt-key add -
    $ echo "deb http://deb.ocsinventory-ng.org/debian/ <distribution_codename> main" | sudo tee /etc/apt/sources.list.d/ocsinventory.list
    $ sudo apt update 

You will have to replace <distribution_codename> by one of the following term depending on the installation context : 

* bullseye | stable
* buster | oldstable 
* stretch | oldoldstable
* sid | unstable

Then install the agent using : 

    $ sudo apt install ocsinventory-agent

**On Ubuntu-based distributions** you can install the agent with APT

You need to add our repository using the following commands :

    $ curl -sS https://deb.ocsinventory-ng.org/pubkey.gpg | sudo apt-key add -

**Note** : apt-key is now deprecated as of Ubuntu 22.04, and will issue a warning. We recommend using the below gpg alternative :


    $ curl -fsSL https://deb.ocsinventory-ng.org/pubkey.gpg | sudo gpg --dearmor -o /etc/apt/trusted.gpg.d/ocs-archive-keyring.gpg

Then : 
  
    $ echo "deb http://deb.ocsinventory-ng.org/ubuntu/ <distribution_codename> main" | sudo tee /etc/apt/sources.list.d/ocsinventory.list
    $ sudo apt update

You will have to replace <distribution_codename> by one of the following term depending on the installation context : 

* noble | stable 
* jammy | oldstable
* focal | backport

Then install the agent using : 

    $ sudo apt install ocsinventory-agent

## Installing UNIX Agent with RPM

**On Redhat/Centos 7** you can install the unix agent with RPM

You need to have "wget" to download the repo of EPEL and OCS

    $ sudo wget https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
    $ sudo wget https://rpm.ocsinventory-ng.org/ocsinventory-release-latest.el7.ocs.noarch.rpm

You can install the repo with "yum"

    $ sudo  yum install ocsinventory-release-latest.el7.ocs.noarch.rpm epel-release-latest-7.noarch.rpm

To install the unix agent and requirement use this command:

    $ sudo yum install ocsinventory-agent

**On Redhat/Centos 8** you can install the unix agent with RPM

You need to have "wget" to download the repo of EPEL and OCS

    $ sudo wget https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm
    $ sudo wget https://rpm.ocsinventory-ng.org/ocsinventory-release-latest.el8.ocs.noarch.rpm

You can install the repo with "dnf"

    $ sudo  dnf install ocsinventory-release-latest.el8.ocs.noarch.rpm epel-release-latest-8.noarch.rpm

To install the unix agent and requirement use this command:

    $ sudo dnf --enablerepo=PowerTools --enablerepo=epel-playground install ocsinventory-agent

**On Oracle Linux 8 and superior**, you'll have to enable the CodeReady Builder repository instead of Powertools to install the agent, e.g. for Oracle Linux 8 :

    $ sudo dnf --enablerepo=ol8_codeready_builder install ocsinventory-agent


**On Rocky/Alma and RHEL 9** you can install the unix agent with RPM

You need to have "wget" to download the repo of EPEL and OCS

    $ sudo wget https://dl.fedoraproject.org/pub/epel/epel-release-latest-9.noarch.rpm
    $ sudo wget https://rpm.ocsinventory-ng.org/ocsinventory-release-latest.el9.ocs.noarch.rpm

You can install the repo with "dnf"

    $ sudo  dnf install ocsinventory-release-latest.el9.ocs.noarch.rpm epel-release-latest-9.noarch.rpm

To install the unix agent and requirement use this command:

    $ sudo dnf install yum-utils
    $ sudo yum-config-manager --enable crb
    $ sudo dnf install ocsinventory-agent

**On Fedora** you can install the unix agent with RPM

You need to have "wget" to download the OCS's repo

    $ export FEDORA_VERSION=$(awk '{print $3}' /etc/fedora-release)
    $ sudo wget https://rpm.ocsinventory-ng.org/ocsinventory-release-latest.fc$FEDORA_VERSION.ocs.noarch.rpm

You can install the repo with "dnf"

    $ sudo  dnf install ocsinventory-release-latest.fc$FEDORA_VERSION.ocs.noarch.rpm

To install the unix agent and requirement use this command:

    $ sudo dnf install ocsinventory-agent

**`Note : The unix agent gonna be installed with default settings.`**
