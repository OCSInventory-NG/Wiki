# Managing SNMP on Web Console

Since version 2.7, OCS Inventory integrates a new SNMP feature.

## SNMP configuration

To configure SNMP, go to `Configuration > SNMP configuration`.

![SNMP feature menu](../../img/server/reports/snmp_feature_menu.png)

First step, create a new type. SNMP type corresponding to the device that you want to inventory like printer, switch, etc ..

Set your type name, its OID and the string value corresponding to this OID. Click on `Send`.

**`Note : Type name must be unique and must not contain any special characters except underscore. Don't use MySQL keywords or reserved words, refer to : `[MySQL Keywords](https://dev.mysql.com/doc/refman/8.0/en/keywords.html)**

![SNMP feature type](../../img/server/reports/snmp_feature_type.png)

Below, you can see all created type. You can remove type with the red cross.

![SNMP feature type table](../../img/server/reports/snmp_feature_type_table.png)

Second step, create label. SNMP label corresponding to the string OID value like description, version, etc .. 

Click on `Create label` on the right navigation panel. Set the label name and click on `Send`. 

**`Note : Label name must be unique and must not contain any special characters except underscore. Don't use MySQL keywords or reserved words, refer to : `[MySQL Keywords](https://dev.mysql.com/doc/refman/8.0/en/keywords.html)**

![SNMP feature label](../../img/server/reports/snmp_feature_label.png)

Below, you can see all created label. You can remove label with the red cross.

![SNMP feature label table](../../img/server/reports/snmp_feature_label_table.png)

### 1 - Manually type configuration

Now, you can configure your type.

Click on `Type configuration` on the right navigation panel. Select your type, select your label and set the OID coresponding to the label's type. Click on `Send`. 

![SNMP feature configuration type](../../img/server/reports/snmp_feature_config_type.png)

Below, you can see all type configuration. You can remove configuration with the red cross.

If you want to display just one type configuration, select the type you want with the select field `Filter by type` and click on `Filter`.

![SNMP feature configuration table](../../img/server/reports/snmp_feature_config_table.png)

### 2 - Configuring type with MIBs file

Go to `Configuration > General configuration > SNMP` and set your server MIB folder directory. Save your configuration.

![SNMP feature MIB directory configuration](../../img/server/reports/snmp_feature_config.png)

Go to `Configuration > SNMP configuration > Add MIB file`. 

![SNMP feature MIB](../../img/server/reports/snmp_feature_mib.png)

![SNMP feature MIB configuration](../../img/server/reports/snmp_feature_mib_config.png)

On type configuration panel, you can see all type configuration. You can remove configuration with the red cross.

If you want to display just one type configuration, select the type you want with the select field `Filter by type` and click on `Filter`.

![SNMP feature MIB table](../../img/server/reports/snmp_feature_mib_table.png)