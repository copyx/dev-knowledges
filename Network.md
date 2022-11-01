# Network

## Subnet

부분망. 부분 네트워크. IP 주소에서 네트워크 영역을 부분적으로 나눈 망. 이렇게 네트워크 성능 향상을 위해 네트워크를 나누는 것을 **Subnetting**이라고 함.

네트워크를 나누면 불필요한 브로드캐스팅을 줄일 수 있어서 성능 향상을 기대할 수 있고, 필요한만큼 네트워크를 나눠 IP 주소의 낭비 없이 분배가 가능함.

### Subnet Mask

IP 주소에서 어디까지가 Network 영역과 Host 영역을 구분할 수 있게 해주는 비트 표현. IP 주소의 클래스 개념도 같은 역할을 하지만 A, B, C, D, E 클래스에 따라 네트워크 크기가 정해져 있어서 비효율이 생김. Subnet Mask는 클래스의 Default Mask보다 더 세밀한 네트워크 분할이 가능함.

Subnet Mask는 IP 주소를 2진수로 표현했을 때 왼쪽부터 연속되는 1로 표현됨.

```
10000000.00000000.00000000.00000000 - /1
11111111.11111111.11110000.00000000 - /20
```

## NAT(Network Address Translation)

서로 분리된 내부 네트워크와 외부 네트워크 사이에 요청과 응답을 연결지어주는 기술.

```
                Labtop
            (192.168.0.3)
                  |
Request           |
xxx.xxx.xxx.xxx   |
from 192.168.0.3  |
                  ▼
            (192.168.0.1)
            Router with NAT
            (52.38.156.27)
                  | ▲
Request           | | Response
xxx.xxx.xxx.xxx   | | 192.168.0.3(??)
from 192.168.0.3  | | from xxx.xxx.xxx.xxx
                  ▼ |
          (xxx.xxx.xxx.xxx)
                Website
```

내부 네트워크의 IP는 내부에서만 의미가 있기 때문에 외부에서 내부 네트워크의 IP로 연결 불가능. 오직 외부 네트워크에서 라우터에게 할당된 IP로만 요청이나 응답을 보낼 수 있음. 따라서 Labtop에서 웹사이트로 요청을 보내도 웹사이트는 네트워크에서 Labtop의 IP를 찾을 수 없으므로 응답을 받아볼 수 없음. 그래서 NAT가 개발됨.

NAT는 트래픽에 대한 주소 변환을 수행함. 아웃바운드 요청은 내부 원본 주소 & 포트를 외부 대상 주소 & 포트로, 응답은 그 반대로 변환.

```
                Labtop
            (192.168.0.3)
                  | ▲
Request           | | Response
xxx.xxx.xxx.xxx   | | 192.168.0.3
from 192.168.0.3  | | from xxx.xxx.xxx.xxx
                  ▼ |
            (192.168.0.1)
            Router with NAT
            (52.38.156.27)
                  | ▲
Request           | | Response
xxx.xxx.xxx.xxx   | | 52.38.156.27
from 52.38.156.27 | | from xxx.xxx.xxx.xxx
                  ▼ |
          (xxx.xxx.xxx.xxx)
                Website
```

외부에서 내부 서버로 요청하는 경우도 있음. 이 경우 서버에 고정 IP와 포트를 할당함.

### 참고자료

- [NAT(Network Address Translation) - 생활코딩](https://www.opentutorials.org/course/3265/20035)
- [1장 - NAT(Network Address Translation) 소개 | Microsoft Learn](https://docs.microsoft.com/ko-kr/azure/rtos/netx-duo/netx-duo-nat/chapter1)
