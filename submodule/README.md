# Submodule



node.js의 npm, Dart의 pub, iOS 개발 시 pod 같이 개발에 필요한 라이브러리를 쉽게 불러와 사용할 수 있는 패키지 매니저들이 존재한다.

회사에서 개발 업무를 진행하다보면, 하나의 앱만 개발하는 것이 아니라 다양한 앱을 만들게 된다. 다만, 이 앱들이 각자 완전히 별개의 앱이 아니라는 것에서 submodule의 필요성이 느껴졌다.



우리 회사의 경우, 또는 우리 회사와 비슷하게 제품과 통신을 하는 경우에는 회사만의 제품간 프로토콜이 존재하기 마련이다. ~~(물논 없을 수도 있ㄷ...)~~ 특별한 경우가 아니라면 프로토콜이 제품별로 완전히 별개로 동작한다기 보다는 어느정도 규칙성을 가지고, 이를 관리하는 로직도 비슷할 것이다.



문제는 각각의 앱마다 이러한 코드를 단순히 복붙하다보니.. 최신화와 코드 관리에 엄청나게 취약해지게 된다는 것이다. 처음에 한 두개 앱만 있을 때에는 큰 문제가 되지 않았지만... 앱이 점점 늘어나게 되면서 모든 앱들이 저마다의 Business Logic을 가지게 되어 유지보수가 불가능한 상황까지 오게 된다.



상황이 이렇다보니, git의 기능 중 submodule이라는 기능에 대해 알아보게 되었고 이에 대한 기술적 내용을 풀어보고자 한다.



## Submodule?

우선, 간단하게 git 레포 내부에 자식 레포가 있는 것을 말한다. 아니 그러면 그냥 git 레포 안에 또 다른 git 레포를 clone하면 되는 것과 뭐가 다르지..? 라는 생각을 나는 가장 먼저 했다. 하지만, git의 기능을 생각해보면 사실 매우 간단하게 해결이 되는 문제이다. 



git이라는 것 자체가 변화를 기록하여 버전을 관리하는 툴이다. 만약 레포 A 안에 다른 레포 B를 두고, B에 내용을 변경하게 된다면 A 레포 자체에도 변화가 가는 것으로 인식을 해버리게 된다. 즉 독립적으로 관리가 불가능하게 되고, 매번 불필요한 커밋이 필요해지게 되는 것이다.



## 그렇다면 Submodule은 무엇이 다른건가?

Submodule로 레포를 nested로 구성하게 되면, 우선 레포가 디렉토리로 인식되는 것이 아닌, 바로가기로써 부모 레포에서 인식하게 된다. 그리고 이 바로가기 링크는 submoudule의 최신 커밋? 을 가르키가 된다.



즉, 이렇게 되면, 부모 레포에서는 자식 레포의 커밋 번호만 기록하면 됨과 동시에, 부모와 자식 레포를 별개로 관리할 수 있게 되는 것이다.




```bash
# 추가
$ git submodule add --branch <BRANCH> <REPO URL> <OP: DIR_NAME>

# 서브모듈에 변경사항이 있을 경우,
# ->

# PUSH
$ git push --recurse-submodules=on-demand  # 확인 후 자동 푸시
$ git push --recurse-submodules=check      # 확인 후 문제 없으면 푸시

# 항상 쓸거면 아래와 같이 설정해두면 편하다.
$ git config push.recurseSubmodules check
$ git config push.recurseSubmodules on-demand


# PULL
# - no-ff 옵션이 기본인 경우
$ git submodule foreach git pull --ff
# - ff 옵션이 기본인 경우
$ git submodule update --remote --merge
# no-ff 옵션인 경우에서 update를 해버리면 머지 커밋이 생겨 그래프가 엉망이 된다..
```



### 참고 및 출처

https://pinedance.github.io/blog/2019/05/28/Git-Submodule

