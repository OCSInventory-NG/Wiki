# OCS Inventory Service - Green IT

## Introduction

Here is the documentation to understand how works the GreenIT service.

> _IMPORTANT NOTE : Please, be careful with important notes._

## Software version

We made the project on Visual Studio Community 2022 version 17.5.0 on windows 10 Pro x64 (22H2) distribution.

> _IMPORTANT NOTE : It is important to check ".NET desktop development" and ".NET framework 6.0" at the installation to don't have any problem when you are working on the project !_

## Arborescence

The project arborescence is organized like this:

<p align="center">
  <img src="../../img/service/GreenIT_Service_arborescence.png" alt="Service arborescence"/>
</p>

## Packages

There is three important package to allow the service to work:

- ini-parser

  > _NOTE : To read into ini files_

- LibreHardwareMonitorLibCore

  > _NOTE : To gather consumption data_

- TopShelf

  > _NOTE : To allow the project to run as a service_

## Main content

There is two important files of the project:

- GreenITMain.cs
- GreenITService.cs

### GreenITMain.cs

This file create the windows service and define which settings the service will use :

> _NOTE : Like service name, service description, run method, ..._

<p align="center">
  <img src="../../img/service/GreenIT_Service_mainfile.png" alt="Service main file"/>
</p>

### GreenITService.cs

And this file is to define what the service have to do:

### Initialisation of variables

<p align="center">
  <img src="../../img/service/GreenIT_Service_servicefile1.png" alt="Service service file"/>
</p>

<p align="center">
  <img src="../../img/service/GreenIT_Service_servicefile2.png" alt="Service service file"/>
</p>

The file was organized to be upgrade in the future.

The definition of variable is divided by three parts:

- All engine signals

  > _NOTE : To control each engine._

- All background workers

  > _NOTE : A brackground worker is the object which will compose the engine. It is thanks to this object that we can do tasks one by one controlled by the mutex._

- All other settings used by the workers

  > _NOTE : Like the path of the service installation or the destination data file..._

### Engine organization

The service works with three diferents parts at the moment:

- The service engine

  > _NOTE : Keep an eye on the data folder the data file._

- The backup engine

  > _NOTE : Make a backup of the data file between an interval._

- The OpenHardwareMonitor engine

  > _NOTE : Calculate and format the data._

The file function is organized as a chronology line:

- The start function
- The service engine
- The backup engine
- The OpenHardwareMonitor engine
- The stop function
- Other utilities functions

<p align="center">
  <img src="../../img/service/GreenIT_Service_servicefile3.png" alt="Service service file"/>
</p>

### The OpenHardwareMonitor engine

The OpenHardwareMonitor engine will get the consumption data collected from the file OpenHardwareMonitorModel.cs:

<p align="center">
  <img src="../../img/service/GreenIT_Service_openhardwaremonitormodel.png" alt="Service OpenHardwareMonitorModel file"/>
</p>

And calculate the uptime of the service. The engine will also read and write into the data file as a json format.
