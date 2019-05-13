# packer-esxi

A [Packer][] template for building an Ubuntu 18.04 virtual machine on
[VMware ESXi][]. These are built on the remote ESXi host, then shutdown and
left registered. A lot of this work comes out of [boxes][], which provides the
basic template formation. Forked from [Nick Charlton][].

Assume that it's possible to fetch an IP through DHCP on the primary (`VM
Network`) network. If this isn't the case, you can adjust the [boot command to
set a static IP][].

Ensure that the appropriate ports are open on your ESXi host per [this post Nick wrote on how to use Packer with
VMware ESXi][post]. You can also disable your [VMWare firewall][] but that's probably less desirable.

## Prerequisites
1. Install [Packer][] 1.4.1 or greater
2. Have a [VMWare ESXi][] host available (tested through 6.7.0)

## Usage

```sh
packer build -var-file variables.json ubuntu-1804-base.json
```

Ensure that `variables.json` contains valid values. Save the `variables.json.template` file as `variables.json` and fill in the appropriate values for your ESXi host.

*Note: [OVFTool][] is not required for this setup since we're not exporting a template to VMWare. ESXi does _not_ support templates. This implementation uses Packer to create the target VM from .iso, execute target provisioners, and then shutdown the VM. It does not attempt to export the VM into the template, which is the default Packer behavior, nor does it unregister and delete the working VM. The resulting VM is the final product.

## Author

- Updated: JP Toto
- Original: Copyright (c) 2016 Nick Charlton. MIT Licensed.

[Packer]: https://packer.io
[VMware ESXi]: http://www.vmware.com/products/vsphere-hypervisor.html
[boxes]: https://github.com/nickcharlton/boxes
[boot command to set a static IP]: https://help.ubuntu.com/lts/installation-guide/armhf/apbs02.html
[post]: https://nickcharlton.net/posts/using-packer-esxi-6.html
[OVFTool]: https://code.vmware.com/web/tool/4.3.0/ovf
[Nick Charlton]: https://github.com/nickcharlton/packer-esxi
[VMWare firewall]: https://docs.vmware.com/en/VMware-vSphere/6.5/com.vmware.vsphere.security.doc/GUID-7A8BEFC8-BF86-49B5-AE2D-E400AAD81BA3.html
