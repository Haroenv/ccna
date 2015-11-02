---
layout: default
---
{::options parse_block_html="true" /}
<div class="aside">
<div class="aside-content">
* #markdown-toc
{:toc}

*[VLAN]: Virtual Local Area Network

# {{ site.title }}

By [Haroen Viaene](https://github.com/haroenv) and [Elias Meire](https://github.com/eliasmeire).

Fork this on [GitHub](https://github.com/haroenv/ccna-summary)

<ins class="adsbygoogle" style="display:block" data-ad-client="ca-pub-8125962319145805" data-ad-slot="3782140779" data-ad-format="auto"></ins>

</div>
</div>

{::options parse_block_html="true" /}
<div class="content">

# Basic configuration

## Reset device

### Router

Enter these commands in [privileged EXEC mode](#command-modes) to reset the router.This will delete the startup configuration and restart the router respectively.

~~~
erase startup-config
reload
~~~

### Switch
The process for a switch is almost identical. Enter these commands in [privileged EXEC mode](#command-modes) to reset the switch.

~~~
erase startup-config
delete vlan.dat
reload
~~~

The `delete vlan.dat` command is necessary to delete the VLAN configuration.

## Save configuration

You can save the current configuration by copying it to the startup configuration as follows: `copy running-config startup-config` in [privileged EXEC mode](#privileged-exec-mode).

## Name device

You can name a cisco device by entering the `hostname [name]` command in [global configuration mode](#global-configuration-mode).

## Message of the day

You can set a message of the day using the `banner motd "[message]"` (or with the `"` as any identical character) command in [global configuration mode](#global-configuration-mode).

## Disable DNS-lookup

You can disable DNS-lookup using the `no ip domain-lookup` command in [global configuration mode](#global-configuration-mode).

## ip-address

You can set the ip address of any [interface](#interfaceline-configuration-mode), by entering `ip address [ip-address] [subnet mask]`. On a switch this is usually done on a certain VLAN, on a router this can be done in the `loopback [number]` interface. The default gateway is given by `ip default-gateway [ip-address]`.

# Command modes

Cisco networking devices have several command modes, each of them has a different command set, to go back to a lower (less privileged) command mode use the `exit` command.

## User EXEC Mode

After you access the device, you are automatically in user EXEC command mode. The EXEC commands available at the user level are a subset of those available at the privileged level. This is the least privileged command mode. User EXEC mode is denoted by a `>`.

## Privileged EXEC Mode

The privileged command set includes those commands contained in user EXEC mode as well as commands that configure operating parameters. Privileged access should be password-protected to prevent unauthorised use, you can achieve this by entering the `enable secret [password]` command in [global configuration mode](#global-configuration-mode). It is highly recommend to also enable [password encryption](#password-encryption). Privileged EXEC mode is denoted by a `#`.

To access privileged EXEC mode, enter the `enable` command from user EXEC mode.

## Global Configuration Mode

Configuration mode commands apply to features that affect the device as a whole. Global configuration mode is denoted by `(config)#`.

From privileged EXEC mode you can reach global configuration mode by entering the `configure terminal` command.

## Interface/Line Configuration Mode

Interface/Line configuration mode commands let you configure specific interfaces/lines on the router. Interface configuration mode is denoted by `(config-if)#` and line configuration mode by `(config-line)#`.

From global configuration mode you can reach interface configuration mode by entering the `interface [interface-name]` command. Similarly to reach line configuration mode you can enter the `line [line-name]` command.

You can go back to the privileged EXEC mode from this mode by entering the command `end`.

# Management interfaces

## Console

To enable a password for a login through the console port, enter in [global configuration mode](#global-configuration-mode):

~~~
line con 0
 password [password]
 login
 logging synchronous
~~~

## Telnet

To configure telnet (vty or virtual terminal) with password protection, enter in [global configuration mode](#global-configuration-mode):

~~~
line vty 0 15
 password [password]
 login
~~~

## SSH

To configure SSH (Secure SHell) with password protection, enter in [global configuration mode](#global-configuration-mode):

~~~
crypto key generate RSA general-keys modulus 1024
ip domain-name [your-domain.com]
username [username] privilege 15 secret [password]
line vty 0 15
 transport input ssh
 login local
~~~

This will automatically disable telnet connections on the vty lines, if you still want to allow telnet connections enter `transport input ssh telnet` instead of `transport input ssh`.

You can do some extra SSH-specific configuration in [global configuration mode](#global-configuration-mode):

~~~
ip ssh version [ssh-version-number]
ip ssh time-out [seconds]
ip ssh authentication-retries [retries-number]
~~~

## Security

Password protection on management interfaces is always recommended but extra security measures can be taken.

### Password restrictions
You can force extra restrictions on passwords by entering these commands in [global configuration mode](#global-configuration-mode):

~~~
login block-for [seconds-blocked] attempts [attempts-number] within [seconds]
security password min-length [minimum-chars]
~~~

The first line will block access for [seconds-blocked] if the user attemps to login [attempts-number] times within [seconds]. The second line restricts password length to [minimum-chars] or longer.

### Password encryption

To enable password encryption, enter `service password-encryption` in [global configuration mode](#global-configuration-mode). If you do not enable password encryption all password will be stored in clear text in the configuration file.

### Timeout
You can add a timeout for inactive users. To do this, in the [line configuration mode](#interfaceline-configuration-mode) of the line you want to configure, enter: `exec-timeout [minutes]`

# Interface configuration

To enter a certain interface, you enter `interface [interface-name]` while in [global configuration mode](#global-configuration-mode). In that interface you can do the following things:

> TO DO: all options for all interfaces

# Debugging

The following `show` commands will let you monitor the device configuration.

Useful for | command
---|---
Show the entire running configuration | `show running-config`
Show the entire startup configuration | `show startup-config`
Show the IPv4/v6 configuration | `show ip(v6) interface`
Show the IPv4/v6 route | `show ip(v6) route`
Show the vlan configuration | `show vlan`
Show the MAC table | `show mac-address-table`
Show the IOS version | `show version`

There are additional filters and arguments available for all these functions.

# VLAN configuration

A VLAN is a virtual local area network, which is defined by a switch. This can be used to make sure that different devices don't have access to devices with for example a different set of permissions.

> TO DO: everything to configure a VLAN

## VLAN Trunking

In some cases you might want devices to be on the same VLAN, even though they aren't connected to the same Switch. You can solve this by adding a router to one of the switches and setting up a VLAN trunk like this:

> TO DO: commands needed

</div>