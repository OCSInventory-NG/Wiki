# GET Routes

By default, the API can be access trough : 
`http://myocsserver/ocsapi/v1/my/routes`

## Computers Routes

### List computers ID

**URL :** `ocsapi/v1/computers/listID`

**Description :** List all computer IDs.

**Return :** Table of computer IDs in JSON format.

_Usage example :_

Full URL : `http://myocsserver/ocsapi/v1/computers/listID`

Result :

```json
[
  {
    "ID":1942
  },
  {
    "ID":1974
  },
  ...
]
```

### List computers details (start, limit)

**URL :** `ocsapi/v1/computers?start=:startoffset&limit=:limitoffset`

**Descrition :** List X computers details

**Parameters :** (query string)

* _start_ : start offset of the list. Mandatory.
* _limit_ : limit offset of the list. Mandatory.

**Returns :**

* Table of computers details in JSON format.
* `Argu` : missing mandatory parameters.

_Usage example :_

Full URL : `http://myocsserver/ocsapi/v1/computers?start=0&limit=10`

Result :

```json
{
  "16": {
    "accountinfo": [
        {
          "HARDWARE_ID":16,
          "TAG":"DEV-MACHINE"
        }
      ],
    "batteries":[],
    "bios": [
      {
        "ASSETTAG":"",
        "BDATE":"04\/16\/2021",
        "BMANUFACTURER":"Dell Inc.",
        "BVERSION":"2.5.1",
        "HARDWARE_ID":16,
        "MMANUFACTURER":"Dell Inc.",
        "MMODEL":"045M96",
        "MSN":"",
        "SMANUFACTURER":"",
        "SMODEL":"PowerEdge R340",
        "SSN":"",
        "TYPE":"Rack Mount Chassis"
      }
    ], 
    ...
}
```

### List last updated computers

**URL :** `ocsapi/v1/computers/lastupdate/:timestamp`

**Description :** List last updated computer IDs.

**Parameter :** (query string)

* _:timestamp_ (default: 86400, i.e. 1 day) : timestamp of the number of days to count down from the current date. Optional.

**Return :** Table of computer IDs in JSON format.

_Usage example :_

Full URL : `http://myocsserver/ocsapi/v1/computers/lastupdate/172800`

Result :
```json
[
  {
    "ID":16
  },
  {
    "ID":1942
  },
  ...
]
```

### List one computer details

**URL :** `ocsapi/v1/computer/:id/:specificSection`

**Description :** Retrieve details of a specific computer.

**Parameters :** (query string)

* _:id_ : unique identifier of the computer in the database. Mandatory.
* _:specificSection_ (return all details by default) : specific section of inventory to display. Optional.
* _where_ : search on a specific column of the specific section. Optional.
* _operator_ : search operator (`like`, `not like`, `=`, `!=`, `<`, `>`, `<=`, `>=`). Mandatory if _where_ parameter is set.
* _value_ : search value. Mandatory if _where_ parameter is set.

**Return :**

* Table of complet inventory details in JSON format if no specific section is filled in.
* Table of section inventory details in JSON format if specific section is filled in.
* `Raptor not found` : missing mandatory parameter.

_Usage example without specific section :_

Full URL : `http://myocsserver/ocsapi/v1/computer/16`

Result :
```json
{
  "16": {
    "sofwtare": [
      {
        "BITSWIDTH":0,
        "COMMENTS":"query and manipulate user account information",
        "FILENAME":"",
        "FILESIZE":462848,
        "FOLDER":"",
        ...
      },
      ...
    ],
    "bios": [
      {
        "ASSETTAG":"",
        "BDATE":"04\/16\/2021",
        "BMANUFACTURER":"Dell Inc.",
        "BVERSION":"2.5.1",
        ...
      }
    ],
    ...
  }
}
```

_Usage example with specific section :_

Full URL : `http://myocsserver/ocsapi/v1/computer/16/bios`

