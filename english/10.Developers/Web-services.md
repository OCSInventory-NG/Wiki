Learn how to access inventory data collected by OCS Inventory NG through the OCS Inventory NG SOAP
Web Service.

Since version 1.0, OCS Inventory NG Server provides a Web Service usable through SOAP over HTTP.
This Web Service is available from the OCS Inventory NG Communication Server through the URL "/ocsinterface".

# Request types

The Web Service currently implements the following request types:

## get_computers_V1

This function allows to query inventoried computers.

You must supply as parameter an XML structure described by the following DTD:

    <?xml version="1.0" encoding="ISO-8859-1"?>
    <!DOCTYPE REQUEST[
    <!ELEMENT REQUEST(BEGIN,ASKING_FOR,CHECKSUM,WANTED,ID*,TAG*,USERID*)>
    <!ELEMENT ENGINE(#PCDATA)>
    <!ELEMENT BEGIN>
    <!ELEMENT ASKING_FOR(#PCDATA)>
    <!ELEMENT CHECKSUM(#PCDATA)>
    <!ELEMENT WANTED(#PCDATA) *>
    <!ELEMENT ID(#PCDATA) *>
    <!ELEMENT TAG(#PCDATA) *>
    <!ELEMENT USERID(#PCDATA) *>
    ]>

**ENGINE** allows to select a search engine. Actually, only one search engine is available.
So you must use <ENGINE>FIRST</ENGINE> (case insensitive).

**BEGIN** allows to navigate in results using an offset.

**CHECKSUM** allows to query computers for modified inventory sections. This is a 32 bits number
in which each bit identifies an inventory section (inventory sections may be combined):

    Bit 1 => Information about 'Hardware' (1 modified, 0 unchanged),
    Bit 2 => Information about 'Bios' (1 modified, 0 unchanged),
    Bit 3 => Information about 'Memory slots' (1 modified, 0 unchanged),
    Bit 4 => Information about 'System slots' (1 modified, 0 unchanged),
    Bit 5 => Information about 'Registry' (1 modified, 0 unchanged),
    Bit 6 => Information about 'System controllers' (1 modified, 0 unchanged),
    Bit 7 => Information about 'Monitors' (1 modified, 0 unchanged),
    Bit 8 => Information about 'System ports' (1 modified, 0 unchanged),
    Bit 9 => Information about 'Storage peripherals' (1 modified, 0 unchanged),
    Bit 10 => Information about 'Logical drives' (1 modified, 0 unchanged),
    Bit 11 => Information about 'Input devices' (1 modified, 0 unchanged),
    Bit 12 => Information about 'Modems' (1 modified, 0 unchanged),
    Bit 13 => Information about 'Network adapters' (1 modified, 0 unchanged),
    Bit 14 => Information about 'Printers' (1 modified, 0 unchanged),
    Bit 15 => Information about 'Sound adapters' (1 modified, 0 unchanged),
    Bit 16 => Information about 'Video adapters' (1 modified, 0 unchanged),
    Bit 17 => Information about 'Software' (1 modified, 0 unchanged)

**WANTED** is a 32 bits number like CHECKSUM, but for non-standard inventory sections:

    Bit 1 => Information about ACCOUNTINFO (1 modified, 0 unchanged),
    Bit 2 => Information about DICO_SOFT (1 modified, 0 unchanged).

**DICO_SOFT** returns dictionary names of installed software. Here is the DTD:

    <!ELEMENT DICO_SOFT(WORD*)>
    <WORD(#PCDATA)*>

Example: You want software from dictionary, but not all software listed in OCS.

    ...
    <ASKING_FOR>INVENTORY</ASKING_FOR>
    <CHECKSUM>Your_cheksum|(~SOFTWARES_BIT)</CHECKSUM>
    <WANTED>2</WANTED>
    ...

**ACCOUNT_INFO** returns all administrative information. Here is the DTD:

    <!ELEMENT ACCOUNTINFO(ENTRY)>
    <!ELEMENT ENTRY(#PCDATA) *>
    <!ATTLIST ENTRY Name CDATA #REQUIRED>

The attribute "Name" of ENTRY contains the name of a piece of administrative information (TAG, for example), and value is marked up.

Example: You want all inventory section and accountinfo

    ...
    <ASKING_FOR>INVENTORY</ASKING_FOR>
    <CHECKSUM>131071</CHECKSUM>
    <WANTED>1</WANTED>
    ...

The following other search criteria may be used besides of checksum:

* ID may be multivalued, specifies a database ID of a computer.
* TAG may be multivalued.
* USERID may be multivalued, specifies a user logged on the computer when it was last inventoried.

**ASKING_FOR** allows to specify which answer is needed, META or INVENTORY.

In both cases, root is always \<COMPUTERS/>, and every computer is marked up with \<COMPUTER/>.

(\<!ELEMENT COMPUTERS(COMPUTER*)>\<!ELEMENT COMPUTER(Changing here)>...)

META are computers metadata:

    <DEVICEID /> OCS computer unique ID => [hostname]-AAAA-MM-JJ-HH-MM-SS
    <DATABASEID /> Database ID (hardware.ID ou *.HARDWARE_ID)
    <CHECKSUM /> Checksum of last modified inventory sections
    <LAST_COME /> Last contact of agent to Communication server
    <LAST_DATE /> Last inventory date
    <NAME /> Computer name
    <TAG /> Computer TAG value

Here is the DTD of a computer inventory answer:

    <?xml version="1.0" encoding="ISO-8859-1"?>
    <!DOCTYPE RESPONSE[
    <!ELEMENT COMPUTERS(DEVICEID,DATABASEID,CHECKSUM,LASTCOME,LASTDATE,NAME,TAG)>
    <!ELEMENT DEVICEID(#PCDATA)>
    <!ELEMENT DATABASEID(#PCDATA)>
    <!ELEMENT CHECKSUM(#PCDATA)>
    <!ELEMENT LASTCOME(#PCDATA)>
    <!ELEMENT LASTDATE(#PCDATA)>
    <!ELEMENT NAME(#PCDATA)>
    <!ELEMENT TAG(#PCDATA)>
    ]>

Inventory is built from XML sections mapped to database tables (see Database Schema and OCS
Inventory NG Server DTD).

## ocs_config_V1(key, [value])

Returns configuration option value indicated in "key", and optionally set option to "value".

If second parameter is provided, answer must be this value if no error.

## get_dico_soft_element_V1(word)

Returns a category for raw software name provided as parameter.

This method is not currently used.

## get_history_V1(begin, num)

Returns events from deleted_equiv table (delete and duplicates management).

* begin = offset
* num = number of events

The answer is always sorted by date.

DTD is:

    <!ELEMENT EVENTS(EVENT*)>
    <!ELEMENT EVENT(DATE,DELETED,EQUIVALENT) *>
    <!ELEMENT DATE(#PCDATA)>
    <!ELEMENT DELETED(#PCDATA)>
    <!ELEMENT EQUIVALENT(#PCDATA)>

Example:

    <EVENTS>
    <EVENT>
    <DATE>2006-10-16 09:43:52</DATE>
    <DELETED>computer_à_toto-2005-06-09-09-36-41</DELETED>
    <EQUIVALENT>computer_à_toto-2006-08-17-11-37-35</EQUIVALENT>
    </EVENT>
    </EVENTS>
    computer_à_toto-2005-06-09-09-36-41 became computer_à_toto-2006-08-17-11-37-35 on 2006-10-16 09:43:52.

## clear_history_V1(begin, num)

Same as get_history_V1, but reset number of events asked.

You must test SOAP error "Fault" to ensure all is OK. Returns true otherwise.

## reset_checksum_V1(checksum, IDs)

Set checksum for computers where list of IDs are provided in parameter.

Checksum must be value to set, not mask to compute! Returns true if OK, otherwise "Fault".

## Sample client

Here is an example of a Perl script which invokes the Web Service:

    #!/usr/bin/perl -s

    use SOAP::Lite;
    use XML::Entities;

    # Parameters
    # -s='' 			: server to query
    # -u=''				: user to authenticate
    # -pw=''			: user's password
    # -params='...,...,...,...' 	: Method's args
    # -proto='http|https'		: Transport protocol
    #
    # get_computers V1 secific parameters (enable you to easily modify XML values)
    # -o=''				: offset value (to iterate if whome result is upper than OCS_OPT_WEB_SERVICE_RESULTS_LIMIT (see ocsinventory-server.conf)
    # -c=''				: checksum to compare with
    # -w=''				: same principle than checksum but for other sections (dico_soft and accountinfos for the moment)
    # -t=''				: type (META || INVENTORY)) See web service documentation
    #
    # Checksum decimal values
    #'hardware'      => 1,
    #'bios'          => 2,
    #'memories'      => 4,
    #'slots'         => 8,
    #'registry'      => 16,
    #'controllers'   => 32,
    #'monitors'      => 64,
    #'ports'         => 128,
    #'storages'      => 256,
    #'drives'        => 512,
    #'inputs'        => 1024,
    #'modems'        => 2048,
    #'networks'      => 4096,
    #'printers'      => 8192,
    #'sounds'        => 16384,
    #'videos'        => 32768,
    #'softwares'     => 65536

    $s = $s||'localhost';
    $u = $u||'';
    $pw = $pw||'';
    $proto = $proto||'http';
    @params = split ',', $params;
    $f = $f||get_computers_V1;

    # You can modify some XML tags
    $c = $c||131071;
    $t=$t||"META";
    $o=$o||0;
    $w=defined $w?$w:131071;

    if( !defined(@params) && $f eq 'get_computers_V1' ){
      @params=(<<EOF);

    <REQUEST>
      <ENGINE>FIRST</ENGINE>
      <ASKING_FOR>$t</ASKING_FOR>
      <CHECKSUM>$c</CHECKSUM>
      <OFFSET>$o</OFFSET>
      <WANTED>$w</WANTED>
    </REQUEST>

    EOF
    }

    print "Launching soap request to proxy:\n";
    print "$proto://$u".($u?':':'').($u?"$pw\@":'')."$s/ocsinterface\n";

    print "Function: <$f>\n";
    $i++, print "Function Arg $i: `$_'\n" for @params;
    print "\nIn progress... \n\n";


    $lite = SOAP::Lite
      ->uri("$proto://$s/Apache/Ocsinventory/Interface")
      ->proxy("$proto://$u".($u?':':'').($u?"$pw\@":'')."$s/ocsinterface\n")
      ->$f(@params);

    if($lite->fault){
      print "ERROR:\n\n",XML::Entities::decode( 'all', $lite->fault->{faultstring} ),"\nEND\n\n";
    }
    else{
      my $i = 0;
      for( $lite->paramsall ){
        print "===== RESULT $i ===== \n".XML::Entities::decode( 'all', $_ )."\n";
        $i++;
      }
    }