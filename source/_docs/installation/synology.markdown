---
title: "Installation on a Synology NAS"
description: "Instructions to install Open Peer Power on a Synology NAS."
redirect_from: /getting-started/installation-synology/
---

<div class='note warning'>

Synology only provide Python 3.5.1, which is not compatible with Open Peer Power 0.65.0 or later. Until Synology offer an updated version of Python, Open Peer Power 0.64 is the most recent version that will be able to be installed. You can manually specify the version of Open Peer Power to install, for example to install version 0.64.3 you would do `./python3 -m pip install openpeerpower==0.64.3`

</div>

There are 3 alternatives, when using Open Peer Power on Synology NAS:
1. Using Open Peer Power Core on Docker
2. Directly running Open Peer Power Core on DSM
3. Using the Open Peer Power a VM (if you have an Intel based Synology)

Option 1 is described on the [Docker installation page](/docs/installation/docker/).

Option 3 uses the Synology Based Virtual Machine Manager. You can import the VDI image to be found at the [Open Peer Power installation page](/hassio/installation/). Download the image and add it to the image store. The go to "Virtual Machine" in the interface and create a new VM with the image you just added.

The main benefit from this method is that you can assign Open Peer Power its own IP number, so there is no risk regarding TCP/UDP port conflicts. USB dongles an be connected to the VM without the need to install a driver in DSM.

Option 2 is described below.

The following configuration has been tested on Synology 413j running DSM 6.0-7321 Update 1.

Running these commands will:

- Install Open Peer Power
- Enable Open Peer Power to be launched on `http://localhost:8123`

Using the Synology webadmin:

- Install python3 using the Synology Package Center
- Create a `openpeerpower` user and add to the "users" group

SSH onto your Synology & login as admin or root

- Log in with your own administrator account
- Switch to root using:

{% highlight bash %}
$ sudo -i
{% endhighlight %}

Check the path to python3 (assumed to be /volume1/@appstore/py3k/usr/local/bin)

{% highlight bash %}
# cd /volume1/@appstore/py3k/usr/local/bin
{% endhighlight %}

Install PIP (Python's package management system)

{% highlight bash %}
# ./python3 -m ensurepip
{% endhighlight %}

Use PIP to install the Open Peer Power package 0.64.3

{% highlight bash %}
# ./python3 -m pip install openpeerpower==0.64.3
{% endhighlight %}

Create a Open Peer Power configuration directory & switch to it

{% highlight bash %}
# mkdir /volume1/openpeerpower
# chown openpeerpower /volume1/openpeerpower 
# chmod 755 /volume1/openpeerpower
# cd /volume1/openpeerpower
{% endhighlight %}

Hint: alternatively you can also create a "Shared Folder" via Synology WebUI (e.g., via "File Station") - this has the advantage that the folder is visible via "File Station".

Create hass-daemon file using the following code (edit the variables in uppercase if necessary)

{% highlight bash %}
#!/bin/sh

# Package
PACKAGE="openpeerpower"
DNAME="Open Peer Power"

# Others
USER="openpeerpower"
PYTHON_DIR="/volume1/@appstore/py3k/usr/local/bin"
PYTHON="$PYTHON_DIR/python3"
HASS="$PYTHON_DIR/hass"
INSTALL_DIR="/volume1/openpeerpower"
PID_FILE="$INSTALL_DIR/open-peer-power.pid"
FLAGS="-v --config $INSTALL_DIR --pid-file $PID_FILE --daemon"
REDIRECT="> $INSTALL_DIR/open-peer-power.log 2>&1"

start_daemon ()
{
    sudo -u ${USER} /bin/sh -c "$PYTHON $HASS $FLAGS $REDIRECT;"
}

stop_daemon ()
{
    kill `cat ${PID_FILE}`
    wait_for_status 1 20 || kill -9 `cat ${PID_FILE}`
    rm -f ${PID_FILE}
}

daemon_status ()
{
    if [ -f ${PID_FILE} ] && kill -0 `cat ${PID_FILE}` > /dev/null 2>&1; then
        return
    fi
    rm -f ${PID_FILE}
    return 1
}

wait_for_status ()
{
    counter=$2
    while [ ${counter} -gt 0 ]; do
        daemon_status
        [ $? -eq $1 ] && return
        let counter=counter-1
        sleep 1
    done
    return 1
}

case $1 in
    start)
        if daemon_status; then
            echo ${DNAME} is already running
            exit 0
        else
            echo Starting ${DNAME} ...
            start_daemon
            exit $?
        fi
        ;;
    stop)
        if daemon_status; then
            echo Stopping ${DNAME} ...
            stop_daemon
            exit $?
        else
            echo ${DNAME} is not running
            exit 0
        fi
        ;;
        restart)
        if daemon_status; then
            echo Stopping ${DNAME} ...
            stop_daemon
            echo Starting ${DNAME} ...
            start_daemon
            exit $?
        else
            echo ${DNAME} is not running
            echo Starting ${DNAME} ...
            start_daemon
            exit $?
        fi
        ;;
    status)
        if daemon_status; then
            echo ${DNAME} is running
            exit 0
        else
            echo ${DNAME} is not running
            exit 1
        fi
        ;;
    log)
        echo ${LOG_FILE}
        exit 0
        ;;
    *)
        exit 1
        ;;
esac

{% endhighlight %}

Create links to Python folders to make things easier in the future:

{% highlight bash %}
# ln -s /volume1/@appstore/py3k/usr/local/bin/python3 python3
# ln -s /volume1/@appstore/py3k/usr/local/lib/python3.5/site-packages/openpeerpower openpeerpower
{% endhighlight %}

Set the owner and permissions on your configuration folder

{% highlight bash %}
# chown -R openpeerpower:users /volume1/openpeerpower
# chmod -R 664 /volume1/openpeerpower
{% endhighlight %}

Make the daemon file executable:

{% highlight bash %}
# chmod 755 /volume1/openpeerpower/hass-daemon
{% endhighlight %}

Update your firewall (if it is turned on the Synology device):

 - Go to your Synology control panel
 - Go to security 
 - Go to firewall
 - Go to Edit Rules
 - Click Create
 - Select Custom: Destination port "TCP"
 - Type "8123" in port
 - Click on OK
 - Click on OK again


Copy your `configuration.yaml` file into the configuration folder
That's it... you're all set to go

Here are some useful commands:

- Start Open Peer Power:

{% highlight bash %}
$ sudo /volume1/openpeerpower/hass-daemon start
{% endhighlight %}

- Stop Open Peer Power:

{% highlight bash %}
$ sudo /volume1/openpeerpower/hass-daemon stop
{% endhighlight %}

- Restart Open Peer Power:

{% highlight bash %}
$ sudo /volume1/openpeerpower/hass-daemon restart
{% endhighlight %}

- Upgrade Open Peer Power::

{% highlight bash %}
$  /volume1/@appstore/py3k/usr/local/bin/python3 -m pip install --upgrade openpeerpower
{% endhighlight %}
