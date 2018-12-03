# OCS Agent Unix Packager

## Introduction

The aim of this script is to create a all in one package to be deployed on every Linux machine.

The package embeds compiled Perl for the related OS, OCS Agent perl package, and all required perl mudules.

Thus toik can be found on our download section on our website.

## Configuration files

* packageOCSAgent.config: packager configuration
    * PROXY_HOST if you have direct Internet connection
    * OCS_INSTALL_DIR: OCS Agent installation directory
    * PERL_VERSION: Perl version you want to compile and embed in OCS package
    * PERL_DL_LINK: Perl download link
    * OCSAGENT_DL_LINK: OCS Agent download link
    * NMAP_DL_LINK: Nmap download link
    * OCS_AGENT_CRONTAB: ```[0-1]``` Create script to add crontab on system
    * OCS_AGENT_CRONTAB_HOUR: How many hour between each crontab call
    * OCS_AGENT_LAZY: ```[0-1]``` Activate lazy mode on Agent
    * OCS_AGENT_TAG: Set default tag
    * OCS_SERVER_URL: Set server URL
    * OCS_SSL_ENABLED: ```[0-1]``` Enable SSL check
    * OCS_SSL_CERTIFICATE_FULL_PATH: Path to the certificate
    * OCS_LOG_FILE: ```[0-1]``` Enable file logging feature
    * OCS_LOG_FILE_PATH: Set the log path file
* PerlModulesDownloadList.txt: download URL for all Perl modules dependencies
* packageOCSAgent.sh: packager script

## Usage

As root user
```shell
#./packageOCSAgent.sh
```

Output is a tar/gz archive: ocsinventory-agent_*LinuxDistribution*_*MajorVersion*.tar.gz

## Todo

1. Bypass current limitations

## Current Limitation

1. nmap command line path is not referenced in Perl module, thus, IP Discovery function does not work
