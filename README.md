# Prometheus Alerts Playground

This example run 3 containers:
- Prometheus
- AlertManager
- A SMTP server.

It's created 2 alert rules with the file `playground/prometheus/prometheus.rules.yml`,
and the Alert Manager container is configured to send mail to root@localhost for every alert.

One alert rule should be fired after a minute and the other one should be fired after 5 minutes.

### Getting started

```bash
cd playground
docker-compose up -d

docker exec -it smtp tail -F /var/mail/mail
```

The expected behavior is that after the first mail will have been sent the file will be created and the tail command will handle it and  print the mail content.

### Debugging and Troubleshooting

* Open container logs.
* Try the queries defined on `playground/prometheus/prometheus.rules.yml` by using [Prometheus Graph UI]. [Queries example](http://localhost:9090/graph?g0.range_input=1h&g0.expr=rate(prometheus_tsdb_head_samples_appended_total%7Bjob%3D%22prometheus%22%7D%5B1m%5D)&g0.tab=0&g1.range_input=1h&g1.expr=scrape_duration_seconds%7Bjob%3D%22prometheus%22%7D&g1.tab=0).
* On the [Alert tab] inspect the state of the alert rules.
* Go to [Alertmanager UI] and check the alerts were properly received by AlertManager.
* ssh to smtp container and inspect the local `exim`.

** Useful resources:**
* [Grafana / Step-by-step guide to setting up Prometheus Alertmanager with Slack, PagerDuty, and Gmail](https://grafana.com/blog/2020/02/25/step-by-step-guide-to-setting-up-prometheus-alertmanager-with-slack-pagerduty-and-gmail/)
* [NSRC - Workshop 2005 Exim Practical](https://nsrc.org/workshops/2005/pre-SANOG-VI/bc/mail/exim/EximPrac.htm)
* [Useful command to manage Exim](https://hostpapasupport.com/list-useful-commands-manage-exim-mail-server/)
* [Exim / Wiki / TestingExim](https://github.com/Exim/exim/wiki/TestingExim)

[Prometheus Graph UI]: http://localhost:9090/graph
[Alert tab]: http://localhost:9090/alerts
[Alertmanager UI]: http://localhost:9093/#/alerts
