# Git/GitHub 특강


목차
[Git 초기 설정](##Git-초기-설정)  
[로컬 저장소](##로컬-저장소)  
[Gir 기본 명령어](##Gir-기본-명령어)  
[git init](###git-init)  
[git status](###git-status)  
[git add](###git-add)  
[git commit](###git-commit)  
[git log](###git-log)  
[원격 저장소](##원격-저장소)  
[git remote](###git-remote)  
[git push](###git-push)  

---


## Git 초기 설정

1. 누가 커밋 기록을 남겼는지 확인할 수 있도록 이름과 이메일을 설정합니다.
    
    ```bash
    $ git config --global user.name "이름"
    $ git config --global user.email "메일 주소"
    ```
    
2. 작성자 확인
    
    ```bash
    $ git config --global -l
    
    또는
    
    $ git config --global --list
    ```
    

## 로컬 저장소

> **working directory에서 staging area로 변경점을 add 하고 최종적으로 Repository에 커밋**
- `Working Directory (=Working Tree)` : 사용자의 일반적인 작업이 일어나는 곳
- `Staging Area (=Index)` : 커밋을 위한 파일 및 폴더가 추가되는 곳
- `Repository` : Staging area에 있던 파일 및 폴더의 변경 사항을 저장하는 곳

## Git 기본 명령어

### git init

```bash
$ git init

-------------------------------------------------------------------------------
출력:
Initialized empty Git repository in C:/Users/kyle/git-practice/.git/

kyle@KYLE MINGW64 ~/git-practice (master)
```

- 현재 작업 중인 디렉토리를 Git으로 관리한다고 선언
- `.git` 이라는 숨김 폴더를 생성하고, 터미널에는 `(master)` 라고 표기

**주의사항**

1. 이미 Git 저장소인 폴더 내에 또 다른 Git 저장소를 만들면 안된다. (중첩 금지)
2. 절대로 홈 디렉토리에서(다른 파일이나 dir가 많은) git init을 하지 않는다.

### git status

```bash
$ git status

-------------------------------------------------------------------------------
출력:
On branch master

No commits yet

nothing to commit (create/copy files and use "git add" to track)
```

- Working Directory와 Staging Area에 있는 파일의 현재 상태를 알려주는 명령어
- 어떤 작업을 시행하기 전에 수시로 status를 확인(오류 나고 확인 하는 것보다 낫지)

**상태**

1. `Untracked` : Git이 관리하지 않는 파일 (한번도 Staging Area에 올라간 적 없는 파일)
2. `Tracked` : Git이 관리하는 파일
    - `Unmodified` : 최신 상태
    - `Modified` : 수정되었지만 아직 Staging Area에는 반영하지 않은 상태
    - `Staged` : Staging Area에 올라간 상태

### git add

```bash

# 특정 파일
$ git add a.txt

# 특정 폴더
$ git add my_folder/

# 현재 디렉토리에 속한 파일/폴더 전부
$ git add .

```

- Working Directory에 있는 파일을 Staging Area로 올리는 명령어
- Git이 해당 파일을 관리하게끔 함
- `Untracked, Modified → Staged` 로 상태를 변경

예시

```bash
$ touch a.txt b.txt

$ git status

-------------------------------------------------------------------------------
출력:
On branch master

No commits yet

Untracked files: # 트래킹 되고 있지 않는 파일 목록
  (use "git add <file>..." to include in what will be committed)
        a.txt
        b.txt

nothing added to commit but untracked files present (use "git add" to track)

# a.txt만 Staging Area에 올립니다.

$ git add a.txt

$ git status

-------------------------------------------------------------------------------
출력:

On branch master

No commits yet

Changes to be committed: # 커밋 예정인 변경사항(Staging Area)
  (use "git rm --cached <file>..." to unstage)
        new file:   a.txt

Untracked files: # 트래킹 되고 있지 않은 파일
  (use "git add <file>..." to include in what will be committed)
        b.txt
```

### git commit

```bash
$ git commit -m "first commit"

-------------------------------------------------------------------------------
출력:
[master (root-commit) c02659f] first commit
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 a.txt
```

- Staging Area에 올라온 파일의 변경 사항을 하나의 버전(커밋)으로 저장하는 명령어
- 각각의 커밋은 해쉬 값을 ID로 사용
- `(root-commit)` 은 해당 커밋이 최초의 커밋 일 때만 표시

### git log

```bash
$ git log

-------------------------------------------------------------------------------
출력:
commit 1870222981b4731d14ef91d401c68c0bbb2f6e7d (HEAD -> master)
Author: kyle <kyle123@hphk.kr>
Date:   Thu Dec 9 15:26:46 2021 +0900

    first commit
```

- 커밋의 내역(`ID, 작성자, 시간, 메세지 등`)을 조회할 수 있는 명령어
- 옵션
    - `--oneline` : 한 줄로 축약
    - `--graph` : 브랜치와 머지 내역을 그래프로 표시
    - `--all` : 현재 브랜치를 포함한 모든 브랜치의 내역
    - `--reverse` : 커밋 내역의 역순
    - `-p` : 파일이 변경된 내용도 표시
    - `-2` : 원하는 갯수 만큼의 내역

## 원격 저장소

> Github의 원격 저장소를 활용하여 로컬 저장소를 다른 사람과 공유 가능

### git remote

- 로컬 저장소에 원격 저장소를 `등록, 조회, 삭제`할 수 있는 명령어
1. **등록**
    
    `git remote add <이름> <주소>` 
    
    ```bash
    $ git remote add origin https://github.com/edukyle/TIL.git
    ```
    
2. **조회**
    
    `git remote -v` 
    
    ```bash
    $ git remote -v
    origin  https://github.com/edukyle/TIL.git (fetch)
    origin  https://github.com/edukyle/TIL.git (push)
    
    add를 이용해 추가했던 원격 저장소의 이름과 주소를 출
    ```
    
3. **삭제**
    
    `git remote rm <이름>` 혹은 `git remote remove <이름>` 
    
    ```bash
    $ git remote rm origin
    $ git remote remove origin
    ```
    

### git push

- 로컬 저장소의 커밋을 원격 저장소에 업로드 하는 명령어
- git commit이 이루어진 `Staged` 의 데이터를 원격 레파지토리에 올리는 과정

```bash
$ git push origin master

------------------------------------------------

$ git push -u origin master
-u로 한번 잡으면 $ git push 라고만 작성해도 push 가능

$ git push

-------------------------------------------------------------------------------
출력:
Enumerating objects: 4, done.
Counting objects: 100% (4/4), done.
Delta compression using up to 4 threads
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 2.23 KiB | 2.23 MiB/s, done.
Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
To https://github.com/kihyuny/TIL.git
   43a19fc..6f22350  master -> master
```