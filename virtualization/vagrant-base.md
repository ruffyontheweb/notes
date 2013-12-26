Setting up a vagrant base box from scratch
----------------------------------------

**Setting up a vagrant base box consists of two steps:**
* Setting up virtual machine in virtualbox
* Packaging the virtual machine to the .box file


**Setting up virtual machine in virtualbox:**

* Install/remove packages that you want

* Install vim and set it to default editor (*personal reference*)  
    `# apt-get -y install vim && update-alternatives --config editor`

* Setup `tmpfs` for `/tmp` and `/var/log` (*personal reference*)  
    `# echo "tmpfs /tmp tmpfs defaults 0 0" >> /etc/fstab`  
    `# echo "tmpfs /var/log tmpfs defaults 0 0" >> /etc/fstab`  

* Create accounts for vagrant  
    `# groupadd admin`  
    `# mkdir /home/vagrant`  
    `# useradd -G admin -d /home/vagrant -s /bin/bash vagrant`  
    `# usermod -G admin vagrant`

* Edit sudoers so everyone can sudo with `visudo` so we have these lines  
    `Defaults env_keep="SSH_AUTH_SOCK"`  
    `%admin ALL=NOPASSWD: ALL`

* Install VirtualBox Linux Addition (Devices->Install Guest Additions)  
    `# apt-get install linux-headers-$(uname -r) build-essential dkms`  
    `# mkdir /mnt/cdrom`  
    `# mount /dev/cdrom /mnt/cdrom`  
    `# /mnt/cdrom/VBoxLinuxAdditions.run`

* If you want to use your own key then `cp` your `~/.ssh/id_rsa` of the host
that you will launch `vagrant` VMs to `~/.vagrant.d/insecure_private_key` **and**
`~/.ssh/id_rsa.pub` to `~/.ssh/authorized_keys` of vagrant account in VM.

* If you use the default vagrant `insecure_private_key` at `$HOME/.vagrant.d`
then you need to use the vagrant `public key`.  
    `vagrant$ mkdir .ssh && cd .ssh`  
    `vagrant$ wget https://github.com/mitchellh/vagrant/raw/master/keys/vagrant.pub`  
    `vagrant$ mv vagrant.pub authorized_keys`  
* `SSH` Tweak from `docs.vagrantup.com`  
    `# echo "UseDNS no" >> /etc/ssh/sshd_config`

* Clean up cache and system packages  
    `# apt-get autoclean && apt-get autoremove`

* Remove logs of the virtual machine at host machine:  
    `$rm $HOME/VirtualBox\ VMs/DebianWheezy64/Logs/*`

**Packaging the virtual machine to .box file:**

* Change `CWD`  
    `$ cd /path/to/VirtualBox\ VMs/DebianWheezy64`  

* If you have a lot of `RAM`, use `/tmp` with `tmpfs`. Notice the name after `--base` must
be the same name of your VM that you are using with VirtualBox.  
    `$ time vagrant package --base DebianWheezy64  --output /tmp/DebianWheezy64.box`  
This should take less than 3 minutes if you have SSD and plenty of `memory`.

**References:**  
[Creating a vagrant base box for ubuntu 12.04 32bit server](https://github.com/fespinoza/checklist_and_guides/wiki/Creating-a-vagrant-base-box-for-ubuntu-12.04-32bit-server)  
[How I setup Ubuntu 11.04 Server using VirtualBox on a MacBook Air](http://www.idevelopsoftware.com/2011/07/how-i-setup-ubuntu-11-04-server-using-virtualbox-on-a-macbook-air/)  
[Base Boxes](http://docs.vagrantup.com/v1/docs/base_boxes.html)  
[Creating base box from scratch for Vagrant](http://pyfunc.blogspot.com/2011/11/creating-base-box-from-scratch-for.html)  

**Disable Firewall in Debian:**  
[Stop firewall in Debian](http://www.cyberciti.biz/faq/debian-iptables-stop/)  
[Debian Firewall](http://wiki.debian.org/DebianFirewall)
