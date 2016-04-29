# OCS MacOSX agent 2.0 documentation

## Installing OCS MacOSX agent 2.0

**`Note`**`: OCS MacOSX agent 2.0 agent is full compatible under MacOSX 10.6 Snow Leopard and more.
Under MacOSX 10.4 Tiger and MacOSX 10.5 Leopard, SSL layer and packages deployment feature won't work
but inventory using HTTP connection will work. If you want to run OCS MacOSX agent under older MacOSX systems,
you have to use the old OCS MacOSX 1.1 beta1 agent.`

Download OCS MacOSX agent from OCS Inventory NG website download page(
[http://www.ocsinventory-ng.org/en/#home-en](http://www.ocsinventory-ng.org/en/#home-en)
), unzip the file and double click "Ocsinventory_Agent_MacOSX.pkg".

Click "Next".

![Mac OSX Icon](En_macosx_agent_pkg_icon.png)

Validate license agreement by clicking "Continue" and "Agree".

![]()

If you already made an OCS MacOSX agent installation, you may be asked if you want to launch
OCS MacOSX configuration. Click on "Yes" to launch configuration panel or "No" to skip this step.

**`Warning`*`: message=If you click on "Yes", /etc/ocsinventory-agent/ocsinventory-agent.cfg
file content will be erased !`

![]()

Set your OCS MacOSX agent configuration by choosing serveral options:
* Http or https for MacOSX agent communication to OCS server ("http" by default).
* OCS server server name ("ocsinventory-ng" by default).
* MacOSX agent TAG value (optional).
* Log file path for OCS MacOSX agent ("/var/log/ocsng.log" by default).
* Activate or Unactivate OCS MacOSX agent debug mode for logs (activated by default).
* Activate or Unactivate OCS MacOSX agent packages download feature (activated by default).
* Certificate file path (mandatory if you activate OCS MacOSX agent packages download feature). Certificate file must be named as "cacert.pem".

Click on "Continue" to validate configuration.

![]()

If you activate OCS MacOSX agent packages download feature without specifying a certificate file path, you may have this warning.

**`Warning`**`: message=If you activate OCS MacOSX agent packages download feature without specifying a
certificate file path, OCS MacOSX agent packages download feature won't work !!!`

![]()

Set how OCS MacOSX agent will be launched by MacOSX Launchd daemon by choosing several options:
* Periodicity for OCS MacOSX agent to be launched by Launchd daemon (5 hours by default).
* Activate or Unactivate OCS MacOSX agent launch at Launchd daemon start (activated by default).
* Activate or Unactivate OCS MacOSX agent launch, using Launchd daemon, after installation (unactivated by default)

![]()
Select hard drive where you want to install OCS MacOSX agent and click "Continue".

![]()

Click "Install".

![]()

Click "Close" to end installation.

![]()

## Launch MacOSX agent

To launch MacOSX agent, open a new Finder window, go to ''Applications'' and then double click on ''OCSNG.app''.

![]()

A new window appears and click ''Yes'' to launch agent or ''No'' to cancel.

![]()

During agent launch you will see an icon in your Dock and this will disappear when agent launch will be ended.