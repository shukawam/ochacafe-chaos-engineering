# first-step-pumba

## インストール

[https://github.com/alexei-led/pumba/releases](https://github.com/alexei-led/pumba/releases) から自身のプラットフォーム用のバイナリをインストールします。

Oracle Linux の場合

```bash
wget https://github.com/alexei-led/pumba/releases/download/0.9.0/pumba_linux_amd64
```

便宜上、alias を充てておきます。

```bash
alias pumba="pumba_linux_amd64"
```

バイナリを PATH の通った場所(`/usr/local/bin`, etc.)に配置します。

インストールの確認をします。

```bash
pumba --help
NAME:
   Pumba - Pumba is a resilience testing tool, that helps applications tolerate random Docker container failures: process, network and performance.

USAGE:
   pumba_linux_amd64 [global options] command [command options] containers (name, list of names, or RE2 regex if prefixed with "re2:")

VERSION:
   0.9.0 - 2e7ab7b (master) 2021-11-21T10:12:49+0200

AUTHOR:
   Alexei Ledenev <alexei.led@gmail.com>

COMMANDS:
   kill     kill specified containers
   exec     exec specified containers
   restart  restart specified containers
   stop     stop containers
   pause    pause all processes
   rm       remove containers
   stress   stress test a specified containers
   netem    emulate the properties of wide area networks
   help, h  Shows a list of commands or help for one command

GLOBAL OPTIONS:
   --host value, -H value       daemon socket to connect to (default: "unix:///var/run/docker.sock") [$DOCKER_HOST]
   --tls                        use TLS; implied by --tlsverify
   --tlsverify                  use TLS and verify the remote [$DOCKER_TLS_VERIFY]
   --tlscacert value            trust certs signed only by this CA (default: "/etc/ssl/docker/ca.pem")
   --tlscert value              client certificate for TLS authentication (default: "/etc/ssl/docker/cert.pem")
   --tlskey value               client key for TLS authentication (default: "/etc/ssl/docker/key.pem")
   --log-level value, -l value  set log level (debug, info, warning(*), error, fatal, panic) (default: "warning") [$LOG_LEVEL]
   --json, -j                   produce log in JSON format: Logstash and Splunk friendly [$LOG_JSON]
   --slackhook value            web hook url; send Pumba log events to Slack
   --slackchannel value         Slack channel (default #pumba) (default: "#pumba")
   --interval value, -i value   recurrent interval for chaos command; use with optional unit suffix: 'ms/s/m/h' (default: 0s)
   --label value                filter containers by labels, e.g '--label key=value' (multiple labels supported)
   --random, -r                 randomly select single matching container from list of target containers
   --dry-run                    dry run does not create chaos, only logs planned chaos commands [$DRY-RUN]
   --skip-error                 skip chaos command error and retry to execute the command on next interval tick
   --help, -h                   show help
   --version, -v                print the version
```

## Pumba を用いた実験例

### コンテナの kill

実験対象のコンテナを起動します。

```bash
docker run -d -it --name alpine alpine
```

コンテナの起動状況をモニタリングします。

```bash
watch -n 1 -d docker ps
```

実行結果

```bash
Every 1.0s: docker ps                                                                                Mon Apr 25 09:59:09 2022

CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
edce52cc96be        alpine              "/bin/sh"           5 seconds ago       Up 4 seconds                            alpin
e
```

別ターミナルから、コンテナを kill します。

```bash
pumba kill re2:alpine
```

最初のターミナルでコンテナが kill されたことが確認できます。

```bash
Every 1.0s: docker ps                                                                                Mon Apr 25 09:59:48 2022

CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES

```

### コンテナのリソース(CPU)に負荷をかける

実験対象のコンテナを起動します。

```bash
docker run -d -it --name alpine alpine
```

リソースの使用状況をモニタリングします。

```bash
docker exec -it alpine top
```

実行結果

```bash
Mem: 15081716K used, 1014724K free, 844976K shrd, 3824K buff, 9754160K cached
CPU:   0% usr  10% sys  81% nic   7% idle   0% io   0% irq   0% sirq
Load average: 2.80 2.71 2.60 5/511 29
  PID  PPID USER     STAT   VSZ %VSZ CPU %CPU COMMAND
    1     0 root     S     1684   0%   1   0% /bin/sh
   24     0 root     R     1588   0%   1   0% top
```

別ターミナルから負荷をかけます。

```bash
pumba stress \
--duration 10s \
--stress-image alexeiled/stress-ng:latest-ubuntu \
--stressors="--cpu 4 --timeout 60s" \
re2:alpine
```

CPU の使用率が上昇していることを確認できます。

```bash
Mem: 14916112K used, 1180328K free, 847208K shrd, 3824K buff, 9626956K cached
CPU:  60% usr  14% sys  25% nic   0% idle   0% io   0% irq   0% sirq
Load average: 2.66 2.68 2.60 16/553 29
  PID  PPID USER     STAT   VSZ %VSZ CPU %CPU COMMAND
    1     0 root     S     1684   0%   1   0% /bin/sh
   24     0 root     R     1588   0%   1   0% top
```

## ネットワークに遅延を加える

実験対象のコンテナを起動します。

```bash
docker run -d -it --name alpine alpine
```

ping の宛先用のコンテナを作成します。

```bash
docker run -d -it -p 80:80 --name nginx nginx:latest
```

宛先を確認します。

```bash
docker inspect nginx | grep -i ipaddress
```

実行結果

```bash
            "SecondaryIPAddresses": null,
            "IPAddress": "172.17.0.3",
                    "IPAddress": "172.17.0.3",
```

ping で疎通確認をします。

```bash
docker exec -it alpine ping 172.17.0.3
```

別ターミナルからネットワークの遅延を追加します。

```bash
pumba netem \
--tc-image "gaiadocker/iproute2" \
--duration 20s delay \
--time 1000 \
re2:alpine
```

確かに、追加した分遅延が含まれていることが確認できます。

```bash
PING 172.17.0.3 (172.17.0.3): 56 data bytes
64 bytes from 172.17.0.3: seq=0 ttl=64 time=0.121 ms
64 bytes from 172.17.0.3: seq=1 ttl=64 time=0.102 ms
64 bytes from 172.17.0.3: seq=2 ttl=64 time=0.082 ms
64 bytes from 172.17.0.3: seq=3 ttl=64 time=0.077 ms
64 bytes from 172.17.0.3: seq=4 ttl=64 time=0.081 ms
64 bytes from 172.17.0.3: seq=5 ttl=64 time=0.082 ms
64 bytes from 172.17.0.3: seq=6 ttl=64 time=996.062 ms
64 bytes from 172.17.0.3: seq=7 ttl=64 time=994.177 ms
64 bytes from 172.17.0.3: seq=8 ttl=64 time=1004.004 ms
64 bytes from 172.17.0.3: seq=9 ttl=64 time=1002.033 ms
64 bytes from 172.17.0.3: seq=10 ttl=64 time=992.070 ms
64 bytes from 172.17.0.3: seq=11 ttl=64 time=1003.925 ms
64 bytes from 172.17.0.3: seq=12 ttl=64 time=993.472 ms
...
```
