# Deployment template

## Summary

* [How does it works ?](#how-does-it-works-?)
* [Create your template step by step](#create-your-template-step-by-step)
    * [Declare interaction in operating systems](#declare-interaction-in-operating-systems)
    * [Create interaction file](#create-interaction-file)
    * [Create options file](#create-options-file)
* [Template example](#template-example)
    * [Interaction template](#interaction-template)
    * [Options template](#options-template)

## How does it works ?

Since OCS Inventory 2.9, the deployment feature has been reworked. The new deployment options works by XML templates. 

## Create your template step by step

### Declare interaction in operating systems

The first step is to declare your interaction in the operating systems XML file. Edit the file `ocsreports > config > teledeploy > operatingsystems > operatingsystems.xml` and add your interaction line.

```
<interaction order="{display_number}">{interaction_name}</interaction>
```

Example to add interaction to Others category of Windows system :

```
<category id="otherswindows">
    <interaction order="1">store</interaction>
    <interaction order="2">custompackage</interaction>
    <interaction order="3">example</interaction> <-- Line to declare your custom interaction -->
</category>
```

Save and close.

### Create interaction file

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

Explanations :

* id : name of the interaction that you declared in operating systems file.
* name : translation number of your interaction title that will be displayed in the package build page.
* refos : name of the OS (all / windows / linux / macos).
* imgref : path to the icon that will be displayed in the package build page.
* linkedoptions : name of the options parameters file.

Save and close.

### Create options file

Last step, create the options file in `ocsreports > config > teledeploy > options` folder. In this case, the options name file is `exampleopt.xml`. Edit the options file and first, add the base XML code :

```
<?xml version="1.0" encoding="UTF-8"?>
<optionsdefinition>
    <id>exampleopt</id>
    <name>9312</name>
    <refint>example</refint>
    <replace>true</replace>

    <!-- Form options-->
    <formoption>
    </formoption>

    <!-- Package builder -->
    <packagebuilder>
    </packagebuilder>

    <!-- Package definition -->
    <packagedefinition>
    </packagedefinition>
</optionsdefinition>
```

Explanations :

* id : name of the linked options that you delared in interaction file.
* name : translation number of your option title that will be displayed in the package build page.
* refint : name of the interaction.
* replace : true / false, set true if there are dynamic values on your XML.
* formoption : form fields definition ([More infos](#form-options)).
* packagebuilder : replace code field and file field value ([More infos](#package-builder)).
* packagedefinition : default deployment values ([More infos](#package-definition)).

#### **Form options**

A form field is generate by a formblock XML tag with multiple parameters. 

| XML Tag            | Definition                                                    | Optionnal |
|---                 |---                                                            | ---       |
| id                 | Field identifier                                              | NO        |
| label              | Correspond to the translation number of the label field       | NO        |
| mandatory          | Is required ? (true / false)                                  | NO        |
| type               | Field type (code / text / textarea / select / hidden / file)  | NO        |
| disabled           | Is disabled ? (true / false)                                  | NO        |
| defaultvalue       | Default field value                                           | YES       |
| description        | Correspond to the translation number of the description field | YES       |
| specialcharallowed | Are special chars allowed ? (true / false)                    | NO        |
| javascript         | Adding javascript function to the field                       | YES       |

Simple formblock example :

```
<formblock>
    <id>example</id>
    <label>12000</label>
    <mandatory>false</mandatory>
    <type>text</type>
    <disabled>false</disabled>
    <defaultvalue>Example value</defaultvalue>
    <description>12001</description>
    <specialcharallowed>false</specialcharallowed>
    <javascript>onClick="javascriptFunctionExample(this);"</javascript>
</formblock>
```
![Field example](../../img/server/deployment/teledeploy_developpers_example_field.png)

Select formblock example :

```
<formblock>
    <id>SELECT</id>
    <label>12000</label>
    <mandatory>true</mandatory>
    <type>select</type>
    <disabled>false</disabled>
    <options>
        <option>
            <id>0</id>
            <name>12001</name>
        </option>
        <option>
            <id>1</id>
            <name>12002</name>
        </option>
    </options>
    <specialcharallowed>false</specialcharallowed>
</formblock>
```

| XML Tag | Definition                           | Optionnal |
|---      |---                                   | ---       |
| options | Parent TAG to declare select options | NO        |
| option  | Child TAG to declare select options  | NO        |
| id      | Option identifier                    | NO        |
| name    | Translation number of the option     | NO        |

File form block example :

```
<formblock>
    <id>additionalfiles</id> <-- Don't change the id -->
    <label>12004</label>
    <mandatory>false</mandatory>
    <type>file</type>
    <disabled>false</disabled>
    <specialcharallowed>false</specialcharallowed>
</formblock>
```

`Notice : The following formblocks are required and should not be changed except for the default value of FORMTYPE.`

```
<formblock>
    <id>FORMTYPE</id>
    <label/>
    <mandatory/>
    <type>hidden</type>
    <disabled>false</disabled>
    <defaultvalue>exampleopt</defaultvalue> <-- Replace exampleopt by your options file name -->
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
```

#### **Package builder**

```
<codeasfile>
    <targetid>examplescript</targetid>
    <filename>examplescript.ps1</filename>
</codeasfile>
<filesinarchive>
    <targetid>additionalfiles</targetid>
    <replacename>examplecript.ps1</replacename>
</filesinarchive>
```

#### **Package definition**

```
<PRI>5</PRI>
<ACT>EXECUTE</ACT>
<PROTO>HTTP</PROTO>
<COMMAND>examplescript.ps1</COMMAND> 
<NOTIFY_USER>0</NOTIFY_USER>
<NOTIFY_TEXT/>
<NOTIFY_COUNTDOWN/>
<NOTIFY_CAN_ABORT>0</NOTIFY_CAN_ABORT>
<NOTIFY_CAN_DELAY>0</NOTIFY_CAN_DELAY>
<NEED_DONE_ACTION>0</NEED_DONE_ACTION>
<NEED_DONE_ACTION_TEXT/>
```

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

### Options template

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
        <formblock>
            <id>SelectField</id>
            <label>448</label>
            <mandatory>true</mandatory>
            <type>select</type>
            <disabled>false</disabled>
            <defaultvalue/>
            <options>
                <option>
                    <id>0</id>
                    <name>454</name>
                </option>
                <option>
                    <id>1</id>
                    <name>455</name>
                </option>
            </options>
            <specialcharallowed>false</specialcharallowed>
            <javascript>onchange="myFunction()"</javascript>
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