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

## [Container Orchestration](/DevOps/container-orchestration.md)

## 왜 Kubernetes인가?

- 오픈소스: 큰 회사들도 많이 참여하고 커뮤니티가 엄청 발달함.
- 엄청난 인기: 해외는 물론 국내 큰 회사들도 많이 사용함.
- 무한한 확장성
- 사실상의 표준(de facto)

쿠버네티스를 기반으로 만들어진 플랫폼 서비스도 많고, Cloud Native에서 지원하는 많은 부분들에 쿠버네티스가 중요한 역할을 함.

## 쿠버네티스 아키텍쳐

![쿠버네티스 컴포넌트](/images/components-of-kubernetes.svg)

![파드 생성 흐름](/images/pod_creation_flow.gif)

출처: https://kubernetes.io/ko/docs/concepts/overview/components/

- **클러스터** = 노드의 집합
  - 모든 클러스터는 최소 한 개의 노드를 가짐
  - 클러스터는 일반적으로 여러 노드를 실행
- **노드** = 컨테이너화된 애플리케이션을 실행하는 워커 머신
  - 노드는 파드를 호스팅
- **컨트롤 플레인**은 노드와 파드를 관리
  - 프로덕션 환경에서는 일반적으로 컨트롤 플레인이 여러 컴퓨터에 걸쳐 실행됨

쿠버네티스가 완전히 작동하기 위해서는 다양한 컴포넌트가 필요하고, 컴포넌트들은 **컨트롤 플레인 컴포넌트**와 **노드 컴포넌트**로 나뉨.

### [Control Plane(또는 Master) 컴포넌트](/DevOps/kubernetes/control-plane-components.md)

| 컴포넌트 이름                  | 요약                                                          |
| ------------------------------ | ------------------------------------------------------------- |
| etcd                           | 모든 상태와 데이터를 저장하는 분산형 key/value 저장소         |
| kube-apiserver                 | 쿠버네티스 API를 노출하는 컨트롤 플레인의 프론트엔드 컴포넌트 |
| kube-scheduler                 | 어느 노드에 여유가 있는지 확인해 컨테이너를 배치하는 컴포넌트 |
| kube-controller-manager        | 컨트롤러 프로세스를 실행하는 컴포넌트                         |
| cloud-controller-manager (opt) | 클라우드별 컨트롤 로직을 포함하는 컴포넌트                    |

### Node Components

![쿠버네티스 아키텍쳐 - Node](/images/kubernetes_node.png)

#### Kubelet

파드에서 컨테이너가 확실하게 동작하도록 관리하는 컴포넌트

- 각 노드에서 실행
- Pod을 실행/중지하고 상태를 체크
- CRI(Container Runtime Inteface)
  - 도커 말고도 다른 컨테이너 실행 환경이 있는데, 이를 Pod으로 감싸서 사용

#### Proxy(kube-proxy)

각 노드에서 실행되는 네트워크 프로시로 노드의 네트워크 규직을 관리하는 컴포넌트. 쿠버네티스의 서비스 개념의 구현부.

- 네트워크 프록시와 부하 분산 역할
- 성능상의 이유로 별도의 프록시 프로그램 대신 iptables 또는 IPVS를 사용 (설정만 관리)

### Addons

- CNI (네트워크)
- DNS (도메인, 서비스 디스커버리)
- 대시보드 (시각화)

## Objects

쿠버네티스 시스템 안에서 클러스터의 상태를 표현하기위해 사용되는 영속적인 엔터티. 이 엔터티들이 다음을 서술할 수 있음

- 어떤 컨테이너화 된 애플리케이션들이 어느 노드에서 실행되고 있는지
- 이 애플리케이션들이 사용 가능한 리소스들
- 재시작, 업그레이드, 장애 허용같이 이 애플리케이션들이 어떻게 동작해야할지에 대한 정책들

## Workloads

