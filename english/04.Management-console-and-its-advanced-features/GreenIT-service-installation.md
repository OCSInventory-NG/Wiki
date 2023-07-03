# OCS Inventory Service - Green IT

## Introduction

This documentation provides instructions for properly installing and configuring the GreenIT service.

## Description

The GreenIT service is designed to gather power consumption information and is currently supported on Windows.

> _**IMPORTANT NOTE: This service is required when using the GreenIT Plugin, available <a href="https://github.com/PluginsOCSInventory-NG/greenit">here</a>**_

## Prerequisites

- Environment: Windows 10

> _**IMPORTANT NOTE: The service can be installed on a virtual machine, but no consumption data will be retrieved.**_

## Installation

To install the service on your agents :

- Download the service executable from <a href="https://github.com/OCSInventory-NG/greenit_service/releases">here</a> and start it.

- Access the configuration page :

<p align="center">
  <img src="../../img/service/GreenIT_Service_configuration.png" alt="Config Page"/>
</p>

- Configuration :

  - The "Period between collecting information" is the time in seconds that determines how often the service collects information.

    > _Example: If you set it to 5, it will collect information every 5 seconds._

  - The "Period between upload" is the time in minutes that determines how often the service writes the collected information to the data file (C:\ProgramData\GreenIT\data.json)._

    > _Example: If you set it to 5, it will write information to the data file every 5 minutes.
    > **If you set it to 0, it will write information to the data file with each data collection.**_

    > _NOTE : "C:\\ProgramData" is a hidden folder._

  - The "Period between saves" is the time in hours that determines how often the service creates a ".bak" backup file next to the data file to prevent data loss.

    > _Example: If you set it to 5, it will create/update the backup file every 5 hours._

- Click on install and don't forget to check "Run GreenIT Service 1.0" at the end.

> _IMPORTANT NOTE : It is important to check "Run GreenIT Service 1.0" to install the software as a Windows service !_

If you haven't checked the box, you can install it manually after the install of the software :
  - Open a terminal and navigate to the software installation folder.
  - Run the command `.\GreenIT.exe install` and wait until the service is installed.

And yes, that's all :-)
