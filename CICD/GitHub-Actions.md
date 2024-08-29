## workflow 수동 실행하는 법
### workflow 만들기
- `./github/workflows` 폴더에 `deploy.yml` 파일을 작성
### workflow_dispatch 트리거 추가
- `deploy.yml` 파일에 `workflow_dispatch` 키워드를 추가하고 push
```yml
on:
	workflow_dispatch:
```

### GitHub CLI 설치
```bash
brew install gh
```

### workflow 실행
- `gh workflow run`  명령으로 workflow 실행
```bash
gh workflow run {workflow 이름} --ref {branch 이름}
```
- `gh workflow list` 로 workflow 목록을 볼 수 있음
- gh 명령이 실패하면 `gh auth login` 