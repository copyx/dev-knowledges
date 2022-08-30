# ReplicaSet

- 여러개의 파드를 관리
  - 신규 파드를 생성하거나, 기존 파드를 제거해 원하는 수(Replicas)를 유지
  - 레이블을 이용해 파드를 체크함. 레플리카셋 소유가 아닌 파드와 레이블이 겹치지 않게 매우 신경써야함.
- 다만 실전에서 레플리카셋이 단독으로 사용되는 일은 거의 없음. 레플리카셋을 관리하는 Deployment를 주로 사용함.

## 예시

```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: echo-rs
spec:
  replicas: 1 # 몇 개의 파드를 유지할건지
  selector: # 관리 대상인 파드를 무엇으로 구분할지
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
# NAME                READY   STATUS    RESTARTS   AGE     LABELS
# pod/echo-rs-pw2ll   1/1     Running   0          3m41s   app=echo,tier=app
#
# NAME                      DESIRED   CURRENT   READY   AGE     LABELS
# replicaset.apps/echo-rs   1         1         1       3m41s   <none>
```

## 파드의 소속 알아보는 방법

```sh
kubectl get pods [pod-name] -o yaml | grep -A 5 owner
# ownerReferences:
#   - apiVersion: extensions/v1beta1
#	    blockOwnerDeletion: true
#	    controller: true
#	    kind: ReplicaSet
#	    name: web
```

## 레플리카셋과 레이블

- 레플리카셋 소유가 아닌 파드에 레이블을 셋팅하면 레플리카셋 소유가 되고 그 수가 조정됨.
- 반대로 레플리카셋의 파드에서 레이블을 제거하면 해당 파드를 격리하는 효과를 가져옴. 디버깅, 데이터 복구등에 사용 가능.

```sh
kubectl edit pods [pod-name] # 파드의 레이블 수정
```

## 레플리카셋 VS 레플리케이션 컨트롤러(Replication Controller)

- 레플리카셋이 레플리케이션 컨트롤러의 다음 세대 (둘의 목적은 같음)
- Selector 방식이 다름. 레플리카셋은 Set-based, 레플리케이션 컨트롤러는 Equality-based.
- 쿠버네티스 공식문서나 책에서는 레플리케이션 컨트롤러 보다 레플리카셋 사용을 권장하고 있음.

## 레플리카 규모 변경

```sh
# 방법 1. 오브젝트 수정
kubectl edit rs [ReplicaSet Name]
# 방법 2. kubectl scale 명령
kubectl scale --replicas=5 -f nginx_replicaset.yaml
```

HPA(Horizontal Pod Autoscaler)를 이용하면 레플리카셋의 규모가 CPU 부하에 따라 자동적으로 관리됨.

```sh
kubectl autoscale rs [ReplicaSet Name] --max=5
```

## 레플리카셋 삭제

```sh
kubectl delete rs [ReplicaSet Name]
kubectl delete -f definition_file.yaml
kubectl delete rs [ReplicaSet Name] --cascade=false # 파드들을 삭제하지 않음
```
