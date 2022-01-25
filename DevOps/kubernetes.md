# Kubernetes

컨테이너를 쉽고 빠르게 배포/확장하고 관리를 자동화해주는 오픈소스 플랫폼.

- 당시 1주에 20억개의 컨테이너를 생성하던 구글이 만듦.
- 컨테이너 배포 시스템으로 사용하던 **borg**를 기반으로 만듦.
- 현재는 CNCF(Cloud Native Computing Foundation)라는 오픈소스 단체로 이관됨.

> 도커를 모른다면 쿠버네티스를 완벽하게 이해할 수 없다.

## Cloud Native?

> 클라우드 네이티브 기술은 조직이 퍼블릭, 프라이빗, 그리고 하이브리드 클라우드와 같은 현대적이고 동적인 환경에서 확장 가능한 애플리케이션을 개발하고 실행할 수 있게 해준다. 컨테이너, 서비스 메쉬, 마이크로서비스, 불변(Immutable) 인프라, 그리고 선언형(Declarative) API가 이러한 접근 방식의 예시들이다.
>
> 이 기술은 회복성, 관리 편의성, 가시성을 갖춘 느슨하게 결합된 시스템을 가능하게 한다. 견고한 자동화 기능을 함께 사용하면, 엔지니어는 영향이 큰 변경을 최소한의 노력으로 자주, 예측 가능하게 수행할 수 있다.
>
> Cloud Native Computing Foundation은 벤더 중립적인 오픈 소스 프로젝트 생태계를 육성하고 유지함으로써 해당 패러다임 채택을 촉진한다. 우리 재단은 최신 기술 수준의 패턴을 대중화하여 이런 혁신을 누구나 접근 가능하도록 한다.
>
> From [Cloud Native Definition v1.0](https://github.com/cncf/toc/blob/main/DEFINITION.md#%ED%95%9C%EA%B5%AD%EC%96%B4)

## [Container Orchestration](container-orchestration.md)

## 왜 Kubernetes인가?

- 오픈소스: 큰 회사들도 많이 참여하고 커뮤니티가 엄청 발달함.
- 엄청난 인기: 해외는 물론 국내 큰 회사들도 많이 사용함.
- 무한한 확장성
- 사실상의 표준(de facto)

쿠버네티스를 기반으로 만들어진 플랫폼 서비스도 많고, Cloud Native에서 지원하는 많은 부분들에 쿠버네티스가 중요한 역할을 함.

## 쿠버네티스 아키텍쳐

### Master

![쿠버네티스 아키텍쳐 - Master](images/kubernetes_master.png)

#### etcd

분산형 키값 저장소.

- 모든 상태와 데이터 저장 (key-value 형태)
- 분산 시스템으로 구성해 가용성과 안전성을 높임
- TTL(Time To Live), watch 등 부가 기능 제공
- 백업 필수!

#### API Server

- 상태를 바꾸거나 조회하는 모듈
- etcd와 통신하는 유일한 모듈
- REST API 형태로 제공
- 권한 체크 후 적절한 권한 없을 시 요청 차단
- 관리자 요청 및 다양한 내부 모듈과 통신
- 수평으로 확장할 수 있도록 디자인

#### Scheduler

어느 노드에 여유가 있는지 확인해 컨테이너를 배치하는 역할

- 새로 생성된 Pod을 감지하고 실행할 노드를 선택
- 노드의 현재 상태와 Pod의 요구사항을 체크
  - 노드에 라벨 부여 (예: a-zone, b-zone, gpu-enabled 등)

#### Controller

컨테이너의 상태를 확인하고 원하는 상태(Desired State)를 유지하는 역할

- Desired State 유지
  - Observe: 상태 체크 (Current State == Desired State)
  - Diff: 차이점 발견 (Current State != Desired State)
  - Act: 조치 (Current State => Desired State)
- 논리적으로 다양한 컨트롤러 존재 (Replication, Node, Endpoint 등)
- 복잡성을 낮추기 위해 하나의 프로세스로 실행

### Node

![쿠버네티스 아키텍쳐 - Node](images/kubernetes_node.png)

### 쿠버네티스 흐름

![팟 생성 흐름](images/pod_creation_flow.gif)

#### Kubelet

- 각 노드에서 실행
- Pod을 실행/중지하고 상태를 체크
- CRI(Container Runtime Inteface)
  - 도커 말고도 다른 컨테이너 실행 환경이 있는데, 이를 Pod으로 감싸서 사용

#### Proxy

- 네트워크 프록시와 부하 분산 역할
- 성능상의 이유로 별도의 프록시 프로그램 대신 iptables 또는 IPVS를 사용 (설정만 관리)

### Addons

- CNI (네트워크)
- DNS (도메인, 서비스 디스커버리)
- 대시보드 (시각화)

## Objects

### Workload

쿠버네티스에서 구동되는 애플리케이션

#### Pod

- 가장 작은 배포 단위
- 전체 클러스터에서 고유한 IP 할당
- 여러 컨테이너가 하나의 팟에 속할 수 있음
  - 호스트 디렉토리나 로컬호스트 네트워크를 공유할 수 있음

[워크로드 라이프사이클](https://kubernetes.io/ko/docs/concepts/workloads/pods/pod-lifecycle/)

#### ReplicaSet

- 여러개의 팟을 관리
  - 신규 팟을 생성하거나, 기존 팟을 제거해 원하는 수(Replicas)를 유지

#### Deployment

- 내부적으로 ReplicaSet을 이용해 배포 버전을 관리

![Deployment](images/kubernetes_deployment.gif)

### 그 외 다양한 Workload

https://kubernetes.io/ko/docs/concepts/workloads/controllers/

- Daemon Set
  - 모든 노드에 꼭 하나씩만 떠있길 원하는 팟을 만들 때 사용
  - 예) 데이터, 로그 수집
- Stateful Set
  - 순차적인 팟 실행, 볼륨 재활용 등에 사용
- Job / CronJob
  - 한 번 실행하고 죽는 팟
  - CronJob은 Job을 [Cron](https://ko.wikipedia.org/wiki/Cron) 형식으로 쓰여진 주어진 일정에 따라 주기적으로 반복

### Service, LoadBalancer, Network

https://kubernetes.io/ko/docs/concepts/services-networking/

![Common Set](images/kubernetes_common_set.png)

#### Service - ClusterIP

- 클러스터 내부에서 사용하는 프록시
- 팟은 동적이지만 서비스는 고유 IP를 가짐
- 클러스터 내부에서 서비스 연결은 DNS를 이용

#### Service - NodePort

- 노드(host)에 노출되어 외부에서 접근 가능한 서비스

#### Service - LoadBalancer

- 하나의 IP주소를 외부에 노출

#### Ingress

- 도메인 또는 경로별 라우팅

### 그 외 기본 오브젝트

- Volume: Storage (EBS, NFS, ...)
- Namespace: 논리적인 리소스 구분
- ConfigMap/Secret: 설정
- ServiceAccount: 권한 계정
- Role/ClusterRole: 권한 설정(get, list, watch, create, ...)

## API 호출

원하는 상태(desiged state)를 다양한 오브젝트(object)로 명세(spec)를 작성해 API 서버에 YAML 형식으로 전달

![ReplicaSet Creation Flow](images/kubernetes_replicaset_creation_flow.png)

## 더 공부해볼 범위

- 다양한 환경별 특징 (Bare metal, EKS, ...)
- 쿠버네티스 패턴 (사이드카, 어댑터, ...)
- 관련 생태계 (서비스메시, 서버리스, ...)
- GitOps CI/CD
- 승인제어 등 고급 기능

## 참고자료 및 이미지 출처

- [초보를 위한 쿠버네티스 안내서 - 44BITS](https://youtube.com/playlist?list=PLIUCBpK1dpsNf1m-2kiosmfn2nXfljQgb)
- [초보를 위한 쿠버네티스 안내서 - 인프런](https://www.inflearn.com/course/%EC%BF%A0%EB%B2%84%EB%84%A4%ED%8B%B0%EC%8A%A4-%EC%9E%85%EB%AC%B8)
