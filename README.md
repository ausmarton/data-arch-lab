# DataArch Lab
A collection of sample deployments of some of the building blocks of modern data architecture. This repo is mainly experimental and a testbed for recipes that can be used as templates for real-world use cases.

## Pre-requisites
- Kubernetes (minikube on local/GKE/EKS)
- kubectl (to apply the configs)

## Usage
### Kafka cluster
`kafka-cluster.yaml` contains a deployment of a simple kakfa cluster resulting in the following:

```bash
ausmarton@dev:~/data-arch-lab$ kubectl --namespace kafka-cluster get all
NAME                           READY   STATUS    RESTARTS   AGE
pod/kafka-0                    1/1     Running   0          5m
pod/kafka-1                    1/1     Running   0          4m57s
pod/kafka-2                    1/1     Running   0          4m55s
pod/kafka-ui-55f5775f4-glfl9   1/1     Running   0          5m
pod/zookeeper-0                1/1     Running   0          5m

NAME                TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)          AGE
service/kafka       ClusterIP   None             <none>        9092/TCP         5m
service/kafka-ui    NodePort    10.108.242.161   <none>        8080:30000/TCP   5m
service/zookeeper   ClusterIP   None             <none>        2181/TCP         5m

NAME                       READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/kafka-ui   1/1     1            1           5m

NAME                                 DESIRED   CURRENT   READY   AGE
replicaset.apps/kafka-ui-55f5775f4   1         1         1       5m

NAME                         READY   AGE
statefulset.apps/kafka       3/3     5m
statefulset.apps/zookeeper   1/1     5m

```
In order to deploy this, run the following command:
```bash
kubectl apply -f kafka-cluster.yaml
```
To clean up run:
```bash
kubectl delete namespace kafka-cluster
```
To fetch logs for a pod:
```bash
kubectl -n kafka-cluster logs kafka-0
```
To get all resources:
```bash
kubectl --namespace kafka-cluster get all
```

## Issues
Brokers appear to be throwing exceptions:
```bash
2024-12-04 00:54:41,588] WARN [RequestSendThread controllerId=0] Controller 0's connection to broker kafka-1.kafka.kafka-cluster.svc.cluster.local:9092 (id: 1 rack: null) was unsuccessful (kafka.controller.RequestSendThread)
java.io.IOException: Connection to kafka-1.kafka.kafka-cluster.svc.cluster.local:9092 (id: 1 rack: null) failed.
	at org.apache.kafka.clients.NetworkClientUtils.awaitReady(NetworkClientUtils.java:71)
	at kafka.controller.RequestSendThread.brokerReady(ControllerChannelManager.scala:299)
	at kafka.controller.RequestSendThread.doWork(ControllerChannelManager.scala:252)
	at org.apache.kafka.server.util.ShutdownableThread.run(ShutdownableThread.java:136)
```