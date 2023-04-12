---
layout: post
title:  "[WslRegisterDistribution failed with error: 0x80370114] wsl로 ubuntu 설치 시 발생하는 오류"
date: 2023-04-12 18:15:32 +0900
categories: Issue
excerpt: "wsl1 --install -d Ubuntu 명령어 실행 시 발생하는 오류로, WSL 버전을 1로 낮추어 해결하자."
tags: [WSL, Ubuntu]
 
date: 2020-05-25
last_modified_at: 2020-05-25
---

깃헙 블로그를 개설하며 생겼던 이슈사항을 포스팅해볼게요!<br><br>
참고로 저는 [SuperMemi's Study님의 깃헙 블로그 만들기](https://supermemi.tistory.com/entry/%EB%82%98%EB%A7%8C%EC%9D%98-%EB%B8%94%EB%A1%9C%EA%B7%B8-%EB%A7%8C%EB%93%A4%EA%B8%B0-Git-hub-blog-GitHubio?category=997749) 3단계를 따라가며 만들었습니다!<br><br>

---
<br>

- 상황 : wsl 로 ubuntu 설치 시 error 발생<br>

```bash
wsl --install -d Ubuntu
```
![](/public/img/2023-04-12-1.png)
→ 설치 실패<br><br>

---

<br>먼저 시도했지만 되지 않았던 방법도 올릴게요!
먼저 해보시고 안되면 아래 방법을 사용하시면 될 것 같아요~<br><br>

[시도한 방법 1](https://answers.microsoft.com/en-us/insider/forum/all/wsl-2-installing-linux-failed-error-code/bae391d1-4215-4d93-b0c4-3d96404a7c74)

다음 해결법 5단계에서 “C:\WINDOWS\System32\vmcompute.exe”를 찾을 수 없음<br><br>

[시도한 방법 2](https://www.reddit.com/r/bashonubuntuonwindows/comments/zlf0js/error_0x80370114_but_vmcomputeexe_doesnt_even/)

세번째 댓글의 해결법으로 해결한 사람이 몇몇 보이나..  난 저 과정(’Windows 하이퍼바이저 플랫폼’을 켜기)을 거쳐도 ‘vmcompute.exe’ 가 생성되지 않음<br><br>

---
<br>

- 해결 : wsl 기본 설정 버전을 2에서 1로 낮춤

```bash
wsl --set-default-version 1
```

- version 1로 잘 설정되어있는지 확인
```bash
wsl --status
```

- 다시 실행
```bash
wsl --install -d Ubuntu
```

→ "~~~ Enter new UNIX username" 결과 출력 (설치 성공)

![](/public/img/2023-04-12-2.png)

> 참고로, UNIX username은 1~32자 사이의 소문자 알파벳, 숫자, 밑줄(_), 하이픈(-)으로만 이루어져야 하며, 첫 글자는 반드시 소문자 알파벳이어야 한다. (정규표현식 : ^[a-z][a-z0-9_-]{0,31}$)

<br>

---

<br>[시도한 방법 2](https://www.reddit.com/r/bashonubuntuonwindows/comments/zlf0js/error_0x80370114_but_vmcomputeexe_doesnt_even/) 를 했을 때 안되었던 이유들과 해결법을 생각한 과정을 남깁니다...<br><br>

먼저 `vmcompute.exe`가 왜 계속 안생겼는지...<br><br>

- vmcompute.exe란? 
> Windows 하이퍼바이저 플랫폼(=Windows의 서브시스템으로, 여러 가상화 플랫폼을 호스팅하고 사용할 수 있도록 함)에서 제공되는 기능 중 하나인 Hyper-V 가상 머신(Windows의 가상화 기술 중 하나로, Windows 환경에서 가상 머신을 만들고 실행할 수 있도록 함)을 관리하는데 사용되는 프로세스
<br><br>
- 내 컴퓨터는 Windows 11 Home 버전 → Hyper-V 기능을 지원하지 않음 (Windows 11 Pro, Enterprise, Education 버전에서만 지원) → vmcompute.exe 파일이 없음 → 오류 발생
<br><Br>
- 해결법 : Hyper-V를 설치하도록 업그레이드하기 (이런 방법이 있다고 하지만, 난 Hyper-V 기능을 필요로 하지 않는 wsl1을 사용하기로 선택함!)
<br><br>

---

<br>그럼 `WSL1`과 `WSL2`의 차이점은?

- `WSL1` : 가상화 사용X, 윈도우 시스템 콜을 사용하여 Linux 바이너리 실행

- `WSL2` : 가상머신기술(Hyper-V)를 사용하여 Linux 커널 실행

---

<br>이후 `Jekyll` 설치하며 생긴 이슈를 포스팅할 계획입니다~! 안녕~