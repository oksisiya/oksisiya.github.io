---
title: Zsh 설치, Oh-My-Zsh 설치, 관련 Plugins
date: 2025-03-21
categories: Setup
---

&nbsp;  

우리가 터미널을 열었을 때 기본으로 사용하는 쉘은 Bash이다. Bash와 Zsh 중 어떤 것을 선택해도 되며, 사용자의 취향 차이이다. Zsh은 터미널을 예쁘게 꾸밀 수 있는 (사용자 편의에 따라 커스터마이징 할 수 있는) 장점이 있어 Linux 사용자나 Mac 사용자들 사이에서 선호되는 쉘 중 하나이다.

<br>

## 1. Zsh 설치

<br>

아래 코드 박스의 명령어를 한 줄씩 실행한다. `chsh -s $(which zsh)`은 Zsh을 기본 쉘로 설정하는 명령어이다. 설정 후 마음이 바뀌었다면 다시 Bash로 되돌릴 수 있다.

<br>

```bash
sudo apt update
sudo apt install zsh
chsh -s $(which zsh)
# chsh -s $(which bash)
```

<br>

새 터미널을 열면 다음과 같은 안내 문구가 뜨는데 이건 잠깐 무시한다.

<br>

![Warning](/assets/img/2025-03-21/warning.png)

<br>

## 2. Oh-My-Zsh 설치

<br>

Oh-My-Zsh은 Zsh을 쉽고 편하게 설정하도록 지원하는 툴이다. 아래의 명령어를 실행하면 자동으로 설치가 이루어진다. 기본 쉘로 Zsh을 사용할 것인지 물을 때 `Y`를 입력해 설치를 진행한다.

<br>

```bash
sh -c "$(wget https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh -O -)"
```

<br>

![Shell to Zsh](/assets/img/2025-03-21/shell-to-zsh.png)|![Oh-My-Zsh](/assets/img/2025-03-21/oh-my-zsh.png)

<br>

새 터미널을 열면 좀 전의 안내 문구가 사라지고 프롬프트가 바뀐 것을 확인할 수 있다.

<br>

![No warning](/assets/img/2025-03-21/no-warning.png)

<br>

## 3. Powerlevel10k 설치

<br>

Powerlevel10k는 Zsh의 테마 중 하나이다. 테마를 설치하기에 앞서 관련된 폰트를 설치한다.

<br>

### 3-1. MesloLGS NF Regular.ttf

<br>

