# NET HEALER 

NET HEALER centralizes DDoS Attack Reports from FastNetMon collectors, allowing custom notification / mitigation rules, as well integration with lossy count non-gaussian algorithm to help anomaly detection and avoid false positives.

## NET HEALER Stages example
- cleared - no Attack Reports received for any /32 target
- warning - less than 3 Attack Reports received for /32 target(s)
- critical - more than [x] Attack Reports received for /32 target(s)
- under_attack - more than critical :)

Each FNM ban = 1 NET HEALER /32 target report
Lower the ban time, faster NET HEALER advance it stages
Suggested FNM ban time: 30 seconds (from cleared to warning 90 seconds)

## Actions
 - email
 - flowdock
 - pagerduty
 * integrations should be moved to plugins/ in a future

## Requirements
- FastNetMon: a super cool tool written by Pavel Odintsov - https://github.com/FastVPSEestiOu/fastnetmon
- Morgoth (https://github.com/nathanielc/morgoth)
- Redis (https://github.com/antirez/redis)
- InfluxDB (https://github.com/influxdb/influxdb)
- Grafana (https://github.com/grafana/grafana)
- 
##Installation
0. FastNetMon (FNM) should be configured to use:
 - Redis (https://github.com/FastVPSEestiOu/fastnetmon/blob/master/docs/REDIS.md)
 - InfluxDB (https://github.com/FastVPSEestiOu/fastnetmon/blob/master/docs/INFLUXDB_INTEGRATION.md)
1. install ruby (https://www.ruby-lang.org/en/documentation/installation/)
2. `$ gem install bundler`
3. `$ bundle install`
4. `$ bundle exec script/bootstrap`
5. Populate `.env` with a config
6. `$ bundle exec script/start`

optional: Install Grafana for cool dashboard viz (https://github.com/grafana/grafana)


##How to query the API

### GET /healer/v1/ddos/status => query DDoS status
```
{
  "status": "clear",
  "timestamp": "20150913-115403"
}
```

### GET /healer/v1/ddos/reports => query current DDoS reports

```json
{
    "reports": {
        "200.200.200.10": {
            "fqdn": "nethealer.hostingxpto.com",
            "attack_type": "udp_flood",
            "alerts": 2,
            "protocol": [
                "udp"
            ],
            "incoming": {
                "total": {
                    "mbps": 0.96,
                    "pps": 1486,
                    "flows": 128
                },
                "tcp": {
                    "mbps": 1.71,
                    "pps": 2654,
                    "syn": {
                        "mbps": 0.08,
                        "pps": 109
                    }
                },
                "udp": {
                    "mbps": 2761,
                    "pps": 779884
                },
                "icmp": {
                    "mbps": 0,
                    "pps": 0
                }
            }
        }
    }
}
```

### GET /healer/v1/ddos/reports/capture => query current DDoS reports + packet capture

### GET /healer/v1/ddos/brief => query DDoS /32 targets brief

### WORK IN PROGRESS.

PRs are more than welcome !
