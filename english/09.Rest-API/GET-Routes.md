# GET Routes

By default, the API can be access trough : 
`http://myocsserver/ocsapi/v1/my/routes`

## Computers Routes

### List computers ID

`http://myocsserver/ocsapi/v1/computers/listID`

<hr>

### List X computers details (start, limit)

`http://myocsserver/ocsapi/v1/computers?start={startoffset}&limit={limitoffset}`

Start and limit offset are mandatory !

<hr>

### List one computer details

`http://myocsserver/ocsapi/v1/computer/{ID}/{specificSection}`

{ID} is the ID in ocs database
{specificSection} is the section you want to retrieve.

For exemple, `/ocsapi/v1/computer/4/softwares` will retrieve the software list of the computer with the id 4 in database.

For exemple, `/ocsapi/v1/computer/4/officepack` will retrieve the office plugin informations of the computer with the id 4 in database.

In the case you want to retrieve everything from this computer just remove {specificSection} parameters

<hr>

### List computer that match a term

`http://myocsserver/ocsapi/v1/computers/search?start={startoffset}&limit={limitoffset}&{searchCriteria}={valueToMatch}`

Start and limit offset are mandatory !
{searchCriteria} is a fields in the hardware table (other table are not supported atm)
{valueToMatch} is the value to match for this fields. (only equal atm)

For exemple, `/ocsapi/v1/computers/search?workgroup=myworkgroup&start=0&limit=10` will retrieve the computers that are in the workgroup `myworkgroup`

<hr>

## SNMP Devices Routes

### List SNMP Devices ID

`http://myocsserver/ocsapi/v1/snmps/listID`

<hr>

### List X SNMP Devices details (start, limit)

`http://myocsserver/ocsapi/v1/snmps?start={startoffset}&limit={limitoffset}`

Start and limit offset are mandatory !

<hr>

### List one SNMP Device details

`http://myocsserver/ocsapi/v1/snmp/{ID}/{specificSection}`

{ID} is the ID in ocs database
{specificSection} is the section you want to retrieve.

For exemple, `/ocsapi/v1/snmp/4/cartridges` will retrieve the cartridges list of the SNMP Device with the id 4 in database.

In the case you want to retrieve everything from this snmp device just remove {specificSection} parameters

<hr>

## IP Discover Routes

### List IP Discover Networks

`http://myocsserver/ocsapi/v1/ipdiscover/`

<hr>

### List IP Discover Networks Elements

`http://myocsserver/ocsapi/v1/ipdiscover/{networkID}`

{networkID} is the network number

For exemple, `/ocsapi/v1/ipdiscover/172.18.27.0` will retrieve the elements from the network 172.18.27.0

<hr>