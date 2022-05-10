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
├── k8s-helidon-app [Sample Java Application(for JVMChaos)]
│   ├── build_spec.yaml
│   ├── Dockerfile
│   ├── Dockerfile.jlink
│   ├── Dockerfile.native
│   ├── kubernetes
│   │   ├── app.yaml
│   │   ├── ingress.yaml
│   │   └── ns-config.yaml
│   ├── pom.xml
│   ├── README.md
│   └── src
│       ├── main
│       │   ├── java
│       │   │   └── me
│       │   │       └── shukawam
│       │   │           ├── GreetingProvider.java
│       │   │           ├── GreetResource.java
│       │   │           ├── NamespaceProvider.java
│       │   │           ├── NamespaceResource.java
│       │   │           └── package-info.java
│       │   └── resources
│       │       ├── application.yaml
│       │       ├── logging.properties
│       │       └── META-INF
│       │           ├── beans.xml
│       │           ├── microprofile-config.properties
│       │           └── native-image
│       │               └── reflect-config.json
│       └── test
│           └── java
│              └── me
│                  └── shukawam
│                       └── MainTest.java
├── kubernetes
│   ├── chaos-mesh-examples [Chaos Mesh 関連の Manifest]
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
