# Zabbix partition setup on MySQL DB host

## Zabbix DB partition setup

### Recreate appropriate tables to use partitions

TODO

### Add procedures and events for partition maintenance

On Zabbix DB host run following

```bash
mysql -u root -p zabbix < partition_maintenance.sql
```

### Check if event scheduler is on

Make sure event scheduler is turned on

```bash
mysql -u root -p zabbix -e "SHOW GLOBAL VARIABLES LIKE 'event_scheduler';"
```

If it is `OFF` or `0` add following to `my.cnf` file

```bash
event_scheduler = 1
```

as well as enable it without database restart

```bash
SET GLOBAL event_scheduler=1;
```

## Partition monitoring

In order to keep track of last partition expiration following must be done

### Add Zabbix template

Import `zabbix_template_v*.xml` via Zabbix UI

### Zabbix agent userparams

Place `userparameter_mysql_partitions.conf` in `/etc/zabbix/zabbix_agentd.d/` (default)

The script used in user param rely on `.my.cnf` file available for credentials. Place it in zabbix user home directory. File must include user and password.

Example

```bash
$ grep zabbix /etc/passwd | awk -F':' '{print $6}'
/var/lib/zabbix/
$ sudo touch /var/lib/zabbix/.my.cnf && sudo chown zabbix. /var/lib/zabbix/.my.cnf && sudo chmod 600 /var/lib/zabbix/.my.cnf
$ sudo vim /var/lib/zabbix/.my.cnf
```

`/var/lib/zabbix/.my.cnf` contents

```ini
[client]
user = root
password = <redacted>
```

## Todo

- Add `Recreate appropriate tables to use partitions`
- Add zabbix 4+ templates
