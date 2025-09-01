---
title: "로컬에 GPU 개발 환경 구축하기 (VS Code, Anaconda)"
date: 2025-09-01 14:00:00 +0900
categories: [Setup]
---

&nbsp;

이번 포스팅에서는 로컬에서 GPU 서버에 접속하는 방법을 다룬다. 포스팅을 위해 두 개의 블로그 [[1]](<http://youngseokim.tistory.com/147>), [[2]](<https://jstar0525.tistory.com/14>)를 참고했다.

<br>

## VS Code

<br>

**① [Remote - SSH] Extension 설치**

<br>

![Screenshot 2](/assets/img/2025-09-01/server-setup-2.png)

**② 새로운 SSH Host 추가**

<br>

이 과정에서 할당받은 IP 주소와 PW를 입력한다.

<br>

![Screenshot 3](/assets/img/2025-09-01/server-setup-3.png)|![Screenshot 4](/assets/img/2025-09-01/server-setup-4.png)|![Screenshot 5](/assets/img/2025-09-01/server-setup-5.png)

<br>

**③ SSH configuration file을 저장할 장소 선택**

<br>

![Screenshot 6](/assets/img/2025-09-01/server-setup-6.png)

<br>

Host 추가가 완료되었다면 아래의 과정을 통해 GPU 서버에 접속할 수 있다.

<br>

![Screenshot 7](/assets/img/2025-09-01/server-setup-7.png)|![Screenshot 8](/assets/img/2025-09-01/server-setup-8.png)|![Screenshot 9](/assets/img/2025-09-01/server-setup-9.png)|![Screenshot 10](/assets/img/2025-09-01/server-setup-10.png)


<br>

## Anaconda

<br>

아나콘다 공식 홈페이지 [[3]](<https://www.anaconda.com/docs/getting-started/anaconda/install#macos-linux-installation>)에서 최신 버전의 Linux 환경에서의 아나콘다 설치 명령어를 복사/붙여넣기 한다. 이후의 구체적인 설치 과정은 아나콘다 공식 홈페이지와 그 과정이 잘 정리되어 있는 블로그 [[4]](<https://kagus2.tistory.com/63>)를 참고한다.

<br>

```bash
# Download the latest version of Anaconda Distribution
curl -O https://repo.anaconda.com/archive/Anaconda3-2025.06-0-Linux-x86_64.sh

# Install Anaconda Distribution
bash ~/Anaconda3-2025.06-0-Linux-x86_64.sh

source ~/.bashrc
```

<br>

![Screenshot 11](/assets/img/2025-09-01/anaconda-installation-1.png)|![Screenshot 12](/assets/img/2025-09-01/anaconda-installation-2.png)

<br>

설치가 끝나면 아나콘다 명령어를 실행해 설치가 정상적으로 이루어졌는지 확인한다.

<br>

![Screenshot 13](/assets/img/2025-09-01/anaconda-installation-3.png)|![Screenshot 14](/assets/img/2025-09-01/anaconda-installation-4.png)

<br>

![Jukung](/assets/img/2025-09-01/jukung.png)

<center>
포스팅을 마무리하며 주먹밥 쿵야에게 무한 감사합니다. :D
</center>

<br>

---

## References
[1] <http://youngseokim.tistory.com/147>  
[2] <https://jstar0525.tistory.com/14>  
[3] <https://www.anaconda.com/docs/getting-started/anaconda/install#macos-linux-installation>  
[4] <https://kagus2.tistory.com/63>