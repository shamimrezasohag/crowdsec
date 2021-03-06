!!! info 
    
    Please note that the `{{v0X.config.acquis_path}}` should be auto generated by the {{v0X.wizard.name}} in most case.

The acquisition configuration specifies lists of files to monitor and associated "labels".

The `type` label is mandatory as it's later used in the process to determine which parser(s) can handle lines coming from this source.

Acquisition can be found in `{{v0X.config.acquis_path}}`, for example :
<details>
  <summary>Acquisition example</summary>
```yaml
filenames:
  - /var/log/nginx/access-*.log
  - /var/log/nginx/error.log
labels:
  type: nginx
---
filenames:
  - /var/log/auth.log
labels:
  type: syslog

```
</details>


## Testing and viewing acquisition

### At startup

At startup, you will see the monitored files in `{{v0X.crowdsec.main_log}}` :

```
...
time="30-04-2020 08:57:25" level=info msg="Opening file '/var/log/nginx/http.access.log' (pattern:/var/log/nginx/http.access.log)"
time="30-04-2020 08:57:25" level=info msg="Opening file '/var/log/nginx/https.access.log' (pattern:/var/log/nginx/https.access.log)"
time="30-04-2020 08:57:25" level=info msg="Opening file '/var/log/nginx/error.log' (pattern:/var/log/nginx/error.log)"
time="30-04-2020 08:57:25" level=info msg="Opening file '/var/log/auth.log' (pattern:/var/log/auth.log)"
time="30-04-2020 08:57:25" level=info msg="Opening file '/var/log/syslog' (pattern:/var/log/syslog)"
time="30-04-2020 08:57:25" level=info msg="Opening file '/var/log/kern.log' (pattern:/var/log/kern.log)"
...
```

### At runtime

{{v0X.cli.name}} allows you to view {{v0X.crowdsec.name}} metrics info via the `metrics` command.
This allows you to see how many lines are coming from each source, and if they are parsed correctly.

You can see those metrics with the following command:
```
{{v0X.cli.bin}} metrics
```


<details>
  <summary>{{v0X.cli.name}} metrics example</summary>

```bash
## {{v0X.cli.bin}} metrics
...
INFO[0000] Acquisition Metrics:                         
+------------------------------------------+------------+--------------+----------------+------------------------+
|                  SOURCE                  | LINES READ | LINES PARSED | LINES UNPARSED | LINES POURED TO BUCKET |
+------------------------------------------+------------+--------------+----------------+------------------------+
| /var/log/nginx/http.access.log           |         47 |           46 |              1 |                     10 |
| /var/log/nginx/https.access.log          |         25 |           25 | -              |                     18 |
| /var/log/kern.log                        |     297948 |       297948 | -              |                  69421 |
| /var/log/syslog                          |     303868 |       297947 |           5921 |                  71539 |
| /var/log/auth.log                        |      63419 |        12896 |          50523 |                  20463 |
| /var/log/nginx/error.log                 |         65 |           65 | -              | -                      |
+------------------------------------------+------------+--------------+----------------+------------------------+
...
```

</details>


!!! info

    All these metrics are actually coming from {{v0X.crowdsec.name}}'s prometheus agent. See [prometheus](/Crowdsec/v0/observability/prometheus/) directly for more insights.


