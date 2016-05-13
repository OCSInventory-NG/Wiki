# Management of administrative data

If you had migrate ocs 1.* to 2.*, you have to update your accountinfo.

Complex administrative data have been added in 2.0 release of OCS inventory NG. This feature allow
you to simply add informations to machines or SNMP devices.

Once this information added, it is present throughout the OCS web interface (multicriteria search,
columns in tables). By default, just one data exists : TAG. This field is modifiable but not deletable.

![Access Update](../img/EN_administrative_data_1.png)

## Configuration of management console

![Administrative Data Access](../img/EN_administrative_data_2.png)

Click on _Manage_ then _Administrative data_.

The table displays existing data.

![Existing Data](../img/EN_administrative_data_3.png)

Administratives data _Computer_ or _SNMP_ are managed in the same way. First will be available at the
Machine details, and the second will be visible on the details of an SNMP device.

## Add an administrative data category

![Add Data](../img/EN_administrative_data_4.png)

Your data will be divided into different categories that you will define. To do this, click on the plus,
on field ![Field](../img/EN_administrative_data_5.png)

A popup is then displayed and will allow you to manage your administrative data categories.

![Existing Data Plus](../img/EN_administrative_data_6.png)

You can modify/delete/add categories.

![Add Data Plus](../img/EN_administrative_data_7.png)

Now, you can create you own administrative data by categories.

## Different types of administrative data

You can create different data types : text, checkbox, button, textarea, **QR code**, etc. For exemple,
we have created following administrative data :

![Existing Data](../img/EN_administrative_data_3.png)

Going into the details of a machine, we find all these fields.

![Fields in computer](../img/EN_administrative_data_8.png)

### **QR code**

We create a new administrative data with type QR code, and chose the URL (detail of machine)

![New data](../img/EN_administrative_data_9.png)

Going into the details of a machine, we find new tab with QR code.

![QR_CODE](../img/EN_administrative_data_10.png)

## Add data to administrative data with multivalues

If you have created administrative data such as checkbox, select or radiobutton, you'll need to enter
values for these fields. To do this, go to machine details. Click on
![Pencil](../img/EN_administrative_data_14.png) to _edit_ mode. You will find in each multivalued field
this icon ![Plus](../img/EN_administrative_data_11.png). It will allow you to enter your data ..
You'll also notice that icons ![Arrows](../img/EN_administrative_data_12.png)
will allow you to organize your fields to display.

To exit to edit mode, click on ![Validate](../img/EN_administrative_data_13.png), top right.

For example, here's what you might have:

![Client](../img/EN_administrative_data_15.png)

So each of your machines will have, in the _Machine details_, all fields you have created.

These fields will also be present in some tables (_All machines_ for exemple), but also in the
multicriteria search. This will allow you to perform batch processing of your administrative data.