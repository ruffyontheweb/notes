Setting up vagrant 1.0.6
========================

**Debian Wheezy**

    $getconf GNU_LIBC_VERSION
    glibc 2.13

When you install vagrant 1.0.6 deb package file, vagrant has
its own ruby 1.9.1 in `/opt/vagrant/embedded` directory.  This
`ruby` requires `glibc 2.14` or later.  You should edit the last
line **`from`**

    ${EMBEDDED_DIR}/bin/ruby ${VAGRANT_EXECUTABLE} "$@"
**`to`**

    ruby ${VAGRANT_EXECUTABLE} "$@"
to use the system ruby version.

**Ubuntu 12.04.2**

    $getconf GNU_LIBC_VERSION
    glibc 2.15

Installing from .deb file works.

**URLs:**
* vagrant [website](http://www.vagrantup.com/)
* Download [vagrant](http://downloads.vagrantup.com/tags/v1.0.6)


The joy of CentOS 6.4
=====================

`vagrant` 1.2.7 on CentOS 6.4 fails with simple `vagrant --version`.

See [discussion][0].

Fix: Courtesy of Stephen Brown from the above link.

Edit `/opt/vagrant/embedded/gems/gems/vagrant-1.2.7/plugins/hosts/fedora/host.rb`
line `39` to (add `CentOS` to `Fedora`
```
version_number = /[Fedora|CentOS].*release ([0-9]+)/.match(f.gets)[1].to_i
```

Commit [2a9d0c9d7f5d][1] fixed it.

Author: _Tuan T. Pham_

[0]: http://vagrant.1086180.n5.nabble.com/Cant-Install-Vagrant-on-CentOS-6-4-td1446.html "vagrant on CentOS"
[1]: https://github.com/mitchellh/vagrant/commit/2a9d0c9d7f5dc1c428e71cc43a13ae4624744f16#plugins/hosts/fedora/host.rb "github URL"