Result :
```json
{
  "16": {
    "bios": [
      {
        "ASSETTAG":"",
        "BDATE":"04\/16\/2021",
        "BMANUFACTURER":"Dell Inc.",
        "BVERSION":"2.5.1",
        ...
      }
    ]
  }
}
```

_Usage example with simple search :_

Full URL : `http://myocsserver/ocsapi/v1/computer/16/software?where=NAME&operator=like&value=bash`

Result :
```json
{
  "16": {
    "hardware": {
      "ARCH": null,
      "ARCHIVE": null,
      "CATEGORY_ID": null,
      "CHECKSUM": 328449,
      ...
    },
    "software": [
      {
        "BITSWIDTH": 0,
        "COMMENTS": "GNU Bourne Again SHell",
        "FILENAME": "",
        "FILESIZE": 1699840,
        "FOLDER": "",
        "GUID": "",
        "HARDWARE_ID": 16,
        "ID": 303986,
        "INSTALLDATE": "2022-04-03 12:59:54",
        "LANGUAGE": "",
        "NAME": "bash",
        "PUBLISHER": "http://tiswww.case.edu/php/chet/bash/bashtop.html",
        "SOURCE": 0,
        "VERSION": "5.0-6ubuntu1.2"
      },
      {
        "BITSWIDTH": 0,
        "COMMENTS": "programmable completion for the bash shell",
        "FILENAME": "",
        "FILESIZE": 1522688,
        "FOLDER": "",
        "GUID": "",
        "HARDWARE_ID": 16,
        "ID": 303987,
        "INSTALLDATE": "2020-03-25 13:53:39",
        "LANGUAGE": "",
        "NAME": "bash-completion",
        "PUBLISHER": "https://github.com/scop/bash-completion",
        "SOURCE": 0,
        "VERSION": "1:2.10-1ubuntu1"
      }
    ]
  }
}
```

### List computer that match a term

**URL :** `ocsapi/v1/computers/search?start=:startoffset&limit=:limitoffset&:searchCriteria=:valueToMatch`

**Description :** Retrieve computer IDs list with a simple search on hardware table (other table are not supported).

**Parameters :** (query string)

* _start_ : start offset of the list. Mandatory.
* _limit_ : limit offset of the list. Mandatory.
* _:searchCriteria_ : hardware column name. Mandatory.
* _:valueToMatch_ : value to match for the hardware column. Mandatory.

**Return :** Table of computer IDs in JSON format.

_Usage example :_

Full URL : `http://myocsserver/ocsapi/v1/computers/search?start=0&limit=10&userid=root`

Result :
```json
[
  {
    "ID": 16
  },
  {
    "ID": 1972
  },
  ...
]
```

## All Softwares Routes

### List softwares (start, limit, soft)

**URL :** `ocsapi/v1/softwares?start=:startoffset&limit=:limitoffset&soft=:softwarename`

**Description :** List all software details (name, publisher and version)

**Parameters :** (query string)

* _start_ : start offset of the list. Mandatory.
* _limit_ : limit offset of the list. Mandatory.
* _soft_ : software name filter. Optional.

**Return :** Table of software details in JSON format.

_Usage example :_

Full URL : `http://myocsserver/ocsapi/v1/softwares?start=0&limit=10&soft=7-zip`

Result :
```json
[
  {
    "NAME": "7-Zip 16.02",
    "PUBLISHER":"Igor Pavlov",
    "VERSION": "16.02"
  },
  {
    "NAME": "7-Zip 18.01 (x64)",
    "PUBLISHER":"Igor Pavlov",
    "VERSION": "18.01"
  },
  {
    "NAME": "7-Zip 18.05",
    "PUBLISHER":"Igor Pavlov",
    "VERSION": "18.05.00.0"
  },
  ...
]
```

## SNMP Devices Routes

### List SNMP Type

**URL :** `ocsapi/v1/snmps/typeList`

**Description :** List all SNMP types

**Return :** Table of SNMP types in JSON format.

_Usage example :_

Full URL : `http://myocsserver/ocsapi/v1/snmps/typeList`

