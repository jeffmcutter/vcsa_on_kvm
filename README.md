# vcsa_on_kvm
VMware-vCenter-Server-Appliance 8.0 U2 on KVM Install / Ansible Playbook

I found most of this at the link below and adapted it in a quick and dirty manner for the sole purpose of getting VCSA running under KVM as quickly as I could.

https://gist.github.com/infernix/0377af0bc9012e3d5e5e

Original link handled 6.0, so some changes for 6.5 were necessary: an extra disk, different sizes, more RAM, and a different post install mechanism, which I ended up just avoiding from an automation perspective because it seems to have totally changed from 6.0 to 6.5.  Once the appliance installs, you can configure it on the console initially, then via the vcsa administrator web interface on port 5480 by default.

I have since updated to use 6.7 borrowing some efforts from Dr-Shadow's fork.

Note that it can take a while for the appliance to come up to the blue screen where you can do the initial configuration.  After it comes up, it may still be installing packages.  If you configure the management network and disable IPv6, it will reboot the host potentially halting the install in the background.  If this happens the Administrator UI will say installing RPMs indefinitely.

You may want to enable ssh and shell and login and check the contents of `/var/log/firstboot/rpmInstall.json` before proceeding (assuming you have DHCP running and/or IPv6 operational).  Speaking of DHCP it seems like the initial install will fail if DHCP isn't available.  Also, the console will show localhost for a while even after it's requested a DHCP address, after a while the DHCP address should get displayed.

Note also that valid reverse DNS is supposed to be important when configuring the management network for the appliance.

For setting up ESXi 6.5 under KVM, the following settings may help on your host, probably want to reboot after:

```
# cat /etc/modprobe.d/kvm-intel.conf
options kvm ignore_msrs=1
options kvm-intel nested=y ept=y
```

Also when setting up the ESX VM in KVM, choose "Copy host CPU configuration" under CPU and e1000 may be your best choice for NIC model.  ESXi 6.5 seems to require a minimum of 4 GB to run, or at least to install.  I'm using an IDE drive with it.

UPDATE:

January 10, 2024

Updated for VCSA 8.0 U2.

Using SATA disks for ESXi now.

Using e1000e for all nics now.

Sort of cleaned it up just ever so slightly.  At least it's a touch easier to read now.

Failed firstboot install with 16 GB RAM.  Switched to 24 GB and it worked.  Later tried 20 GB and it also worked.

I was not able to get the vCLS VM to run nested.  I disabled it following https://kb.vmware.com/s/article/91890.

Moved to https://knowledge.broadcom.com/external/article?legacyId=91890.


UPDATE:

August 2025

I got the initial install to work with DHCP for the IP but manual DNS and hostname.  However, not having reverse DNS seems to break the login screen.
