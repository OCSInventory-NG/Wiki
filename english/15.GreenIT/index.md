# OCS Inventory NG - GreenIT Documentation

## Introduction

### What are Green IT practices ?
Green IT aims to minimize the negative effects of IT operations on the environment by designing, manufacturing, operating and disposing of servers, PCs and other computer-related products in an environmentally friendly manner. The motives behind green IT practices include reducing the use of hazardous materials, maximizing energy efficiency during a product's lifetime, and promoting the biodegradability of unused and outdated products.

### What does the GreenIT plugin do ?
The GreenIT plugin is designed to collect electrical consumption data from IT infrastructure and to estimate its cost. By providing this information, an organization can assess the energy footprint of its IT assets and make informed decisions to potentially reduce its IT-related expenses.

## Server side
- [Installation](01.Server/Server-installation-and-configuration.md#installation)
    - [Prerequisites](01.Server/Server-installation-and-configuration.md#prerequisites)
    - [Plugin](01.Server/Server-installation-and-configuration.md#plugin)
    - [Datafilter](01.Server/Server-installation-and-configuration.md#datafilter)
- [Configuration](01.Server/Server-installation-and-configuration.md#configuration)
    - [Allow access to pages](01.Server/Server-installation-and-configuration.md#allow-access-to-greenit-pages)
    - [Plugin Configuration](01.Server/Server-installation-and-configuration.md#plugin-configuration)
    - [Crontab](01.Server/Server-installation-and-configuration.md#crontab)
- [Dashboard](01.Server/Dashboard.md)
    - [Menu](01.Server/Dashboard.md)
    - [Global stats view](01.Server/Dashboard.md#the-global-statistics)
    - [Filtered stats view](01.Server/Dashboard.md#filtered-statistics)
    - [Operating system stats view](01.Server/Dashboard.md#operating-system-statistics)
    - [Computer type stats view](01.Server/Dashboard.md#computer-type-statistics)
    - [Manufacturer stats view](01.Server/Dashboard.md#manufacturer-statistics)

## Agent side
- [Installation](02.Agent/Agent-installation-and-configuration.md#installation)
    - [Plugin](02.Agent/Agent-installation-and-configuration.md#plugin)
    - [Service](02.Agent/Agent-installation-and-configuration.md#service)
- [Configuration](02.Agent/Agent-installation-and-configuration.md#configuration)
    - [Service configuration](02.Agent/Agent-installation-and-configuration.md#service-configuration)
- [Service](02.Agent/Agent-service-for-developpers.md)
    - [Software version](02.Agent/Agent-service-for-developpers.md#software-version)
    - [Structure](02.Agent/Agent-service-for-developpers.md#structure)
    - [Packages](02.Agent/Agent-service-for-developpers.md#packages)
    - [Main content](02.Agent/Agent-service-for-developpers.md#main-content)
    - [GreenITMain.cs](02.Agent/Agent-service-for-developpers.md#greenitmaincs)
    - [GreenITService.cs](02.Agent/Agent-service-for-developpers.md#greenitservicecs)
    - [Variables definition](02.Agent/Agent-service-for-developpers.md#variables-definition)
    - [Engine organization](02.Agent/Agent-service-for-developpers.md#engine-organization)
    - [OpenHardwareMonitor engine](02.Agent/Agent-service-for-developpers.md#openhardwaremonitor-engine)