---
title: Management interfaces
order: 3
---

<section>

### Console

To enable a password for a login through the console port, enter in [global configuration mode](#global-configuration-mode):

```
line con 0
 password [password]
 login
 logging synchronous
```

</section>
<section>

### Telnet

To configure telnet (vty or virtual terminal) with password protection, enter in [global configuration mode](#global-configuration-mode):

```
line vty 0 15
 password [password]
 login
```

</section>
<section>

### SSH

To configure SSH (Secure SHell) with password protection, enter in [global configuration mode](#global-configuration-mode):

```
crypto key generate RSA general-keys modulus 1024
ip domain-name [your-domain.com]
username [username] privilege 15 secret [password]
line vty 0 15
 transport input ssh
 login local
```

This will automatically disable telnet connections on the vty lines, if you still want to allow telnet connections enter `transport input ssh telnet` instead of `transport input ssh`.

You can do some extra SSH-specific configuration in [global configuration mode](#global-configuration-mode):

```
ip ssh version [ssh-version-number]
ip ssh time-out [seconds]
ip ssh authentication-retries [retries-number]
```

</section>
<section>

### Security

Password protection on management interfaces is always recommended but extra security measures can be taken.

<section>

#### Password restrictions
You can force extra restrictions on passwords by entering these commands in [global configuration mode](#global-configuration-mode):

```
login block-for [seconds-blocked] attempts [attempts-number] within [seconds]
security password min-length [minimum-chars]
```

The first line will block access for [seconds-blocked] if the user attemps to login [attempts-number] times within [seconds]. The second line restricts password length to [minimum-chars] or longer.

</section>
<section>

#### Password encryption

To enable password encryption, enter `service password-encryption` in [global configuration mode](#global-configuration-mode). If you do not enable password encryption all password will be stored in clear text in the configuration file.

</section>
<section>

#### Timeout
You can add a timeout for inactive users. To do this, in the [line configuration mode](#interfaceline-configuration-mode) of the line you want to configure, enter: `exec-timeout [minutes]`

</section>
<section>
<section>
