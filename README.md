# ochacafe-chaos-engineering

2022/05/11 に開催された OCHaCafe Season5 - 実験！カオスエンジニアリングで使用したリソースです。
主に、[Chaos Mesh](https://chaos-mesh.org/) を用いて [WordPress](https://wordpress.com/ja/) へ様々なカオス実験を行います。

## overview

```bash
.
├── .github
│   └── workflows
│   │   └── chaos-mesh.yaml [GitHub Actions と Chaos Mesh の統合例]
├── docs
│   └── install-chaos-mesh.md [OKE への Chaos Mesh のインストール方法]
├── kubernetes
│   ├── chaos-mesh [Chaos Mesh 関連の Manifest]
│   │   ├── chaos-workflow.yaml
│   │   ├── nw-delay.yaml
│   │   ├── pod-kill-scheduled.yaml
│   │   └── pod-kill.yaml
│   ├── grafana [Grafana 関連の Manifest]
│   │   ├── grafana-ingress.yaml
│   │   └── grafana.yaml [Chaos Mesh の DataSource Plugin を含んだ Manifest]
│   ├── prometheus [Prometheus 関連の Manifest]
│   │   └── prometheus-ingress.yaml
│   └── wordpress [WordPress 関連のリソース]
│       ├── mysql-test-secret.yaml
│       ├── mysql.yaml
│       ├── wordpress-ingress.yaml
│       └── wordpress.yaml
└── README.md
```

```bash
├── docs
│   └── install-chaos-mesh.md
├── .github
│   └── workflows
│       └── chaos-mesh.yaml [GitHub Actions と Chaos Mesh の統合例]
├── kubernetes
│   ├── chaos-mesh-scenario
│   │   └── pod-kill.yaml [Chaos Mesh 関連の Manifest]
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
