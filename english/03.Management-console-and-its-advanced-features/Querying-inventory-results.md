# Querying inventory results

Point your browser to the URL
[http://administration_server/ocsreports](http://administration_server/ocsreports)
and log into Administration console (default credentials are “admin” as login and “admin” as password).

![OCS admin home](../../img/server/reports/querying_ir_1.png)

You must use All Computers and Inventory menu to run predefined queries.

![Inventory Menu](../../img/server/reports/querying_ir_2.png)

Each of these queries' results can be customized by adding, removing columns, or changing
the number of displayed lines per page. Theses customizations settings are saved per
user between sessions. This means that when you come back to OCS Inventory NG Administration
console, your settings are restored as they were during your last visit.

**`Note: Computers marked with a red bullet at beginning of line are computers whith
specific customization parameters. They may have specific inventory frequency,
ipdiscover status or have package deployment affected.`**

## All computers

This query will allow you to display all inventoried computers. Computers marked with a red bullet at
the beginning of line are those which have specific customized options.

![All computers table](../../img/server/reports/querying_ir_3.png)

Just click on a computer name to display its properties in a new browser window.


![Top banner](../../img/server/reports/querying_ir_4.png)

* Links section – Just click on the appropriate link to display the corresponding information.

![Links section](../../img/server/reports/querying_ir_5.png)

* Special section Administrative data – Use this section to display the device's administrative data.
This page fits with your settings in “admininfo” tab. Use the “update” button to change values.


* Special section Customization – Use this section to customize configuration option for individual computer.
You can also select here package to deploy on a particular computer.

![Special section Customization](../../img/server/reports/querying_ir_7.png)

## TAG / number of PC repartition

This query allows you to display all machines grouped by TAG account info. Click on the computer
count to retrieve the corresponding devices.

For example, if you’ve choosen to set your TAG information to reflect your geographical sites,
this will display the number of computer in each different site.

![TAG Informations](../../img/server/reports/querying_ir_8.png)

## Search with various criteria

This query allows you to search for computers having specific feature.

![Search with various criteria](../../img/server/reports/querying_ir_9.png)

You can add new parameters to the search query by dropping down the combo-box and selecting it in the list.

Default search parameters are:

* BIOS Manufacturer
* BIOS Version
* Computer name
* Customized IpDiscover
* Deployment Package
* Description
* Domain
* Free Disk Space
* Inventory Frequency
* Gateway
* IP Address
* IpDiscover Status
* Last Inventory
* MAC Address
* System Manufacturer
* Memory
* System Model
* Monitor Caption
* Monitor Manufacturer
* Monitor Serial Number
* Network Number
* Operating System
* Processor Speed
* Registry Key Value
* System Serial Number
* Software
* Tag value
* User logged in
* User agent (show OCS NG agent version)
* And all the administrative information you’ve defined.

For each parameter, you can use one of the following comparison operators, depending of the
parameter you’ve selected:

* EQUAL
* DIFFERENT
* SMALLER
* BIGGER
* BETWEEN
* OUT OF

**`Note: Don’t forget to enable the parameter in the search!`**

![Results search criteria](../../img/server/reports/querying_ir_10.png)

**`Note: You can search for as many software as you wish in one search query. This means that you
can search for computers having software1 and software2 and… Unlike software field, all others
fields can only be specified once in any given search query.`**

## Search Software

You can now search a software and see on witch computers it's installed.

![Results software](../../img/server/reports/querying_ir_11.png)
