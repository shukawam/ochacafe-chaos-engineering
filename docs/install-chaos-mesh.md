# install-chaos-mesh

Oracle Container Engine for Kubernertes(以下、OKE)に [Chaos Mesh](https://chaos-mesh.org/) をインストールします。

## 手順

Helm を使ってインストールします。まずは、Helm repository を追加します。

```bash
helm repo add chaos-mesh https://charts.chaos-mesh.org
```

インストール可能なバージョンを確認します。

```bash
helm search repo chaos-mesh
```

実行結果は以下の通り

```bash
chaos-mesh/chaos-mesh   2.1.3           2.1.3           Chaos Mesh is a cloud-native Chaos Engineering ...
```

Chaos Mesh をインストールするための Namespace を作成します。

```bash
kubectl create ns chaos-testing
```

OKE にインストールします。

```bash
helm install chaos-mesh chaos-mesh/chaos-mesh -n=chaos-testing --set chaosDaemon.runtime=crio --set chaosDaemon.socketPath=/var/run/crio/crio.sock --version 2.1.3
```

インストールされたことを確認します。

```bash
kubectl get po -n chaos-testing
```

以下のように表示されればインストールは完了です。

```bash
NAME                                        READY   STATUS    RESTARTS   AGE
chaos-controller-manager-86665d8d8c-4wc4t   1/1     Running   0          18d
chaos-controller-manager-86665d8d8c-bctq5   1/1     Running   0          22h
chaos-controller-manager-86665d8d8c-mq65j   1/1     Running   0          18d
chaos-daemon-62n6v                          1/1     Running   0          18d
chaos-daemon-nkfm5                          1/1     Running   0          18d
chaos-daemon-sv9bg                          1/1     Running   0          18d
chaos-dashboard-7887bf5fcf-tnj5p            1/1     Running   0          18d
```

## ダッシュボードの外部公開

Load Balancer で `chaos-dashboard` を公開します。(LB の帯域はご自由に決定してください)

```bash
kubectl patch service chaos-dashboard \
--patch '{"spec": {"type": "LoadBalancer"}, "metadata": {"annotations": {"service.beta.kubernetes.io/oci-load-balancer-shape": "flexible", "service.beta.kubernetes.io/oci-load-balancer-shape-flex-min": "10", "service.beta.kubernetes.io/oci-load-balancer-shape-flex-max": "20"}}}' \
--namespace chaos-testing
```

割り当てられた Public IP を確認します。

```bash
kubectl get svc -n chaos-testing
```

実行結果配下の通り。

```bash
NAME                            TYPE           CLUSTER-IP      EXTERNAL-IP       PORT(S)                                 AGE
chaos-daemon                    ClusterIP      None            <none>            31767/TCP,31766/TCP                     18d
chaos-dashboard                 LoadBalancer   10.96.127.26    150.230.198.157   2333:30034/TCP                          18d
chaos-mesh-controller-manager   ClusterIP      10.96.195.166   <none>            443/TCP,10081/TCP,10082/TCP,10080/TCP   18d
```

ブラウザで、`http://<EXTERNA-IP>:2333` と指定すると、ダッシュボードの参照が可能です。
