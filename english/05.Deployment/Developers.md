# Deployment template

Since OCS Inventory 2.9, the deployment feature has been reworked. The new deployment options works by XML templates. 

## How does it works ?

## Create your template step by step



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