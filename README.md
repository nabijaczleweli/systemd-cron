systemd-cron
================
[systemd][1] units to run [cron][2] scripts

Description
---------------
systemd units to provide cron daemon functionality by running scripts in cron directories.  
The crontabs are automaticaly translated using /usr/lib/systemd/system-generators/[systemd-crontab-generator][6].

Usage
---------
Add executable scripts to the appropriate cron directory (e.g. `/etc/cron.daily`) and enable systemd-cron:

    # systemctl daemon-reload
    # systemctl enable cron.target
    # systemctl start cron.target

The project also includes simple crontab command equivalent, which behaves like standard crontab command (and accepts the same main options).

The scripts should now be automatically run by systemd. See man:systemd.cron(7) for more information.

Dependencies
----------------
* systemd ≥ 236
* UsrMerged system
* [run-parts][3] (optional)
* /usr/sbin/sendmail (optional, evaluated at runtime)
* C and C++20 compilers
* libssl (-lcrypto)
* support for /usr/lib/sysusers.d/*.conf (optional)

Dependencies history
------------------------
* systemd ≥ 197, first support for timers
* systemd ≥ 209, yearly timers
* systemd ≥ 212, persistent timers
* systemd ≥ 217, minutely, quarterly & semi-annually timers
* systemd ≥ 229, real random delay support with `RandomizedDelaySec` option [(bug)][90]
* systemd ≥ 236, `LogLevelMax` option

Installation
----------------
There exists packages avaible for:
* [Debian][7]
* [Ubuntu][8]
* [Arch][9]
* [Gentoo][10]
* [OpenMamba][11] (RPM based)

There is also a old .spec file for Fedora in [contrib/][12].

A complete list of all packages can be browsed at [Repology][13].

You can also build it manually from source.


Packaging
--------------

### Building

    $ ./configure
    $ make

### Staging

    $ make DESTDIR="$destdir" install

### Configuration

The `configure` script takes command line arguments to configure various details of the build. The following options
follow the standard GNU [installation directories][4]:

* `--prefix=<path>`
* `--bindir=<path>`
* `--datadir=<path>`
* `--libdir=<path>`
* `--libexecdir=<path>`
* `--statedir=<path>`
* `--mandir=<path>`
* `--docdir=<path>`

Other options include:

* `--unitdir=<path>` Path to systemd unit files.
  Default: `<libdir>/systemd/system`.
* `--enable-boot[=yes|no]` Include support for the boot timer.
  Default: `yes`.
* `--enable-minutely[=yes|no]` Include support for the minutely timer.
  Default: `no`.
* `--enable-hourly[=yes|no]` Include support for the hourly timer.
  Default: `yes`.
* `--enable-daily[=yes|no]` Include support for the daily timer.
  Default: `yes`.
* `--enable-weekly[=yes|no]` Include support for the weekly timer.
  Default: `yes`.
* `--enable-monthly[=yes|no]` Include support for the monthly timer.
  Default: `yes`.
* `--enable-quarterly[=yes|no]` Include support for the quarterly timer.
  Default: `no`.
* `--enable-semi_annually[=yes|no]` Include support for the semi-annually timer.
  Default: `no`.
* `--enable-yearly[=yes|no]` Include support for the yearly timer.
  Default: `no`.
* `--enable-setgid[=yes|no]` Compile setgid C helper for crontab. Needs C compiler.
  Default: `no`.
* `--libcrypto=<flags>` Compiler and linker flags required to build and link to libcrypto.
  Default: `pkgconf --cflags --libs libcrypto` or `-lcrypto`.

A typical configuration for the latest systemd would be:

    $ ./configure

(the default settings are a common ground between what is seen on current Arch/Debian/Gentoo packaging)

Alternatively you can also generate individual .timer/.service for all the jobs
in /etc/cron.{hourly,daily,weekly,monthly,...}:

    $ ./configure --enable-runparts=no

### Caveat

Your package should also run these extra commands before starting cron.target
to ensure that @reboot scripts doesn't trigger right away:

    # touch /run/crond.reboot
    # touch /run/crond.bootdir

See Also
------------
`systemd.cron(7)` or in source tree `man -l src/man/systemd.cron.7`


License
-----------
The project is licensed under MIT.
It vendors a derived work of voreutils, which is available under the 0BSD licence.


Copyright
-------------
© 2014, Dwayne Bent : original package with static units  
© 2014, Konstantin Stepanov (me@kstep.me) : author of crontab generator  
© 2014, Daniel Schaal : review of crontab generator  
© 2014, Alexandre Detiste (alexandre@detiste.be) : manpage for crontab generator  


[1]: http://www.freedesktop.org/wiki/Software/systemd/ "systemd"
[2]: http://en.wikipedia.org/wiki/Cron "cron"
[3]: https://tracker.debian.org/pkg/debianutils "debianutils"
[4]: https://www.gnu.org/prep/standards/html_node/Directory-Variables.html "Directory Variables"
[5]: http://www.freedesktop.org/software/systemd/man/systemd.timer.html#Persistent= "systemd.timer"
[6]: https://github.com/kstep/systemd-crontab-generator "crontab generator"
[7]: http://packages.debian.org/systemd-cron
[8]: http://packages.ubuntu.com/search?suite=all&searchon=names&keywords=systemd-cron
[9]: https://aur.archlinux.org/packages/systemd-cron
[10]: https://packages.gentoo.org/package/sys-process/systemd-cron
[11]: https://openmamba.org/en/packages/?tag=devel&pkg=systemd-cron.source
[12]: https://github.com/systemd-cron/systemd-cron/blob/master/contrib/systemd-cron.spec
[13]: https://repology.org/project/systemd-cron/packages

[90]: https://bugs.freedesktop.org/show_bug.cgi?id=82084
