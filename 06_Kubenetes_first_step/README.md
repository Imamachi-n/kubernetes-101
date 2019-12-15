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
