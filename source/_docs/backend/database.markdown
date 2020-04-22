---
title: "Database"
description: "Details about the database used by Open Peer Power."
redirect_from: /details/database/
---

Database is used in by Open Peer Power as history and tracker only, to store the events and its parameters. The default database used by Open Peer Power is [SQLite](https://www.sqlite.org/), and the database file is stored in your [configuration directory](/getting-started/configuration/) (e.g., `<path to config dir>/.openpeerpower/open-peer-power_v2.db`). If you prefer to run a database server (e.g.,  PostgreSQL), use the [`recorder` component](/integrations/recorder/).

To work with the SQLite database manually from the command-line, you will need an [installation](http://www.sqlitetutorial.net/download-install-sqlite/) of `sqlite3`. Alternatively [DB Browser for SQLite](http://sqlitebrowser.org/) provides a viewer for exploring the database data and an editor for executing SQL commands.
First load your database with `sqlite3`:

{% highlight bash %}
$ sqlite3 open-peer-power_v2.db
SQLite version 3.13.0 2016-05-18 10:57:30
Enter ".help" for usage hints.
sqlite>
{% endhighlight %}

It helps to set some options to make the output more readable:

{% highlight bash %}
sqlite> .header on
sqlite> .mode column
{% endhighlight %}

You could also start `sqlite3` and attach the database later. Not sure what database you are working with? Check it, especially if you are going to delete data.

{% highlight bash %}
sqlite> .databases
seq  name             file
---  ---------------  ----------------------------------------------------------
0    main             /home/fab/.openpeerpower/open-peer-power_v2.db
{% endhighlight %}

### Schema

Get all available tables from your current Open Peer Power database:

{% highlight bash %}
sqlite> SELECT sql FROM sqlite_master;

-------------------------------------------------------------------------------------
CREATE TABLE events (
	event_id INTEGER NOT NULL,
	event_type VARCHAR(32),
	event_data TEXT,
	origin VARCHAR(32),
	time_fired DATETIME,
	created DATETIME,
	PRIMARY KEY (event_id)
)
CREATE INDEX ix_events_event_type ON events (event_type)
CREATE TABLE recorder_runs (
	run_id INTEGER NOT NULL,
	start DATETIME,
	"end" DATETIME,
	closed_incorrect BOOLEAN,
	created DATETIME,
	PRIMARY KEY (run_id),
	CHECK (closed_incorrect IN (0, 1))
)
CREATE TABLE states (
	state_id INTEGER NOT NULL,
	domain VARCHAR(64),
	entity_id VARCHAR(64),
	state VARCHAR(255),
	attributes TEXT,
	event_id INTEGER,
	last_changed DATETIME,
	last_updated DATETIME,
	created DATETIME,
	PRIMARY KEY (state_id),
	FOREIGN KEY(event_id) REFERENCES events (event_id)
)
CREATE INDEX states__significant_changes ON states (domain, last_updated, entity_id)
CREATE INDEX states__state_changes ON states (last_changed, last_updated, entity_id)
CREATE TABLE sqlite_stat1(tbl,idx,stat)
{% endhighlight %}

To only show the details about the `states` table (since we are using that one in the next examples):

{% highlight bash %}
sqlite> SELECT sql FROM sqlite_master WHERE type = 'table' AND tbl_name = 'states';
{% endhighlight %}

### Query

The identification of the available columns in the table is done and we are now able to create a query. Let's list your Top 10 entities:

{% highlight bash %}
sqlite> .width 30, 10,
sqlite> SELECT entity_id, COUNT(*) as count FROM states GROUP BY entity_id ORDER BY count DESC LIMIT 10;
entity_id                       count
------------------------------  ----------
sensor.cpu                      28874
sun.sun                         21238
sensor.time                     18415
sensor.new_york                 18393
cover.kitchen_cover             17811
switch.mystrom_switch           14101
sensor.internet_time            12963
sensor.solar_angle1             11397
sensor.solar_angle              10440
group.all_switches              8018
{% endhighlight %}

### Delete

If you don't want to keep certain entities, you can delete them permanently:

{% highlight bash %}
sqlite> DELETE FROM states WHERE entity_id="sensor.cpu";
{% endhighlight %}

The `VACUUM` command cleans your database.

{% highlight bash %}
sqlite> VACUUM;
{% endhighlight %}

For a more interactive way of working with the database, check the [Data Science Portal](https://data.openpeerpower.io/).