쿠버네티스에서 구동되는 애플리케이션. 워크로드가 하나의 컴포넌트든 여러 컴포넌트가 함께하든, 쿠버네티스에서는 애플리케이션을 파드 집합 내에서 실행함.

각각의 파드를 직접 관리하는 대신 워크로드 리소스 사용 가능.

### [Pod](/DevOps/kubernetes/pod.md)

하나 이상의 컨테이너를 실행하는 쿠버네티스에서 생성/관리할 수 있는 배포 가능한 가장 작은 컴퓨팅 단위

### Workload Resources

사용자를 대신해 파드 집합을 관리하는 리소스. 컨트롤러(지정한 상태와 일치하도록 올바른 수와 유형의 파드이 실행되고 있는지 확인)를 구성.

#### ReplicaSet

언제든 주어진 시간에 실행하는 복제 파드들의 안정적인 집합을 관리하는 것이 목적.

#### Deployment

- 내부적으로 ReplicaSet을 이용해 배포 버전을 관리

![Deployment](/images/kubernetes_deployment.gif)

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: echo-deploy
spec:
  replicas: 4
  selector:
    matchLabels:
      app: echo
      tier: app
  template:
    metadata:
      labels:
        app: echo
        tier: app
    spec:
      containers:
        - name: echo
          image: ghcr.io/subicura/echo:v1
# NAME                               READY   STATUS    RESTARTS   AGE
# pod/echo-deploy-77dbb94b69-ft5p2   1/1     Running   0          71s
# pod/echo-deploy-77dbb94b69-fvjnq   1/1     Running   0          71s
# pod/echo-deploy-77dbb94b69-kt8jz   1/1     Running   0          71s
# pod/echo-deploy-77dbb94b69-nplhl   1/1     Running   0          71s
#
# NAME                          READY   UP-TO-DATE   AVAILABLE   AGE
# deployment.apps/echo-deploy   4/4     4            4           71s
#
# NAME                                     DESIRED   CURRENT   READY   AGE
# replicaset.apps/echo-deploy-77dbb94b69   4         4         4       71s

# 이미지 태그를 v2로 변경하고 재배포한 결과
# NAME                               READY   STATUS    RESTARTS   AGE
# pod/echo-deploy-665857f7dd-48fwd   1/1     Running   0          10s
# pod/echo-deploy-665857f7dd-6jmjd   1/1     Running   0          10s
# pod/echo-deploy-665857f7dd-7dmpx   1/1     Running   0          4s
# pod/echo-deploy-665857f7dd-r9bwr   1/1     Running   0          5s
#
# NAME                          READY   UP-TO-DATE   AVAILABLE   AGE
# deployment.apps/echo-deploy   4/4     4            4           92s
#
# NAME                                     DESIRED   CURRENT   READY   AGE
# replicaset.apps/echo-deploy-665857f7dd   4         4         4       10s
# replicaset.apps/echo-deploy-77dbb94b69   0         0         0       92s
```

`kubectl rollout` 명령을 이용해 히스토리를 확인할 수 있고, 손쉽게 롤백할 수 있음.

```bash
kubectl rollout history deployment/echo-deploy
kubectl rollout history deployment/echo-deploy --revision
kubectl rollout undo deployment/echo-deploy
kubectl rollout undo deployment/echo-deploy --to-revision=2
```

이 외에도 이미지 변경, 롤아웃 상태, scale 변경 명령어가 있음.

```sh
# nginx-deploy Deployment의 nginx 컨테이너 이미지를 nginx:1.9.1로 변경
kubectl set image deploy nginx-deploy nginx=nginx:1.9.1

# nginx-deploy Deployment의 가장 최근 롤아웃 상태 조회
kubectl rollout status deployment/nginx-deploy

