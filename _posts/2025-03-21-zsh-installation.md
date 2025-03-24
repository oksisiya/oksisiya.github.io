---
title: Zsh Installation
date: 2025-03-21
categories: Setup
---

&nbsp;  

## Introduction

우리가 터미널을 열었을 때 기본으로 사용하는 쉘은 Bash이다. Bash와 Zsh 중 어떤 것을 선택해도 되며, 사용자의 취향 차이이다. Zsh은 터미널을 예쁘게 꾸밀 수 있는 (사용자 편의에 따라 커스터마이징 할 수 있다는) 장점이 있어 Linux 사용자나 Mac 사용자들 사이에서 많이 사용되는 쉘 중 하나이다.

<br>

## 1. Zsh 설치

<br>

아래 코드 박스의 명령어를 한 줄씩 실행한다. `chsh -s $(which zsh)`은 기본 쉘을 설정하는 명령어이다. Zsh 설정 후 마음이 변했다면 다시 Bash로(`chsh -s $(which bash)`) 바꾸면 된다.

<br>

```bash
sudo apt update
sudo apt install zsh
chsh -s $(which zsh)
```

<br>

설정 후 새 터미널을 열어 보보면 다음과 같은 경고(안내) 문구가 뜨는데 이건 잠깐 무시한다.

<br>

## 2. Oh-My-Zsh 설치

<br>

Oh-My-Zsh은 Zsh을 쉽고 편하게 설정할 수 있도록 지원하는 툴이다. 아래의 명령어를 입력하면 자동으로 설치된다. 기본 쉘로 Zsh을 사용할지 물을 때 `Y`를 입력하고 비밀번호를 입력하면 설치가 완료된 터미널을 확인할 수 있다.

<br>

```bash
sh -c "$(wget https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh -O -)"
```

<br>

마찬가지로 새 터미널을 열어 보면 좀 전의 경고(안내) 문구가 사라지고, 터미널이 미묘하게 바뀐 것을 확인할 수 있다.

<br>

## 3. Powerlevel10k 설치

<br>

Zsh의 테마인 Powerlevel10k을 설치한다. 테마를 설치하기에 앞서 관련 폰트를 설치한다.

<br>

### 3-1. MesloLGS NF Regular.ttf 설치

<br>

Powerlevel10k 깃허브 리포지토리 > Fonts > MesloLGS NF Regular.ttf 폰트를 설치한다. (하나를 다운 받으면 네 개의 폰트 모두가 설치된다.)  
(링크: https://github.com/romkatv/powerlevel10k?tab=readme-ov-file#fonts)
  
파일 다운로드 후 Downloads 디렉토리에서 다운 받은 파일을 열고 Install 버튼 클릭 후 Installed로 바뀐 것을 확인한다.

<br>

### 3-1. Powerlevel10k 설치

<br>

3-1. 과정을 마쳤다면 Powerlevel10k 깃허브 리포지토리 > Installation > Oh My Zsh에 들어가 Oh My Zsh에 해당하는 코드를 카피하고 붙여넣기 해서 클론한다.
(링크: https://github.com/romkatv/powerlevel10k?tab=readme-ov-file#oh-my-zsh)

<br>

> **Tips!** 만약 이 과정에서 git이 없어 에러가 뜬다면 `sudo apt install git` 명령어를 실행한 뒤 진행하면 된다.


<br>

```bash
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git "${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k"
```

<br>

Home 디렉토리에서 파일 숨김 해제 후(`Ctrl + H`) Zsh의 기본 설정에 관한 파일인 .zshrc 파일을 연다. (Bash의 기본 설정에 관한 파일은 .bashrc이다.)
  
Tips! .zshrc 파일을 열었을 때 다 흰색으로 나와서 뭐가 뭔지 모르겠다면. 파일 창 하단의 Plain Text를 sh로 바꾸면 된다.

<br>

파일의 11번째 줄을 보면 Zsh 테마가 `robbyrussell`인 것을 확인할 수 있다. `robbyrussell` 대신 `powerlevel10k/powerlevel10k`로 바꾸고 Save 버튼을 누른다.

<br>

새 터미널을 열어 보면 Powelevel10k 설정 창이 나온다. 앞서 폰트가 정상적으로 설치되었는지 확인하기 위해 다이아몬드가 보이는지, 자물쇠가 보이는지 등등을 묻는다. 만약 안내 문구 대로 관련 기호가 보이지 않는다면 폰트 설치를 확인한다.

<br>

여기서부터는 개인의 취향이다. 취향 껏 터미널을 설정하면 된다.

<br>

> **Tips!** 만약 다시 세팅하고 싶다면 `p10k configure` 명령어를 통해 재설정하고 기존에 생성된 설정 파일에 overwrite 하면 된다.

![Reset Powerlevel10k](\assets\img\resetting-powerlevel10k.png)

<br>


## 4. Zsh Plugin

<br>

Oh-My-Zsh과 관련된 유용한 플러그인이 있다.

### zsh-syntax-highlighting


![Before highlighting](\assets\img\sudo-apt-install-no-color.png)


### zsh-autosuggestions


&nbsp;

---