---
title: Command modes
order: 2
---

Cisco networking devices have several command modes, each of them has a different command set, to go back to a lower (less privileged) command mode use the `exit` command.


<section>

### User EXEC Mode

After you access the device, you are automatically in user EXEC command mode. The EXEC commands available at the user level are a subset of those available at the privileged level. This is the least privileged command mode. User EXEC mode is denoted by a `>`.

</section>
<section>

### Privileged EXEC Mode

The privileged command set includes those commands contained in user EXEC mode as well as commands that configure operating parameters. Privileged access should be password-protected to prevent unauthorised use, you can achieve this by entering the `enable secret [password]` command in [global configuration mode](#global-configuration-mode). It is highly recommend to also enable [password encryption](#password-encryption). Privileged EXEC mode is denoted by a `#`.

To access privileged EXEC mode, enter the `enable` command from user EXEC mode.

</section>
<section>

### Global Configuration Mode

Configuration mode commands apply to features that affect the device as a whole. Global configuration mode is denoted by `(config)#`.

From privileged EXEC mode you can reach global configuration mode by entering the `configure terminal` command.

To execute a command that's usually only available from privileged EXEC or User EXEC mode, like `show ip interface brief`, you have to preced it by `do`. [^1]

</section>
<section>

### Interface/Line Configuration Mode

Interface/Line configuration mode commands let you configure specific interfaces/lines on the router. Interface configuration mode is denoted by `(config-if)#` and line configuration mode by `(config-line)#`.

From global configuration mode you can reach interface configuration mode by entering the `interface [interface-name]` command. Similarly to reach line configuration mode you can enter the `line [line-name]` command.

You can go back to the privileged EXEC mode from this mode by entering the command `end`.

</section>
