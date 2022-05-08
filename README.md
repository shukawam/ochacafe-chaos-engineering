# ochacafe-chaos-engineering

2022/05/11 に開催された [OCHaCafe Season5 - 実験！カオスエンジニアリング](https://ochacafe.connpass.com/event/243297/)で使用したリソースです。

## overview

```bash
.
├── docs
│   └── install-chaos-mesh.md
├── .github
│   └── workflows
│       └── chaos-mesh.yaml [GitHub Actions と Chaos Mesh の統合例]
├── kubernetes
│   ├── chaos-mesh-scenario [Chaos Mesh 関連の Manifest]
│   │   ├── complex-scenario.yaml
│   │   └── network-latency-workflow.yaml
│   ├── grafana [Grafana 関連の Manifest]
│   │   ├── grafana-ingress.yaml
│   │   └── grafana.yaml [Chaos Mesh の DataSource Plugin を含んだ Manifest]
│   ├── influxdb [InfluxDB 関連の Manifest]
│   │   ├── influxdb-ingress.yaml
│   │   ├── influxdb-secret.yaml
│   │   └── influxdb.yaml
│   ├── k6 [k6 関連の Manifest]
│   │   ├── example [k6 のサンプルスクリプト]
│   │   │   └── script.js
│   │   ├── k6-configmap.yaml
│   │   └── k6-dummy-client.yaml
│   └── wordpress [WordPress 関連の Manifest]
│       ├── chaos
│       │   ├── mysql-chaos.yaml
│       │   └── toxiproxy-ingress.yaml
│       ├── mysql-secret.yaml
│       ├── mysql.yaml
│       ├── wordpress-ingress.yaml
│       └── wordpress.yaml
└── README.md
```
