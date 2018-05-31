**`Warning: This documentation is for the 2.0 Unix OCS Agent. It won't work with 1.1.x Unix OCS Agent.`**

# Presentation

You can easily extend the inventoried information thank to the modular architecuture of OCS. You'll
just need a basic Perl knowledge.

The _backend module_ are Perl module localized in Ocsinventory-Agent-XXX/lib/Ocsinventory/Agent/Backend.
Your new backend module will be autodetected by the agent during it's start up.

    # The complet name of the package
    # the path MUST be valid or the package won't be loaded
    package Ocsinventory::Agent::Backend::OS::Linux;

    use strict;

    # I need to declare $runAfter because of the strict mode
    use vars qw($runAfter);

    # The package must be run after OS::Generic
    $runAfter = ["Ocsinventory::Agent::Backend::OS::Generic"];

    # This is the check function. The agent runs it just once the module is loaded.
    # If the function return false, the module and its children are not executed
    # eg: OS::Linux and OS::Linux::* won't executed if this run() function return
    # false
    # It's a easy way to target a module just for a system like Linux in this case.
    sub check { $^O =~ /^linux$/ }

    # its the main function of the script, it's called during the hardware inventory
    sub run {
      my $params = shift; # I fetch the parameter

    # I need to get the common object to acces to the differents functions for inventory
      my $common = $params->{common};

      chomp (my $osversion = `uname -r`);
    # I get the OS version

    # and I updated the information collected
      $common->setHardware({
          OSNAME => "Linux",
          OSCOMMENTS => "Unknow Linux distribution",
          OSVERSION => $osversion
        });
    }

    1;

# Some remarks

You can start the agent directly without any installation. You'll just need the dependancy installed:

    $gunzip Ocsinventory-Agent-XXX.tar.gz
    $tar xf Ocsinventory-Agent-XXX.tar
    $cd Ocsinventory-Agent-XXX
    $./ocsinventory-agent --devlib

If you want to create a module, use the _--debug_ flag and turn of the server mode
with _--local /tmp_ or _--stdout_

This page could also be tagged: "custom XML", "extend ocsinventory-agent"