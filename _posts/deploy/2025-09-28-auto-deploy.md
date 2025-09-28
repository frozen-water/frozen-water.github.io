---
layout: post
title: 배포 자동화 실습
date: 2025-09-28 18:37:01 +0900
categories: [Deploy]
tags: [Kubernetes,Github Actions,Helm,ArgoCD]
---

### 배포 자동화 실습

---

### 준비물

---

- ArgoCD
- Github
- Helm
- Docker
- Minikube
- Spring Kafka

### 구조

---
![img.png]({{ site.baseurl }}/assets/img/auto-deploy.png)
GitOps 배포 흐름
1. 애플리케이션 코드 Git push
   - 개발자가 Application 저장소에 코드를 푸시하면, GitHub Actions가 트리거됨.
2. 도커 이미지 빌드 & DockerHub 푸시
   - GitHub Actions가 애플리케이션을 Docker 이미지로 빌드하고, DockerHub에 새로운 태그로 푸시함.
3. Manifest 저장소 업데이트
   - GitHub Actions가 values.yaml 등 manifest repo의 image tag를 새 태그로 변경 후 커밋 & 푸시.
4. ArgoCD 동작
   - ArgoCD는 manifest repo를 지속적으로 모니터링.
   - 수동(Manual) 또는 자동(Automated)으로 sync 시 manifest 변경사항을 Minikube 클러스터에 적용.
5. Minikube에서 배포 반영
   - Minikube는 manifest에 지정된 image:tag를 확인하고, DockerHub에서 해당 이미지를 pull하여 Pod를 재시작/갱신.
