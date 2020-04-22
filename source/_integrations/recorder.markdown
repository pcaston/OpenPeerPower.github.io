---
title: Recorder
description: Instructions on how to configure the data recorder for Open Peer Power.
ha_category:
  - History
ha_release: pre 0.7
ha_quality_scale: internal
ha_domain: recorder
excerpt: none
---

The `recorder` integration is responsible for storing details in a database, which then are handled by the [`history` integration](/integrations/history/).

<div class='note'>

This integration constantly saves data. If you use the default configuration, the data will be saved on the media Open Peer Power is installed on. In case of Raspberry Pi with an SD card, it might affect your system's reaction time and life expectancy of the storage medium (the SD card). It is therefore recommended to store the data elsewhere (e.g., another system) or limit the amount of stored data (e.g., by excluding devices).

</div>

Open Peer Power uses [SQLAlchemy](https://www.sqlalchemy.org/), which is an Object Relational Mapper (ORM). This means that you can use **any** SQL backend for the recorder that is supported by SQLAlchemy, like [MySQL](https://www.mysql.com/), [MariaDB](https://mariadb.org/), [PostgreSQL](https://www.postgresql.org/), or [MS SQL Server](https://www.microsoft.com/en-us/sql-server/).

The default database engine is [SQLite](https://www.sqlite.org/) which does not require any configuration. The database is stored in your Open Peer Power configuration directory ('/config/') and is named `open-peer-power_v2.db`.

To change the defaults for the `recorder` integration in your installation, add the following to your `configuration.yaml` file:

{% highlight yaml %}
# Example configuration.yaml entry
recorder:
{% endhighlight %}

Defining domains and entities to `exclude` (aka. blacklist) is convenient when you are basically happy with the information recorded, but just want to remove some entities or domains. Usually, these are entities/domains that do not change or rarely change (like `updater` or `automation`).

{% highlight yaml %}
# Example configuration.yaml entry with exclude
recorder:
  purge_keep_days: 5
  db_url: sqlite:////home/user/.openpeerpower/test
  exclude:
    domains:
      - automation
      - updater
    entities:
      - sun.sun # Don't record sun data
      - sensor.last_boot # Comes from 'systemmonitor' sensor platform
      - sensor.date
    event_types:
      - call_service # Don't record service calls
{% endhighlight %}

define domains and entities to record by using the `include` configuration (aka. whitelist) is convenient if you have a lot of entities in your system and your `exclude` lists possibly get very large, so it might be better just to define the entities or domains to record.

{% highlight yaml %}
# Example configuration.yaml entry with include
recorder:
  include:
    domains:
      - sensor
      - switch
      - media_player
{% endhighlight %}

You can also use the `include` list to define the domains/entities to record, and exclude some of those within the `exclude` list. This makes sense if you, for instance, include the `sensor` domain, but want to exclude some specific sensors. Instead of adding every sensor entity to the `include` `entities` list just include the `sensor` domain and exclude the sensor entities you are not interested in.

{% highlight yaml %}
# Example configuration.yaml entry with include and exclude
recorder:
  include:
    domains:
      - sensor
      - switch
      - media_player
  exclude:
    entities:
      - sensor.last_boot
      - sensor.date
{% endhighlight %}

If you only want to hide events from your history, take a look at the [`history` integration](/integrations/history/). The same goes for the [logbook](/integrations/logbook/). But if you have privacy concerns about certain events or want them in neither the history or logbook, you should use the `exclude`/`include` options of the `recorder` integration. That way they aren't even in your database, you can reduce storage and keep the database small by excluding certain often-logged events (like `sensor.last_boot`).

### Service `purge`

Call the service `recorder.purge` to start a purge task which deletes events and states older than x days, according to `keep_days` service data.

| Service data attribute | Optional | Description                                                                                                                                                           |
| ---------------------- | -------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `keep_days`            | yes      | The number of history days to keep in recorder database (defaults to the integration `purge_keep_days` configuration)                                                 |
| `repack`               | yes      | Rewrite the entire database, possibly saving some disk space. Only supported for SQLite and requires at least as much disk space free as the database currently uses. |

## Custom database engines

| Database engine        | `db_url`                                                                                     |
| :--------------------- | :------------------------------------------------------------------------------------------- |
| SQLite                 | `sqlite:////PATH/TO/DB_NAME`                                                                 |
| MariaDB                | `mysql+pymysql://SERVER_IP/DB_NAME?charset=utf8`                                             |
| MariaDB                | `mysql+pymysql://user:password@SERVER_IP/DB_NAME?charset=utf8`                               |
| MariaDB (omit pymysql) | `mysql://user:password@SERVER_IP/DB_NAME?charset=utf8`                                       |
| MySQL                  | `mysql://SERVER_IP/DB_NAME?charset=utf8`                                                     |
| MySQL                  | `mysql://user:password@SERVER_IP/DB_NAME?charset=utf8`                                       |
| PostgreSQL             | `postgresql://SERVER_IP/DB_NAME`                                                             |
| PostgreSQL             | `postgresql://user:password@SERVER_IP/DB_NAME`                                               |
| PostgreSQL (Socket)    | `postgresql://@/DB_NAME`                                                                     |
| MS SQL Server          | `mssql+pyodbc://username:password@SERVER_IP/DB_NAME?charset=utf8;DRIVER={DRIVER};Port=1433;` |

<div class='note'>

Some installations of MariaDB/MySQL may require an ALTERNATE_PORT (3rd-party hosting providers or parallel installations) to be added to the SERVER_IP, e.g., `mysql://user:password@SERVER_IP:ALTERNATE_PORT/DB_NAME?charset=utf8`.

</div>

<div class='note'>

If using an external MariaDB backend (e.g., running on a separate NAS) with Open Peer Power, you should omit `pymysql` from the URL. `pymysql` is not included in the base Docker image, and is not necessary for this to work.

</div>

<div class='note'>

Unix Socket connections always bring performance advantages over TCP, if the database is on the same host as the `recorder` instance (i.e., `localhost`).

</div>

<div class='note warning'>

If you want to use Unix Sockets for PostgreSQL you need to modify the `pg_hba.conf`. See [PostgreSQL](#postgresql)

</div>

<div class='note warning'>

If you are using the default `FULL` recovery model for MS SQL Server you will need to manually backup your log file to prevent your transaction log from growing too large. It is recommended you change the recovery model to `SIMPLE` unless you are worried about data loss between backups.

</div>

### Database startup

If you are running a database server instance on the same server as Open Peer Power then you must ensure that this service starts before Open Peer Power. For a Linux instance running Systemd (Raspberry Pi, Debian, Ubuntu and others) you should edit the service file.
To help facilitate this, db_max_retry and db_retry_wait variables have been added to ensure the recorder retries the connection to your database enough times, for your database to start up.

{% highlight bash %}
sudo nano /etc/systemd/system/open-peer-power@openpeerpower.service
{% endhighlight %}

and add the service for the database, for example, PostgreSQL:

{% highlight txt %}
[Unit]
Description=Open Peer Power
After=network.target postgresql.service
{% endhighlight %}

Save the file then reload `systemctl`:

{% highlight bash %}
sudo systemctl daemon-reload
{% endhighlight %}

## Installation notes

Not all Python bindings for the chosen database engine can be installed directly. This section contains additional details that should help you to get it working.

### MariaDB and MySQL

If you are in a virtual environment, don't forget to activate it before installing the `mysqlclient` Python package described below.

{% highlight bash %}
pi@openpeerpower:~ $ sudo -u openpeerpower -H -s
openpeerpower@openpeerpower:~$ source /srv/openpeerpower/bin/activate
(openpeerpower) openpeerpower@openpeerpower:~$ pip3 install mysqlclient
{% endhighlight %}

For MariaDB you may have to install a few dependencies. If you're using MariaDB version 10.2, `libmariadbclient-dev` was renamed to `libmariadb-dev`. If you're using MariaDB 10.3, the package `libmariadb-dev-compat` must also be installed. For MariaDB v10.0.34 only `libmariadb-dev-compat` is needed. Please install the correct packages based on your MariaDB version.

On the Python side we use the `mysqlclient`:

{% highlight bash %}
sudo apt-get install libmariadbclient-dev libssl-dev
pip3 install mysqlclient
{% endhighlight %}

For MySQL you may have to install a few dependencies. You can choose between `pymysql` and `mysqlclient`:

{% highlight bash %}
sudo apt-get install default-libmysqlclient-dev libssl-dev
pip3 install mysqlclient
{% endhighlight %}

After installing the dependencies, it is required to create the database manually. During the startup, Open Peer Power will look for the database specified in the `db_url`. If the database doesn't exist, it will not automatically create it for you.

Once Open Peer Power finds the database, with the right level of permissions, all the required tables will then be automatically created and the data will be populated accordingly.

### PostgreSQL

For PostgreSQL you may have to install a few dependencies:

{% highlight bash %}
sudo apt-get install postgresql-server-dev-X.Y
pip3 install psycopg2
{% endhighlight %}

For using Unix Sockets, add the following line to your [`pg_hba.conf`](https://www.postgresql.org/docs/current/static/auth-pg-hba-conf.html):

`local  DB_NAME USER_NAME peer`

Where `DB_NAME` is the name of your database and `USER_NAME` is the name of the user running the Open Peer Power instance (see [securing your installation](/docs/configuration/securing/)).

Reload the PostgreSQL configuration after that:
{% highlight bash %}
$ sudo -i -u postgres psql -c "SELECT pg_reload_conf();"
 pg_reload_conf
----------------
 t
(1 row)
{% endhighlight %}
A service restart will work as well.

### MS SQL Server

For MS SQL Server you will have to install a few dependencies:

{% highlight bash %}
sudo apt-get install unixodbc-dev
pip3 install pyodbc
{% endhighlight %}

If you are in a virtual environment, don't forget to activate it before installing the pyodbc package.

{% highlight bash %}
sudo -u openpeerpower -H -s
source /srv/openpeerpower/bin/activate
pip3 install pyodbc
{% endhighlight %}

You will also need to install an ODBC Driver. Microsoft ODBC drivers are recommended, however FreeTDS is available for systems that are not supported by Microsoft. Instrucitons for installing the Microsoft ODBC drivers can be found [here](https://docs.microsoft.com/en-us/sql/connect/odbc/linux-mac/installing-the-microsoft-odbc-driver-for-sql-server).

<div class='note'>

If you are using Hass.io, FreeTDS is already installed for you. The db_url you need to use is `mssql+pyodbc://username:password@SERVER_IP/DB_NAME?charset=utf8;DRIVER={FreeTDS};Port=1433;`.

</div>
