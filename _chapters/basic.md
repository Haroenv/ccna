---
title: Basic configuration
order: 1
---

<section>

### Reset devices

<section>

#### Router

Enter these commands in [privileged EXEC mode](#command-modes) to reset the router.This will delete the startup configuration and restart the router respectively.

```
erase startup-config
reload
```

</section>
<section>

#### Switch
The process for a switch is almost identical. Enter these commands in [privileged EXEC mode](#command-modes) to reset the switch.

```
erase startup-config
delete vlan.dat
reload
```

The `delete vlan.dat` command is necessary to delete the VLAN configuration.

</section>

</section>
<section>

### Save configuration

You can save the current configuration by copying it to the startup configuration as follows: `copy running-config startup-config` in [privileged EXEC mode](#privileged-exec-mode).

</section>
<section>

### Name device

You can name a cisco device by entering the `hostname [name]` command in [global configuration mode](#global-configuration-mode).

</section>
<section>

### Message of the day

You can set a message of the day using the `banner motd "[message]"` (or with the `"` as any identical character) command in [global configuration mode](#global-configuration-mode).

</section>
<section>

### Disable DNS-lookup

You can disable DNS-lookup using the `no ip domain-lookup` command in [global configuration mode](#global-configuration-mode).

</section>
<section>

### IP Address

You can set the IP address of any [interface](#interfaceline-configuration-mode), by entering `ip address [ip-address] [subnet mask]`. On a switch this is usually done on a certain VLAN, on a router this can be done in the `loopback [number]` interface. The default gateway is given by `ip default-gateway [ip-address]`.

</section>
