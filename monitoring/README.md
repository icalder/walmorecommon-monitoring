# Monitoring
Some sources:

https://flood-warning-information.service.gov.uk/station/2051

https://environment.data.gov.uk/flood-monitoring/doc/reference

[Historic river level data](https://riverlevels.uk/river-severn-minsterworth#.X9Cbctj7Q2w)

## Setup
First, run init.sh to create data folders for the containers to mount.

## Influxdb
```
docker exec -it influxdb bash
cd /tmp
influx -precision rfc3339 -database walmorecommon
show databases
show measurements
select value from "Minsterworth-15_min-mASD"

http://alarmpi:8086/query?q=SHOW%20DATABASES
```

## Telegraf
https://github.com/influxdata/telegraf/blob/master/docs/CONFIGURATION.md

https://www.influxdata.com/blog/connecting-the-things-network-to-influxdb/

https://console.thethingsnetwork.org/

```
# to generate a default telegraf config file:
docker compose run --rm telegraf telegraf config > telegraf-example.conf

# to test telegraf
docker compose run --rm telegraf --test
```

## Grafana
Example queries
```
SELECT mean("value") FROM "15_min-river-mASD" WHERE time >= now() - 6h GROUP BY time(20s), "station" fill(null)

SELECT mean("value") FROM "15_min-river-mASD" WHERE $timeFilter GROUP BY time($__interval), "station" fill(null)

SELECT mean("value") FROM "15_min-rainfall-mm" WHERE $timeFilter GROUP BY time($__interval), "station" fill(null)

SELECT mean("temperature") AS "temperature" FROM "walmore-ttn-water-level-test" WHERE $timeFilter GROUP BY time($__interval) fill(null)

SELECT mean("humidity") AS "humidity" FROM "walmore-ttn-water-level-test" WHERE $timeFilter GROUP BY time($__interval) fill(null)
```