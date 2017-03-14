# Cloud-Init compatible cidata iso creator

Purpose: to configure a cloud-init compatible virtual machine on a "no-cloud" setup by using a "cidata" cdrom iso file.

+ Use one of the prebuilt vagrant-*.iso files to configure a instance to create a vagrant user with optional "vagrant" password.
    + This is also useful outside vagrant for any testing, because it enables a user to login the machine using a standard key/password 
    + the insecure default secret key of vagrant is available under vagrant.id_rsa
    + the insecure default password of vagrant is "vagrant"

+ Use your ssh publickey to create a cidata iso that configures the root user with your key.
    + Requisites for creating new iso files: bash, mkisoimage, openssl

Example:

+ Create a seed iso with your id_rsa public key installed as root user:
    + `./create-seed-iso.sh --key ~/.ssh/id_rsa.pub --growroot mykey-seed.iso`
    + user can login as root with the matching ssh secret key.

+ Custom Static IP Config with vagrant user and password:
    + **Warning**: This is a very unusual configuration, you should only set static ip's when there is absolutly no other way to configure a working internet connection for a virtual machine.
    + file: staticip-metadata:

```
network-interfaces: |
  iface eth0 inet static
  address 192.168.1.10
  network 192.168.1.0
  netmask 255.255.255.0
  broadcast 192.168.1.255
  gateway 192.168.1.254
```

    + file: staticip-userdata:
```
manage-resolv-conf: true
resolv_conf:
  nameservers: ['8.8.4.4', '8.8.8.8']
  domain: example.org
```

    + `./create-seed-iso.sh --vagrant-password --add-meta-data staticip-metadata --add-user-data staticip-userdata --grow-root staticip-seed.iso`
