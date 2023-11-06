# Dashboard

## Introduction
This documentation explains the GreenIT dashboard views and provides examples of the data that can be displayed.


To access the dashboard, go to `Inventory >> GreenIT Manager` :

<p align="center">
  <img src="../../../img/server/greenit/dashboard_1.png"/>
</p>

Then on the left side, you'll have this menu:

<p align="center">
  <img src="../../../img/server/greenit/dashboard_2.png"/>
</p>

As you can see, there are 5 different statistic views:
- Global <br> *(Overview of your IT infrastructure)*
- Filtered <br> *(Statistics for group of assets, based on filters)*
- Operating system <br> *(Statistics per operating system)*
- Computer type <br> *(Statistics per computer type)*
- Manufacturer <br> *(Statistics per manufacturer)*

## The global statistics
This is the global statistics and the default view of your IT infrastructure.

<p align="center">
  <img src="../../../img/server/greenit/dashboard_global_1.png"/>
</p>

It displays two blocks:
- Yesterday's statistics
- Statistics comparator

The "yesterday's statistics" block has 3 parts:
- Consumption
- Uptime
- Cost

Each block has a total and an average of the data.

Whereas the statistics comparator has a cost part and a graphical view, which compares the total consumption with the cost. 
Two time periods are used for comparison, their length can be changed in the configuration page.

Example of the global statistic view with data:

<p align="center">
  <img src="../../../img/server/greenit/dashboard_global_2.png"/>
</p>

## Filtered statistics
The filtered view allows you to filter the global data.
The OCS-Inventory filters are used to filter by assets category, groups, operating system or tags.

<p align="center">
  <img src="../../../img/server/greenit/dashboard_filtered_1.png"/>
</p>

If the cronjob has been runned and data is available, the page should display a table similar to this one :

<p align="center">
  <img src="../../../img/server/greenit/dashboard_filtered_2.png"/>
</p>

By clicking on a computer's name, you can access detailed data about it :

<p align="center">
  <img src="../../../img/server/greenit/dashboard_filtered_3.png"/>
</p>

Or you can apply a filter. When applying it, a message informs you that the filter is active. Below this message, a `Generate filtered stats` button is displayed.

<p align="center">
  <img src="../../../img/server/greenit/dashboard_filtered_4.png"/>
</p>

When clicking on this button, the page will display a similar view to the Global Statistics view but with filtered data:

<p align="center">
  <img src="../../../img/server/greenit/dashboard_filtered_5.png"/>
</p>

## Operating system statistics
The operating system statistics view is a preset to compare Windows clients and servers.

<p align="center">
  <img src="../../../img/server/greenit/dashboard_os_1.png"/>
</p>

This view is split into 2 parts:
- D-1 uptime and total consumption
- 3 doughnut diagrams of cost per operating system (D-1, period X, period Y, which are configurable in the configuration page of the plugin, see [dashboard configuration](Server-installation-and-configuration.md#dashboard-configuration))

Example of operating system stats view with data:

<p align="center">
  <img src="../../../img/server/greenit/dashboard_os_2.png"/>
</p>

## Computer type statistics
This view is a preset comparing the consumption of 3 different types of assets:
- Desktop
- Laptop
- Other

<p align="center">
  <img src="../../../img/server/greenit/dashboard_computerType_1.png"/>
</p>

This view has 2 parts:
- The first part cannot be displayed if there is not data available. Statistics show the average cost per computer type for period X and Y.
- The 3 vertical bar diagrams on the second part show the total consumption and cost per computer type.

Example of operating system stats view with data:

<p align="center">
  <img src="../../../img/server/greenit/dashboard_computerType_2.png"/>
</p>

## Manufacturer statistics
This preset shows the consumption of the 5 first manufacturers.

<p align="center">
  <img src="../../../img/server/greenit/dashboard_manufacturer_1.png"/>
</p>

This view follows the structure of the Computer type statistics view:
- The first part cannot be displayed if there is not data available. Statistics show the average cost per manufacturer for period X and Y (limited to 5 first manufacturers).
- The 3 honrizontal bar diagrams on the second part show the total consumption and cost for the 5 first manufacturers.

Example picture of manufacturer statistics view with data:

<p align="center">
  <img src="../../../img/server/greenit/dashboard_manufacturer_2.png"/>
</p>