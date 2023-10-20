sshudo
======

Synopsis
--------

```sh
PBUILDERROOTCMD=sshudo pbuilder --build --debbuildopts "" ../somedebianpackage_123-4.dsc

alias sudo=sshudo
alias pkexec=sshudo

sshudo ln -vis sshudo /usr/bin/sudo
sshudo ln -vis sshudo /usr/bin/pkexec
```

Description
-----------

sshudo is an SSH based minimal drop-in replacement for very basic
`sudo` and `pkexec` usage with commands which still contain parameters
with spaces or empty parameters.

Options
-------

_None so far._

Limitations
-----------

* Won't work properly if parameters contain one or more single quotes.
* Might make your brain hurt if you try to use backslash escaping.
* No SSH option passing. Use `~/.ssh/config` for that.
* Only works for gaining root privileges.

Motivation
----------

[`sudo`](https://www.sudo.ws/) is notoriously hazardous to system
security, regularily having security issues like
e.g. [CVE-2021-3156](https://blog.qualys.com/vulnerabilities-research/2021/01/26/cve-2021-3156-heap-based-buffer-overflow-in-sudo-baron-samedit).
(And PolicyKit and its `pkexec` isn't much better either.)

So I wanted to get rid of it once and forever. Didn't seem that hard
as I use `ssh root@localhost` anyway for most purposes other people
would think about using `sudo` for.

But unfortunately using `pdebuild` from the [pbuilder
suite](https://pbuilder-team.pages.debian.net/pbuilder/) with
`PBUILDERROOTCMD="ssh root@localhost"` didn't work, because `pdebuild`
unconditionally passes an empty string as one of the parameters to the
command in `$PBUILDERROOTCMD` and this is lost after the command has
been passed as parameters to SSH. So I needed a wrapper which does
proper escaping for this case. And that's how `sshudo` came into
existence.

License
-------

This program is free software: you can redistribute it and/or modify
it under the terms of the [DO WHAT THE FUCK YOU WANT TO PUBLIC
LICENSE](http://www.wtfpl.net/about/) (WTFPL), either version 2 of the
License, or (at your option) any later version.

### Full Text of the License

#### DO WHAT THE FUCK YOU WANT TO PUBLIC LICENSE

##### TERMS AND CONDITIONS FOR COPYING, DISTRIBUTION AND MODIFICATION

0. You just DO WHAT THE FUCK YOU WANT TO.
