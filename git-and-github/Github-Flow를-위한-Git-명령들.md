# Git Commands for Github

Github로 협업 시 필요한 작업들과 작업 단계별 필요한 명령

## 기본적인 Github Flow

### 1. 프로젝트를 Fork 한다.

Fork는 해당 저장소 웹페이지에서 진행. Fork 후 로컬로 코드를 Clone.

```bash
git clone <forked-repo-url>
```

### 2. `main` 기반으로 토픽 브랜치를 만든다.

```bash
git switch -c <topic-branch-name>
```

### 3. 뭔가 수정해서 Commit한다.

```bash
git add <files>
git commit
```

### 4. 자신의 GitHub 프로젝트에 브랜치를 Push 한다.

```bash
git push origin <topic-branch-name>
```

### 5. GitHub에 Pull Request를 생성한다.

### 6. 토론하면서 그에 따라 계속 Commit & Push 한다.

### 7. 프로젝트 소유자는 Pull Request를 Merge 하고 닫는다.

## Conflict가 발생한 경우

`main` 브랜치를 기반으로 설명함.

### 1. 원 저장소를 “upstream” 이라는 이름의 리모트로 추가한다

```bash
git remote add upstream <original-repo-url>
```

### 2. 리모트에서 최신 데이터를 Fetch 한다

```bash
git fetch upstream
```

### 3. `main` 브랜치를 토픽 브랜치에 Merge 한다

```bash
git merge upstream/main
```

### 4. 충돌을 해결한다

```bash
# Edit code to fix conflicts
git add <files>
git commit
```

### 5. 동일한 토픽 브랜치에 도로 Push 한다

```bash
git push origin <topic-branch-name>
```

# 참고자료

[Git - Github 프로젝트에 기여하기](https://git-scm.com/book/ko/v2/GitHub-GitHub-%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B8%EC%97%90-%EA%B8%B0%EC%97%AC%ED%95%98%EA%B8%B0)
