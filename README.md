# check_Mikrotik_OS
Python3 plugin to Check Mikrotik firmware and os versions on routerboard devices.

This script has been tested on RB2011L & RB750GL.

Requirements Setup & Testing
----------------------

This Nagios plug-in uses python 3 with the following module dependencies:

1. argparse
2. pysnmp
3. packaging

SNMP must be enabled on the routerboard device to allow read access from the Nagios host.  

Only SNMP v2c is currently supported.   I might be tempted to implement SNMP v3 on request.

The nagios server must have access to the internet (specifically upgrade.mikrotik.com) in order to retrieve the latest version.

Prior to configuring within Nagios is recommended you verify the script works from the command line first.

The script must always be supplied with the following options:

  -H, –hostIP    Mikrotik Router IP Address
  -v, --snmpVersion SNMP Version (only 2c currently supported)
  -C, --snmpCommunity SNMP Community String (only v1 and 2c)
  -c, --mikrotikReleaseChannel 
                        The mikrotik release channel to check for updates
                        against: Bugfix, Current or ReleaseCandidate

Example Command Line
-----------------------

./check_Mikrotik_OS.py -H 192.168.1.1 -v 2c -C public -c Current 


Example Nagios Configuration
------------------------------


define command{
        command_name    Mikrotik_Version
        command_line    /usr/local/MyNagiosPlugins/check_Mikrotik_OS.py -H $HOSTADDRESS$ -v $ARG1$ -C $ARG2$ -c $ARG3$
        }


define service{
        use                    generic-service
        host_name              router1, router2
        service_description     Mikrotik Router OS And Firmware Version
        check_command           Mikrotik_Version!2c!public!Current
        }
