# Kubernetes

컨테이너화 된 애플리케이션을 관리하는 도구


```
$ kubectl apply -f <yaml-name>.yaml
$ kubectl get pods
$ kubectl delete pod <pod-name>
```


# Component

<img width="684" alt="스크린샷 2025-03-11 오후 3 32 26" src="https://github.com/user-attachments/assets/9ba27163-9ef9-4078-8e3b-a25aaedd0b5f" />


# MasterNode Component

## 1. API Server

* 클러스터의 모든 요청을 처리하는 진입점
* kubectl, 내부 컴포넌트들이 이곳을 통해 통신

## 2. Controller Manager

* 여러 컨트롤러들을 실행하여 클러스터 상태를 원하는 상태로 유지
* 논리적으로 각 컨트롤러는 개별 프로세스이나, 복잡성을 낮추기 위해 모두 단일 binary로 컴파일되고 단일 프로세스에서 실행
* 종류 : Node Controller, Job Controller, EndpointSlice, ServiceAccount

## 3. Scheduler

* 새롭게 생긴 파드 감지하고, 적절한 노드에 배치함

## 4. etcd

* 클러스터의 모든 상태 정보 (메타 데이터, 네트워크 정보 등)를 저장하는 Key-Value 저장소

# Worker Node Component

## 1. Kubelet

* 클러스터의 각 노드에서 실행되며, 노드에 할당된 파드의 라이프사이클을 관리

## 2. Container Runtime

* 컨테이너 실행을 담당하는 SW, Docker Engine을 포함
* k8s 여러 Container Runtime 지원

## 3. Kube Proxy

* 네트워크를 관리하고 서비스 트래픽을 올바른 파드로 전달

# Terminology

## 1. Pod

* 가장 작은 배포 단위로, 하나 이상의 컨테이너를 묶어서 관리함
* Pod는 언제든지 삭제되거나 재시작될 수 있음 (ex. 노드장애, 스케일링 등) 따라서 새로 생성될 때마다 새로운/고유한 IP가 할당됨
* 같은 Pod 내의 컨테이너는 볼륨, IP주소, Port, 데이터 등을 공유
  * 같은 Pod 내의 컨테이너는 IP와 포트를 공유하기 때문에, 서로 localhost를 통해 통신이 가능

### Create Pod

<img width="582" alt="스크린샷 2025-03-11 오후 3 43 27" src="https://github.com/user-attachments/assets/f0107a50-4426-465e-bddb-098d4cf9e8f6" />

## 2. ReplicaSet

* Pod를 복제 생성하고, 복제된 파드의 개수를 Spec에 정의된 개수만큼 유지하는 오브젝트 (자동 복구 및 스케일링)
* 직접적으로 ReplicaSet을 사용하기보다 Deployment등 다른 오브젝트에 의해서 사용되는 경우가 많음

## 3. Service

* Pod의 IP는 동적으로 변경되므로, Pod끼리 또는 외부에서 접근할 수 있도록 고정된 네트워크 엔드포인트를 제공
* 라벨링을 통해 같은 라벨을 가진 Pod를 묶어 단일 엔드포인트(ClusterIP, NodePort, LoadBalancer, ExternalName)를 제공

`ClusterIP`

* 기본값 - 내부 통신용
* 클러스터 내부의 다른 리소스들과 통신할 수 있도록 하는 클러스터 전용 IP (외부 접근 불가)
* 클러스터 내부 트래픽을 해당 Pod의 <ClusterIP>:<targetPort(Pod-Port)>로 넘겨줌

`NodePort`

* 외부에서 노드IP의 특정 포트로 넘어오는 요청을 해당 포트와 연결된 Pod로 트래픽을 전달
* 모든 노드(VM)의 특정 포트를 열어 두고, 이 포트로 보내지는 모든 트래픽을 포워딩함
* 포트당 하나의 서비스만 할당할 수 있으며 30000-32767 사이의 포트만 사용 가능

`LoadBalancer`

* 외부 IP를 가지고 있는 로드밸런서를 할당 (클러스터 외부에서 접근 가능)

`ExternalName`

* 외부 서비스를 클러스터 내부에서 호출하고자할 때 사용

## 4. Ingress

* Service를 외부로 노출할 떄 사용


## 5. ConfigMap & Secret

* 환경 변수, 설정 파일 등을 관리하는 객체

## 6. NameSpace

> 쿠버네티스 클러스터 내의 리소스들을 논리적으로 구분하는 단위

---
## Controller

--

## Rolling Update


