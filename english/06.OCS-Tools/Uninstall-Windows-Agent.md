## Uninstall Windows client

### **Create a .bat file for uninstall**

Here is a sample of a batch file which is able to uninstall silently Ocs Agent

    rem File uninstall_agent.cmd
    rem Untested on W9X (command.com) - please run with cmd.exe
    rem To only remove service:
    rem    sc.exe delete "OCS INVENTORY"
    rem    On Windows 2000 use delsrv.exe instead of sc.exe

    %SystemDrive%
    cd "%ProgramFiles%"
    if not exist "OCS Inventory Agent" cd "%ProgramFiles(X86)%"
    if not exist "OCS Inventory Agent" goto end

    cd "OCS Inventory Agent"
    if exist uninst.exe call uninst.exe /S
    del *.* /s /q
    cd ..
    rd "OCS Inventory Agent" /s /q

    :end
    cd \

This batch file uninstalls only the old agent version (40XX), which had service.ini and which cannot
be upgraded by using "/DEPLOY=2.0.3.0" as its version is "bigger" (4.0.5.4 for example)

    %SystemDrive%
    cd "%ProgramFiles%"
    if not exist "OCS Inventory Agent" cd "%ProgramFiles(X86)%"
    if not exist "OCS Inventory Agent" goto end

    cd "OCS Inventory Agent"
    if not exist service.ini goto end
    call uninst.exe /S
    del *.* /s /q
    cd ..
    rd "OCS Inventory Agent" /s /q

    :end
    cd \

### **Create a .bat file to launch uninstall_agent.bat via schedule**

This file will be able to set a scheduled task the next hour after the deployment by the agent
(at least 11 min after, at most 1h10 after)

    #File schedule_uninst.bat
    copy uninstall_agent.bat C:\
    for /f (tokens=1) %%a in ('time /T') do set /A heure=%%a + 1
    at %heure%:10 "C:\uninstall_agent.bat"

### **Create a zip with the two files**

Select the two files and create a uninstall_agent_bat.zip file.

### **Teledeploy it!**

Create a teledeployment package with the launch method and put the command "schedule_uninst.bat" in it. Activate the package.

### **Select the machines**

Select the machine where you want to uninstall the agent and activate the deployement.

NOTE : perhaps you'll need to set a schedule time >= PROLOG_FREQ + 1 to be sure that the package have been deployed.