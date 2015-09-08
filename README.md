# openstack

This repository contains a few files necessary for the configuration of openstack cloud servers at CERN.

## Getting Started

1. Request cloud infrastructure resources <br />
Go to the [CERN Resource Portal -> List Services](https://resources.web.cern.ch/resources/Manage/ListServices.aspx) and subscribe to "Cloud Infrastructure"
* Login to `lxplus` and clone this repository into your favourite directory <br />
`> git clone git@github.com:Crazy-Coding-Crew/openstack.git`
* Source the openstack configuration file <br /> 
`> cd openstack && source .openrc`
* Register your ssh keys to openstack <br />
`> openstack keypair create --public-key ~/.ssh/id_dsa.pub <name>` <br />
where `<name>` is a string identifier used later to select the right key pair (e.g. `lxplus`)
* If you want to use an image other than the standard images provided by CERN (SLC5/6, Win7),
you need to register this image to openstack. E.g. for using the Ubuntu 14.04 LTS Server image, do the following:<br />
`> wget http://uec-images.ubuntu.com/trusty/current/trusty-server-cloudimg-i386-disk1.img` <br />
`> openstack image create "Ubuntu Trusty 14.04 LTS Server" --disk-format=raw --container-format=bare
--property hypervisor-type=qemu --file trusty-server-cloudimg-i386-disk1.img`<br />
You can then check the available images with `> openstack image list`. For more options see 
[here](http://clouddocs.web.cern.ch/clouddocs/details/user_supplied_images.html)
* Create VM running the server <br />
`> openstack server create --key-name <key name> --flavor m1.small --image <image> <hostname>` <br />
where `<key name>` should be the one used to register the key pair (e.g. `lxplus`), `<image>` specifies
which of the available images you want to boot (e.g. `"Ubuntu Trusty 14.04 LTS Server"`) and `<hostname>` is 
the desired hostname for the server (e.g. `cgumpert01`)
* Check the status of the server with <br />
`> openstack server show <hostname>`
* Once active, you should be able to login from lxplus using <br />
`> ssh <hostname>` (for the Ubuntu example do `> ssh ubuntu@<hostname>`)
* Check the [dashboard](https://openstack.cern.ch/dashboard/project/)

Alternative: Use the web interface as described [here](http://clouddocs.web.cern.ch/clouddocs/tutorial_using_a_browser/README.html).

## Contextualisation

In order to customise some settings during the first boot of the virtual machine, 
add the following option to the `openstack server create` command: <br />
`--user-data <user data file>`

## Helpful Links

* [CERN OpenStack Private Cloud Guide](http://clouddocs.web.cern.ch/clouddocs/index.html)
* [Examples for cloud-config](https://cloudinit.readthedocs.org/en/latest/topics/examples.html)
* [Openstack CLI documentation](http://docs.openstack.org/cli-reference/content/openstackclient_commands.html)
