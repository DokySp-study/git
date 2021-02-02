# git stash



현재 변경사항들을 임시로 저장해둘 수 있는 유용한 기능



```bash
$ git stash
```

위 명령어를 입력하면 현재 변경사항들이 stash 되어 별도의 공간에 저장된다. 다시 사용하고 싶을 때,



```bash
$ git stash pop
```

이렇게 입력하면 최근 stash한 내용들이 내 레포지토리에 복원된다.



```bash
$ git stash list
```

stash한 내용들을 모두 확인할 수 있다.

