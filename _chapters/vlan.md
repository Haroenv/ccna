---
title: VLAN configuration
order: 6
---

A VLAN is a virtual local area network, which is defined by a switch. This can be used to make sure that different devices don't have access to devices with for example a different set of permissions.

## Making a new VLAN

in [global configuration mode](#global-configuration-mode), you can make a new VLAN using the command `vlan {number}`. After that you give it the name using `name {the name of the VLAN}`; the state to active and don't shut down.

```
state active
no shutdown
```

> TO DO: everything to configure a VLAN

## VLAN Trunking

In some cases you might want devices to be on the same VLAN, even though they aren't connected to the same Switch. You can solve this by adding a router to one of the switches and setting up a VLAN trunk like this:
For adding a VLAN trunk you need to go to an interface, for example f0/24.

Adding mode trunk | `switchport mode trunk`
Adding allowed vlan's | `switchport trunk allowed vlan *`
Adding native vlan's | `switchport trunk native vlan `

Example code:

```
int f0/24
switchport mode trunk
switchport trunk allowed vlan 10,20,30,40
switchport trunk native vlan 99
no shutdown
```

## Port-Security

In some cases you want your device to be secure from overloading. For example you bandwidth, you want to make sure that the connections on your port has some bandwidth. Because if you have 100 devices on one port, the bandwidth has to be shared. With only 1 device on your port you have the whole bandwidth for that device. Another reason would be the Availability of the port, because if 100 devices have to communicate through 1 port, you'll have a huge que of actions your port has to finish.

If you want a maximum of dynamic max-addresses you can use `switchport port-security maximum *` ( on * you can put a number ). If more addresses are detected, those will be deactivated.

`switchport port-security violation protect` | Drops all the packets from the insecure hosts at the port-security process level but does not increment the security-violation count.
`switchport port-security violation restrict` | Drops all the packets from the insecure hosts at the port-security process level and increments the security-violation count.
`switchport port-security violation shutdown` | Shuts down the port if there is a security violation.
