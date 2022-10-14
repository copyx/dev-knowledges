# Service, Load Balancing and Networking

## 서비스

> 쿠버네티스에서 서비스는 파드의 논리적 집합과 그것들에 접근할 수 있는 정책을 정의하는 추상적 개념이다.
>
> From [서비스 리소스 | Kubernetes](https://kubernetes.io/ko/docs/concepts/services-networking/service/#service-resource)

서비스는 파드보다 위에 있는 레이어로서 동작. 파드의 수, 내부 IP, 드러낸 포트 등을 항상 파악함. 서비스가 대상으로 하는 파드 집합은 일반적으로 셀렉터가 결정. 파드의 노출 범위에 따라 **ClusterIP**, **NodePort**, **LoadBalancer**로 나뉨.

서비스의 레이블 이름은 [RFC 1035](https://kubernetes.io/ko/docs/concepts/overview/working-with-objects/names/#rfc-1035-label-names)에 정의된 DNS 레이블 표준을 따라야함.

서비스는 아래 문제들을 해결해줄 수 있음.

- 웹앱을 배포한다고 상상했을 때
  - 외부에 어떻게 공개하지?
  - 백엔드 DB에 어떻게 연결하지?
  - 파드가 죽어서 IP가 바뀌면 어떻게 해야하지?

### 서비스 생성

- 서비스가 생성되면
  - 서비스 타입의 기본 값은 ClusterIP
  - 각 서비스에는 고유한 IP(CLUSTER-IP: 클러스터 내부에서만 사용 가능한 IP) 주소가 할당됨.
  - 서비스가 활성화 되어있는 동안에는 변경되지 않음.
  - 서비스와의 통신은 서비스 멤버 중 일부 파드에 자동적으로 로드밸런싱 됨.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-nginx
  labels:
    app: my-nginx
spec:
  ports:
    - name: http
      port: 80
      protocol: TCP
      targetPort: 80
    - name: https
      port: 443
      protocol: TCP
      targetPort: 443
  selector:
    app: my-nginx
```

|          Field          |                    Description                     |
| :---------------------: | :------------------------------------------------: |
|    `spec.ports.name`    |            포트 이름. 복수 오픈 시 필수            |
|    `spec.ports.port`    |                서비스가 오픈할 포트                |
| `spec.ports.targetPort` | 서비스가 접근할 파드의 포트 (기본값은 port와 동일) |
|     `spec.selector`     |        서비스가 접근할 파드의 레이블 선택자        |

서비스와 연결된 논리적 파드 집합의 멤버 파드들은 `endpoints`를 통해 노출됨. 서비스의 셀렉터는 지속적으로 평가되고, 이 결과를 엔드포인트 오브젝트에 전달해 변경이 일어남. 셀렉터가 변하지 않아도 파드가 죽으면 자동으로 엔드포인트에서 제거됨.

```sh
# 서비스와 엔드포인트 확인 명령
kubectl describe svc my-nginx
kubectl get ep my-nginx
```

![Service creation flow](/images/kubernetes_service_creation_flow.svg)

### 서비스 접근

쿠버네티스는 두 가지 주요 서비스 검색 모드를 제공.

#### 환경변수

`kubectl exec [POD_NAME] -- printenv | grep SERVICE` 로 환경변수 확인 가능

- 단점
  - 서비스 생성 전에 만들어진 레플리카들은 환경변수 적용이 안됨.
    - 서비스 생성 전에 만들어진 모든 레플리카들을 재생성하면 정상화 됨.
    - > 이 작업을 수행할 때 또 다른 단점은 스케줄러가 두 파드를 모두 동일한 머신에 배치할 수도 있다는 것이며, 이로 인해 전체 서비스가 중단될 수 있다.
    - 뭔소리야... 그럼 서비스를 연결하면 자동으로 서로 다른 머신에 배치해준다는건가? 그건 아닌 것 같은데...

#### DNS

쿠버네티스는 DNS 이름을 다른 서비스에 자동으로 할당하는 DNS 클러스터 애드온(kube-dns, CoreDNS) 서비스를 제공함.

```sh
# DNS 애드온 실행중인지 확인하는 명령
kubectl get services kube-dns --namespace=kube-system
```

서비스 이름을 도메인으로 사용. 클러스터 네트워크 내부에서 표준 방법(예: `gethostbyname()`)으로 클러스터의 모든 파드에서 서비스와 통신 가능.

```sh
# DNS 테스트를 위한 curl 앱 실행
kubectl run curl --image=radial/busyboxplus:curl -i --tty
# 파드에서 nslookup으로 도메인 확인
[ root@curl-131556218-9fnch:/ ]$ nslookup my-nginx
```

만약 CoreDNS가 실행 중이 아니라면 [CoreDNS README](https://github.com/coredns/deployment/tree/master/kubernetes) 또는 [CoreDNS 설치](https://kubernetes.io/ko/docs/tasks/administer-cluster/coredns/#coredns-%EC%84%A4%EC%B9%98)를 참조

### NodePort

- 노드(host)에 노출되어 외부에서 접근 가능한 서비스
  - 클러스터의 모든 노드에 포트를 오픈
  - 여러 개의 노드가 있다면 아무 노드로 접근해도 지정한 파드로 접근
- NodePort는 ClusterIP의 기능을 포함함.
- 노드가 사라졌을 때 자동으로 다른 노드를 통해 접근이 불가능함.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: frontend
spec:
  type: NodePort
  ports:
    - port: 80
      nodePort: 30000 # 노드에 오픈할 포트
      targetPort: 80
  selector:
    app: my-nginx
```

![Multi-NodePort](/images/kubernetes_nodeport-multi.png)

### LoadBalancer

- 하나의 IP주소를 외부에 노출
- 로드밸런서에 요청하면 알아서 살아있는 노드에 접근함.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: frontend
spec:
  type: LoadBalancer
  selector:
    app: my-nginx
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
```

**현재와 같은 구조라면 앱이 추가될 때마다 로드밸런서를 추가해야함.** 이를 해결하기 위해 Ingress를 사용함.

## 헤드리스(Headless) 서비스

- 로드밸런싱과 단일 서비스 IP가 필요없을 때 사용.
- 명시적으로 클러스터 IP(.spec.clusterIP)에 "None"을 지정.
- 쿠버네티스의 구현이 아닌 다른 서비스 디스커버리 매커니즘과 인터페이스 가능.
- 클러스터 IP 할당되지 않음.
- kube-proxy가 서비스를 처리하지 않음.
- 플랫폼에 의해 로드밸런싱 또는 프록시 하지 않음.
- 셀렉터 정의 여부에 따라 DNS 자동 구성 방법이 달라짐.
  - 셀렉터가 있으면 엔드포인트 레코드를 생성하고, DNS 구성을 수정해 파드를 직접 가르키는 A 레코드 반환.
  - 셀렉터가 없으면 엔드포인트 레코드를 생성하지 않지만, DNS 시스템은 다음 중 하나를 찾고 구성함.
    - ExternalName-유형 서비스에 대한 CNAME 레코드
    - 다른 모든 유형에 대해, 서비스의 이름을 공유하는 모든 엔드포인트에 대한 레코드

## Ingress

인그레스는 K8s 클러스터 외부에서 내부 서비스들로의 HTTP, HTTPS 경로를 노출함. 어떤 인바운드 연결을 어떤 서비스에 닿게 해줄지 규칙을 선언해 접근들을 설정할 수 있음.

클러스터의 애플리케이션이 늘어날 때마다 해당 애플리케이션의 포트를 위한 서비스도 늘어나야함. 이를 편하게 하기위해 인그레스가 나옴.

- 같은 IP로 접근해도 도메인 또는 경로별 라우팅
- 인그레스 하나로 여러 개의 로드밸런서와 같은 역할을 할 수 있음.
