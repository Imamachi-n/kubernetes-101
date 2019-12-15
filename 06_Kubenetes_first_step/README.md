## クラスタ構成の確認

### k8s クラスタ環境の情報開示

```bash
$ kubectl cluster-info
Kubernetes master is running at https://kubernetes.docker.internal:6443
KubeDNS is running at https://kubernetes.docker.internal:6443/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy

To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'.
```

### k8s クラスタのノード表示

```bash
$ kubectl get node
NAME             STATUS   ROLES    AGE   VERSION
docker-desktop   Ready    master   15m   v1.14.8
```

## ポッドの実行

ポッドのみを動かすときは、`--restart=Never`オプションを付加。  
Hello-world の Docker イメージは、標準出力した後、起動終了するため。

### hello-world コンテナの実行

```bash
$ kubectl run hello-world --image=hello-world -it --restart=Never
If you don't see a command prompt, try pressing enter.
Error attaching, falling back to logs:

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
```

### ポッドの表示

```bash
$ kubectl get pod
NAME          READY   STATUS      RESTARTS   AGE
hello-world   0/1     Completed   0          103s
```

### ポットの削除とポッド再実行

```bash
$ kubectl delete pod hello-world
pod "hello-world" deleted
```

### ポッド終了後の自動削除オプション`-rm`の使用

```bash
$ kubectl run hello-world --image=hello-world -it --restart=Never --rm
Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/

pod "hello-world" deleted
```

```bash
$ kubectl get pod
No resources found.
```

### ポッドのバックグラウンド実行とログ表示例

```bash
$ kubectl run hello-world --image=hello-world --restart=Never
pod/hello-world created

$ kubectl logs hello-world
Hello from Docker!
...

$ kubectl delete pod hello-world
```

## コントローラによるポッドの実行

### kubectl による Deployment の実行

```bash
$ kubectl create deployment --image hello-world hello-world
deployment.apps/hello-world created
```

### デプロイメントの実行状況のリスト表示

```bash
$ kubectl get all
NAME                               READY   STATUS             RESTARTS   AGE
pod/hello-world-7fd49b7477-7wff4   0/1     CrashLoopBackOff   2          50s

NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   33m

NAME                          READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/hello-world   0/1     1            0           50s

NAME                                     DESIRED   CURRENT   READY   AGE
replicaset.apps/hello-world-7fd49b7477   1         1         0       50s
```

### ポッドのログ出力

```bash
$ kubectl logs pod/hello-world-7fd49b7477-7wff4
Hello from Docker!
...
```

### デプロイメントの削除

```bash
$ kubectl get deployment
NAME          READY   UP-TO-DATE   AVAILABLE   AGE
hello-world   0/1     1            0           10m

$ kubectl delete deployment hello-world
deployment.extensions "hello-world" deleted

$ kubectl get all
NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   44m
```

## ジョブによるポッドの実行

### ジョブ制御化でのポッドの実行

```bash
$ kubectl create job hello-world --image hello-world
job.batch/hello-world created

$ kubectl get all
NAME                    READY   STATUS      RESTARTS   AGE
pod/hello-world-c925p   0/1     Completed   0          17s

NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   49m

NAME                    COMPLETIONS   DURATION   AGE
job.batch/hello-world   1/1           6s         17s

$ kubectl delete job hello-world
job.batch "hello-world" deleted
```
