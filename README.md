# Cloud-Init compatible cidata iso creator

Purpose: to configure a cloud-init compatible virtual machine on a "no-cloud" setup by using a "cidata" cdrom iso file.

+ Use one of the prebuilt vagrant-*.iso files to configure a instance to create a vagrant user with optional "vagrant" password.
    + This is also useful outside vagrant for any testing, because it enables a user to login the machine using a standard key/password 
    + the insecure default secret key of vagrant is available under vagrant.id_rsa
    + the insecure default password of vagrant is "vagrant"

+ Use your ssh publickey to create a cidata iso that configures the root user with your key.
    + Requisites for creating new iso files: bash, mkisoimage, openssl

Example:

+ `./create-seed-iso.sh --key ~/.ssh/id_rsa.pub --growroot mykey-seed.iso`

   + user can login as root with the matching ssh secret key.
