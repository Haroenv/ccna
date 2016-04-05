---
title: RIP
order: 7
---

## Between networks with no subinterfaces

If you want to make sure networks can communicate with eachother you'll have to configure RIP. [^2]

Going into RIP stance on router  | `router rip`
Choicing RIP version | `version 2`
Disable auto-summary | `no auto-summary`
Putting RIP for a | `network xxx.xxx.xxx.x` ( network 172.16.30.0 )

## Between networks with subinterfaces

If you're having a whole subnetworking ( vlan's ) on one end of your router you'll need some more settings

Making your port a passive interface  | `passive-interface gigabitEthernet0/*`
Going to surtain subinterface | `Interface g0/*.*` ( for example int gigabitEthernet0/0.10, using vlan 10 )
Making vlan able to send through | `encapsulation dot1Q *` ( for encapsulation dot1Q 10, for vlan 10 )
Connecting network | `ip address xxx.xxx.xxx.xxx xxx.xxx.xxx.xxx` ( for example ip address 172.16.30.254 255.255.255.0 )
