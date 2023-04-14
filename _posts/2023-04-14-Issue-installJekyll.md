---
layout: post
title:  "[Jekyll 설치 시 발생하는 오류들] You don't have write permissions for the /var/lib/gems/3.0.0 directory."
date: 2023-04-14 09:26:23 +0900
categories: Issue
excerpt: "gem install bundler 또는 bundle install 실행 시 발생하는 오류로, rbenv와 ruby-build를 이용하여 해결하자."
tags: [Jekyll, Ubuntu]
---

안녕하세요! 깃헙 블로그를 만들며 생겼던 오류 2탄입니다! 이번에는 블로그에 테마를 입히기 위한 Jekyll을 설치할 때 생긴 오류들입니다~ (자잘한 이슈들부터 하나씩 적을게요!)
<br><br>

참고로 저는 [SuperMemi's Study님의 깃헙 블로그 만들기](https://supermemi.tistory.com/entry/%EB%82%98%EB%A7%8C%EC%9D%98-%EB%B8%94%EB%A1%9C%EA%B7%B8-%EB%A7%8C%EB%93%A4%EA%B8%B0-Git-hub-blog-GitHubio?category=997749) 의 3단계를 따라가며 만들었습니다! 여기서는 2번째 단계 실행 시 발생한 이슈입니다~!
<br><br><br><br><br>


### 0. 먼저, `gem`과 `bundler`와 `gemfile`에 대해 모르신다면 이 블로그를 읽어보세요!
[https://ideveloper2.tistory.com/80](https://ideveloper2.tistory.com/80)
<br><br><br><br>


### 1. ruby 패키지 관리자 설치 시 오류 발생
<br>

```bash
sudo apt install ruby-rubygems
```

→ **`Unable to fetch some archives, maybe run apt-get update or try with --fix-missing?`** 오류 메세지 발생
<br><br>

```bash
sudo apt-get update
```

→ `apt-get`을 업데이트하여 해결
<br><br><br>

> 참고로, ruby-rubygems은 ruby 패키지 관리자 (js의 npm 같은 것)

<br><br><br><br>


### 2. bundler 설치 시 오류 발생
<br>

```bash
gem install bundler
```

→ **`You don't have write permissions for the /var/lib/gems/3.0.0 directory.`** 오류 메세지 발생
<br><br>

```bash
sudo gem install bundler
```

→ sudo (루트 권리로 실행) 를 사용하여 해결
<br><br>

→ 하지만, sudo를 사용하면 모든 프로젝트에 적용된다는 단점이 있음
<br><br>

→ 추천하는 해결법 : 기존 ruby를 삭제하고, rbenv(ruby 버전 관리)와 ruby-build를 이용하여 ruby 재설치
<br>
다음 링크를 참조 : [rbenv와 ruby-build를 이용하기 (by. stackoverflow)](https://stackoverflow.com/questions/37720892/you-dont-have-write-permissions-for-the-var-lib-gems-2-3-0-directory)
<br><br><br><br>


### 3. jekyll 설치 시 오류 발생
<br>

```bash
gem install jekyll
```

→ **`error installing jekyll: error: failed to build gem native extension.`** 오류 메세지 발생
<br><br>

```bash
sudo apt-get install ruby-full build-essential zlib1g-dev
```

→ jekyll 설치에 필요한 파일 실행하여 해결
<br><br>
> [jekyll 공식문서](https://jekyllrb.com/docs/installation/ubuntu/)를 참고함

<br><br><br><br>


### 4. bundler로 gem 파일 설치 시 오류 발생
<br>

```bash
bundle install
```

→ **`You don't have write permissions for the /var/lib/gems/3.0.0 directory.`** 오류 메세지 발생
<br><br>

→ 아까와 같은 방법으로 해결 (위에서 이미 했다면 오류 안뜰 것) : 기존 ruby를 삭제하고, rbenv(ruby 버전 관리)와 ruby-build를 이용하여 ruby 재설치
<br>
다음 링크를 참조 : [rbenv와 ruby-build를 이용하기 (by. stackoverflow)](https://stackoverflow.com/questions/37720892/you-dont-have-write-permissions-for-the-var-lib-gems-2-3-0-directory)
<br><br><br><br><br>

***
<br>
그럼 안녕~