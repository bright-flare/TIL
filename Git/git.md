# 🧩 Git, GitHub 간단 정리

## Git❓
- git은 파일의 상태를 저장하는 프로그램이다.
- commit한 시점의 파일들의 상태를 저장해놓는다고 생각하면 된다. (branch를 옮겼을 때에도 마찬가지)

## Git, GitHub ❓
- 정말 간단하게 github은 저장소이고, git은 local version 관리 프로그램이다 !
- **git**에서 마음껏 version 관리를 해놓은 **commit들**을 push해서 **github**에 저장한다. 

## Stage ❓
- commit할 준비를 시키는 것.

## ⌨️ 명령어

<br>

### git 설정, 상태

```
# git version 확인 !
$ git --version
```

```
# 🌱 git 초기화. 초기화 하면 .git 이라는 폴더가 생기며 version관리 시작가능 !
$ git init
```

```
# ⚙️ git 사용자 글로벌 설정 목록 조회 
$ git config --global --list
```

```
# 글로벌 사용자 설정
$ git config --global user.name "User name"
```

```
> 글로벌 이메일 설정
$ git config --global user.email "git@gmail.com"
```

```
> 기본 브랜치를 main으로 설정
$ git config --global init.defaultBranch main
```

``` 
> 👀 현재 상태 확인 commit할 파일이 있는지 add할 파일이 있는지
$ git status
```

``` 
# commit log 확인하기
$ git log
```

``` 
# 현재 branch 확인하기
$ git branch
```

```
# 전체 파일 스테이지에 올리기 dot 대신에 파일명을 쓰면 해당 파일만 올라감
$ git add .
```