# nginx-deploy Deployment의 레플리카 수를 6개로 변경
kubectl scale deployment nginx-deploy --replicas=6
```

##### Deployment Types

- Recreate
  - A가 완전히 꺼진 다음 B를 올림
- Rolling Updates(Ramped or Incremental)
  - 쿠버네티스 기본 배포 전략
  - 배포 완료까지 시간이 좀 걸림
  - B 버전 하나를 준비해 트래픽을 받을 수 있으면, 로드밸런서와 연결된 풀에서 A 하나를 제거
- Canary
  - 새 버전을 모두 배포하기 전 일부 인스턴스에만 배포해서 테스트할 때 사용
- Blue Green
  - Blue(구버전)과 동일한 규모의 Green(신버전)을 한꺼번에 만들고 로드밸런서 연결만 바꾸는 방식

#### 그 외

https://kubernetes.io/ko/docs/concepts/workloads/controllers/

- DaemonSet
  - 모든 노드에 꼭 하나씩만 떠있길 원하는 파드를 만들 때 사용
  - 예) 데이터, 로그 수집
- StatefulSet
  - 순차적인 파드 실행, 볼륨 재활용 등에 사용
- Job / CronJob
  - 한 번 실행하고 죽는 파드
  - CronJob은 Job을 [Cron](https://ko.wikipedia.org/wiki/Cron) 형식으로 쓰여진 주어진 일정에 따라 주기적으로 반복

## [Service, Load Balancing and Networking](/DevOps/kubernetes/service-load-balancing-and-networking.md)

파드는 자체 IP를 가짐. 그래서 파드끼리 직접 통신할 수 있지만, 쉽게 생성되고 사라지는 특성 때문에 이는 위험함. 이를 해결하기 위해 고정된 IP를 가진 서비스를 두고 이를 통해 통신.

![Common Set](/images/kubernetes_common_set.png)

## [Statefulset](/DevOps/kubernetes/statefulset.md)

디플로이먼트와 비슷하게 파드 집합의 배포와 규모를 관리하며, 디플로이먼트와 다르게 파드들의 순서 및 고유성을 보장.

## [Volume](https://kubernetes.io/ko/docs/concepts/storage/volumes/)

데이터를 영속성을 가진 별도의 저장소에 저장하지 않으면 파드이 제거됐을 때 사라짐. 쿠버네티스의 볼륨이 컨테이너의 디렉토리를 외부 저장소와 연결해줌. 그리고 다양한 플러그인 통해 흔히 사용하는 대부분의 스토리지를 별도 설정없이 사용할 수 있음.

- AWS Elastic Block Storage
- Azure Disk
- GCE Persistent Disk

### Storage Class

사용자들이 클라우드 공급자에게 요구하는 스토리지의 종류를 명시하는 쿠버네티스 오브젝트. 스토리지 클래스 별로 디스크 지연시간, 처리량 같은 다양한 서비스 퀄리티를 표현하며, 사용되는 시나리오와 클라우드 공급자의 지원에 따라 선택됨.

Persistent Volumes, Persistent Volumes Claims가 스토리지 클래스를 사용함.

#### Persistent Volumes

영구 볼륨은 사용자가 각 클라우드 제공업체에서 스토리지를 관리하고 프로비저닝하는 방법에 대한 세부 정보를 입력하지 않도록 하는 추상화 계층 역할을 함.

#### Persistent Volumes Claims

영구 볼륨 사용 요청. 파드와 노드에 비유하자면, 영구 볼륨을 "노드", 영구 볼륨 클레임을 "파드"로 여길 수 있음.

## [ConfigMap](https://kubernetes.io/ko/docs/concepts/configuration/configmap/)

기밀이 아닌 데이터를 저장하는데 사용하는 API 오브젝트.

- 키-값 쌍으로 구성됨.
- 최대 1MiB 크기의 데이터 저장 가능.
- `spec` 대신 `data`(UTF-8), `binaryData`(Base64) 필드가 있음.
- 다양한 방식으로 파드에서 활용 가능
  - 컨테이너 커맨드와 인수
  - 컨테이너에 대한 환경변수
  - 읽기 전용 볼륨에 파일 추가
  - 쿠버네티스 API를 이용해 컨피그맵 읽는 코드 파드에서 실행

```yml
apiVersion: v1
kind: ConfigMap
metadata:
  name: alpine-config