[Powerlevel10k 깃허브 리포지토리 > Fonts](https://github.com/romkatv/powerlevel10k?tab=readme-ov-file#fonts) > MesloLGS NF Regular.ttf 폰트를 다운로드한다. 하나의 폰트를 다운로드하면 모든 폰트가 다운로드된다.

<br>

![Fonts repository](/assets/img/2025-03-21/repository-fonts.png)

<br>
  
`Home` > `Downloads` 디렉토리에서 다운로드한 파일을 열어 Install 클릭 후 Installed로 바뀐 것을 확인한다.

<br>

![Downloads](/assets/img/2025-03-21/downloads.png)|![Before installing font](/assets/img/2025-03-21/install-font-before.png)|![After installing font](/assets/img/2025-03-21/install-font-after.png)

<br>

다운로드한 폰트를 적용하기 위해 터미널에서 오른쪽 마우스 클릭 > Preferences > Profiles에 들어가 보면 'Use the system fixed width font'가 체크되어 있고 폰트는 'Ubuntu mono regular'가 선택되어 있다. 체크를 해제하고 다운로드한 폰트로 변경한다.

<br>

![Terminator preferences](/assets/img/2025-03-21/terminator-preferences.png)|![Yes Font File](/assets/img/2025-03-21/yes-font-file.png)|![Terminator preferences changed](/assets/img/2025-03-21/terminator-preferences-done.png)

<br>

<div class="box-info">
<div class="title"> Choose A Terminal Font에서 다운로드한 폰트가 검색되지 않는다면? </div>
열려 있는 모든 터미널을 닫고 새 터미널에서 오른쪽 마우스 클릭 > Preferences > Profiles에 들어가면 된다.
</div>

<br>

### 3-1. Powerlevel10k 설치

<br>

폰트 적용이 끝났다면 [Powerlevel10k 깃허브 리포지토리 > Installation > Oh My Zsh](https://github.com/romkatv/powerlevel10k?tab=readme-ov-file#oh-my-zsh)에서 Powerlevel10k 설치와 관련된 명령어를 복사/붙여넣기 하여 리포지토리를 클론한다.

<br>

```bash
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git "${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k"
```

<br>

<div class="box-info">
<div class="title"> Git 설치 명령어 </div>
<code> sudo apt install git </code>
</div>

<br>

![Install powerlevel10k](/assets/img/2025-03-21/install-powerlevel10k.png)|![Install powerlevel10k done](/assets/img/2025-03-21/install-powerlevel10k-done.png)

<br>

`Home` 디렉토리에서 숨김 파일을 해제하고(`Ctrl + H`) Zsh의 기본 설정과 관련된 파일인 `.zshrc` 파일을 연다. (Bash의 기본 설정과 관련된 파일은 `.bashrc`이다.)

<br>

![Home directory](/assets/img/2025-03-21/home-directory.png)|![Home directory hidden](/assets/img/2025-03-21/home-directory-hidden.png)

<br>

<div class="box-info">
<div class="title"> .zshrc 파일을 편하게 읽으려면? </div>
.zshrc 파일 창 하단의 Plain Text를 sh로 바꾼다.
</div>

<br>

![Plain text](/assets/img/2025-03-21/plain-text.png)|![Plain text setting](/assets/img/2025-03-21/plain-text-setting.png)|![Plain text to sh](/assets/img/2025-03-21/plain-text-to-sh.png)

<br>

`.zshrc` 파일의 11번째 줄을 보면 Zsh 테마(`ZSH_THEME`)가 `robbyrussell`인 것을 확인할 수 있다. `robbyrussell`을 `powerlevel10k/powerlevel10k`로 수정하고 저장한다.

<br>

![robbyrussell](/assets/img/2025-03-21/robbyrussell.png)|![robbyrussell-to-powerlevel10k](/assets/img/2025-03-21/robbyrussell-powerlevel10k.png)

<br>

새 터미널을 열면 Powelevel10k 설정과 관련된 안내 문구가 나온다. 이전 단계에서 폰트가 정상적으로 설치되었는지 확인하기 위해 다이아몬드가 보이는지, 자물쇠가 보이는지 등을 묻는다. 만약 묻는 것처럼 관련 기호가 보이지 않는다면 폰트 설치를 다시 확인한다.

<br>

![Check font 1](/assets/img/2025-03-21/check-font-1.png)|![Check font 2](/assets/img/2025-03-21/check-font-2.png)| ![Check font 3](/assets/img/2025-03-21/check-font-3.png)| ![Check font 4](/assets/img/2025-03-21/check-font-4.png)|![Check font 5](/assets/img/2025-03-21/check-font-5.png)

<br>

여기서부터는 취향껏 터미널을 설정한다.

<br>

![Rainbow](/assets/img/2025-03-21/rainbow.png)|![Rainbow>Unicode](/assets/img/2025-03-21/rainbow-unicode.png)

<br>

<div class="box-info">
<div class="title"> Powerlevel10k 테마를 처음부터 다시 설정하고 싶다면? </div>
<code> p10k configure </code>
</div>

<br>

![Resetting](/assets/img/2025-03-21/resetting-powerlevel10k.png)|![Before resetting](/assets/img/2025-03-21/resetting-powerlevel10k-before.png)|![After resetting](/assets/img/2025-03-21/resetting-powerlevel10k-after.png)

<br>

## 4. Zsh Plugins

<br>

Zsh 사용과 관련된 유용한 플러그인이 있다. 지금은 명령어를 입력하면 모든 명령어가 단순하게 흰색으로 표시된다.

<br>

![Before setting](/assets/img/2025-03-21/sudo-apt-install-no-color.png)

<br>

### 4-1. zsh-syntax-highlighting

<br>

존재하지 않거나 틀린 명령어를 입력하면 글자가 빨간색으로 표시된다. 존재하고 올바른 명령어를 입력하면 글자가 초록색으로 표시된다. 설치 후 바로 적용되는 것은 아니며 `.zshrc` 파일에서 설정해 주어야 한다.

<br>

```bash
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
```

<br>

![Install highlighting](/assets/img/2025-03-21/install-highlighting.png)|![After highlighting](/assets/img/2025-03-21/install-highlighting-before-setting.png)

<br>

### 4-2. zsh-autosuggestions

<br>

이전에 실행했던 명령어들이 자동완성으로 입력된다.

<br>

```bash
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
```

<br>

zsh-syntax-highlighting과 마찬가지로 설치 후 적용하지 않았기 때문에 명령어를 입력해도 아직은 글자가 흰색으로 표시된다.

<br>

![Install highlighting](/assets/img/2025-03-21/install-autosuggestions.png)|![After highlighting](/assets/img/2025-03-21/install-autosuggestions-before-setting.png)

<br>

설치한 플러그인을 적용하기 위해 `.zshrc` 파일의 80번째 줄의 `plugins`에 `zsh-syntax-highlighting`을 추가하고 저장한다.

<br>

![Before setting .zshrc](/assets/img/2025-03-21/setting-zshrc-plugin-before.png)|![After setting .zshrc](/assets/img/2025-03-21/setting-zshrc-plugin-after.png)

<br>

새 터미널을 열어 명령어를 입력해 보면 플러그인이 적용된 것을 확인할 수 있다. 설치한 zsh-autosuggestions은 `.zshrc` 파일에 추가하지 않았기 때문에 해당 기능은 적용되지 않는 것을 확인할 수 있다.

<br>

![After setting](/assets/img/2025-03-21/sudo-apt-install-color.png)

<br>

---

&nbsp;