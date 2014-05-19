# Percona Server Audit Log remixed
## Adding JSON and CSV

All copyrights and trademarks are property of their respective
owners...yadda...yadda.

Percona have released an alternative to the MySQL Enterprise Audit Plugin. The
first release of the plugin writes events to the audit.log file using two
formats of XML. This is in line with the specification of the Oracle version of
the plugin. Whilst this is tested and functional it is not a format favoured
with everyone. 

The patched version includes the ability to log out to the file using JSON and
CSV formatting. The default is JSON

JSON example
```
{"audit_record":{"name":"Query","record":"1952967_2014-05-16T11:41:01","timestamp":"2014-05-16T12:45:34
UTC","command_class":"select","connection_id":"96","status":0,"sqltext":"S
ELECT intcol1,charcol1 FROM t1 WHERE id =  '48'","user":"moore[moore] @
localhost []","host":"localhost","os_user":"","ip":""}}
```

CSV example
```
"Query","7459961_2014-05-16T21:31:25","2014-05-16T21:51:08
UTC","select","1",0,"select * from world.City","moore[moore] @ localhost
[]","localhost","",""
```

Hopefully the need for these patches are limited as the development of the
product evolves.

## Installation

Find out the location of your plugin directory
```
mysql> show global variables like 'plugin_dir';
+---------------+--------------------------+
| Variable_name | Value                    |
+---------------+--------------------------+
| plugin_dir    | /usr/lib64/mysql/plugin/ |
+---------------+--------------------------+
1 row in set (0.00 sec)
```

Copy the compiled plugin (shared object file) to the plugin directory
```
$ sudo cp audit_log.5x.so /usr/lib64/mysql/plugin/audit_log.so
```

Issue the install command for the audit plugin.
```
mysql> INSTALL PLUGIN audit_log SONAME 'audit_log.so';
```

Review the defaults for the plugin option variables.
```
mysql> show global variables like '%audit%';
+--------------------------+--------------+
| Variable_name            | Value        |
+--------------------------+--------------+
| audit_log_buffer_size    | 1048576      |
| audit_log_file           | audit.log    |
| audit_log_flush          | OFF          |
| audit_log_format         | JSON         |
| audit_log_policy         | ALL          |
| audit_log_rotate_on_size | 0            |
| audit_log_rotations      | 0            |
| audit_log_strategy       | ASYNCHRONOUS |
+--------------------------+--------------+
8 rows in set (0.00 sec)
```