data:
  env1: "test"
  colors.properties: |
    color.good=purple
    color.bad=yellow
    allow.textmode=true
---
apiVersion: v1
kind: Pod
metadata:
  name: alpine
spec:
  containers:
    - name: alpine
      image: alpine:latest
      command: ["sleep"]
      args: ["100000"]
      # 파일 추가 방식
      volumeMounts:
        - name: config-vol
          mountPath: /etc/config
      # 환경변수 방식
      env:
        - name: hello
          valueFrom:
            configMapKeyRef:
              name: alpine-config
              key: hello
  volumes:
    - name: config-vol
      configMap:
        name: alpine-config
```

## Secret

암호, 토큰, 키 같은 중요 데이터를 보관하는 오브젝트. 중요 데이터를 코드나 파드 스펙, 컨테이너 이미지 등에 포함시키지 않고 분리하기 위해 사용.

**단, 암호화되지 않은 상태로 etcd에 저장됨.(Base64로 인코딩 되는 것 외에는 없음) 암호화를 적용하려면 외부솔루션(예: vault) 적용 필요**

## 그 외 기본 오브젝트

- Namespace: 논리적인 리소스 구분
- Secret: 설정
- ServiceAccount: 권한 계정
- Role/ClusterRole: 권한 설정(get, list, watch, create, ...)

### API 호출

원하는 상태(desiged state)를 다양한 오브젝트(object)로 명세(spec)를 작성해 API 서버에 YAML 형식으로 전달

![ReplicaSet Creation Flow](/images/kubernetes_replicaset_creation_flow.png)

## 실습 환경

https://subicura.com/k8s/prepare/kubernetes-setup.html

- minikube or k3s
- kubectl

### [minikube](https://minikube.sigs.k8s.io/docs/)

minikube는 VM, Container, Bare-metal에 모두 배포할 수 있지만, 서로 다른 환경들과 상호작용하기 위한 일관된 방법을 제공하기 위해 [Docker Machine](https://github.com/docker/machine) 라이브러리를 사용함.

docker 드라이버를 사용한다면 쿠버네티스 네트워크에 접근할 때 별도의 포트를 프록시하여 사용해야함.

```bash
# 로컬 클러스터의 서비스에 접근하기 위한 URL을 반환하는 명령
minikube service [service-name]
```

## `kubectl` Commands

### `apply`

쿠버네티스 클러스터에 설정을 적용시키는 명령

```bash
kubectl apply -f [yaml/file/path.yml]
```

### `get`

리소스들을 조회하는 명령

```bash
kubectl get all
kubectl get pod
kubectl get po, service

# 결과 포맷 변경
kubectl get pod -o wide
kubectl get pod -o yaml
kubectl get pod -o json

