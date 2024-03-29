## 브랜치 합치기
### merge
- 두 브랜치를 하나로 합침
```
// bugFix 브랜치에서 커밋한 뒤 메인 브랜치와 병합
git checkout -b bugFix
git commit
git checkout main
git merge bugFix
```

### rebase
- Rebase를 하든지, Merge를 하든지 최종 결과물은 같고 커밋 히스토리만 다르다는 것이 중요하다.
- 리베이스는 커밋들의 흐름을 한 줄로 만들 수 있다는 장점이 있다.
```
git checkout -b bugFix
git commit
git checkout main
git commit
git checkout bugFix

git rebase main
```

## 커밋 이동하기
### checkout
- `checkout <브랜치 이름>`으로 현재 가리키는 브랜치를 변경할 수 있다.
- `checkout -b <브랜치 이름>` 으로 브랜치 생성과 이동을 동시에 할 수 있다.
- `checkout <커밋 해시값>`으로 HEAD가 브랜치가 아니라 커밋을 가리키게 할 수 있다.
	- HEAD는 현재 체크아웃 된 커밋을 가리킨다.

### 상대 참조
- 한번에 한 커밋 위로 움직이는 (^)
	- `git checkout HEAD^` : 현재로부터 한 커밋 위로 이동
- 한번에 여러 커밋 위로 움직이는 (~\<num>)
	- `git checkout HEAD~4` : 현재로부터 4 커밋 위로 이동
- `-f` 옵션으로 브랜치를 특정 커밋에 직접적으로 재지정 할 수 있다.
	- `git branch -f main HEAD~3` : main 브랜치를 HEAD에서 3 커밋 위로 지정

## Git에서 작업 되돌리기
### reset
- `git reset`은 브랜치로 하여금 예전의 커밋을 가리키도록 이동시키는 방식으로 변경 내용을 되돌린다.
- 즉, `git reset`은 마치 애초에 커밋하지 않은 것처럼 예전 커밋으로 브랜치를 옮기는 것이다.
- ex) `git reset HEAD~1` : 현재 커밋을 삭제하고, 브랜치를 한 커밋 위로 이동

### revert
- `reset`은 히스토리를 고쳐쓰기 때문에 리모트 브랜치에서는 쓸 수 없다.
- `revert`는 현재 커밋의 변경 내역을 되돌린 새로운 커밋을 만들어 낸다.
- `git revert HEAD` : 현재 커밋의 변경사항을 되돌린 새로운 커밋을 만들어 이동


## Push & Pull
### fetch
- `git fetch`로 원격 저장소에 존재하는 데이터를 가져올 수 있다.
- fetch는 두 가지 동작을 수행한다.
	1. 원격 저장소에 있지만 로컬에는 없는 커밋들을 다운로드 받기
	2. 우리의 원격 브랜치가 가리키는곳을 업데이트합니다 (예를들어, `origin/main`)
- 그러나 `git fetch`는 ==로컬 상태는 전혀 바꾸지 않는다.== 오로지 **원격 브랜치**만 업데이트 한다.
- 새 커밋들을 로컬에 내려받은 이후에는 그냥 다른 브랜치에있는 일반 커밋처럼 활용할 수 있다.

### pull
- `git pull`은 `fetch`와 `merge`를 한번에 한다.
- `git pull --rebase`는 `fetch`와 `rebase`를 한번에 한다.