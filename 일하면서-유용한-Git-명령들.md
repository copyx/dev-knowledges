# 일하면서 유용한 Git 명령들

## 코드의 특정 부분이 수정된 코드를 찾고싶을 때

```bash
git log -S <string>
```

## 리모트 브랜치 삭제

```bash
git push --delete <remote> <branch>
```

## 브랜치 변경 혹은 pull하기 위해 현재 수정사항 임시 저장

### Stash

워킹 디렉토리에서 Modified이면서 Tracked 상태인 파일과 Staging Area에 있는 파일들을 보관해두는 장소. 아직 끝나지 않은 수정사항을 스택에 잠시 저장했다 나중에 다시 적용 가능. (브랜치가 달라도 가능)

참고: [Git - Stashing과 Cleaning](https://git-scm.com/book/ko/v2/Git-%EB%8F%84%EA%B5%AC-Stashing%EA%B3%BC-Cleaning)

```bash
git stash # 스택에 새로운 Stash 생성
git stash save # 스택에 새로운 Stash 생성
git stash save --keep-index # Staged 파일 제외
git stash save --patch # 대화형 프롬프트를 이용한 저장 파일 선택
git stash list # 저장된 Stash 확인
git stash apply <stash-name> # 이름 없으면 최근, 있으면 Stash 이름을 이용한 적용
git stash apply --index # Staged 상태까지 적용
git stash drop <stash-name> # 이름 없으면 최근, 있으면 이름에 해당하는 Stash 제거
git stash pop # 적용 및 제거 한 번에
git stash push
```
