# Packer Example - CentOS 7 minimal Hardened Vagrant Box using Ansible provisioner

**Current CentOS Version Used**: 7.4 (1708)

**Pre-built Vagrant Box (without hardening and done by original owner of this repo)**:

  - [`vagrant init geerlingguy/centos7`](https://vagrantcloud.com/geerlingguy/boxes/centos7)
  - See older versions: http://files.midwesternmac.com/
  
This example build configuration installs and configures CentOS 7 x86_64 minimal using Ansible, hardens it, and then generates a Vagrant box file for VirtualBox.

The example can be modified to use more Ansible roles, plays, and included playbooks to fully configure (or partially) configure a box file suitable for deployment for development environments.

This has been designed to test out new additions to a security-hardened base build of CentOS 7, to better reflect what could be used in real environments.

NB: LUKS password has to be supplied in plaintext (in http/ks.cfg), but this would be changed in a real deployment (either by crypsetup or other means)

## Requirements

The following software must be installed/present on your local machine before you can use Packer to build the Vagrant box file:

  - [Packer](http://www.packer.io/)
  - [Vagrant](http://vagrantup.com/)
  - [VirtualBox](https://www.virtualbox.org/) (if you want to build the VirtualBox box)
  - [Ansible](http://docs.ansible.com/intro_installation.html)

The ansible provisioner in packer makes use of some pre-made roles by geerlingguy; these make the raw .iso used compatible with vagrant by installing the dependencies for VBox Guest Additions and similar.

## Usage

Make sure all the required software (listed above) is installed, then cd to the directory containing this README.md file, and run:

    $ packer build centos7.json

After a few minutes, Packer should tell you the box was generated successfully.

## Testing built boxes

There's an included Vagrantfile that allows quick testing of the built Vagrant boxes. From this same directory, run one the following command after building the box:

    $ vagrant up

## AWS AMI Builder
variables.json currently uses envvars to pass the keys in. Set these in your environment before using this file with your aws builds.

## Using only one provisioner
To just build an AMI: packer build centos7.json -only=amazon-ebs
To just build a vbox box: packer build centos7.json -only=virtualbox-iso

## License

MIT license.

## Author Information

Created in 2014 by [Jeff Geerling](https://www.jeffgeerling.com/), author of [Ansible for DevOps](https://www.ansiblefordevops.com/).
