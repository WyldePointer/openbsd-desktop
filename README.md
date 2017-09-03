
### Step 0: Updating the default installation

This is similar to what you do in Debian-ish distributions via
 `apt-get update && apt-get upgrade`: [1]

```sh
# echo "https://ftp.openbsd.org/pub/OpenBSD" >> /etc/installurl
# syspatch
```

What it does is simply getting the list of patches(or updates, if you prefer)
 and then downloading and applying them accordingly. 

It's highly recommended to "patch your shit"(R)(TM) before anything else and
 if you prefer to see what you're going to download / apply, you can visit
<a href="https://www.openbsd.org/errata61.html" target="_blank">https://www.openbsd.org/errata61.html</a>
 or simply add the `-c` option to your `syspatch` utility:

```sh
# syspatch -c                                                                  
001_dhcpd
002_vmmfpu
003_libressl
004_softraid_concat
005_pf_src_tracking
006_libssl
007_freetype
008_exec_subr
009_icmp_opts
010_perl
012_wsmux
013_icmp6_linklocal
014_libcrypto
015_sigio
016_sendsyslog
017_fuse
018_recv
019_tcp_usrreq
020_sockaddr
021_ptrace
022_fcntl
023_wsdisplay
024_sosplice
025_ieee80211
026_smap
027_net80211_replay
```

### Step 1: Installing the XFCE

After rebooting the machine, it's time to install the XFCE packages:

```sh
# pkg_add xfce xfce-extras consolekit2
```

### Step 2: Enabling services

Append the following lines to your `/etc/rc.conf.local`:

```
multicast_host=YES
pkg_scripts="messagebus"
```

### Step 3: Enabling XFCE

As a user(not the root), we should enable the XFCE in our `~/.xsession` file:
 (`$ vi ~/.xsession`)

```
exec ck-launch-session startxfce4
```

Done! Reboot your machine and it should work like a charm.

Special thanks goes to **Keith Burnett** for his straightforward guide at
<a href="http://sohcahtoa.org.uk/openbsd.html" target="_blank">http://sohcahtoa.org.uk/openbsd.html</a>

<br />
<br />
<hr />

[1] The `echo` command is not mandatory for the next runs.

[2] Legacy way: (`# vi /root/pkg_add.sh`)

```sh
#!/bin/sh
#export PKG_PATH=https://ftp.openbsd.org/pub/OpenBSD/`uname -r`/packages/`uname -m`/
pkg_add $*
```
(The `export` line is commented out because we've already created the
 `/etc/installurl` and it's just there due to historical reasons and backward
 compatibility with the versions prior to `6.1`.)

After a `# chmod +x /root/pkg_add.sh`, we should be able to install our packages
 using `/root/pkg_add.sh name_of_the_package`:

```sh
# /root/pkg_add.sh git
```
