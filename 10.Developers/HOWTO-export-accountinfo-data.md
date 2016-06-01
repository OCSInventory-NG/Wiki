On 2.0 serie of OCS Inventory NG, you can choose complexe type for your accountinfo. If you want to
plug an application to ocs database, you have to know how it's work.

Show data on accountinfo table:

![accountinfo table](../img/Ex_accoutinfo_data.png)

Show data on accountinfo_config:

![accountinfo_config](../img/Ex_accoutinfo_config_data.png)

Type: 0 => Text, 1 => TEXTAREA, 2=>SELECT, 3=> Show the data, 4=> CHECKBOX, 5=>BLOB (FILE),
6=>DATE, 7=>radiobutton

How to find the accountinfo information?

Column fields_3 in accountinfo table => id=3 from accountinfo_config

Column fields_4 in accountinfo table => id=4 from accountinfo_config


How to find values when type in (2,7,4)?

you have to find the name of the accountinfo in accountinfo_config (NAME field)

and search in config table name like ACCOUNT_VALUE_[NAME]_%

![accountinfo_config data](../img/Ex_config_accountinfo_data.png)

all functions of accountinfo are in /require/function_admininfo.php
