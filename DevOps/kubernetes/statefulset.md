# Statefulset

디플로이먼트와 비슷하게 파드 집합의 배포와 규모를 관리하며, 디플로이먼트와 다르게 파드들의 순서 및 고유성을 보장.

스테이트풀셋으로 만든 파드들은 동일한 스펙으로 생성되지만, 각 파드들이 재스케줄링 간에도 **지속적으로 유지되는 식별자**를 가져서 서로 교체가 불가능함.

그리고 스테이트풀셋의 파드에 에러가 발생해 죽더라도 파드 식별자가 변하지 않으므로, 기존에 연결됐던 볼륨을 새로 생성된 파드에 더 쉽게 일치시킬 수 있음.

## 왜 필요하지?

**Stateless VS Stateful**

상태를 저장해서 상호작용의 연속성을 유지해야하는지 여부에 따라 Stateless인지, Stateful인지 갈림.

- Stateless
  - 각 상호작용은 모두 처음부터 시작되며, 이전 상호작용의 결과에 영향을 받지 않음.
  - 상호작용하는 대상이 바뀌어도 문제가 생기지 않음.
- Stateful
  - 이전 상호작용의 컨텍스트에 따라 수행되며, 이전 상호작용의 결과에 영향을 받음.
  - 상호작용하는 대상이 바뀌면 문제가 생김.

스테이트풀한 서버를 만드려면 이전 상호작용에 연결한 서버를 다시 찾아 연결할 수 있도록 고유한 네트워크 식별자가 필요하고, 서버가 재시작되도 안정적이고 지속성을 가진 스토리지가 필요함.

## 언제 사용하지?

다음 중 하나 이상이 필요할 때 스테이트풀셋이 유용함.

- 안정적인, 고유한 네트워크 식별자.
- 안정적인, 지속성을 갖는 스토리지.
- 순차적인, 정상 배포(graceful deployment)와 스케일링.
- 순차적인, 자동 롤링 업데이트.

안정적인 = 파드의 (재)스케줄링 전반에 걸친 지속성

## 제약사항

- 파드에 지정된 스토리지는 요청받은 `storage class` 기반의 퍼시스턴트 볼륨 프로비저너로 프로비전되거나, 관리자에 의해 사전에 프로비전이 되어야 함.
- 스테이트풀셋을 삭제 또는 스케일 다운해도 연관된 볼륨이 삭제되지 않음.
  - 연관 리소스 자동 제거 < 데이터 안전 보장
- 스테이트풀셋은 현재 파드의 네트워크 신원을 책임지고 있는 [헤드리스 서비스](/DevOps/kubernetes/service-load-balancing-and-networking.md#헤드리스-headless-서비스)가 필요함.
  - 사용자가 이 서비스를 생성할 책임이 있음.
- 스테이트풀셋은 스테이트풀셋의 삭제 시 파드의 종료에 대해 어떤 것도 보증하지 않음.
  - 파드가 순차적으로 정상 종료(graceful termination)되도록 하려면, 삭제 전 스케일을 0으로 축소하면 가능함.
- 롤링 업데이트와 기본 파드 매니지먼트 폴리시(`OrderedReady`)를 함께 사용 시, 복구를 위한 수동 개입이 필요한 파손 상태로 빠질 수 있음.

## 구성 요소

```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx
  labels:
    app: nginx
spec:
  ports:
    - port: 80
      name: web
  clusterIP: None
  selector:
    app: nginx
---
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: web
spec:
  selector:
    matchLabels:
      app: nginx # .spec.template.metadata.labels 와 일치해야 한다
  serviceName: "nginx" # 이 스테이트풀셋을 통제하는 서비스 이름. 서비스가 스테이트풀셋보다 먼저 생성되어야함.
  replicas: 3 # 기본값은 1
  minReadySeconds: 10 # 기본값은 0
  template:
    metadata:
      labels:
        app: nginx # .spec.selector.matchLabels 와 일치해야 한다
    spec:
      terminationGracePeriodSeconds: 10
      containers:
        - name: nginx
          image: k8s.gcr.io/nginx-slim:0.8
          ports:
            - containerPort: 80
              name: web
          volumeMounts:
            - name: www
              mountPath: /usr/share/nginx/html
  volumeClaimTemplates:
    - metadata:
        name: www
      spec:
        accessModes: ["ReadWriteOnce"]
        storageClassName: "my-storage-class"
        resources:
          requests:
            storage: 1Gi
```

## 파드 독자성

> 파드 고유의 독자성 = 순서 + 안정적인 네트워크 신원 + 안정적인 스토리지

이 독자성은 파드가 어떤 노드에 (재)스케줄됐는지 상관 없이 파드에 붙어있음.

- 순서 색인
  - 각 파드에 스테이트풀셋 내에서 고유한 0 ~ N-1까지의 정수 색인이 순서대로 할당 됨.
- 파드 도메인
  - `$(pod name) = $(statefulset name)-$(ordinal)`
  - `$(governing service domain) = $(service name).$(namespace).svc.$(cluster domain)`
  - `$(pod domain) = $(pod name).$(governing service domain)`

## 배포와 스케일링 보증

- 파드 생성 순서는 {0..N-1} 로 연속해서 생성
  - 생성하려면 모든 선행 파드가 Running이나 Ready 상태여야함.
- 파드 삭제 순서는 {N-1..0} 로 생성의 역순
  - 삭제하려면 모든 후속 파드가 완전히 종료되어야함.

### 파드 관리 정책

| `.spec.podManagementPolicy` | Description                                                             |
| :-------------------------: | ----------------------------------------------------------------------- |
|       `OrderedReady`        | 위의 순차 정책                                                          |
|         `Parallel`          | 모든 파드를 병렬로 생성, 삭제 가능. 오직 스케일링 작업에만 영향을 미침. |

## 업데이트 전략

| `.spec.updateStrategy` | Description                                                            |
| :--------------------: | ---------------------------------------------------------------------- |
|       `OnDelete`       | 수동으로 파드를 삭제해야 변경된 스펙이 반영된 새로운 파드를 생성.      |
|    `RollingUpdate`     | 모든 파드를 병렬로 생성, 삭제 가능. 오직 스케일링 작업에만 영향을 미침 |

## 퍼시스턴트 볼륨 클레임 보유 정책

`.spec.persistentVolumeClaimRetentionPolicy` 는 스테이트풀셋의 생애주기동안 PVC를 삭제할 것인지, 삭제한다면 어떻게 삭제하는지를 관리함.

- 설정 가능한 정책
  - `whenDeleted`: 스테이트풀셋이 삭제될 때 적용될 동작을 설정
  - `whenScaled`: 스테이트풀셋이 스케일 다운할 때 적용될 동작을 설정
- 각 정책에 설정 가능한 값
  - `Retain`(기본값): 스테이트풀셋이 삭제되도, 스케일 다운되도 PVC 유지
  - `Delete`: 파드가 삭제되면 PVC도 삭제
