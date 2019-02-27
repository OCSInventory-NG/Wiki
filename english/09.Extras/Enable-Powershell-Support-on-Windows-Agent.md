# Enable powershell support on windows agent

## Introduction

Since OCS Inventory Windows Agent 2.4.0.0, the agent can now execute powershell script in the plugin directory.

However, some manipulation are required to enable powershell script.
Windows' default execution policy is really restrictive and will prevent the OCS Agent to properly create a powershell process

## List execution policies by scope

In the first place, you may want to see what policies are active on your computer. To list the policies you need to open a powershell window and type:

```Get-ExecutionPolicy -List```

The output will look like this : 

```
Scope          ExecutionPolicy
-----          ---------------
MachinePolicy  Undefined
UserPolicy     Undefined
Process        Undefined
CurrentUser    AllSigned
LocalMachine   RemoteSigned
```

If you want more informations on ExecutionPolicies, please see [Windows' MSDN link on execution policies](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_execution_policies?view=powershell-6)

## Set execution policies by scope

To make the OCS Agent work, you need to set the ```Process``` and ```LocalMachine``` to ```Unrestricted```

The command is the following : 

```Set-ExecutionPolicy -Scope {MyScope} -ExecutionPolicy {Policy Level}```

In our case:
* ```Set-ExecutionPolicy -Scope LocalMachine -ExecutionPolicy Unrestricted```
* ```Set-ExecutionPolicy -Scope Process -ExecutionPolicy Unrestricted```

OCS Agent will now be able to execute powershell scripts for plugins.

For more informations, see : [Set-ExecutionPolicy Documentation](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.security/set-executionpolicy?view=powershell-6)

## Lift execution policy restrictions using GPO

If you would like to contribute to this section, our Github repository below is directly linked to our documentation.

See [our GitHub repository](https://github.com/OCSInventory-NG/Wiki)

