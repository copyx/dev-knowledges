# Pod

- 쿠버네티스에서 생성/관리할 수 있는 배포 가능한 가장 작은 컴퓨팅 단위
  - 왜 최소 단위가 컨테이너가 아니라 파드일까?
    - 여러 종류의 컨테이너가 있는데 각각 필요한 추가 정보들이 다름. 쿠버네티스는 이를 모두 합치는 방향이 아닌 새로운 엔터티를 만드는 방향으로 선택함.
- 하나 이상의 컨테이너 그룹
  - 왜 한 파드에 복수의 컨테이너를 허용하지?
    - 한 파드 안에 있으면 컨테이너간 통신이 용이하고, 볼륨도 공유할 수 있으며, 데이터 지역성을 보장할 수 있음.
    - 그럼 왜 한 컨테이너 안에 모든걸 넣지 않는거지?
      - "한 컨테이너에 한 프로세스"라는 원칙을 파괴함.
      - 한 컨테이너에 여러 프로세스가 있으면 트러블슈팅이 더 힘듦.
  - 스토리지 및 네트워크 공유
  - 호스트 디렉토리나 로컬호스트 네트워크를 공유할 수 있음
- 전체 클러스터에서 고유한 IP 할당

`kubectl run` 명령을 이용해 파드를 생성하기도 하지만, 대부분 YAML 파일로 많이 만듦.

```sh
kubectl create -f pod.yaml  # 이미 파드가 있으면 에러 발생
kubectl apply -f pod.yaml   # 이미 파드가 있으면 변경사항 적용

# 쉘 연결 명령 (컨테이너에 쉘이 있어야 가능)
kubectl exec -it pod -- /bin/bash
kubectl exec -it pod -c container -- /bin/bash
```

## [Pod 라이프사이클](https://kubernetes.io/ko/docs/concepts/workloads/pods/pod-lifecycle/)

```text
Pending → Running → Succeeded
    ↘︎      ↓
      Failed
```

| 값        | 의미                                                                                                      |
| --------- | --------------------------------------------------------------------------------------------------------- |
| Pending   | 파드가 쿠버네티스 클러스터에서 승인됐지만, 하나 이상의 컨테이너가 설정되지 않았고 실행할 준비가 되지않음. |
| Running   | 파드가 노드에 바인딩 & 모든 컨테이너 생성. 최소 하나의 컨테이너가 실행 중 or 시작 중 or 재시작 중.        |
| Succeeded | 파드가 성공적으로 종료됨. 재시작 X.                                                                       |
| Failed    | 파드의 모든 컨테이너 종료 & 최소 하나 이상의 컨테이너가 실패(Non-zero exited or terminated).              |
| Unknown   | 파드의 상태를 알 수 없음. 보통 노드와의 통신 오류.                                                        |

> 파드가 삭제될 때, 일부 kubectl 커맨드에서 Terminating 이 표시된다. **이 Terminating 상태는 파드의 단계에 해당하지 않는다.** 파드에는 그레이스풀하게(gracefully) 종료되도록 기간이 부여되며, 그 기본값은 30초이다. 강제로 파드를 종료하려면 --force 플래그를 설정하면 된다.

![Pod 생성 과정](/images/kubernetes_pod_creation_sequence.svg)
이미지 출처: [Pod 생성 분석 | 쿠버네티스 안내서](https://subicura.com/k8s/guide/pod.html#pod-%E1%84%89%E1%85%A2%E1%86%BC%E1%84%89%E1%85%A5%E1%86%BC-%E1%84%87%E1%85%AE%E1%86%AB%E1%84%89%E1%85%A5%E1%86%A8)

## 컨테이너 상태 모니터링

`컨테이너 생성`과 `서비스 준비`는 서로 다른 상태. 컨테이너를 생성했다해도 워크로드가 모두 준비되서 동작 가능할 때까지는 초기화 시간이 필요함. 쿠버네티스는 이 `서비스 준비` 상태를 체크하는 옵션을 제공.

- 보통 livenessProbe와 readinessProbe를 같이 적용함. 상세한 설정은 애플리케이션 환경에 따라 적절히 조절.

### livenessProbe

컨테이너가 정상적으로 동작하는지 체크하고 정상적으로 동작하지 않는다면 컨테이너를 재시작하여 문제를 해결.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: echo-lp
  labels:
    app: echo
spec:
  containers:
    - name: app
      image: ghcr.io/subicura/echo:v1
      livenessProbe: # HTTP GET 요청을 보내 생존 여부 체크
        httpGet:
          path: /does/not/exist
          port: 8080
        initialDelaySeconds: 5
        timeoutSeconds: 2 # Default 1
        periodSeconds: 5 # Defaults 10
        failureThreshold: 1 # Defaults 3

# NAME      READY   STATUS             RESTARTS      AGE
# echo-lp   0/1     CrashLoopBackOff   4 (24s ago)   69s

# Events:
#   Type     Reason     Age                From               Message
#   ----     ------     ----               ----               -------
#   Normal   Scheduled  96s                default-scheduler  Successfully assigned default/echo-lp to minikube
#   Warning  BackOff    76s (x2 over 76s)  kubelet            Back-off restarting failed container
#   Normal   Pulled     56s (x5 over 95s)  kubelet            Container image "ghcr.io/subicura/echo:v1" already present on machine
#   Normal   Created    56s (x5 over 95s)  kubelet            Created container app
#   Normal   Started    56s (x5 over 95s)  kubelet            Started container app
#   Warning  Unhealthy  56s (x4 over 86s)  kubelet            Liveness probe failed: Get "http://172.17.0.3:8080/does/not/exist": dial tcp 172.17.0.3:8080: connect: connection refused
#   Normal   Killing    56s (x4 over 86s)  kubelet            Container app failed liveness probe, will be restarted
```

### readinessProbe

컨테이너 준비 여부 체크 후 준비되지 않으면 파드로 들어오는 요청을 제외. livenessProbe는 재시작하지만 readinessProbe는 요청만 제외시킴.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: echo-rp
  labels:
    app: echo
spec:
  containers:
    - name: app
      image: ghcr.io/subicura/echo:v1
      readinessProbe: # HTTP GET 요청을 보내 생존 여부 체크
        httpGet:
          path: /does/not/exist
          port: 8080
        initialDelaySeconds: 5
        timeoutSeconds: 2 # Default 1
        periodSeconds: 5 # Defaults 10
        failureThreshold: 1 # Defaults 3

# NAME      READY   STATUS    RESTARTS   AGE
# echo-rp   0/1     Running   0          8s

# Events:
#   Type     Reason     Age               From               Message
#   ----     ------     ----              ----               -------
#   Normal   Scheduled  27s               default-scheduler  Successfully assigned default/echo-lp to minikube
#   Normal   Pulled     26s               kubelet            Container image "ghcr.io/subicura/echo:v1" already present on machine
#   Normal   Created    26s               kubelet            Created container app
#   Normal   Started    26s               kubelet            Started container app
#   Warning  Unhealthy  2s (x4 over 17s)  kubelet            Readiness probe failed: Get "http://172.17.0.3:8080/does/not/exist": dial tcp 172.17.0.3:8080: connect: connection refused
```

## 다중 컨테이너

하나의 파드에 속한 컨테이너는 네트워크를 localhost로 공유하고, 디렉토리 또한 공유할 수 있음.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: counter
  labels:
    app: counter
spec:
  containers:
    - name: app
      image: ghcr.io/subicura/counter:latest
      env:
        - name: REDIS_HOST
          value: "localhost"
    - name: db
      image: redis
```
