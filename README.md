# app-with-argocd-basic

Actions 워크플로는 ArgoCD와 관련된 YAML 파일을 수정하고, 변경 사항을 Git 저장소에 커밋 및 푸시하는 작업을 자동화

ArgoCD 관련 작업 (argocd job)

1.Repository Checkout
- 지정된 GitHub 리포지토리(repository)를 main 브랜치 기준으로 체크아웃
- token: 인증에 사용됩니다. 보통 GitHub Personal Access Token을 secrets에 저장하여 사용
```yaml
- uses: actions/checkout@v4
  with:
    repository: 
    ref: main
    token: ${{ secrets.TOKEN }}
```
2. YAML 파일 수정
- sed 명령어를 사용해 YAML 파일(argocd-deployment.yaml)의 내용을 수정
- 기존: image: nginx:1.25 => 변경 후: image: nginx:latest
- ArgoCD 배포 시 사용되는 컨테이너 이미지를 최신 버전(nginx:latest)으로 업데이트
```yaml
- name: Set up Image
  run: |
    sed -i "s%image: nginx:1.25%image: nginx:latest%" ./argocd-deployment.yaml
```
3. 변경사항 커밋 및 푸시
-  YAML 파일의 수정 내용이 GitHub 저장소에 반영
```
- name: Commit and push changes
  run: |
    git config --local user.email "peekaboo32@naver.com"
    git config --local user.name "SUJINJEONG012"
    git add .
    git commit -m "Update nginx image to latest"
    git push

```




ArgoCD와 Kubernetes 연계
- argocd-deployment.yaml 파일은 Kubernetes 클러스터에서 ArgoCD를 통해 애플리케이션을 배포하는 데 사용
- 매번 수동으로 YAML 파일을 수정하고 커밋할 필요 없이, GitHub Actions 워크플로로 이를 자동화
