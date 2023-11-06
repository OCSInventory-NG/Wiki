# GreenIT - Server installation

## Introduction
This guide provides instructions on how to install the GreenIT plugin, along with its data filter and the associated crontab for scheduled tasks.

## Installation

### Prerequisites
The following dependency needs to be installed on your server:

- [Perl module] DateTime.pm

### Plugin
> ***NOTE**:  For instructions on installing the plugin, please refer to the [plugin installation documentation](https://wiki.ocsinventory-ng.org/10.Plugin-engine/Using-plugins-installer/#installation)*.

### Datafilter
The datafilter aims to limit the data coming from clients, filtering the data received by the server to only keep modified or new data.
Installation of the datafilter is highly recommended to avoid overloading the server with unnecessary data.

After installing and activating the plugin, copy the files from the datafilter folder *(`greenit/datafilter`)* into your server folder *(`/etc/ocsinventory-server`)*:

<p align="center">
  <img src="../../../img/server/greenit/datafilter_1.png"/>
</p>

## Configuration

### Allow access to GreenIT pages
When the plugin is installed, you will have to configure your profiles to allow access to the two GreenIT pages. Here is a full walkthrough : 

First, go to `Configuration >> Users`:

<p align="center">
  <img src="../../../img/server/greenit/allow_access_1.png"/>
</p>

On the `Users` page, select `Profiles` on the left menu:

<p align="center">
  <img src="../../../img/server/greenit/allow_access_2.png"/>
</p>

Choose a profile to configure : 

<p align="center">
  <img src="../../../img/server/greenit/allow_access_3.png"/>
</p>

Under the `Pages` section, check **ms_greenit_config** to allow access to the GreenIT configuration page and **ms_greenit_dashboard** to grant access to the GreenIT dashboard:

<p align="center">
  <img src="../../../img/server/greenit/allow_access_4.png"/>
</p>

> ***IMPORTANT NOTE**: For changes to take effect, users associated with the profile must log out and log back in again.* 


### Plugin configuration
You will need to have access to the configuration page to configure the dashboard (see previous [section](#allow-access-to-greenit-pages)). 
To access the configuration page, go to `Management >> GreenIT`:

<p align="center">
  <img src="../../../img/server/greenit/configuration_1.png"/>
</p>

The below screenshot shows the configuration page. There are two sections:
- Interface's settings: dashboard configuration
- API configuration: used to retrieve electricity prices

<p align="center">
  <img src="../../../img/server/greenit/configuration_2.png"/>
</p>

#### Dashboard configuration

First two items are expressed in days and represent :
- **Data collection period** : period of collection expressed in days (referred to as "Period X" in the rest of this documentation)
- **Data comparator period** : period of comparison expressed in days (referred to as "Period Y" in the rest of this documentation)

Second two items allow you to define the precision of the data displayed on the dashboard :
- **Consumption decimal precision**
- **Cost decimal precision**

Last two items are formatting options :
- **Cost format** : currency symbol to use (€, $, £, etc.)
- **Uptime format** : format to use for uptime values

#### API configuration
- **API key** : If you subscribed to the OCS-Inventory GreenIT offer, you received an API key by email. Enter it here.
- **Consumption type** : Based on the tresholds defined by the electricity provider, the price per kWh varies. If you don't know which treshold/segment you are in, leave the default value. If you know the treshold, enter it here to get a more accurate cost approximation.

### Crontab
The GreenIT plugin has two cronjobs that need to be configured for the dashboard to work properly.

Since calculations of statistics can be time-consuming, the plugin uses cronjobs to calculate the statistics in the background, ideally during off-peak hours. This allows faster loading of the dashboard when accessing it.

There are two execution modes:
- `full`: This mode calculates the power consumption statistics for all computers in the database using the data gathered by the plugin. Please note that executing this mode may take a long time, particularly if there is a large number of computers in the database.
- `delta`: This mode calculates the power consumption statistics for the current day only. Since it processes only the data of the current day, this mode is significantly faster than the full mode. As a result, it can be executed more frequently to update the statistics with the latest data.

To configure the two cronjobs, open a terminal and enter the following command:
```bash
crontab -e
```

Then add these two lines:
- `0 5 * * 1 php /usr/share/ocsinventory-reports/ocsreports/extensions/greenit/script/cron_stats.php --mode full`
- `0 5 * * * php /usr/share/ocsinventory-reports/ocsreports/extensions/greenit/script/cron_stats.php --mode delta`

> ***NOTE**: These two cronjobs are the default ones. You are allowed to change the execution time. (By default, every Monday at 5 a.m for full mode and every day at 5 a.m. for delta mode)*

And it's done :-)
