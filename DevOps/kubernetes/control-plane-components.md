# Control Plain(또는 Master) 컴포넌트

![쿠버네티스 아키텍쳐 - Master](/images/kubernetes_master.png)

## etcd

분산형 키값 저장소.

- 모든 상태와 데이터 저장 (key-value 형태)
- 분산 시스템으로 구성해 가용성과 안전성을 높임
- TTL(Time To Live), watch 등 부가 기능 제공
- 백업 필수!

## API Server(kube-apiserver)

쿠버네티스 API를 노출하는 컴포넌트. 컨트롤 플레인의 프론트엔드.

- 상태를 바꾸거나 조회하는 모듈
- etcd와 통신하는 유일한 모듈
- REST API 형태로 제공
- 권한 체크 후 적절한 권한 없을 시 요청 차단
- 관리자 요청 및 다양한 내부 모듈과 통신
- 수평으로 확장할 수 있도록 디자인

## Scheduler(kube-scheduler)

어느 노드에 여유가 있는지 확인해 컨테이너를 배치하는 컴포넌트.

- 새로 생성된 Pod을 감지하고 실행할 노드를 선택
- 노드의 현재 상태와 Pod의 요구사항을 체크
  - 노드에 라벨 부여 (예: a-zone, b-zone, gpu-enabled 등)

### [Node Selector](https://kubernetes.io/ko/docs/concepts/scheduling-eviction/assign-pod-node/#nodeselector)

사용자가 명시한 레이블을 가진 노드에만 파드을 스케줄링. 없으면 안함.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
  labels:
    env: test
spec:
  containers:
    - name: nginx
      image: nginx
  nodeSelector:
    gpu: true
```

### [Node Affinity](https://kubernetes.io/ko/docs/concepts/scheduling-eviction/assign-pod-node/#affinity-and-anti-affinity)

개념적으로 nodeSelector와 비슷함. 레이블을 이용해 파드이 어느 노드에 스케줄링되는 것을 허용할지 표현 가능.

두 가지 종류의 Node Affinity가 있음.

- `requiredDuringSchedulingIgnoredDuringExecution`
  - 강한 요구사항. 규칙이 만족되지 않으면 스케줄러가 파드를 스케줄링할 수 없음.
- `preferredDuringSchedulingIgnoredDuringExecution`
  - 약한 요구사항. 해당되는 노드가 없더라도, 스케줄러는 여전히 파드를 스케줄링 함.

이 외에도 할당하지 말아야될 노드를 표현하는 Anti Node Affinity도 있음.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
        - matchExpressions:
          - key: mynode
            operator: In
            values:
            - worker-1
          - key: mynode
            operator: NotIn # NotIn을 이용해 Anti Node Affinity도 가능
            values:
            - worker-3
 containers:
 - name: nginx
   image: nginx
```

### [Taints & Tolerations](https://kubernetes.io/ko/docs/concepts/scheduling-eviction/taint-and-toleration/)

Node Selector, Node Affinity가 노드를 고르는 파드의 속성이라면, 테인트는 그 반대로 노드가 파드를 제외.

```sh
kubectl taint nodes node1 key1=value1:NoSchedule
```

톨러레이션은 파드에 적용. 톨러레이션과 일치하는 테인트를 무시하게 만듦.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
  labels:
    env: test
spec:
  containers:
    - name: nginx
      image: nginx
      imagePullPolicy: IfNotPresent
  tolerations:
    - key: "example-key"
      operator: "Exists" # Exists 인 경우 value 를 지정하지 않아야 함
      effect: "NoSchedule"
    - key: "key1"
      operator: "Equal"
      value: "value1"
      effect: "NoSchedule"
```

무시되지 않은 테인트가 하나 이상 있으면 이펙트별로 효과 발생.

- `NoSchedule` : 해당 노드에 파드를 스케줄하지 않음.
- `PreferNoSchedule` : 파드를 해당 노드에 스케줄하지 않으려고 **시도**.
- `NoExecute` : 이미 실행중인 파드를 노드에서 축출하고, 아직 실행되지 않았으며 노드에 스케줄하지 않음.

## Controller Manager(kube-controller-manager)

컨트롤러 프로세스를 실행하는 컨트롤 플레인 컴포넌트.

여러 종류의 컨트롤러가 있으나, 복잡성을 낮추기 위해 단일 바이너리로 컴파일되고 단일 프로세스로 실행됨.

- 노드 컨트롤러: 노드가 다운되었을 때 통지와 대응
- 잡 컨트롤러: 일회성 작업을 나타내는 잡 오브젝트를 감시한 다음, 해당 작업을 완료할 때까지 실행하는 파드를 생성한다.
- 엔드포인트 컨트롤러: 엔드포인트 오브젝트를 채운다(즉, 서비스와 파드를 연결시킨다.)
- 서비스 어카운트 & 토큰 컨트롤러: 새로운 네임스페이스에 대한 기본 계정과 API 접근 토큰을 생성한다.

### Controller

kube-contoller-manager 컴포넌트에 의해 실행되는 프로세스.

컨테이너의 상태를 확인하고 원하는 상태(Desired State)를 유지하는 역할

- Desired State 유지
  - Observe: 상태 체크 (Current State == Desired State)
  - Diff: 차이점 발견 (Current State != Desired State)
  - Act: 조치 (Current State => Desired State)
- 논리적으로 다양한 컨트롤러 존재 (Replication, Node, Endpoint 등)
  - 한 루프 안에서 모든 상태 요소를 관리하지 않고 상태 요소마다 따로 루프를 가짐.
- 복잡성을 낮추기 위해 하나의 프로세스로 실행

## Cloud Controller Manager(cloud-conroller-manager)

클라우드 서비스별 컨트롤 로직을 포함하는 컴포넌트. 이를 통해 클러스터를 클라우드 공급자의 API에 연결하고, 오직 자신의 클러스터와만 상호작용하는 컴포넌트들로부터 클라우드 플랫폼과 상호작용하는 컴포넌트를 구분한다.

kube-controller-manager와 마찬가지로 단일 바이너리, 단일 프로세스로 실행됨.

다음 컨트롤러들은 클라우드 공급자 의존성을 가질 수 있음.

- 노드 컨트롤러: 노드가 응답을 멈춘 후 클라우드 상에서 삭제되었는지 판별하기 위해 클라우드 제공 사업자에게 확인하는 것
- 라우트 컨트롤러: 기본 클라우드 인프라에 경로를 구성하는 것
- 서비스 컨트롤러: 클라우드 제공 사업자 로드밸런서를 생성, 업데이트 그리고 삭제하는 것
