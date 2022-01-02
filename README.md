# Prometheus_Service_File

> The service file tells systemd to run Prometheus as the root user, with the installed located /opt/prometheus/. Please change the path as per the installed location and user specified.
```
# cat /etc/systemd/system/prometheus.service
[Unit]
Description=Prometheus
Wants=network-online.target
After=network-online.target

[Service]
#User=prometheus
#Group=prometheus
User=root
Restart=on-failure
Type=simple
ExecStart=/opt/prometheus/prometheus \
    --config.file /opt/prometheus/prometheus.yml \
    --storage.tsdb.path /opt/prometheus/ \
    --web.console.templates=/opt/prometheus/consoles \
    --web.console.libraries=/opt/prometheus/console_libraries

[Install]
WantedBy=multi-user.target
```
# Result

```sh
# systemctl daemon-reload
# systemctl status prometheus
● prometheus.service - Prometheus
   Loaded: loaded (/etc/systemd/system/prometheus.service; disabled; vendor preset: disabled)
   Active: inactive (dead)
# systemctl start prometheus
# systemctl status prometheus
● prometheus.service - Prometheus
   Loaded: loaded (/etc/systemd/system/prometheus.service; disabled; vendor preset: disabled)
   Active: active (running) since Sun 2022-01-02 12:05:47 UTC; 1s ago
 Main PID: 30316 (prometheus)
   CGroup: /system.slice/prometheus.service
           └─30316 /opt/prometheus/prometheus --config.file /opt/prometheus/prometheus.yml --storage.tsdb.path /opt/prometheus/ --web.console.templates=/opt/prometheus...

Jan 02 12:05:48 ip-172-31-17-10.us-east-2.compute.internal prometheus[30316]: ts=2022-01-02T12:05:48.094Z caller=head.go:522 level=info component=tsdb msg="On-d…n=9.494µs
Jan 02 12:05:48 ip-172-31-17-10.us-east-2.compute.internal prometheus[30316]: ts=2022-01-02T12:05:48.095Z caller=head.go:528 level=info component=tsdb msg="Repl... while"
Jan 02 12:05:48 ip-172-31-17-10.us-east-2.compute.internal prometheus[30316]: ts=2022-01-02T12:05:48.105Z caller=tls_config.go:195 level=info component=web msg=...2=false
Jan 02 12:05:48 ip-172-31-17-10.us-east-2.compute.internal prometheus[30316]: ts=2022-01-02T12:05:48.109Z caller=head.go:599 level=info component=tsdb msg="WAL ...gment=0
Jan 02 12:05:48 ip-172-31-17-10.us-east-2.compute.internal prometheus[30316]: ts=2022-01-02T12:05:48.110Z caller=head.go:605 level=info component=tsdb msg="WAL ….031061ms
Jan 02 12:05:48 ip-172-31-17-10.us-east-2.compute.internal prometheus[30316]: ts=2022-01-02T12:05:48.111Z caller=main.go:945 level=info fs_type=XFS_SUPER_MAGIC
Jan 02 12:05:48 ip-172-31-17-10.us-east-2.compute.internal prometheus[30316]: ts=2022-01-02T12:05:48.111Z caller=main.go:948 level=info msg="TSDB started"
Jan 02 12:05:48 ip-172-31-17-10.us-east-2.compute.internal prometheus[30316]: ts=2022-01-02T12:05:48.112Z caller=main.go:1129 level=info msg="Loading configurat...eus.yml
Jan 02 12:05:48 ip-172-31-17-10.us-east-2.compute.internal prometheus[30316]: ts=2022-01-02T12:05:48.127Z caller=main.go:1166 level=info msg="Completed loading of conf…µs
Jan 02 12:05:48 ip-172-31-17-10.us-east-2.compute.internal prometheus[30316]: ts=2022-01-02T12:05:48.128Z caller=main.go:897 level=info msg="Server is ready to ...uests."
Hint: Some lines were ellipsized, use -l to show in full.
```
