# vcsa_on_kvm
VMware-vCenter-Server-Appliance 6.5 on KVM Install / Ansible Playbook

I found most of this at the link below and adapted it for my own purposes.

https://gist.github.com/infernix/0377af0bc9012e3d5e5e

Original link handled 6.0, so some changes for 6.5 were necessary: an extra disk, different sizes, more RAM, and a different post install mechanism, which I ended up just avoiding from an automation perspective.  Once the appliance installs, you can configure it on the console initially, then via the vcsa administrator web interface on port 5480 by default.

