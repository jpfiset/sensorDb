# sensorDb

```
sudo su -l postgres

export SDBUSER=<user>

createuser $SDBUSER 

createdb --owner=$SDBUSER observations

psql --file=./observations.sql observations

psql -d observations -c "COPY device_sensor_profiles (device_type,manufacturer,manufacturer_device_name,sensor_label,sensor_type_of_measurement,sensor_unit_of_measurement,sensor_accuracy,sensor_precision,sensor_height_in_metres) FROM STDIN CSV HEADER;" < device_sensor_profiles.csv

for tbl in `psql -qAt -c "select tablename from pg_tables where schemaname = 'public';" observations` ; do  psql -c "alter table $tbl owner to $SDBUSER" observations ; done

for tbl in `psql -qAt -c "select sequence_name from information_schema.sequences where sequence_schema = 'public';" observations` ; do  psql -c "alter table $tbl owner to $SDBUSER" observations ; done

for tbl in `psql -qAt -c "select table_name from information_schema.views where table_schema = 'public';" observations` ; do  psql -c "alter table $tbl owner to $SDBUSER" observations ; done

for tbl in `psql -qAt -c "select tablename from pg_tables where schemaname = 'topology';" observations` ; do  psql -c "alter table $tbl owner to $SDBUSER" observations ; done

for tbl in `psql -qAt -c "select sequence_name from information_schema.sequences where sequence_schema = 'topology';" observations` ; do  psql -c "alter table $tbl owner to $SDBUSER" observations ; done
```

# Queries

select name,responsible_party,ST_AsText(coordinates),elevation from locations;


# Dump / Restore

> pg_dump observations > 201XMMDD_observations_data.txt

> psql observations < 201XMMDD_observations_data.txt