# FreeBSD port for WebCalendar

This repository contains a port skeleton for the PHP WebCalendar application.  It's based on the origional PHP 5.x
port [www/webcalendar](https://www.freshports.org/www/webcalendar)

## Using with Portshaker

Create a file `/usr/local/etc/portshaker.d/webcalendar` with the following contents.
```
#!/bin/sh
. /usr/local/share/portshaker/portshaker.subr
method="git"
if	[ "$1" != '--' ]; then
	err 1 "Extra arguments"
fi
shift
git_clone_uri="https://github.com/tuaris/FreeBSD-WebCalendar.git"
git_branch="master"
run_portshaker_command $*
```

Then add **webcalendar** to your **_merge_from** line in `/usr/local/etc/portshaker.conf`.  For example.

```
#---[ Base directory for mirrored Ports Trees ]---
mirror_base_dir="/var/cache/portshaker"

#---[ Directories where to merge ports ]---
ports_trees="default"

use_zfs="yes"
poudriere_dataset="poudriere/poudriere"
poudriere_ports_mountpoint="/usr/local/poudriere/ports"
default_poudriere_tree="default"
default_merge_from="ports webcalendar"
```
