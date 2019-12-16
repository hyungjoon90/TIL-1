# Git Commit Log 확인

```
# git commit log 출력
git log .

# git 최근 10개 commit log 출력
git log -10
```

git에서 log 출력이지만 개수 지정도 가능하지만 터미널에서 전부 출력이 되지 않는다.


```
git log --pretty=oneline -10

or 

git log --oneline -n 10
```

이렇게 하면 최근 10개의 커밋을 이쁘게 출력할 수 있다.

```
-- author=<your name>

git log --oneline -n 5 --author=JHyeok
```

위 명령행을 사용하여 해당 사용자가 커밋한 내용만 확인할 수 있다.

---
#### 참고

https://stackoverflow.com/questions/13542213/git-see-a-list-of-comments-of-my-last-n-commits
