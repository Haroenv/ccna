---
layout: default
---
{::options parse_block_html="true" /}
<div class="aside">
<div class="aside-content">
* #markdown-toc
{:toc}

# {{ site.title }}

By [Haroen Viaene](https://github.com/haroenv) and [Elias Meire](https://github.com/eliasmeire).

Fork this on [GitHub](https://github.com/haroenv/ccna-summary)
</div>
</div>

{::options parse_block_html="true" /}
<div class="content">

# Reset device

## Router

Enter these commands in [privileged EXEC mode](#command-modes) to reset the router.This will delete the startup configuration and restart the router respectively.

~~~
erase startup-config
reload
~~~

## Switch
The process for a switch is almost identical. Enter these commands in [privileged EXEC mode](#command-modes) to reset the switch.

~~~
erase startup-config
delete vlan.dat
reload
~~~

The `delete vlan.dat` command is necessary to delete the VLAN configuration.

# Command modes

Cisco networking devices have several command modes, each of them has a different command set, to go back to a lower (less privileged) command mode use the `exit` command.

## User EXEC Mode

After you access the device, you are automatically in user EXEC command mode. The EXEC commands available at the user level are a subset of those available at the privileged level. This is the least privileged command mode.

## Privileged EXEC Mode

The privileged command set includes those commands contained in user EXEC mode as well as commands that configure operating parameters. [Privileged access should be password-protected](#passwords) to prevent unauthorised use.

To access privileged EXEC mode, enter the `enable` command from user EXEC mode.

## Global Configuration Mode

Configuration mode commands apply to features that affect the device as a whole.

From privileged EXEC mode you can reach global configuration mode by entering the `configure terminal` command.

## Interface Configuration Mode

Interface configuration mode commands let you configure specific interfaces on the router.

From global configuration mode you can reach interface configuration mode by entering the `interface [interface-name]` command.

You can go back to the privileged EXEC mode from this mode by entering the command `end`.

# Passwords

You can have two different passwords for any connection: one for the User EXEC mode, and one for the privileged EXEC mode.

## Privileged EXEC mode

In [global configuration mode](#global-configuration-mode): first enable password encryption by entering `service password-encryption`. Then you can enter the password for user EXEC mode like this: `enable secret [password]`.

## Console

To enable a password for a login through the console port, enter in [global configuration mode](#global-configuration-mode):

~~~
line con 0
 password [password]
 login
 logging synchronous
~~~

## Telnet

To enable a password for a login through a telnet connection (vty or virtual terminal), enter in [global configuration mode](#global-configuration-mode):

~~~
line vty 0 15
 password [password]
 login
~~~

## SSH

To enable a password for a login through an ssh connection (secure shell), enter in [global configuration mode](#global-configuration-mode):

~~~
ip domain-name [your-domain.com]
username [username] privilege 15 secret [password]
line vty 0 15
 transport input ssh
 login local
~~~

</div>