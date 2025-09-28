---
layout: post
title: Kafka in Minikube
date: 2025-09-28 18:42:00 +0900
categories: [Minikube,Kafka]
tags: [Kafka,Kubernetes]
---

### Kafka in Minikube

---

bitnami 차트
```shell
$ helm repo add bitnami https://charts.bitnami.com/bitnami
$ helm repo update
```

kafka helm 설치
```shell
helm install kafka bitnami/kafka \
  --namespace kafka \
  --set replicaCount=1 \
  --set listeners.client.protocol=PLAINTEXT \
  --set allowPlaintextListener=true \
  --set service.type=NodePort \
  --set zookeeper.enabled=false
```


### Kafka UI (minikube)

---

```shell
piVersion: apps/v1
kind: Deployment
metadata:
  name: kafka-ui
  namespace: kafka
  labels:
    app: kafka-ui
spec:
  replicas: 1
  selector:
    matchLabels:
      app: kafka-ui
  template:
    metadata:
      labels:
        app: kafka-ui
    spec:
      containers:
        - name: kafka-ui
          image: provectuslabs/kafka-ui:latest
          ports:
            - containerPort: 8080
          env:
            - name: KAFKA_CLUSTERS_0_NAME
              value: local
            - name: KAFKA_CLUSTERS_0_BOOTSTRAPSERVERS
              value: kafka.my-spring-kafka.svc.cluster.local:9092
---
apiVersion: v1
kind: Service
metadata:
  name: kafka-ui
  namespace: kafka
spec:
  type: NodePort   # minikube에서 외부접속 하려면 NodePort
  ports:
    - port: 8080
      targetPort: 8080
      nodePort: 30080   # 원하는 포트
  selector:
    app: kafka-ui
```


# minikube 서비스 외부 접근 허용
---
```shell
$ minikube service -n kafka kafka --url
```

# PVC 삭제
---
```shell

```
