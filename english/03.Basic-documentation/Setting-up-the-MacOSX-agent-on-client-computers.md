# OCS MacOSX agent 2.X documentation

## Installing OCS MacOSX agent 2.X

**`Note: OCS MacOSX agent 2.3 and newer agent are fully compatible under MacOSX 10.11 El captain and more.
For older macos system you will have to user older versions of our agent. We don't support Mac OS versions that are not maintained by Apple`**

Download OCS MacOSX agent from OCS Inventory NG website download page (
[Download page](https://ocsinventory-ng.org/?page_id=1548&lang=en)
), unzip the file and double click "Ocsinventory_Agent_MacOSX.pkg".

![Mac OSX Icon](../../img/agent/macOS/macosx_agent_pkg_icon.png)

Click "Continue".

![Ocs agent installation on Mac OSX](../../img/agent/macOS/macosx_agent_install_introduction.png)

Validate license agreement by clicking "Continue" and "Agree".

![Accept licence](../../img/agent/macOS/macosx_agent_install_licence.png)

If you already made an OCS MacOSX agent installation, you may be asked if you want to launch
OCS MacOSX configuration. Click on "Yes" to launch configuration panel or "No" to skip this step.

**`Warning: message=If you click on "Yes", /etc/ocsinventory-agent/ocsinventory-agent.cfg
file content will be erased !`**

![Ocs agent already exist](../../img/agent/macOS/macosx_agent_install_configuration_warn.png)

Set your OCS MacOSX agent configuration by choosing several options:
* Http or https for MacOSX agent communication to OCS server ("http" by default).
* OCS server server name ("ocsinventory-ng" by default).
* MacOSX agent TAG value (optional).
* Log file path for OCS MacOSX agent ("/var/log/ocsng.log" by default).
* Activate or Unactivate OCS MacOSX agent debug mode for logs (activated by default).
* Activate or Unactivate OCS MacOSX agent packages download feature (activated by default).
* Activate or Unactivate OCS MacOSX agent SSL certificate feature (activated by default).
* Certificate file path (mandatory if you activate OCS MacOSX agent packages download feature). Certificate file must be named as "cacert.pem".

Click on "Continue" to validate configuration.

![Ocs agent configuration](../../img/agent/macOS/macosx_agent_install_configuration.png)

If you activate OCS MacOSX agent packages download feature and SSL feature without specifying a certificate file path, you may have this warning.

**`Warning: message=If you activate OCS MacOSX agent packages download feature without specifying a
certificate file path, OCS MacOSX agent packages download feature won't work !!!`**

![No folder specify](../../img/agent/macOS/macosx_agent_download_warn.png)

Set how OCS MacOSX agent will be launched by MacOSX Launchd daemon by choosing several options:
* Periodicity for OCS MacOSX agent to be launched by Launchd daemon (5 hours by default).
* Activate or Unactivate OCS MacOSX agent launch at Launchd daemon start (activated by default).
* Activate or Unactivate OCS MacOSX agent launch, using Launchd daemon, after installation (inactivated by default)

![Daemon Option](../../img/agent/macOS/macosx_agent_install_daemon_options.png)

Select hard drive where you want to install OCS MacOSX agent and click "Continue".

![Select Destination](../../img/agent/macOS/macosx_agent_install_destination.png)

Click "Install".

![Installation Type](../../img/agent/macOS/macosx_agent_install_type.png)

Click "Close" to end installation.

![End of the installation](../../img/agent/macOS/macosx_agent_install_end.png)

## Launch MacOSX agent

To launch MacOSX agent, open a new Finder window, go to _Applications_ and then double click on _OCSNG.app_.

![Agent OCS's icon](../../img/agent/macOS/macosx_agent_finder_icon.png)

A new window appears and click _Yes_ to launch agent or _No_ to cancel.

![Launch window](../../img/agent/macOS/macosx_agent_launch_window.png)

During agent launch you will see an icon in your Dock and this will disappear when agent launch will be ended.
