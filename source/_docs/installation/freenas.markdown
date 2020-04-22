---
title: "Installation on FreeNAS 11.2"
description: "Installation of Open Peer Power on your FreeNAS."
---

[FreeNAS](https://www.freenas.org) is a free and open-source network-attached storage (NAS) software based on FreeBSD and the OpenZFS file system. It is licensed under the terms of the BSD License and runs on commodity x86-64 hardware.

This has been tested on FreeNAS 11.2 and should also work on FreeBSD 11.x as well. These instructions assume you already have a running and accessible jail. For more information on creating a jail read the official FreeNAS User Guide regarding [Jails](https://www.ixsystems.com/documentation/freenas/11.2/jails.html). Once you have the jail available, follow the steps below. Directories used follow standard BSD conventions but can be adjusted as you wish.

Enter the Open Peer Power jail. If you don't know which name you have given the jail, you can use the `iocage list` command to check.

{% highlight bash %}
# If the jail is called 'OpenPeerPower'
iocage exec OpenPeerPower
{% endhighlight %}

Install the suggested packages:

{% highlight bash %}
pkg update
pkg upgrade
pkg install -y autoconf bash ca_root_nss gmake pkgconf python37 py37-sqlite3
{% endhighlight %}

Create the user and group that Open Peer Power will run as. The user/group ID of `8123` can be replaced if this is already in use in your environment.

{% highlight bash %}
pw groupadd -n openpeerpower -g 8123
echo 'openpeerpower:8123:8123::::::/usr/local/bin/bash:' | adduser -f -
{% endhighlight %}

Create the installation directory:

{% highlight bash %}
mkdir -p /usr/local/share/openpeerpower
chown -R openpeerpower:openpeerpower /usr/local/share/openpeerpower
{% endhighlight %}

Create the virtualenv and install Open Peer Power itself:

{% highlight bash %}
su openpeerpower
cd /usr/local/share/openpeerpower
python3.7 -m venv .
source ./bin/activate
pip3 install --upgrade pip
pip3 install openpeerpower
{% endhighlight %}

While still in the `venv`, start Open Peer Power to populate the configuration directory.

{% highlight bash %}
hass --open-ui
{% endhighlight %}

Wait until you see:

{% highlight bash %}
(MainThread) [openpeerpower.core] Starting Open Peer Power
{% endhighlight %}

Then escape and exit the `venv`.

{% highlight bash %}
deactivate
exit
{% endhighlight %}

Create the directory and the `rc.d` script for the system-level service that enables Open Peer Power to start when the jail starts.

{% highlight bash %}
mkdir /usr/local/etc/rc.d/
{% endhighlight %}

Then create a file at `/usr/local/etc/rc.d/openpeerpower` and insert the content below:

{% highlight bash %}
vi /usr/local/etc/rc.d/openpeerpower
{% endhighlight %}

{% highlight bash %}
#!/bin/sh
#
# Based upon work by tprelog at https://github.com/tprelog/iocage-openpeerpower/blob/11.3-RELEASE/overlay/usr/local/etc/rc.d/openpeerpower
#
# PROVIDE: openpeerpower
# REQUIRE: LOGIN
# KEYWORD: shutdown
#
# openpeerpower_user: The user account used to run the openpeerpower daemon.
#   This is optional, however do not specifically set this to an
#   empty string as this will cause the daemon to run as root.
#   Default: openpeerpower
# openpeerpower_group: The group account used to run the openpeerpower daemon.
#   This is optional, however do not specifically set this to an
#   empty string as this will cause the daemon to run with group wheel.
#   Default: openpeerpower
#
# openpeerpower_venv: Directory where openpeerpower virtualenv is installed.
#       Default:  "/usr/local/share/openpeerpower"
#       Change:   `sysrc openpeerpower_venv="/srv/openpeerpower"`
#       UnChange: `sysrc -x openpeerpower_venv`
#
# openpeerpower_config_dir: Directory where openpeerpower config is located.
#       Default:  "/home/openpeerpower/.openpeerpower"
#       Change:   `sysrc openpeerpower_config_dir="/home/hass/openpeerpower"`
#       UnChange: `sysrc -x openpeerpower_config_dir`

# -------------------------------------------------------
# Copy this file to '/usr/local/etc/rc.d/openpeerpower'
# `chmod +x /usr/local/etc/rc.d/openpeerpower`
# `sysrc openpeerpower_enable=yes`
# `service openpeerpower start`
# -------------------------------------------------------

. /etc/rc.subr
name=openpeerpower
rcvar=${name}_enable

pidfile_child="/var/run/${name}.pid"
pidfile="/var/run/${name}_daemon.pid"
logfile="/var/log/${name}.log"

load_rc_config ${name}
: ${openpeerpower_enable:="NO"}
: ${openpeerpower_user:="openpeerpower"}
: ${openpeerpower_group:="openpeerpower"}
: ${openpeerpower_config_dir:="/home/openpeerpower/.openpeerpower"}
: ${openpeerpower_venv:="/usr/local/share/openpeerpower"}

command="/usr/sbin/daemon"
extra_commands="check_config restart test upgrade"

start_precmd=${name}_precmd
openpeerpower_precmd() {
    rc_flags="-f -o ${logfile} -P ${pidfile} -p ${pidfile_child} ${openpeerpower_venv}/bin/hass --config ${openpeerpower_config_dir} ${rc_flags}"
    [ ! -e "${pidfile_child}" ] && install -g ${openpeerpower_group} -o ${openpeerpower_user} -- /dev/null "${pidfile_child}"
    [ ! -e "${pidfile}" ] && install -g ${openpeerpower_group} -o ${openpeerpower_user} -- /dev/null "${pidfile}"
    [ -e "${logfile}" ] && rm -f -- "${logfile}"
    install -g ${openpeerpower_group} -o ${openpeerpower_user} -- /dev/null "${logfile}"
    if [ ! -d "${openpeerpower_config_dir}" ]; then
      install -d -g ${openpeerpower_group} -o ${openpeerpower_user} -m 775 -- "${openpeerpower_config_dir}"
    fi
}

stop_postcmd=${name}_postcmd
openpeerpower_postcmd() {
    rm -f -- "${pidfile}"
    rm -f -- "${pidfile_child}"
}

upgrade_cmd="${name}_upgrade"
openpeerpower_upgrade() {
    service ${name} stop
    su ${openpeerpower_user} -c '
      source ${@}/bin/activate || exit 1
      pip3 install --upgrade openpeerpower
      deactivate
    ' _ ${openpeerpower_venv} || exit 1
    [ $? == 0 ] && openpeerpower_check_config && service ${name} start
}

check_config_cmd="${name}_check_config"
openpeerpower_check_config() {
    [ ! -e "${openpeerpower_config_dir}/configuration.yaml" ] && return 0
    echo "Performing check on Open Peer Power configuration:"
    #eval "${openpeerpower_venv}/bin/hass --config ${openpeerpower_config_dir} --script check_config"
    su ${openpeerpower_user} -c '
      source ${1}/bin/activate || exit 2
      hass --config ${2} --script check_config || exit 3
      deactivate
    ' _ ${openpeerpower_venv} ${openpeerpower_config_dir}
}

restart_cmd="${name}_restart"
openpeerpower_restart() {
    openpeerpower_check_config || exit 1
    echo "Restarting Open Peer Power"
    service ${name} stop
    service ${name} start
}

test_cmd="${name}_test"
openpeerpower_test() {
    echo -e "\nTesting virtualenv...\n"
    [ ! -d "${openpeerpower_venv}" ] && echo -e " NO DIRECTORY: ${openpeerpower_venv}\n" && exit
    [ ! -f "${openpeerpower_venv}/bin/activate" ] && echo -e " NO FILE: ${openpeerpower_venv}/bin/activate\n" && exit

    ## switch users / activate virtualenv / get version
    su "${openpeerpower_user}" -c '
      source ${1}/bin/activate || exit 2
      echo " $(python --version)" || exit 3
      echo " Open Peer Power $(pip3 show openpeerpower | grep Version | cut -d" " -f2)" || exit 4
      deactivate
    ' _ ${openpeerpower_venv}

    [ $? != 0 ] && echo "exit $?"
}

load_rc_config ${name}
run_rc_command "$1"
{% endhighlight %}

Make the `rc.d` script executable:

{% highlight bash %}
chmod +x /usr/local/etc/rc.d/openpeerpower
{% endhighlight %}

Configure the service to start on boot and start the Open Peer Power service:

{% highlight bash %}
sysrc openpeerpower_enable="YES"
service openpeerpower start
{% endhighlight %}

You can also restart the jail to ensure that Open Peer Power starts on boot.

<div class='note'>

USB Z-Wave sticks may give `dmesg` warnings similar to "data interface 1, has no CM over data, has no break". This doesn't impact the function of the Z-Wave stick in Open Peer Power. Just make sure the proper `/dev/cu*` is used in the Open Peer Power `configuration.yaml` file.

</div>

## Adding support for Z-Wave stick

The following two packages need to be installed in the jail

{% highlight bash %}
pkg install gmake
pkg install libudev-devd
{% endhighlight %}

Then you can install the Z-Wave package

{% highlight bash %}
su openpeerpower
cd /usr/local/share/openpeerpower
source ./bin/activate.csh
pip3 install openpeerpower-pyozw==0.1.7
deactivate
exit
{% endhighlight %}

Stop the Open Peer Power Jail

{% highlight bash %}
sudo iocage stop OpenPeerPower
{% endhighlight %}

Edit the devfs rules on the FreenNAS Host

{% highlight bash %}
vi /etc/devfs.rules
{% endhighlight %}

Add the following lines

{% highlight bash %}
[devfsrules_jail_allow_usb=7]
add path 'cu\*' mode 0660 group 8123 unhide
{% endhighlight %}

Reload devfs

{% highlight bash %}
sudo service devfs restart
{% endhighlight %}

Edit the ruleset used by the jail in the FreeNAS GUI by going to Jails -> `hass` -> Edit ->  Jail Properties ->  devfs_ruleset
Set it to 7

Start the Open Peer Power jail

{% highlight bash %}
sudo iocage start OpenPeerPower
{% endhighlight %}

Connect to the Open Peer Power jail and verify that you see the modem devices

{% highlight bash %}
sudo iocage console OpenPeerPower
{% endhighlight %}

{% highlight bash %}
ls /dev/cu*
{% endhighlight %}

This should output the following

{% highlight bash %}
/dev/cuau0      /dev/cuaU0
{% endhighlight %}

Add the Z-Wave configuration to your `configuration.yaml` and restart Open Peer Power

{% highlight bash %}
vi /home/openpeerpower/.openpeerpower/configuration.yaml
{% endhighlight %}

{% highlight yaml %}
zwave:
  usb_path: /dev/cuaU0
  polling_interval: 10000
{% endhighlight %}

{% highlight bash %}
service openpeerpower restart
{% endhighlight %}

## Updating

Before updating, read the changelog to see what has changed and how it affects your Open Peer Power instance. Enter the jail using `iocage exec <jailname>`. Stop the Open Peer Power service:

{% highlight bash %}
service openpeerpower stop
{% endhighlight %}

Then, enter the `venv`:

{% highlight bash %}
su openpeerpower
cd /usr/local/share/openpeerpower
source ./bin/activate
{% endhighlight %}

Upgrade Open Peer Power:

{% highlight bash %}
pip3 install openpeerpower --upgrade
{% endhighlight %}

Log out of the `openpeerpower` user and start Open Peer Power:

{% highlight bash %}
exit
service openpeerpower start
{% endhighlight %}
