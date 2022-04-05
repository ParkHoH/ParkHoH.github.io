---
title: "[Git] git innore 적용 안 될 때 해결 방법"
excerpt: ""

categories: Git
tags: [Git, Git ignore, .ignore]

date: 2022-04-05
last_modified_at: 2022-04-05
---

제외하고 싶은 properties가 있어 .gitignore에 넣었는데 작동이 되지 않는다.


이유를 찾아보니 git의 캐시 문제로 인해 발생하는 것이고 해결 방법은 간단하다.


먼저 추적을 원하지 않는 파일만 남기고 커밋을 한 뒤에 아래 명령어를 순차적으로 입력하면 해결된다.

```console
git rm -r --cathed .
git add .
git commit -m "원하는 메시지"
```