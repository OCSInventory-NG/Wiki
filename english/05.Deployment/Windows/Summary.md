# Windows deployment templates

## Summary

### Install / Uninstall

* [Install MSI application](#install-msi-application)
* [Update Windows Agent](#update-windows-agent)
* [Execute an exe](#execute-an-exe)
* [Uninstall application](#uninstall-application)

### Scripts

* [Powershell script](#powershell-script)
* [Batch script](#batch-script)

### Others

* [Store file or folder](#store-file-or-folder)
* [Custom package](#custom-package)


## Install MSI application

To install MSI application, go to `Deployment > Build > Windows > Install / Uninstall` and click on `Install MSI application`.

![Install MSI application form](../../../img/server/deployment/teledeploy_windows_msi_example.png)

List of MSI installation's form parameters :

* **Package Name** : the package display name
* **Description** : the package description
* **MSI file** : MSI file to be deployed
* **Arguments** : MSI command line arguments (optionnal)

Below, the list of default MSI installation's parameters (not configurable by the user):

* **Priority** : 5
* **Action** : Execute
* **Protocole** : HTTP
* **Command** : msiexec /i application.msi {custom arguments}
* **Notify User** : No
* **Notify can abort** : No
* **Notify can delay** : No
* **Need done action** : No

Next, click on `Validate`. After the package has been created its resume is displayed.

![Install MSI application resume](../../../img/server/deployment/teledeploy_windows_msi_resume.png)

The fragments number and the Activated column value depends on your deployment configuration. For more informations see [Deployment configuration](../Configuration.md).

## Update Windows Agent

`Note : To create update package, you need to create a Windows Agent setup with OCS Packager and all command line parameters inside before. For more informations, read` [Use OCSPackager](../Configuration.md) `documentation.`

To update Windows Agent, go to `Deployment > Build > Windows > Install / Uninstall` and click on `Update Agent`.

![Update agent example](../../../img/server/deployment/teledeploy_windows_update_agent_example.png)

List of Update agent's form parameters :

* **Package Name** : the package display name
* **Description** : the package description
* **Packager file** : OCS Agent setup generate by OCS Packager

Below, the list of default Update agent's parameters (not configurable by the user):

* **Priority** : 5
* **Action** : Execute
* **Protocole** : HTTP
* **Command** : powershell.exe -File scheduledupdateagent.ps1
* **Notify User** : No
* **Notify can abort** : No
* **Notify can delay** : No
* **Need done action** : No

Next, click on `Validate`. After the package has been created its resume is displayed.

![Update agent resume](../../../img/server/deployment/teledeploy_windows_update_agent_resume.png)

The fragments number and the Activated column value depends on your deployment configuration. For more informations see [Deployment configuration](../Configuration.md).

`Note : the Update Agent deployment uninstall the old agent to install the new agent properly.`

## Execute an exe

To execute an exe, go to `Deployment > Build > Windows > Install / Uninstall` and click on `Execute an exe`.

![Execute an exe example](../../../img/server/deployment/teledeploy_windows_exe_example.png)

List of Execute an exe's form parameters :

* **Package Name** : the package display name
* **Description** : the package description
* **EXE file** : executable file
* **Arguments** : command line arguments (optionnal)
* **Warn user** : warn user before deployment (No by default)
* **Text** : warn user message (Not visible if warn user at No)
* **Countdown** : warn user message coutdown (Not visible if warn user at No)

Below, the list of default Execute an exe's parameters (not configurable by the user):

* **Priority** : 5
* **Action** : Launch
* **Protocole** : HTTP
* **Command** : executable.exe {custom arguments}
* **Notify can abort** : No
* **Notify can delay** : No
* **Need done action** : No

Next, click on `Validate`. After the package has been created its resume is displayed.

![Execute an exe resume](../../../img/server/deployment/teledeploy_windows_exe_resume.png)

The fragments number and the Activated column value depends on your deployment configuration. For more informations see [Deployment configuration](../Configuration.md).

## Uninstall application

To uninstall application, go to `Deployment > Build > Windows > Install / Uninstall` and click on `Uninstall application`.

![Uninstall application example](../../../img/server/deployment/teledeploy_windows_uninstall_example.png)

List of Uninstall application's form parameters :

* **Package Name** : the package display name
* **Description** : the package description
* **Application identifier** : application name (to find wmic application name, open a cmd as administrator and run `wmic product get name`)
* **Warn user** : warn user before deployment (No by default)
* **Text** : warn user message (Not visible if warn user at No)
* **Countdown** : warn user message coutdown (Not visible if warn user at No)

Below, the list of default Uninstall application's parameters (not configurable by the user):

* **Priority** : 5
* **Action** : Execute
* **Protocole** : HTTP
* **Command** : wmic product where (name='{application identifier}') call uninstall
* **Notify can abort** : No
* **Notify can delay** : No
* **Need done action** : No

Next, click on `Validate`. After the package has been created its resume is displayed.

![Execute an exe resume](../../../img/server/deployment/teledeploy_windows_uninstall_resume.png)

The fragments number and the Activated column value depends on your deployment configuration. For more informations see [Deployment configuration](../Configuration.md).

`Notice : some application uninstallations might trigger a system reboot.`

