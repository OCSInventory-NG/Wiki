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

For example, `/ocsapi/v1/computer/4/software` will retrieve the software list of the computer with the id 4 in database.

For example, `/ocsapi/v1/computer/4/officepack` will retrieve the office plugin informations of the computer with the id 4 in database.

If you want to do a simple search on a computer, add parameters at the end.
For example, `/ocsapi/v1/computer/4/software?where=PUBLISHER&operator=like&value=OCS`.

Operator list :

* Like / Not like
* = / !=
* < / > / <= / >= 

In the case you want to retrieve everything from this computer just remove {specificSection} parameters

<hr>

### List computer that match a term

`http://myocsserver/ocsapi/v1/computers/search?start={startoffset}&limit={limitoffset}&{searchCriteria}={valueToMatch}`

Start and limit offset are mandatory !
{searchCriteria} is a fields in the hardware table (other table are not supported atm)
{valueToMatch} is the value to match for this fields. (only equal atm)

For example, `/ocsapi/v1/computers/search?workgroup=myworkgroup&start=0&limit=10` will retrieve the computers that are in the workgroup `myworkgroup`

<hr>

## All Softwares Routes

### List softwares (start, limit, soft)

`http://myocsserver/ocsapi/v1/softwares?start={startoffset}&limit={limitoffset}&soft={software}`

Start and limit offset are mandatory !

{software} can be used to filter the list, and only show those softwares starting with this string.

For example, `/ocsapi/v1/softwares?&start=0&limit=10&soft=7-zip` will retrieve all softwares whose name start with `7-zip`

Example of result:

```json
[
  {
    "NAME": "7-Zip 16.02",
    "VERSION": "16.02"
  },
  {
    "NAME": "7-Zip 18.01 (x64)",
    "VERSION": "18.01"
  },
  {
    "NAME": "7-Zip 18.05",
    "VERSION": "18.05.00.0"
  },
  {
    "NAME": "7-Zip 18.05 (x64 edition)",
    "VERSION": "18.05.00.0"
  },
  {
    "NAME": "7-Zip 18.05 (x64)",
    "VERSION": "18.05"
  },
  {
    "NAME": "7-Zip 18.06 (x64 edition)",
    "VERSION": "18.06.00.0"
  },
  {
    "NAME": "7-Zip 18.06 (x64)",
    "VERSION": "18.06"
  },
  {
    "NAME": "7-Zip 19.00 (x64 edition)",
    "VERSION": "19.00.00.0"
  },
  {
    "NAME": "7-Zip 19.00 (x64)",
    "VERSION": "19.00"
  },
  {
    "NAME": "7-Zip 20.00 alpha (x64)",
    "VERSION": "20.00 alpha"
  }
]
```

<hr>

## SNMP Devices Routes

### List SNMP Type

`http://myocsserver/ocsapi/v1/snmps/typeList`

<hr>

### List X SNMP Type details (start, limit)

`http://myocsserver/ocsapi/v1/snmp/{TABLE_TYPE_NAME}?start={startoffset}&limit={limitoffset}`

{TABLE_TYPE_NAME} is the type you want to retrieve.

For example, `/ocsapi/v1/snmp/snmp_printer?start=0&limit=10` will retrieve the first 10 SNMP Printer Type results.

Start and limit offset are mandatory !

<hr>

### List one SNMP Type details

`http://myocsserver/ocsapi/v1/snmp/{TABLE_TYPE_NAME}/{ID}`

{TABLE_TYPE_NAME} is the type you want to retrieve.
{ID} is the ID in ocs database

For example, `/ocsapi/v1/snmp/snmp_printer/1` will retrieve the SNMP Printer Type with the id 1 in database.

<hr>

## IP Discover Routes

### List IP Discover Networks

`http://myocsserver/ocsapi/v1/ipdiscover/`

<hr>

### List IP Discover Networks Elements

`http://myocsserver/ocsapi/v1/ipdiscover/network/{networkID}`

{networkID} is the network number

For example, `/ocsapi/v1/ipdiscover/172.18.27.0` will retrieve the elements from the network 172.18.27.0

<hr>

### List IP Discover Elements by TAG

`http://myocsserver/ocsapi/v1/ipdiscover/tag/{TAG}`

{TAG} is the computer TAG

For example, `/ocsapi/v1/ipdiscover/NA` will retrieve the elements from ipdiscover with NA tag

<hr>

## CVE Routes

### List CVE by CVSS

`http://myocsserver/ocsapi/v1/cve/cvss/`

<hr>

### List CVE by software

`http://myocsserver/ocsapi/v1/cve/software/`

<hr>

### List CVE by computer

`http://myocsserver/ocsapi/v1/cve/computer/`

<hr>
