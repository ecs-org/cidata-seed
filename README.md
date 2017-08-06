# Cloud-Init compatible cidata iso creator

Purpose: to configure a cloud-init compatible virtual machine on a "no-cloud" setup by using a "cidata" cdrom iso file.

+ Use one of the prebuilt vagrant-*.iso files to configure a instance to create a vagrant user with optional "vagrant" password.
    + This is also useful outside vagrant for any testing, because it enables a user to login the machine using a standard key/password 
    + the insecure default secret key of vagrant is available under vagrant.id_rsa
    + the insecure default password of vagrant is "vagrant"

+ Use your ssh publickey to create a cidata iso that configures the root user with your key.
    + Requisites for creating new iso files: bash, mkisoimage, openssl

## Examples

+ Create a seed iso with vagrant user and password:
    + `./create-seed-iso.sh --vagrant-password vagrant-seed.iso`
    + user can login as vagrant with password vagrant or the vagrant secret key

+ Create a seed iso with your id_rsa public key installed as root user and grow the root file system to the maximum available space at startup:
    + `./create-seed-iso.sh --key ~/.ssh/id_rsa.pub --growroot mykey-seed.iso`
    + user can login as root with the matching ssh secret key.

+ Custom Static IP Config (with vagrant user and password):
    + **Warning**: Static IP configurations are usually bad and break easy. (eg. change of virtual ethernet adapter will change interface name).

file: staticip-metadata:

```
network-interfaces: |
  auto lo
  iface lo inet loopback
  #
  auto ens5
  iface ens5 inet static
    address 10.1.30.20
    network 10.1.30.0
    netmask 255.255.255.0
    broadcast 10.1.30.255
    gateway 10.1.30.1
```

file: staticip-userdata:

```
manage-resolv-conf: true
resolv_conf:
  nameservers: ['8.8.4.4', '8.8.8.8']
  domain: example.org
```

Execute: `./create-seed-iso.sh --vagrant-password --add-meta-data staticip-metadata --add-user-data staticip-userdata staticip-seed.iso`
