Copyright &copy; 2013-2014 Max Oberberger

This files are free software: you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation, either version 3 of the License, or
(at your option) any later version.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the
GNU General Public License for more details.

You should have received a copy of the GNU General Public License
along with this program. If not, see <http://www.gnu.org/licenses/>.

* * *

Table of Contents
=================
1. [Introduction]()
2. [Installation/Configuration]()


1. Introduction
=================
Customized scripts to use with icinga monitoring tool on debian. These scripts
should be stored into the nagios plugin folder (/usr/lib/nagios/plugins) of the
hosts you want to monitor. They will be executed via nrpe daemon.


2. Installation/Configuration
=================
## Server side

Define the service for a specific node (HOSTNAME) at his config file `/etc/icinga/objects/HOSTNAME_icinga.cfg`:

    define service{
            use                             generic-service         ; Name of service template to use
            host_name                       HOSTNAME 
            service_description             Jenkins
            check_command                   check_nrpe_1arg!check_NAME
            }

## Client side
On the client add the script to the default folder `/usr/lib/nagios/plugins` and
make sure it has executable permissions (`chmod a+x`).

Add the new command to nrpe-config (`/etc/nagios/nrpe.cfg`):

    command[check_NAME]=/usr/lib/nagios/plugins/check_NAME

and restart nrpe daemon:

    sudo service nagios-nrpe-server restart

## Jenkins

Jenkins plugin uses `/etc/init.d/jenkins status` to get the status of the service.
To enable this call for nagios user add following line to `/etc/sudoers`:

    nagios ALL=NOPASSWD:/etc/init.d/jenkins status
