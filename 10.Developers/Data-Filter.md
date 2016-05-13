# Data Filter

Data Filter is a Perl module for OCS engine. Its goal is to filter inventory data by deleting the wanted
data directly in XML received by OCS server. The data will be deleted in XML just before storing inventory
in database. This module will be integrated in OCS 2.0 .

**`Warning: For the moment, Data Filter is only able to delete data from the hardware database table.
A more sophisticated Data filter will be integrated and deletion of any databae data will be possible.
The more sophisticated Data Filter module will only be OCS 2.0 compatible`**

## Source Code

This is the source of Data Filter :

    ################################################################################
    ## OCSINVENTORY-NG
    ## Copyleft Pascal DANEK 2005
    ## Web : http://www.ocsinventory-ng.org
    ##
    ## This code is open source and may be copied and modified as long as the source
    ## code is always made freely available.
    ## Please refer to the General Public Licence http://www.gnu.org/ or Licence.txt
    ################################################################################

    package Apache::Ocsinventory::Server::Capacities::Datafilter;

    use strict;

    # This block specify which wrapper will be used ( your module will be compliant with all mod_perl versions )
    BEGIN{
      if($ENV{'OCS_MODPERL_VERSION'} == 1){
        require Apache::Ocsinventory::Server::Modperl1;
        Apache::Ocsinventory::Server::Modperl1->import();
      }elsif($ENV{'OCS_MODPERL_VERSION'} == 2){
        require Apache::Ocsinventory::Server::Modperl2;
        Apache::Ocsinventory::Server::Modperl2->import();
      }
    }

    # These are the core modules you must include in addition
    use Apache::Ocsinventory::Server::System;
    use Apache::Ocsinventory::Server::Communication;
    use Apache::Ocsinventory::Server::Constants;

    use Apache::Ocsinventory::Map;

    # Initialize option
    push @{$Apache::Ocsinventory::OPTIONS_STRUCTURE},{
      'NAME' => 'DATAFILTER',
      'HANDLER_PROLOG_READ' => undef, #or undef # Called before reading the prolog
      'HANDLER_PROLOG_RESP' => undef, #or undef # Called after the prolog response building
      'HANDLER_PRE_INVENTORY' => \&datafilter_pre_inventory, #or undef # Called before reading inventory
      'HANDLER_POST_INVENTORY' => undef, #or undef # Called when inventory is stored without error
      'REQUEST_NAME' => undef,  #or undef # Value of <QUERY/> xml tag
      'HANDLER_REQUEST' => undef, #or undef # function that handle the request with the <QUERY>'REQUEST NAME'</QUERY>
      'HANDLER_DUPLICATE' => undef,#or undef # Called when a computer is handle as a duplicate
      'TYPE' => OPTION_TYPE_SYNC, # or OPTION_TYPE_ASYNC ASYNC=>with pr without inventory, SYNC=>only when inventory is required
      'XML_PARSER_OPT' => {
          'ForceArray' => ['']
      }
    };

    sub datafilter_pre_inventory{

      my $current_context = shift;
      my $xml = $current_context->{'XML_ENTRY'};
      my $apache = $current_context->{'APACHE_OBJECT'};

      if ($ENV{'OCS_OPT_DATA_FILTER'}) {
        my ($map_section, $multi_sections, $field, $mask);

        #Geting table and field from configuration file
        my %DATA_TO_FILTER = $apache->dir_config->get('OCS_OPT_DATA_TO_FILTER');

        for my $section ( keys %DATA_TO_FILTER) {
          $map_section = lc $section;
          $field = $DATA_TO_FILTER{$section};

          #Deleting data from XML
          unless ($DATA_MAP{$map_section}{multi}) {
            delete $xml->{'CONTENT'}->{$section}->{$field} if $xml->{'CONTENT'}->{$section}->{$field};
            &_log(1,'datafilter',"$section $field field deleted") if $ENV{'OCS_OPT_LOGLEVEL'};;
          }
        }
      }
      return INVENTORY_CONTINUE;
    }

    1;

## Installation

### **Create the module file**

To install Data Filter, paste the source code in a file named _Datafilter.pm_.

### **Copy the module file**

After creating _Datafilter.pm_, copy it in the _Capacities_ folder in your OCS server. This directory
is located in your Perl installation directory. Under Linux distribution, this directory is often
_/usr/local/share/perl/5.10.1/Apache/Ocsinventory/Server/Capactities_
(or _/usr/local/share/perl/5.8.8/Apache/Ocsinventory/Server/Capactities_).

    cp Datafiler.pm /usr/local/share/perl/5.10.1/Apache/Ocsinventory/Server/Capactities

### **Modify ocsinvnentory-server.conf**

Modify your _ocsinventory-server.conf_ (or _z-ocsinventory-server.conf_) to include the new
configuration lines needed by OCS server.

Add the following lines before the # ===== DEPRECATED ===== line :

    # ===== DATA FILTER =====

      #Enable the dat filtering capacity
      PerlSetEnv OCS_OPT_DATA_FILTER  1

      # Set the table names and the field associated you want to filter
      PerlAddVar OCS_OPT_DATA_TO_FILTER TABLE
      PerlAddVar OCS_OPT_DATA_TO_FILTER FIELD

Add the following line just after the _PerlModule Apache::Ocsinventory::Server::Capacities::Notify_ line :

    PerlModule Apache::Ocsinventory::Server::Capacities::Datafilter

### **Restart apache**

To activate the module, OCS server need to be restarted :

    /etc/init.d/apache2

(for Debian based distributions)

    /etc/init.d/httpd restart

(for Redhat based distributions)

## Configure the data to filter

To set the data we want to filter, go in your _ocsinventory-server.conf_ (or _z-ocsinventory-server.conf_)
and set the _OCS_OPT_DATA_TO_FILTER_ parameters. This parameters go by pair. The first parameter
is the database table name associated to the data and the second is the database field we want to delete.

For example, if you want to delete the username in inventory, set _OCS_OPT_DATA_TO_FILTER_ like this :

    PerlAddVar OCS_OPT_DATA_TO_FILTER HARDWARE
    PerlAddVar OCS_OPT_DATA_TO_FILTER USERID

Your Apache server need to be restarted to take care of your modifications :

    /etc/init.d/apache2

(for Debian based distributions)

    /etc/init.d/httpd restart

(for Redhat based distributions)