Result :
```json
[
  {
    "ID": 13,
    "TABLE_TYPE_NAME": "snmp_default",
    "TYPE_NAME": "Default"
  },
  ...
]
```

### List X SNMP Type details (start, limit)

**URL :** `ocsapi/v1/snmp/:TABLE_TYPE_NAME?start=:startoffset&limit=:limitoffset`

**Description :** List SNMP equipment details of a type.

**Parameters :** (query string)

* _:TABLE_TYPE_NAME_ : SNMP table type name. Mandatory.
* _start_ : start offset of the list. Mandatory.
* _limit_ : limit offset of the list. Mandatory.

**Return :** Table of SNMP equipment details of a type.

_Usage example :_

Full URL : `http://myocsserver/ocsapi/v1/snmp/snmp_default?start=0&limit=10`

Result :
```json
[
  {
    "DefaultAddressIP": "127.0.0.1",
    "DefaultDescription": "This is a description",
    "DefaultGateway": "172.18.25.254",
    "DefaultLocation": "Here",
    "DefaultMacAddress": null,
    "DefaultName": "My Equipment",
    "DefaultUptime": null,
    "ID": 1,
    "LASTDATE": "2023-06-08 12:22:18"
  },
  ...
]
```

_Usage example to retrieve SNMP accountinfo :_

Full URL : `http://myocsserver/ocsapi/v1/snmp/snmp_accountinfo?start=0&limit=10`

Result :
```json
[
  {
    "ID": 27,
    "SNMP_RECONCILIATION_FIELD": "DefaultName",
    "SNMP_RECONCILIATION_VALUE": "My Equipment",
    "SNMP_TYPE": "snmp_default",
    "TAG": "Default TAG",
    "fields_19": null,
    "fields_21": null
  },
  ...
]
```

### List one SNMP Type details

**URL :** `ocsapi/v1/snmp/:TABLE_TYPE_NAME/:id`

**Description :** Retrieve SNMP equipment details by equipment type id.

**Parameters :** (query string)

* _:TABLE_TYPE_NAME_ : SNMP table type name. Mandatory.
* _:id_ : unique identifier of the SNMP equipment in the database. Mandatory.

**Return :** Table of the SNMP equipment details in JSON format.

_Usage example :_

Full URL : `http://myocsserver/ocsapi/v1/snmp/snmp_default/1`

Result :
```json
[
  {
    "DefaultAddressIP": "127.0.0.1",
    "DefaultDescription": "This is a description",
    "DefaultGateway": "172.18.25.254",
    "DefaultLocation": "Here",
    "DefaultMacAddress": null,
    "DefaultName": "My Equipment",
    "DefaultUptime": null,
    "ID": 1,
    "LASTDATE": "2023-06-08 12:22:18"
  }
]

```

_Usage example to retrieve SNMP accountinfo :_

Full URL : `http://myocsserver/ocsapi/v1/snmp/snmp_accountinfo/27`

Result :
```json
[
  {
    "ID": 27,
    "SNMP_RECONCILIATION_FIELD": "DefaultName",
    "SNMP_RECONCILIATION_VALUE": "My Equipment",
    "SNMP_TYPE": "snmp_default",
    "TAG": "Default TAG",
    "fields_19": null,
    "fields_21": null
  }
]
```

## IpDiscover Routes

### List IpDiscover Networks

**URL :** `ocsapi/v1/ipdiscover`

**Description :** List all IpDiscover networks.

**Parameters :** (query string)

* _start_ : start offset of the list. Optional.
* _limit_ : limit offset of the list. Optional.

**Return :** Table of IpDiscover networks in JSON format.

_Usage example :_

Full URL : `http://myocsserver/ocsapi/v1/ipdiscover`

Result :
```json
[
  {
    "NETID": "172.18.25.0"
  },
  {
    "NETID": "172.18.26.0"
  },
  ...
]
```

### List IpDiscover Networks Elements

