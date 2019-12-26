## デプロイメントの生成と削除

### デプロイメントの生成

```bash
$ kubectl apply -f ./deployment1.yml
deployment.apps/web-deploy created
```

### デプロイメントの状態を確認

```bash
$ kubectl get deploy
NAME         READY   UP-TO-DATE   AVAILABLE   AGE
web-deploy   3/3     3            3           10d
```

### レプリカセットの状態を確認

```bash
$ kubectl get rs
NAME                    DESIRED   CURRENT   READY   AGE
web-deploy-6b5f67b747   3         3         3       10d
```

### ポッドの状態（IP アドレスとノード）

```bash
$ kubectl get po -o wide
NAME                          READY   STATUS    RESTARTS   AGE   IP          NODE             NOMINATED NODE   READINESS GATES
web-deploy-6b5f67b747-69ntd   1/1     Running   0          10d   10.1.0.10   docker-desktop   <none>           <none>
web-deploy-6b5f67b747-cstf4   1/1     Running   0          10d   10.1.0.12   docker-desktop   <none>           <none>
web-deploy-6b5f67b747-jrjpj   1/1     Running   0          10d   10.1.0.11   docker-desktop   <none>           <none>
```

### オートスケール

```bash
# 更新前のレプリカ数を確認
$ kubectl get po
NAME                          READY   STATUS    RESTARTS   AGE
web-deploy-6b5f67b747-69ntd   1/1     Running   0          10d
web-deploy-6b5f67b747-cstf4   1/1     Running   0          10d
web-deploy-6b5f67b747-jrjpj   1/1     Running   0          10d

# レプリカ数を変更したマニフェストを適応(3 -> 10に変更)
$ kubectl apply -f ./deployment2.yml
deployment.apps/web-deploy configured

# 更新後のレプリカ数を確認
$ kubectl get po
NAME                          READY   STATUS    RESTARTS   AGE
web-deploy-6b5f67b747-5bbtg   1/1     Running   0          62s
web-deploy-6b5f67b747-69ntd   1/1     Running   0          10d
web-deploy-6b5f67b747-785cd   1/1     Running   0          62s
web-deploy-6b5f67b747-cfq2q   1/1     Running   0          62s
web-deploy-6b5f67b747-cstf4   1/1     Running   0          10d
web-deploy-6b5f67b747-gl4h6   1/1     Running   0          62s
web-deploy-6b5f67b747-jrjpj   1/1     Running   0          10d
web-deploy-6b5f67b747-ndh5t   1/1     Running   0          62s
web-deploy-6b5f67b747-sgwbn   1/1     Running   0          62s
web-deploy-6b5f67b747-wmq6s   1/1     Running   0          62s
```
