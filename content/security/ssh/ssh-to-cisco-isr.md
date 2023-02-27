+++
title = "SSH To Cisco devices"
weight = 1
+++


It looks like MacOS Ventura (13.2.1 in my case) uses OPenSSH_9.0p1 and LibreSSL 3.3.6. There are plenty of article here and there on the web but I wanted to do my version as well to document it.

``` bash
$ sw_vers            
ProductName:		macOS
ProductVersion:		13.2.1
BuildVersion:		22D68


$ /usr/bin/ssh -V   
OpenSSH_9.0p1, LibreSSL 3.3.6
```



When you try to connect to a legacy Cisco IOS you might have some issues during the SSH key exchange.


```bash
$ ssh nmichel@10.1.1.1

Unable to negotiate with 10.1.1.1 port 22: no matching key exchange method found. Their offer: diffie-hellman-group-exchange-sha1,diffie-hellman-group14-sha1
```

The error message is pretty straightforward but you can have more details using the following command and debug


``` bash

nmichel@vPackets-Mac-Mini.local:/Users/nmichel/code/Blogs/vPackets-blog git:(main*) $ ssh -vvvv admin@10.1.1.1
OpenSSH_9.0p1, LibreSSL 3.3.6
debug1: Reading configuration data /Users/nmichel/.ssh/config
debug3: kex names ok: [diffie-hellman-group1-sha1,]
debug3: kex names ok: [diffie-hellman-group14-sha1]
debug3: kex names ok: [diffie-hellman-group1-sha1]
debug1: Reading configuration data /etc/ssh/ssh_config
debug1: /etc/ssh/ssh_config line 21: include /etc/ssh/ssh_config.d/* matched no files
debug1: /etc/ssh/ssh_config line 54: Applying options for *
debug2: resolve_canonicalize: hostname 10.1.1.1 is address
debug3: expanded UserKnownHostsFile '~/.ssh/known_hosts' -> '/Users/nmichel/.ssh/known_hosts'
debug3: expanded UserKnownHostsFile '~/.ssh/known_hosts2' -> '/Users/nmichel/.ssh/known_hosts2'
debug1: Authenticator provider $SSH_SK_PROVIDER did not resolve; disabling
debug3: ssh_connect_direct: entering
debug1: Connecting to 10.1.1.1 [10.1.1.1] port 22.
debug3: set_sock_tos: set socket 3 IP_TOS 0x48
debug1: Connection established.
debug1: identity file /Users/nmichel/.ssh/id_rsa type -1
debug1: identity file /Users/nmichel/.ssh/id_rsa-cert type -1
debug1: identity file /Users/nmichel/.ssh/id_ecdsa type -1
debug1: identity file /Users/nmichel/.ssh/id_ecdsa-cert type -1
debug1: identity file /Users/nmichel/.ssh/id_ecdsa_sk type -1
debug1: identity file /Users/nmichel/.ssh/id_ecdsa_sk-cert type -1
debug1: identity file /Users/nmichel/.ssh/id_ed25519 type 3
debug1: identity file /Users/nmichel/.ssh/id_ed25519-cert type -1
debug1: identity file /Users/nmichel/.ssh/id_ed25519_sk type -1
debug1: identity file /Users/nmichel/.ssh/id_ed25519_sk-cert type -1
debug1: identity file /Users/nmichel/.ssh/id_xmss type -1
debug1: identity file /Users/nmichel/.ssh/id_xmss-cert type -1
debug1: identity file /Users/nmichel/.ssh/id_dsa type -1
debug1: identity file /Users/nmichel/.ssh/id_dsa-cert type -1
debug1: Local version string SSH-2.0-OpenSSH_9.0
debug1: Remote protocol version 1.99, remote software version Cisco-1.25
debug1: compat_banner: match: Cisco-1.25 pat Cisco-1.* compat 0x60000000
debug2: fd 3 setting O_NONBLOCK
debug1: Authenticating to 10.1.1.1:22 as 'admin'
debug3: record_hostkey: found key type RSA in file /Users/nmichel/.ssh/known_hosts:11
debug3: load_hostkeys_file: loaded 1 keys from 10.1.1.1
debug1: load_hostkeys: fopen /Users/nmichel/.ssh/known_hosts2: No such file or directory
debug1: load_hostkeys: fopen /etc/ssh/ssh_known_hosts: No such file or directory
debug1: load_hostkeys: fopen /etc/ssh/ssh_known_hosts2: No such file or directory
debug3: order_hostkeyalgs: prefer hostkeyalgs: rsa-sha2-512-cert-v01@openssh.com,rsa-sha2-256-cert-v01@openssh.com,rsa-sha2-512,rsa-sha2-256,ssh-rsa
debug3: send packet: type 20
debug1: SSH2_MSG_KEXINIT sent
debug3: receive packet: type 20
debug1: SSH2_MSG_KEXINIT received
debug2: local client KEXINIT proposal
debug2: KEX algorithms: sntrup761x25519-sha512@openssh.com,curve25519-sha256,curve25519-sha256@libssh.org,ecdh-sha2-nistp256,ecdh-sha2-nistp384,ecdh-sha2-nistp521,diffie-hellman-group-exchange-sha256,diffie-hellman-group16-sha512,diffie-hellman-group18-sha512,diffie-hellman-group14-sha256,diffie-hellman-group1-sha1,ext-info-c
debug2: host key algorithms: rsa-sha2-512-cert-v01@openssh.com,rsa-sha2-256-cert-v01@openssh.com,rsa-sha2-512,rsa-sha2-256,ssh-rsa,ssh-ed25519-cert-v01@openssh.com,ecdsa-sha2-nistp256-cert-v01@openssh.com,ecdsa-sha2-nistp384-cert-v01@openssh.com,ecdsa-sha2-nistp521-cert-v01@openssh.com,ssh-ed25519,ecdsa-sha2-nistp256,ecdsa-sha2-nistp384,ecdsa-sha2-nistp521
debug2: ciphers ctos: aes128-ctr,aes192-ctr,aes256-ctr,aes128-cbc,3des-cbc
debug2: ciphers stoc: aes128-ctr,aes192-ctr,aes256-ctr,aes128-cbc,3des-cbc
debug2: MACs ctos: umac-64-etm@openssh.com,umac-128-etm@openssh.com,hmac-sha2-256-etm@openssh.com,hmac-sha2-512-etm@openssh.com,hmac-sha1-etm@openssh.com,umac-64@openssh.com,umac-128@openssh.com,hmac-sha2-256,hmac-sha2-512,hmac-sha1
debug2: MACs stoc: umac-64-etm@openssh.com,umac-128-etm@openssh.com,hmac-sha2-256-etm@openssh.com,hmac-sha2-512-etm@openssh.com,hmac-sha1-etm@openssh.com,umac-64@openssh.com,umac-128@openssh.com,hmac-sha2-256,hmac-sha2-512,hmac-sha1
debug2: compression ctos: none,zlib@openssh.com,zlib
debug2: compression stoc: none,zlib@openssh.com,zlib
debug2: languages ctos:
debug2: languages stoc:
debug2: first_kex_follows 0
debug2: reserved 0
debug2: peer server KEXINIT proposal
debug2: KEX algorithms: diffie-hellman-group-exchange-sha1,diffie-hellman-group14-sha1
debug2: host key algorithms: ssh-rsa
debug2: ciphers ctos: aes256-ctr
debug2: ciphers stoc: aes256-ctr
debug2: MACs ctos: hmac-sha2-512
debug2: MACs stoc: hmac-sha2-512
debug2: compression ctos: none
debug2: compression stoc: none
debug2: languages ctos:
debug2: languages stoc:
debug2: first_kex_follows 0
debug2: reserved 0
debug1: kex: algorithm: (no match)
Unable to negotiate with 10.1.1.1 port 22: no matching key exchange method found. Their offer: diffie-hellman-group-exchange-sha1,diffie-hellman-group14-sha1
```