# 레이블 표시
kubectl get pod --show-labels
```

### `describe`

쿠버네티스에 선언된 리소스의 상세한 상태를 확인하는 명령

```bash
kubectl describe [type]/[name]
kubectl describe po/wordpress-5cf6c98486-twmrc
```

여기서 Events 항목을 확인할 수 있는데, 해당 파드에게 발생한 일들을 확인할 수 있음. 이를 통해 원하는 대로 파드이 안뜨거나 할 때 원인 분석 가능.

### `delete`

쿠버네티스에 선언된 리소스를 삭제하는 명령

```bash
kubectl delete [type]/[name]
kubectl delete po/wordpress-5cf6c98486-twmrc
```

### `logs`

파드, 컨테이너의 로그를 확인하는 명령

```bash
kubectl logs [pod-name]
# 특정 컨테이너의 로그
kubectl logs [pod-name] -c [container-name]
```

### `exec`

컨테이너에 명령을 실행하는 명령

```bash
kubectl exec [-it] [pod-name] -- [command]
kubectl exec -it wordpress-5cf6c98486-twmrc -- bash
```

### `config`

여러 개의 쿠버네티스 클러스터 컨텍스트를 저장하고 필요에 따라 선택 가능.

```bash
kubectl config current-context
kubectl config use-context minikube
```

### `run`

`docker run` 명령처럼 이미지를 기반으로 파드를 생성시키는 명령

```bash
kubecel run [pod-name] --image=[image-path]
```

`--env`, `-it` 등 `docker run`과 비슷한 다양한 옵션 사용 가능.

하지만 대부분 `run` 명령보다 YAML 파일을 더 많이 사용함.

### 그 외

- `api-resources` : 서버에서 지원하고 있는 API 리소스들을 나열. 여기서 리소스들의 shortname 확인 가능.
- `explain` : 특정 오브젝트 설명 보기

## [YAML](yaml.md)

쿠버네티스의 YAML 파일에는 다음 4가지 필수요소가 있음.

|    Field     |                  Description                  |                Examples                 |
| :----------: | :-------------------------------------------: | :-------------------------------------: |
| `apiVersion` | 이 오브젝트를 생성하기 위해 사용하는 API 버전 |  v1, app/v1, networking.k8s.io/v1, ...  |
|    `kind`    |         생성하고자 하는 오브젝트 종류         | Pod, ReplicaSet, Deployment, Service 등 |
|  `metadata`  |     오브젝트를 유일하게 구분지어줄 데이터     |         name, uid, namespace 등         |
|    `spec`    |    오브젝트에 대해 어떤 상태를 의도하는지     |         오브젝트 종류마다 다름          |

## 쿠버네티스 패턴

### Sidecar

애플리케이션의 일부일 필요는 없지만 애플리케이션에 연결되어 있어야하는 기능들을 위해 만들어진 패턴. 애플리케이션에 통합되어 구현되어있으면 언어, 실행환경 등 신경써야하는 제약사항들이 생김. 이를 애플리케이션과 독립적으로 동작하는 별도의 보조 프로세스 혹은 컨테이너를 붙이는 형태로 해결. 주로 어플리케이션에 대한 모니터링, 로깅, 네트워킹 등 지원 기능을 제공. 어플리케이션과 수명주기를 공유함. 어플리케이션 없이 존재할 의미가 없음.

- Props
  - 애플리케이션과 다른 언어, 프레임워크, 환경의 사이드카 사용 가능
  - 애플리케이션과 사이드카가 동일한 리소스에 접근 가능
  - 애플리케이션과 사이드카 간의 통신 오버헤드가 적음
  - 사이드카의 오류로 애플리케이션이 중지되지 않음
- 고려사항
  - 라이브러리나 확장 매커니즘으로 구현하는게 더 나은건 아닌지
  - 배포 형식
  - 프로세스 간 통신 매커니즘

단, 프로세스간 통신을 최적화해야하거나, 애플리케이션이 너무 작거나, 사이드카를 확장해야 할 경우 적합하지 않을 수 있음.

참고: https://docs.microsoft.com/ko-kr/azure/architecture/patterns/sidecar

## 더 공부해볼 범위

- 다양한 환경별 특징 (Bare metal, EKS, ...)
- 쿠버네티스 패턴 (사이드카, 어댑터, ...)
- 관련 생태계 (서비스메시, 서버리스, ...)
- GitOps CI/CD
- 승인제어 등 고급 기능

## 참고자료 및 이미지 출처

- [초보를 위한 쿠버네티스 안내서 - 44BITS](https://youtube.com/playlist?list=PLIUCBpK1dpsNf1m-2kiosmfn2nXfljQgb)
- [초보를 위한 쿠버네티스 안내서 - 인프런](https://www.inflearn.com/course/%EC%BF%A0%EB%B2%84%EB%84%A4%ED%8B%B0%EC%8A%A4-%EC%9E%85%EB%AC%B8)
- [쿠버네티스 안내서](https://subicura.com/k8s/)
- [쿠버네티스 문서](https://kubernetes.io/ko/docs/home/)
