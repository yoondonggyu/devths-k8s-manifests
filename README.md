# devths-k8s-manifests

devths 프로젝트 kubeadm 클러스터의 GitOps 배포 매니페스트 리포. ArgoCD가 이 리포를 감시하며 클러스터 상태(네임스페이스 `devths`)를 자동 동기화한다.

## 구조

```
apps/
  fe/   Next.js FE — kustomization.yaml(namespace: devths), Deployment/Service/Ingress
  be/   Spring Boot BE — Deployment/Service/Ingress
  ai/   FastAPI+Celery AI — ai-core(ai-endpoint+celery-worker-extract 멀티 컨테이너)/
        celery-worker-trend/celery-beat/vectordb(ChromaDB)
```

각 폴더의 `kustomization.yaml`이 `namespace: devths`를 지정하므로, ArgoCD가 자동으로 Kustomize 렌더링해서 배포한다.

## 이 리포가 다루지 않는 것

- **Secret**: 실제 값(DB 비밀번호, JWT_SECRET, API 키 등)은 이 리포에 절대 커밋하지 않는다. `kubectl create secret`으로 클러스터에 직접(out-of-band) 적용하고, 매니페스트는 `envFrom`/`secretRef`로 참조만 한다
- **기반 인프라(Postgres/Redis/RabbitMQ)**: 이 앱들의 데이터 저장소는 GitOps 대상이 아니라 클러스터에 `kubectl apply`로 직접 관리한다(Calico/local-path-provisioner 등 플랫폼 컴포넌트와 동일 취급)

## 왜 public인가

이 리포엔 시크릿이 애초에 안 들어가므로 private로 두고 ArgoCD용 GitHub 인증 정보(PAT)를 별도로 관리하는 것보다, public으로 두는 쪽이 관리 부담이 적다고 판단했다.

## 관련 문서

`0.DevOps_Project` 리포(로컬)의 `0.ADR/[AI] 00_ADR_ 011-015.md`(ADR-013/014) 및 `1.work/10.kubeadm-setup/`(01~07번 문서, 실행 기록·트러블슈팅).
