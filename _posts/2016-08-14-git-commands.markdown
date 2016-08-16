---
layout: post
title:  "GIT 커맨드 정리"
date:   2016-08-15 17:36:15 +0900
categories: memo
---

local git repository 생성
========================

* git init

branch 관련
========================

* git branch : 브랜치 목록 보기
* git branch -r : 원격브랜치 목록 보기
* git branch <new branch name>
* git branch <new branch name> -b : brach 를 생성하고 바로 checkout
* git branch <new branch name><org branch name> : <org branch name> 브랜치 에서 새로운 <new branch name> 브랜치 생성
* git branch -d <branch name> : 브랜치 제거
* git push origin :<branch name> : 원격 브랜치 제거
* git checkout <branch name> : branch 변경
* git add <filename> : git staging area 추가
* git commit -a : git 에서 알고 있는 모든 변경된 파일을 커밋
* git branch --set-upstream-to=origin/<branch> <branch> : <branch> 원격이랑 연결 해서 추후에 귀찮은 입력 안하도록 세팅
* git fetch : 지역저장소의 원격 브랜치가 갱신됨
* git pull : 원격 서버에서 변경 사항을 가져와서 동시에 지역 브랜치에 합침 (fetch 이후에 지역브랜치에 합침)
* git push : origin 원격저장소에 push 하기
* git push --dry-run: 푸싱괸 변경 사항 확인
* git push origin <branchname> : origin 원격 저장소에 branchname 을 push 하기

merge 관련
=========

* git merge <alternate> : <alternate>브랜치와 현재 branch merge
* git merge --squash <alternate> : <alternate> 블랜치의 변경내역을 모두 한개의 커밋으로 합침
* git cherry-pick 34abcd  ( contact 브랜치의 34abcd 커밋을 master 브랜치에 커밋한다)
* git reset --hard : merge 내역을 포함한 커밋되지 않은 모든 내역 제거 ( merge 후 마음에 컨플릭트 많이 나서 되돌리고 싶을때 사용 )

remote repository 와의 비교
=========

* git diff origin/<branchname> : remote 와 현재 branch 비교
* git diff --stat origin/<branchname> : remote 와 현재 branch 비교하여 시각화
* git remote remove origin : remote repository detach
* git remote add orgin https://github.com/cloudera/hue.git : cloudera hue repository 추가
* git remote update : fetch
* git branch -r : 외부 브랜치 보기
* git remote set-url origin git-repo@domain:project.git

archive
=======

* git archive --format=tar --prefix=hue/ HEAD | gzip > hue.tar.gz : archive 예제

롤백
===

개별파일 원복
----------
* git checkout  -- <파일> : 워킹트리의 수정된 파일을 index에 있는 것으로 원복
* git checkout HEAD -- <파일명> : 워킹트리의 수정된 파일을 HEAD에 있는 것으로 원복(이 경우 --는 생략가능)
* git checkout FETCH_HEAD -- <파일명> : 워킹트리의 수정된 파일의 내용을 FETCH_HEAD에 있는 것으로 원복? merge?(이 경우 --는 생략가능)

index 추가 취소
-------------
* git reset -- <파일명> : 해당 파일을 index에 추가한 것을 취소(unstage). 워킹트리의 변경내용은 보존됨. (--mixed 가 default)
* git reset HEAD <파일명> : 위와 동일

commit 취소
----------
* git reset HEAD^ : 최종 커밋을 취소. 워킹트리는 보존됨. (커밋은 했으나 push하지 않은 경우 유용)
* git reset HEAD~2 : 마지막 2개의 커밋을 취소. 워킹트리는 보존됨.
* git reset --hard HEAD~2 : 마지막 2개의 커밋을 취소. index 및 워킹트리 모두 원복됨.
* git reset --hard ORIG_HEAD : 머지한 것을 이미 커밋했을 때,  그 커밋을 취소. (잘못된 머지를 이미 커밋한 경우 유용)
* git revert HEAD : HEAD에서 변경한 내역을 취소하는 새로운 커밋 발행(undo commit). (커밋을 이미 push 해버린 경우 유용)

워킹트리 전체 원복
-------------
* git reset --hard HEAD : 워킹트리 전체를 마지막 커밋 상태로 되돌림. 마지막 커밋이후의 워킹트리와 index의 수정사항 모두 사라짐. (변경을 커밋하지 않았다면 유용)
* git checkout HEAD . : ??? 워킹트리의 모든 수정된 파일의 내용을 HEAD로 원복.
* git checkout -f : 변경된 파일들을 HEAD로 모두 원복(아직 커밋하지 않은 워킹트리와 index 의 수정사항 모두 사라짐. 신규추가 파일 제외)

푸시push 한거 취소
---------------

* git reset HEAD^
* git push origin -f

> #### 참조 : reset 옵션
 1. --soft : index 보존, 워킹트리 보존. 즉 모두 보존.
 2. --mixed : index 취소, 워킹트리만 보존 (기본 옵션)
 3. --hard : index 취소, 워킹트리 취소. 즉 모두 취소.

untracked 파일 제거
=================

* git clean -f
* git clean -f -d : 디렉토리까지 제거

서브모듈
======

* git submodule add git-repo-addr

서브모듈 제거 remove
-----------------

* mv asubmodule asubmodule_tmp
* git submodule deinit asubmodule    
* git rm asubmodule
> Note: asubmodule (no trailing slash)
  or, if you want to leave it in your working tree
* git rm --cached asubmodule
* mv asubmodule_tmp asubmodule

remote 에 있는 tag 와 branch까지 다른 remote 에 push 하기
===================================================

>
1. cd /path/to/my/repo
2. git remote add origin {remote repository}
3. git push -u origin --all # pushes up the repo and its refs for the first time
4. git push origin --tags # pushes up any tags


환경설정 관련
==========

변수 출력
-------
* $ git config -l (글로벌 변수 만 보려면 git config --global -l)

사용자 정보 세팅
------------

* $ git config --global user.name "John Doe"
* $ git config --global user.email johndoe@example.com
