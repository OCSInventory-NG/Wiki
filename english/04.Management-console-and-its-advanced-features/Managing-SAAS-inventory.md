# Managing SAAS Inventory

SAAS (Software as a Service) Inventory feature has been added in release 2.7. This feature is completed by an OCS Windows Agent plugin named : Saas.ps1. 

`Note : Saas plugin is automatically installed on Windows Agent version 2.6 and later.`

## Enable SAAS Inventory 

To activate SAAS Inventory, go to ```Configuration > General configuration```, and click on _Inventory_ entry in left navigation pane. At the bottom page click on _ON_ for **INVENTORY_SAAS_ENABLED**.

![saas enabled](../../img/server/reports/SAAS_activate.png)

## Register SAAS to be inventoried

Go to ```Inventory > SAAS Inventory``` and click on _Add SAAS_ in left menu.

![saas menu](../../img/server/reports/SAAS_menu.png)

In order, enter the name that corresponds to the SAAS you want to register and its associated DNS. After, click on _OK_.   

![saas register](../../img/server/reports/SAAS_add.png)


## Make a Software as a Service Inventory

When SAAS Inventory has been enabled, Saas.ps1 on the Windows Agent plugin's folder and have been created an entry on SAAS Inventory menu, execute a Windows machine inventory. Go to ```Inventory > SAAS Inventory``` and click on _SAAS list_ in left menu.

Here the different informations display :

+ Name : SAAS name you have been created before.
+ Entry : associated DNS.
+ Data : IP address associated to the DNS.
+ Count : Number of machine with this entry.

![saas list](../../img/server/reports/SAAS_list_all.png)

When you click on the count, you will be redirected to a search reporting based on the Entry.

Also, you can see SAAS inventoried per machine. Go to computer details page and click on _Software_ in left navigation pane. Below software table, the SAAS Inventory table list all Software as a Service entry for this machine.

![saas computer details](../../img/server/reports/SAAS_computer_detail.png)