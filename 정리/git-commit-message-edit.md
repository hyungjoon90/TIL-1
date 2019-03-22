# push 된 git commit message 수정
---

```git commit --amend -m "메세지 수정"```

로 커밋 메세지 새롭게 수정한 후에
아래와 같은 방법으로 푸시하면 된다.

```
git push [remote] [branch] --force
```
```
git push <remote> [branch] -f
```
