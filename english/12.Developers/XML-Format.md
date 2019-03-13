## XML-Format

OCSInventory is using XML for nearly every transactions, DTDs had been written to have strict XML.

We have 7 DTDs, one for each kind of XML:

* inventory_request.dtd
* inventory_reply.dtd
* update_request.dtd
* update_reply.dtd
* prolog_request.dtd
* prolog_reply.dtd
* file_request.dtd

Every time you make changes on a particular XML or you had datas in (by modifying Inventory.pm on an Agent
for example) you need to update the corresponding DTD.

The biggest one is for the inventory request, we have every field defined in.

For example:

When a Storage inventory is done you have an XML like that:

    <STORAGES>
      <DESCRIPTION>SATA</DESCRIPTION>
      <DISKSIZE>117220</DISKSIZE>
      <FIRMWARE>3.AD</FIRMWARE>
      <MANUFACTURER>Seagate</MANUFACTURER>
      <MODEL>ST9120823AS</MODEL>
      <NAME>sda</NAME>
      <SERIALNUMBER>5NJ0NJ09</SERIALNUMBER>
      <TYPE>disk</TYPE>
    </STORAGES>

For each field you have a corresponding line in the DTD:

This one define the subtree STORAGES, you have a list of every field allowed in, the separator is a | because we don't want a special order, and we put a * after the brackets, because there are not all mandatory.

    <!ELEMENT STORAGES (MANUFACTURER | NAME | MODEL | DESCRIPTION | TYPE | DISKSIZE | FIRMWARE | SERIALNUMBER)*>

Then you have a line for each field which can contain text datas (#PCDATA).

    <!ELEMENT MANUFACTURER (#PCDATA)>
    <!ELEMENT NAME (#PCDATA)>
    <!ELEMENT MODEL (#PCDATA)>
    <!ELEMENT DESCRIPTION (#PCDATA)>
    <!ELEMENT TYPE (#PCDATA)>
    <!ELEMENT DISKSIZE (#PCDATA)>
    <!ELEMENT FIRMWARE (#PCDATA)>
    <!ELEMENT SERIALNUMBER (#PCDATA)>

So each time you want to add a field in a XML file, you need to create the field and to add it in the
list for the subtree where it goes. Every field is usable on every subtree, you just have to add it
in the list if it already exist.