We can see that the Cisco Routers is offering 

```bash
KEX algorithms: diffie-hellman-group-exchange-sha1,diffie-hellman-group14-sha1 
```

while our local machine offers

```bash
 KEX algorithms: sntrup761x25519-sha512@openssh.com,curve25519-sha256,curve25519-sha256@libssh.org,ecdh-sha2-nistp256,ecdh-sha2-nistp384,ecdh-sha2-nistp521,diffie-hellman-group-exchange-sha256,diffie-hellman-group16-sha512,diffie-hellman-group18-sha512,diffie-hellman-group14-sha256,diffie-hellman-group1-sha1,ext-info-c
 ```


In this case, you might want to enable the ssh algorithms asked by the Cisco routers. (I haven't checked what are the latest KexAlgorithm supported by the latest IOS.... Might be a good opportunity to update the router.)

It is initially possible to influence the algorith used by openssh using the command line 

``` bash
ssh -oKexAlgorithms=diffie-hellman-group14-sha1 admin@10.1.1.1
```

This method is not really scalable and doesn't really work for me as I am using [RoyalTSX](https://royalapps.com/ts/mac/features) to manage my SSH connections.

So I prefer to tackle the problem in a different way and [OpenSSH](https://www.openssh.com) offers a way to have a more granular configuration for your hosts, you can create [an SSH config file for your OpenSSH client](https://www.ssh.com/academy/ssh/config).

You can either create a global config using /etc/ssh/ssh_config or create a more user-centric configuration: ~/.ssh/config 

Here is a working configuration:


``` bash

# settings for all hosts
HostkeyAlgorithms +ssh-rsa
KexAlgorithms +diffie-hellman-group14-sha1,
Ciphers aes128-ctr,aes192-ctr,aes256-ctr,aes128-cbc,3des-cbc


# host specific settings
Host c1111
HostName 10.1.1.1
KexAlgorithms +diffie-hellman-group14-sha1
user nmichel

Host c3560
HostName 10.1.1.1
KexAlgorithms +diffie-hellman-group1-sha1
````


Now I can initiate an SSH connection towards my "legacy" devices.... Might be a good opportunity to update them :) 


