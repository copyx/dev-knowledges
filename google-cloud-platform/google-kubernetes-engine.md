# Google Kubernetes Engine

## 클러스터 생성

https://collabnix.github.io/kubelabs/gke-setup.html 에서 첫 GKE 클러스터 생성 시 사용하는 명령

```sh
gcloud container clusters create CLUSTER_NAME --disk-size 10 --zone asia-northeast3-a --machine-type n1-standard-2 --num-nodes 3 --scopes compute-rw
```

- `--disk-size` : 노드 VM 부트 디스크 크기. 단위 GB. 기본 값 100GB.
- `--machine-type` : 노드에 사용할 머신 종류. `gcloud compute machine-types list`로 사용 가능한 머신 타입들 가져올 수 있음.
- `--scopes` : 접근 가능한 범위. [OAuth2.0 Google API Scopes](https://developers.google.com/identity/protocols/oauth2/scopes)

## 클러스터 연결

클러스터를 생성하면 자동으로 kubeconfig에 생성한 클러스터 엔트리를 추가해줌. 수동으로 하려면 `gcloud container clusters get-credentials CLUSTER_NAME` 명령을 사용하면 됨.

그런데 에러 발생.

```sh
$ kubectl get pods
W0802 01:22:59.128952   10782 gcp.go:120] WARNING: the gcp auth plugin is deprecated in v1.22+, unavailable in v1.25+; use gcloud instead.
To learn more, consult https://cloud.google.com/blog/products/containers-kubernetes/kubectl-auth-changes-in-gke
No resources found in default namespace.
```

원래는 kubectl에 기본적으로 제공되는 gcp auth plugin이 있었으나 Deprecated 됨. 그래서 따로 설치해야함.

https://cloud.google.com/blog/products/containers-kubernetes/kubectl-auth-changes-in-gke

위 가이드에 따르면 gcloud 명령으로 설치 가능.

```sh
gcloud components install gke-gcloud-auth-plugin
gke-gcloud-auth-plugin --version
```

설치 후

1. 쉘 설정파일에 `export USE_GKE_GCLOUD_AUTH_PLUGIN=True`를 추가
2. `gcloud components update` 명령으로 gcloud 업데이트
3. `gcloud container clusters get-credentials CLUSTER_NAME` 실행
