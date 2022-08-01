# Google Cloud Platform

## `gcloud`

`--verbosity=debug` 옵션을 추가하면 더 많은 정보를 확인할 수 있음.

### 구성 전환 자동화

https://cloud.google.com/sdk/docs/configurations?hl=ko#automating_configuration_switching

- direnv를 이용하는 방법
  1. 구성 생성 `gcloud config configurations create [config_name]`
  1. .envrc 파일에 `export CLOUDSDK_ACTIVE_CONFIG_NAME=[config_name]` 셋팅
  1. `direnv allow`를 실행시켜 추가된 환경변수 적용

이렇게 설정 후 디렉토리에 진입하면 해당 구성이 자동으로 활성화됨.
