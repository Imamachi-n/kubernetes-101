# Kubernetesことはじめ

## マニフェストとリソースの作成・削除・更新

```bash
# podの作成
$ kubectl create -f sample-pod.yaml

# podの作成（推奨）
$ kubectl apply -f sample-pod.yaml

# podの状態確認
$ kubectl get pod
NAME         READY   STATUS    RESTARTS   AGE
sample-pod   1/1     Running   0          25s

# podの削除(強制的にゼロタイムで削除)
$ kubectl delete -f sample-pod.yaml --grace-period 0 --force
```

## マニフェストファイルの設計

```bash
# DeployementとServiceの同時実行
$ kubectl apply -f sample-multi-resource-manifest.yaml
deployment.apps/order1-deployment created
service/order2-service created

# podの状態確認
$ kubectl get pod
NAME                                 READY   STATUS    RESTARTS   AGE
order1-deployment-86b68b4c5b-nbw6p   1/1     Running   0          11s
order1-deployment-86b68b4c5b-v22tx   1/1     Running   0          11s
order1-deployment-86b68b4c5b-zljtg   1/1     Running   0          12s

# podの削除(強制的にゼロタイムで削除)
$ kubectl delete -f sample-multi-resource-manifest.yaml --grace-period 0 --force
```

### 複数のマニフェストファイルを同時に適応・削除する

```bash
$ kubectl apply -f ./ -R
deployment.apps/order1-deployment created
service/order2-service created
pod/sample-pod created

$ kubectl delete -f ./ -R
deployment.apps "order1-deployment" deleted
service "order2-service" deleted
pod "sample-pod" deleted
```

### ラベル

```bash
$ kubectl apply -f ./ -R
pod/sample-label created
deployment.apps/order1-deployment created
service/order2-service created
pod/sample-pod created

# label1を表示する
$ kubectl get pod -L label1
NAME                                 READY   STATUS    RESTARTS   AGE   LABEL1
order1-deployment-86b68b4c5b-b8m79   1/1     Running   0          45s
order1-deployment-86b68b4c5b-svwgj   1/1     Running   0          45s
order1-deployment-86b68b4c5b-tnfql   1/1     Running   0          45s
sample-label                         1/1     Running   0          45s   development
sample-pod                           1/1     Running   0          45s

# label1を持つPodのみ表示する
$ kubectl get pod -L label1 -l label1
NAME           READY   STATUS    RESTARTS   AGE   LABEL1
sample-label   1/1     Running   0          1m    development

# label1がdevelopmentであるPodのみ表示する
$ kubectl get pod -L label1 -l label1=development
NAME           READY   STATUS    RESTARTS   AGE   LABEL1
sample-label   1/1     Running   0          2m    development

# お片付け
$ kubectl delete -f ./ -R
```