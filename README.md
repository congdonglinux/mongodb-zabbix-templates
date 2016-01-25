# mongodb-zabbix-templates

#Zabbix templates for mongodb monitoring

For a long time I've used the Mikoomi mongodb template for mongodb monitoring. It was work fine for us for all mongodb 2.x versions. 
Since mongodb 3.x it fails and after a few patches I decide to made my own simple templates.
The main tasks of monitoring for us:

1. Watch and notify the support team if mongodb instances is not running or not reachable
2. Watch a basic mongodb and mongos opcounters
3. Watch and notify for Mongodb Replicasets health
4. Monitor more than one mongodb instance per host
5. Watch DiskIO counters - it runs well with outstanding third party Iostat-Disk-Utilization-Template (https://github.com/lesovsky/zabbix-extensions/tree/master/files/iostat)
6. Provide easy-to-use and easy-to-understand user view for staff.

Technically, for fine mongodb performance tuning you need much more information than provides this templates, but its harder to 
collect, store and visualize with Zabbix, and anyway it would be better to use specially designed mongodb tools. 
Google it for more information.

This package contains 3 templates for monitoring mongodb instances (Mongo-DB template), mongos instance (Mongos template) and replicaset health (Mongo-RS Health template).
By customising your hosts configurations with this templates you can monitor lagre mongodb clusters. For us it monitors 10 mongodb instances in sharded 5 big replicasets, 4 instances of small metadata database, config database replicaset, 5 mongos routers and backup server.

#How to install Mongo-DB template

Mongo-DB template is used to collect the basic opcounters and  
1. Downoad package from github.
2. Import xml templates to Zabbix server via browser
Configuration -> Templates -> Import template
3. Upload \*.pl files to zabbix server, put it to external scrips folder (/usr/lib/zabbix/externalscrips for my server), and
chown +x mongo\*.pl
4. Install on zabbix server required perl modules via command line
<pre><code>sudo cpan List::Util
sudo cpan MongoDB
sudo cpan Zabbix::Sender
sudo cpan Getopt::Long
</code></pre>
5. Navigate to hosts configuration in zabbix server and add Mongo-DB template to host with mongodb instance.
6. In Macros tab add the following data
<pre><code>{$MONGODB_HOSTNAME}  IP or DNS name of interface used by mongodb instance
{$MONGODB_USER}  User name
{$MONGODB_PASS}  Password
{$MONGODB_PORT}  Port used by mongodb instance
{$MONGODB_ZABBIX_NAME}  Server hostname, as it registered in zabbix (I use two different interfaces for each mongodb server, mongodb instance is using one, other services including zabbix agent - another).
</code></pre>

In a few minutes it starts to collect data. See the Misc: Data collector in zabbix latest data for easy debug.

For Mongo-DB graphs go to Monitoring -> Graphs, then select the host with mongodb instance and select Mongo-DB Opcounters and Mongo-DB Network IO graphs. The best way to use this data by your team is to compose special zabbix screens with this graphs with all your servers. How to do this see https://www.zabbix.com/documentation/2.4/manual/config/visualisation/screens



