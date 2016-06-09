# OCS MacOSX agent 2.0 documentation

## Installing OCS MacOSX agent 2.0

**`Note: OCS MacOSX agent 2.0 agent is full compatible under MacOSX 10.6 Snow Leopard and more.
Under MacOSX 10.4 Tiger and MacOSX 10.5 Leopard, SSL layer and packages deployment feature won't work
but inventory using HTTP connection will work. If you want to run OCS MacOSX agent under older MacOSX systems,
you have to use the old OCS MacOSX 1.1 beta1 agent.`**

Download OCS MacOSX agent from OCS Inventory NG website download page(
[http://www.ocsinventory-ng.org/en/#home-en](http://www.ocsinventory-ng.org/en/#home-en)
), unzip the file and double click "Ocsinventory_Agent_MacOSX.pkg".

![Mac OSX Icon](../img/EN_macosx_agent_pkg_icon.png)

Click "Next".

![Ocs agent installation on Mac OSX](../img/EN_macosx_agent_install_introduction.png)

Validate license agreement by clicking "Continue" and "Agree".

![Accept licence](../img/EN_macosx_agent_install_licence.png)

If you already made an OCS MacOSX agent installation, you may be asked if you want to launch
OCS MacOSX configuration. Click on "Yes" to launch configuration panel or "No" to skip this step.

**`Warning: message=If you click on "Yes", /etc/ocsinventory-agent/ocsinventory-agent.cfg
file content will be erased !`**

![Ocs agent already exist](../img/EN_macosx_agent_install_configuration_warn.png)

Set your OCS MacOSX agent configuration by choosing serveral options:
* Http or https for MacOSX agent communication to OCS server ("http" by default).
* OCS server server name ("ocsinventory-ng" by default).
* MacOSX agent TAG value (optional).
* Log file path for OCS MacOSX agent ("/var/log/ocsng.log" by default).
* Activate or Unactivate OCS MacOSX agent debug mode for logs (activated by default).
* Activate or Unactivate OCS MacOSX agent packages download feature (activated by default).
* Certificate file path (mandatory if you activate OCS MacOSX agent packages download feature). Certificate file must be named as "cacert.pem".

Click on "Continue" to validate configuration.

![Ocs agent configuration](../img/EN_macosx_agent_install_configuration.png)

If you activate OCS MacOSX agent packages download feature without specifying a certificate file path, you may have this warning.

**`Warning: message=If you activate OCS MacOSX agent packages download feature without specifying a
certificate file path, OCS MacOSX agent packages download feature won't work !!!`**

![No folder specify](../img/EN_macosx_agent_configuration_download_warn.png)

Set how OCS MacOSX agent will be launched by MacOSX Launchd daemon by choosing several options:
* Periodicity for OCS MacOSX agent to be launched by Launchd daemon (5 hours by default).
* Activate or Unactivate OCS MacOSX agent launch at Launchd daemon start (activated by default).
* Activate or Unactivate OCS MacOSX agent launch, using Launchd daemon, after installation (unactivated by default)

![Daemon Option](../img/EN_macosx_agent_install_daemon_options.png)

Select hard drive where you want to install OCS MacOSX agent and click "Continue".

![Select Destination](../img/EN_macosx_agent_install_destination.png)

Click "Install".

![Installation Type](../img/EN_macosx_agent_install_type.png)

Click "Close" to end installation.

![End of the installation](../img/EN_macosx_agent_install_end.png)

## Launch MacOSX agent

To launch MacOSX agent, open a new Finder window, go to _Applications_ and then double click on _OCSNG.app_.

![Agent OCS's icon](../img/EN_macosx_agent_finder_icon.png)

A new window appears and click _Yes_ to launch agent or _No_ to cancel.

![Launch window](../img/EN_macosx_agent_launch_window.png)

During agent launch you will see an icon in your Dock and this will disappear when agent launch will be ended.