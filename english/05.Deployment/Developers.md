# Deployment template

Since OCS Inventory 2.9, the deployment feature has been reworked. The new deployment options works by XML templates. 

## How does it works ?

## Create your template step by step

The first step is to declare your interaction to the operating system XML file. Edit the file `ocsreports > config > teledeploy > operatingsystems > operatingsystems.xml` and add your interaction line.

Example to add interaction to Others category of Windows system :

```
<category id="otherswindows">
    <interaction order="1">store</interaction>
    <interaction order="2">custompackage</interaction>
    <interaction order="3">example</interaction> <-- Line to declare your custom interaction -->
</category>
```

Save and close.

Next, create your interaction file in `ocsreports > config > teledeploy > interactions` folder. In our example the name file is `example.xml`. Edit this file and add this lines :

```
<?xml version="1.0" encoding="UTF-8"?>
<interactiondefinition>
    <id>example</id>
    <name>9323</name>
    <refos>all</refos>
    <imgref>image/example.png</imgref>
    <linkedoptions>exampletopt</linkedoptions>
</interactiondefinition>
```

Explainations :

* id : name of the interaction that you declared in operating systems file.
* name : translation number of your interaction.
* refos : name of the OS (all / windows / linux / macos).
* imgref : path to the icon that will be displayed in the package build page.
* linkedoptions : name of the options parameters file.

## Template example

You can find an example of interaction and option template below.

### Interaction template

```
<?xml version="1.0" encoding="UTF-8"?>
<interactiondefinition>
    <id>example</id>
    <name>9312</name>
    <refos>windows</refos>
    <imgref>image/example.png</imgref>
    <linkedoptions>exampleopt</linkedoptions>
</interactiondefinition>
```

### Option template

```
<?xml version="1.0" encoding="UTF-8"?>
<optionsdefinition>
    <id>exampleopt</id>
    <name>9315</name>
    <refint>example</refint>
    <replace>true</replace>

    <formoption>
        <formblock>
            <id>FORMTYPE</id>
            <label/>
            <mandatory/>
            <type>hidden</type>
            <disabled>false</disabled>
            <defaultvalue>exampleopt</defaultvalue>
            <specialcharallowed>false</specialcharallowed>
        </formblock>
        <formblock>
            <id>NAME</id>
            <label>1037</label>
            <mandatory>required</mandatory>
            <type>text</type>
            <disabled>false</disabled>
            <defaultvalue/>
            <specialcharallowed>false</specialcharallowed>
            <javascript>onkeyup="verifPackageName(this);"</javascript>
        </formblock>
        <formblock>
            <id>DESCRIPTION</id>
            <label>53</label>
            <mandatory>required</mandatory>
            <type>text</type>
            <disabled>false</disabled>
            <defaultvalue/>
            <specialcharallowed>true</specialcharallowed>
        </formblock>
    </formoption>

    <packagebuilder>
        <filesinarchive/>
    </packagebuilder>

    <packagedefinition>
        <PRI>5</PRI>
        <ACT>EXECUTE</ACT>
        <PROTO>HTTP</PROTO>
        <COMMAND>example.exe</COMMAND> 
        <NOTIFY_USER>0</NOTIFY_USER>
        <NOTIFY_TEXT/>
        <NOTIFY_COUNTDOWN/>
        <NOTIFY_CAN_ABORT>0</NOTIFY_CAN_ABORT>
        <NOTIFY_CAN_DELAY>0</NOTIFY_CAN_DELAY>
        <NEED_DONE_ACTION>0</NEED_DONE_ACTION>
        <NEED_DONE_ACTION_TEXT/>
    </packagedefinition>
</optionsdefinition>
```