**URL :** `ocsapi/v1/ipdiscover/network/:NETID`

**Description :** List all devices for a network.

**Parameter :**

* _:NETID_ : Network number. Mandatory.

**Return :** Table of network devices in JSON format.

__Usage example :_

Full URL : `http://myocsserver/ocsapi/v1/ipdiscover/network/172.18.25.0`

Result :
```json
[
  {
    "DATE": "2023-06-08 12:15:08",
    "HARDWARE_ID": 16,
    "IP": "172.18.25.254",
    "MAC": "00:0d:b9:51:fc:aa",
    "MASK": "255.255.255.0",
    "NAME": "-",
    "NETID": "172.18.25.0",
    "TAG": "DEV-MACHINE"
  },
  {
    "DATE": "2023-06-08 12:15:08",
    "HARDWARE_ID": 16,
    "IP": "172.18.25.154",
    "MAC": "0a:30:e2:4f:fb:09",
    "MASK": "255.255.255.0",
    "NAME": "-",
    "NETID": "172.18.25.0",
    "TAG": "DEV-MACHINE"
  },
  ...
]
```

### List IpDiscover Elements by TAG

**URL :** `ocsapi/v1/ipdiscover/tag/:TAG`

**Description :** List all devices for a TAG.

**Parameter :**

* _:TAG_ : TAG value. Mandatory.

**Return :** Table of network devices for a TAG in JSON format.

__Usage example :_

Full URL : `http://myocsserver/ocsapi/v1/ipdiscover/tag/DEV-MACHINE`

Result :
```json
[
  {
    "DATE": "2023-06-08 12:15:08",
    "HARDWARE_ID": 16,
    "IP": "172.18.25.254",
    "MAC": "00:0d:b9:51:fc:aa",
    "MASK": "255.255.255.0",
    "NAME": "-",
    "NETID": "172.18.25.0",
    "TAG": "DEV-MACHINE"
  },
  {
    "DATE": "2023-06-08 12:15:08",
    "HARDWARE_ID": 16,
    "IP": "172.18.25.154",
    "MAC": "0a:30:e2:4f:fb:09",
    "MASK": "255.255.255.0",
    "NAME": "-",
    "NETID": "172.18.25.0",
    "TAG": "DEV-MACHINE"
  },
  ...
]
```

## CVE Routes

### List CVE by CVSS

**URL :** `ocsapi/v1/cve/cvss`

**Description :** List all CVE by CVSS.

**Parameters :** (query string)

* _start_ : start offset of the list. Optional.
* _limit_ : limit offset of the list. Optional.

**Return :** Table of CVE details in JSON format.

_Usage example :_

Full URL : `http://myocsserver/ocsapi/v1/cve/cvss?start=0&limit=10`

Result :
```json
[
  {
    "CVE": "CVE-2020-10878",
    "CVSS": 7.5,
    "ID": 260,
    "LINK": "https://github.com/Perl/perl5/compare/v5.30.2...v5.30.3",
    "MAJOR": 5,
    "MINOR": 30,
    "NAME": "perl",
    "NAME_ID": 578,
    "PATCH": 0,
    "PRETTYVERSION": "5.30.0",
    "PUBLISHER": "http://dev.perl.org/perl5/",
    "PUBLISHER_ID": 217,
    "VERSION": "5.30.0-9ubuntu0.2",
    "VERSION_ID": 260,
    "id": "https://github.com/Perl/perl5/compare/v5.30.2...v5.30.3",
    "search": "perl;5.30.0-9ubuntu0.2"
  },
  {
    "CVE": "CVE-2020-12723",
    "CVSS": 5,
    "ID": 260,
    "LINK": "https://github.com/Perl/perl5/compare/v5.30.2...v5.30.3",
    "MAJOR": 5,
    "MINOR": 30,
    "NAME": "perl",
    "NAME_ID": 578,
    "PATCH": 0,
    "PRETTYVERSION": "5.30.0",
    "PUBLISHER": "http://dev.perl.org/perl5/",
    "PUBLISHER_ID": 217,
    "VERSION": "5.30.0-9ubuntu0.2",
    "VERSION_ID": 260,
    "id": "https://github.com/Perl/perl5/compare/v5.30.2...v5.30.3",
    "search": "perl;5.30.0-9ubuntu0.2"
  },
  ...
]
```

