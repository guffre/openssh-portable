# Portable OpenSSH - Deobfuscate passwords
This is a fork that deobfuscates the "fake_password" function.
For people who have tried logging passwords through PAM before, and have run across the "\b\n\r\177INCORRECT" string, that is what this fork removes.

The reason I do not just "return wire_password" is that the pointer returned by this function is free'd later at some point, and you will crash with a double-free if you don't allocate here.

## Building Portable OpenSSH

### Dependencies

gcc autoconf make libssl-dev zlib1g-dev libpam0g-dev

### Building from git

If building from git, you'll need [autoconf](https://www.gnu.org/software/autoconf/) installed to build the ``configure`` script. The following commands will check out and build portable OpenSSH from git:

```
git clone https://github.com/openssh/openssh-portable # or https://anongit.mindrot.org/openssh.git
cd openssh-portable
autoreconf
./configure # [options] # I use ./configure --with-pam
make && make tests
```

Important note: If you are building on Debian (and probably other distro's) you will hit an error complaining about "/var/empty"
Simply "mkdir /var/empty" to clear this error.

### Build-time Customisation

There are many build-time customisation options available. All Autoconf destination path flags (e.g. ``--prefix``) are supported (and are usually required if you want to install OpenSSH).

For a full list of available flags, run ``configure --help`` but a few of the more frequently-used ones are described below. Some of these flags will require additional libraries and/or headers be installed.

Flag | Meaning
--- | ---
``--with-pam`` | Enable [PAM](https://en.wikipedia.org/wiki/Pluggable_authentication_module) support. [OpenPAM](https://www.openpam.org/), [Linux PAM](http://www.linux-pam.org/) and Solaris PAM are supported.
``--with-libedit`` | Enable [libedit](https://www.thrysoee.dk/editline/) support for sftp.
``--with-kerberos5`` | Enable Kerberos/GSSAPI support. Both [Heimdal](https://www.h5l.org/) and [MIT](https://web.mit.edu/kerberos/) Kerberos implementations are supported.
``--with-selinux`` | Enable [SELinux](https://en.wikipedia.org/wiki/Security-Enhanced_Linux) support.
``--with-security-key-builtin`` | Include built-in support for U2F/FIDO2 security keys. This requires [libfido2](https://github.com/Yubico/libfido2) be installed.
