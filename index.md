---
layout: default
---
{::options parse_block_html="true" /}
<div class="aside">
<div class="aside-content">
* #markdown-toc
{:toc}

*[VLAN]: Virtual Local Area Network
*[IOS]: Internetwork Operating System
*[RIP]: Routing Information Protocol

# {{ site.title }}

By [Haroen Viaene](https://github.com/haroenv) and [Elias Meire](https://github.com/eliasmeire).

Fork this on [GitHub](https://github.com/haroenv/ccna-summary/blob/gh-pages/index.md) to add info.

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

## IP Address

You can set the IP address of any [interface](#interfaceline-configuration-mode), by entering `ip address [ip-address] [subnet mask]`. On a switch this is usually done on a certain VLAN, on a router this can be done in the `loopback [number]` interface. The default gateway is given by `ip default-gateway [ip-address]`.

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

To execute a command that's usually only available from privileged EXEC or User EXEC mode, like `show ip interface brief`, you have to preced it by `do`. [^1]

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
Show cdp neighbors | `show cdp neighbors`
Show list of trunked ports | `show interface trunk`
Show list of settings | `show run`
Show port-security | `show port-security`
Shop RIP database | `show ip rip database`

There are additional filters and arguments available for all these functions.

# VLAN configuration

A VLAN is a virtual local area network, which is defined by a switch. This can be used to make sure that different devices don't have access to devices with for example a different set of permissions.

## Making a new VLAN

in [global configuration mode](#global-configuration-mode), you can make a new VLAN using the command `vlan {number}`. After that you give it the name using `name {the name of the VLAN}`; the state to active and don't shut down.

~~~
state active
no shutdown
~~~

> TO DO: everything to configure a VLAN

## VLAN Trunking

In some cases you might want devices to be on the same VLAN, even though they aren't connected to the same Switch. You can solve this by adding a router to one of the switches and setting up a VLAN trunk like this:
For adding a VLAN trunk you need to go to an interface, for example f0/24.

Adding mode trunk | `switchport mode trunk`
Adding allowed vlan's | `switchport trunk allowed vlan *`
Adding native vlan's | `switchport trunk native vlan `

Example code:

~~~
int f0/24
switchport mode trunk
switchport trunk allowed vlan 10,20,30,40
switchport trunk native vlan 99
no shutdown
~~~

## Port-Security

In some cases you want your device to be secure from overloading. For example you bandwidth, you want to make sure that the connections on your port has some bandwidth. Because if you have 100 devices on one port, the bandwidth has to be shared. With only 1 device on your port you have the whole bandwidth for that device. Another reason would be the Availability of the port, because if 100 devices have to communicate through 1 port, you'll have a huge que of actions your port has to finish.

If you want a maximum of dynamic max-addresses you can use `switchport port-security maximum *` ( on * you can put a number ). If more addresses are detected, those will be deactivated. 

`switchport port-security violation protect`
Drops all the packets from the insecure hosts at the port-security process level but does not increment the security-violation count.
`switchport port-security violation restrict`
Drops all the packets from the insecure hosts at the port-security process level and increments the security-violation count.
`switchport port-security violation shutdown`
Shuts down the port if there is a security violation.

## RIP between networks with no subinterfaces

If you want to make sure networks can communicate with eachother you'll have to configure RIP. [^2]

Going into RIP stance on router  | `router rip`
Choicing RIP version | `version 2`
Disable auto-summary | `no auto-summary`
Putting RIP for a | `network xxx.xxx.xxx.x` ( network 172.16.30.0 )

## RIP between networks with subinterfaces

If you're having a whole subnetworking ( vlan's ) on one end of your router you'll need some more settings

Making your port a passive interface  | `passive-interface gigabitEthernet0/*`
Going to surtain subinterface | `Interface g0/*.*` ( for example int gigabitEthernet0/0.10, using vlan 10 )
Making vlan able to send through | `encapsulation dot1Q *` ( for encapsulation dot1Q 10, for vlan 10 )
Connecting network | `ip address xxx.xxx.xxx.xxx xxx.xxx.xxx.xxx` ( for example ip address 172.16.30.254 255.255.255.0 )

# Footnotes

[^1]: <https://supportforums.cisco.com/document/31051/executing-show-commands-global-configuration-mode>
[^2]: <http://www.9tut.com/rip-routing-protocol-tutorial>
</div>
