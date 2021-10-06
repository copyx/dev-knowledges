# Googme Cloud Platform

## Cloud Run

완전 관리형 서버리스 플랫폼. 컨테이너를 기반으로 빌드와 배포, 오토 스케일링 등을 쉽고 빠르게 이용할 수 있음.

## Cloud Run과 Cloud SQL 연결하기

처음에는 'VPC 있으니 비공개 IP로 연결하면 되겠지'라고 생각함. 그러나 문서를 찾아보니 Cloud Run은 이미 서버리스 환경이라 비공개 IP로 VPC와 연결해주려면 서버리스 VPC 액세스 커넥터를 통해 연결해야함.

서버리스 VPC 액세스 커넥터를 구성하면 추가비용을 내야하므로 공개 IP와 Cloud SQL 인증 프록시를 이용했음.

### Cloud SQL 인증 프록시

승인된 네트워크 추가나 SSL 구성 없이 Cloud SQL 인스턴스에 대한 보안 액세스를 제공.

- TLS 1.2(128bit AES 암호화 지원)를 사용한 트래픽 암호화
- 고정 IP 주소가 없어도 Cloud SQL 인증 프록시가 연결을 관리해줌

## 커스텀 도메인 매핑

사용 불가능한 리전이 있음. (https://cloud.google.com/run/docs/mapping-custom-domains?hl=en#limitations)
