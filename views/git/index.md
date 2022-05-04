---
layout: views
title: git
nav_order: 6
has_children: true
permalink: /git
---

# ![git icon](/assets/images/icon/Git-Icon-1788C.png){:class="icon-title"} git

* [Git Command](#git-command)

git 자주 쓰는 command
{:class="desc-text"}

# Git Command


## branch 생성
---

### local branch 생성

```bash
git checkout -b feature-01
```

### remote branch push
```bash
git push origin feature-01
```

### remote branch 동기화
```bash
git branch --set-upstream-to origin/feature-01
```

## branch 삭제
---

### remote branch 동기화
```bash
git branch --delete feature-01
```

### remote branch 동기화
```bash
git push origin :feature-01
```

## commit message 변경(push 이전)
---

```bash
git commit --amend -m "바꿀 메시지"
```

---
