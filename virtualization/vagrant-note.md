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

Author: _Tuan T. Pham_