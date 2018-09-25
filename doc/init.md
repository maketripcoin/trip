# Sample init scripts and service configuration for tripd

Sample scripts and configuration files for systemd, Upstart and OpenRC
can be found in the contrib/init folder.

    contrib/init/tripd.service:    systemd service unit configuration
    contrib/init/tripd.openrc:     OpenRC compatible SysV style init script
    contrib/init/tripd.openrcconf: OpenRC conf.d file
    contrib/init/tripd.conf:       Upstart service configuration file
    contrib/init/tripd.init:       CentOS compatible SysV style init script

# Service User

All three startup configurations assume the existence of a "trip" user
and group.  They must be created before attempting to use these scripts.

# Configuration

At a bare minimum, tripd requires that the rpcpassword setting be set
when running as a daemon.  If the configuration file does not exist or this
setting is not set, tripd will shutdown promptly after startup.

This password does not have to be remembered or typed as it is mostly used
as a fixed token that tripd and client programs read from the configuration
file, however it is recommended that a strong and secure password be used
as this password is security critical to securing the wallet should the
wallet be enabled.

If tripd is run with `-daemon` flag, and no rpcpassword is set, it will
print a randomly generated suitable password to stderr.  You can also
generate one from the shell yourself like this:

```bash
bash -c 'tr -dc a-zA-Z0-9 < /dev/urandom | head -c32 && echo'
```

Once you have a password in hand, set `rpcpassword=` in `/etc/trip/trip.conf`

For an example configuration file that describes the configuration settings,
see `contrib/debian/examples/trip.conf`.

# Paths

All three configurations assume several paths that might need to be adjusted.
```
Binary:              /usr/bin/tripd
Configuration file:  /etc/trip/trip.conf
Data directory:      /var/lib/tripd
PID file:            /var/run/tripd/tripd.pid (OpenRC and Upstart)
                     /var/lib/tripd/tripd.pid (systemd)
```
The configuration file, PID directory (if applicable) and data directory
should all be owned by the trip user and group.  It is advised for security
reasons to make the configuration file and data directory only readable by the
trip user and group.  Access to trip-cli and other tripd rpc clients
can then be controlled by group membership.

# Installing Service Configuration

## systemd

Installing this .service file consists on just copying it to
`/usr/lib/systemd/system` directory, followed by the command
`systemctl daemon-reload` in order to update running systemd configuration.

To test, run "systemctl start tripd" and to enable for system startup run
`systemctl enable tripd`

## OpenRC

Rename tripd.openrc to tripd and drop it in `/etc/init.d`.  Double
check ownership and permissions and make it executable.  Test it with
`/etc/init.d/tripd start` and configure it to run on startup with
`rc-update add tripd`

## Upstart (for Debian/Ubuntu based distributions)

Drop tripd.conf in `/etc/init`.  Test by running "service tripd start"
it will automatically start on reboot.

NOTE: This script is incompatible with CentOS 5 and Amazon Linux 2014 as they
use old versions of Upstart and do not supply the start-stop-daemon uitility.

## CentOS

Copy tripd.init to `/etc/init.d/tripd`. Test by running "service tripd start".

Using this script, you can adjust the path and flags to the tripd program by
setting the tripd and FLAGS environment variables in the file
`/etc/sysconfig/tripd`. You can also use the DAEMONOPTS environment variable here.

# Auto-respawn

Auto respawning is currently only configured for Upstart and systemd.
Reasonable defaults have been chosen but YMMV.
