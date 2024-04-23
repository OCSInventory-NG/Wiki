# Using IPDiscover scanner

This script is meant as an alternative to the existing IpDiscover perl script included with ocsreports. It will not insert data into the database, but the results of the scan will be output to a file in XML format. The files can then be injected into an OCS server, either by using the perl injector script or any tool able to POST data to /ocsinventory.

## Requirements
- Python 3.6+
- fping
- nmap

First, you can use this command to install requirements:
```shell
apt install fping nmap
```

## Usage

Example usage:
```shell
python3 ipd_scanner.py --scantype=fping --subnets=172.18.25.0/24,172.18.15.0/24:tag15 --output_dir=/tmp/ --debug
```

This will scan the subnets 172.18.25.0/24 and 172.18.15.0/24 using fping and output the results to the /tmp/ directory. The "tag15" tag will be added to all the hosts in the 172.18.15.0/24 subnet. The tag is optional and can be omitted. The debug flag will output additional information to the console such as command outputs.

## Options
- `--scantype` - The type of scan to perform, fping or nmap. Required.
- `--subnets` - A comma separated list of subnets to scan. Required. The format is subnet:tag, where tag is optional.
- `--output_dir` - The directory where the output files will be saved. Defaults to the `results` directory in the script's directory.
- `--debug` - Enable debug output. Optional.

## Injecting the results into OCS

See the [OCS documentation](https://wiki.ocsinventory-ng.org/08.Multi-site-network-architecture/Synchronisation-between-OCS-server-master-slaves/#synchronsation-of-one-inventory) for more information on how to inject the results into OCS.

The perl injector script is included with the OCS installation. It can be found in the `binutils` directory of the server. The script is called `ocsinventory-injector.pl` (see [ocsinventory-injector.pl on Github](https://github.com/OCSInventory-NG/OCSInventory-Server/blob/master/binutils/ocsinventory-injector.pl))