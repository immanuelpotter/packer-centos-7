# Packer Example - CentOS 7 minimal Hardened Vagrant Box using Ansible provisioner

**Current CentOS Version Used**: 7.4 (1708)

**Pre-built Vagrant Box (without hardening and done by original owner of this repo)**:

  - [`vagrant init geerlingguy/centos7`](https://vagrantcloud.com/geerlingguy/boxes/centos7)
  - See older versions: http://files.midwesternmac.com/
  
This example build configuration installs and configures CentOS 7 x86_64 minimal using Ansible, hardens it, and then generates a Vagrant box file for VirtualBox.

The example can be modified to use more Ansible roles, plays, and included playbooks to fully configure (or partially) configure a box file suitable for deployment for development environments.

This has been designed to test out new additions to a security-hardened base build of CentOS 7, to better reflect what could be used in real environments.

NB: LUKS password has to be supplied in plaintext (in http/ks.cfg), but this would be changed in a real deployment (either by cryptsetup or other means)

### Requirements

The following software must be installed/present on your local machine before you can use Packer to build the Vagrant box file:

  - [Packer](http://www.packer.io/)
  - [Vagrant](http://vagrantup.com/)
  - [VirtualBox](https://www.virtualbox.org/) (if you want to build the VirtualBox box)
  - [Ansible](http://docs.ansible.com/intro_installation.html)

And for AWS, the AWS CLI:

	pip install awscli

If building AMIs, from an ISO, you should create an IAM Role called "vmimport" (you can specify otherwise in the packer template). You can do this by, from the root of this repo::

	aws iam create-role --role-name vmimport --assume-role-policy-document file://aws-iam-policies/trust-policy.json

Now edit the aws-iam-policies/role-policy.json file with your bucket name (specified in centos7-ami.json), and add this to the role:

	aws iam put-role-policy --role-name vmimport --policy-name vmimport --policy-document file://aws-iam-policies/role-policy.json

The ansible provisioner in packer makes use of some pre-made roles by geerlingguy; these make the raw .iso used compatible with vagrant by installing the dependencies for VBox Guest Additions and similar.

## Usage

To build an AMI (From a minimal ISO), you must build a vbox box, then import to Amazon. This is why there is a separate json file, to prevent clogging up your disk.
You will need to set your environment variables: $AWS_ACCESS_KEY, $AWS_SECRET_KEY, and $BUCKET_NAME.
I'm currently also overriding $AWS_MAX_ATTEMPTS and $AWS_POLL_DELAY_SECONDS on the machine running the packer build, as the import in Amazon takes bloody ages. The defaults are 300 attempts on a 5 second delay. I've overridden this to 2000 attempts on an 8 second delay.

     packer build centos7-ami.json

To just build a vbox box, don't worry about those env vars:

    packer build centos7-vagrant.json

Make sure all the required software (listed above) is installed.
Package up the hardening scripts:

    $ cd scripts/ && tar cvzf harden.tar.gz harden

and then cd to the directory containing this README.md file, and run:

    $ packer build centos7.json

After a few minutes, Packer should tell you that both boxes were generated successfully.

## Testing built boxes

There's an included Vagrantfile that allows quick testing of the built Vagrant boxes. From this same directory, run one the following command after building the box:

    $ vagrant up

## License

MIT license.

## Author Information

Created in 2014 by [Jeff Geerling](https://www.jeffgeerling.com/), author of [Ansible for DevOps](https://www.ansiblefordevops.com/).
