# devths-k8s-manifests

devths 프로젝트 kubeadm 클러스터의 GitOps 배포 매니페스트 리포. ArgoCD가 이 리포를 감시하며 클러스터 상태를 동기화한다.

- `apps/hello-world/` — ArgoCD GitOps 루프 검증용 테스트 워크로드
- 실제 서비스(BE/AI/FE) 매니페스트는 이후 순차 추가 예정 (ADR-013/014 참고)

관련 문서: `0.DevOps_Project` 리포의 [ADR-014](https://github.com/yoondonggyu) 및 `1.work/10.kubeadm-setup/`