### List CVE by software

**URL :** `ocsapi/v1/cve/software`

**Description :** List all CVE by software.

**Parameters :** (query string)

* _start_ : start offset of the list. Optional.
* _limit_ : limit offset of the list. Optional.

**Return :** Table of CVE details in JSON format.

_Usage example :_

Full URL : `http://myocsserver/ocsapi/v1/cve/software?start=0&limit=10`

Result :
```json
[
  {
    "CVE": "CVE-2020-10878",
    "CVSS": 7.5,
    "ID": 260,
    "LINK": "https://github.com/Perl/perl5/compare/v5.30.2...v5.30.3",
    "MAJOR": 5,
    "MINOR": 30,
    "NAME": "perl",
    "NAME_ID": 578,
    "PATCH": 0,
    "PRETTYVERSION": "5.30.0",
    "PUBLISHER": "http://dev.perl.org/perl5/",
    "PUBLISHER_ID": 217,
    "VERSION": "5.30.0-9ubuntu0.2",
    "VERSION_ID": 260,
    "id": "https://github.com/Perl/perl5/compare/v5.30.2...v5.30.3",
    "nameid": 578,
    "search": "perl"
  },
  ...
]
```

### List CVE by computer

**URL :** `ocsapi/v1/cve/computer`

**Description :** List all CVE by computer.

**Parameters :** (query string)

* _start_ : start offset of the list. Optional.
* _limit_ : limit offset of the list. Optional.

**Return :** Table of CVE details in JSON format.

_Usage example :_

Full URL : `http://myocsserver/ocsapi/v1/cve/computer?start=0&limit=10`

Result :
```json
[
  {
    "CVE": "CVE-2020-10878",
    "CVSS": "7.50",
    "HARDWARE_ID": 1971,
    "HARDWARE_NAME": "OCS-Serveur-MySQL",
    "ID": 1,
    "LINK": "https://github.com/Perl/perl5/compare/v5.30.2...v5.30.3",
    "PUBLISHER": "http://dev.perl.org/perl5/",
    "SOFTWARE_NAME": "perl",
    "VERSION": "5.30.0-9ubuntu0.2"
  },
  ...
]
```

### List computers vulnerable (at least one CVE)

**URL :** `ocsapi/v1/cve/computerslist`

**Description :** List of computers with at least one CVE.

**Return :** Table of computers in JSON format.

_Usage example :_

Full URL : `http://myocsserver/ocsapi/v1/cve/computerslist`

Result :
```json
[
  {
    "HARDWARE_ID": 1971,
    "HARDWARE_NAME": "OCS-Serveur-MySQL"
  },
  {
    "HARDWARE_ID": 16,
    "HARDWARE_NAME": "DEV-CAU"
  },
  {
    "HARDWARE_ID": 1972,
    "HARDWARE_NAME": "OCS-CAU-SRV"
  },
  ...
]
```

### Get CVE history

**URL :** `ocsapi/v1/cve/history`

**Description :** List CVE history.

**Return :** Table of CVE history details in JSON format.

_Usage example :_

Full URL : `http://myocsserver/ocsapi/v1/cve/history`

Result :
```json
[
  {
    "CVE_NB": 0,
    "FLAG_DATE": "2023-06-01 14:37:59",
    "ID": 1,
    "PUBLISHER": "http://code.woong.org/wcwidth.js",
    "PUBLISHER_ID": 694
  },
  {
    "CVE_NB": 0,
    "FLAG_DATE": "2023-06-01 14:38:16",
    "ID": 2,
    "PUBLISHER": "http://dev.mysql.com/",
    "PUBLISHER_ID": 187
  },
  ...
]
```