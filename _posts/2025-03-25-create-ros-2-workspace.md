---
title: "[ROS 2] Workspace 생성"
date: 2025-03-25 11:11:00 +0900
categories: ROS
---

&nbsp;  

ROS 2 Humble 설치가 완료되었다면 터미널에서 ROS 2와 관련된 명령어를 사용할 수 있도록 설정하는 과정이 필요하다. 현재는 터미널에서 `ros2` 명령어를 실행하면 명령어를 찾을 수 없다는 에러 메시지가 표시된다.

<br>

![Command not found](/assets/img/2025-03-25/command-not-found.png)

<br>

---

## 1. Set Run Commands

<br>

현재 기본으로 사용하고 있는 쉘의 설정 파일(.bashrc 또는 .zshrc)에 아래 코드를 추가해야 한다.

<br>

### 1-1. Add to ~/.zshrc for zsh

```bash
source /opt/ros/humble/setup.zsh
source ~/ros2_ws/install/local_setup.zsh

source /usr/share/colcon_argcomplete/hook/colcon-argcomplete.zsh
source /usr/share/vcstool-completion/vcs.zsh
source /usr/share/colcon_cd/function/colcon_cd.sh
export _colcon_cd_root=~/ros2_ws

# argcomplete for ros2 & colcon
eval "$(register-python-argcomplete3 ros2)"
eval "$(register-python-argcomplete3 colcon)"

# export ROS_NAMESPACE=robot1

export RMW_IMPLEMENTATION=rmw_fastrtps_cpp
# export RMW_IMPLEMENTATION=rmw_connext_cpp
# export RMW_IMPLEMENTATION=rmw_cyclonedds_cpp
# export RMW_IMPLEMENTATION=rmw_gurumdds_cpp

# export RCUTILS_CONSOLE_OUTPUT_FORMAT='[{severity} {time}] [{name}]: {message} ({function_name}() at {file_name}:{line_number})'
export RCUTILS_CONSOLE_OUTPUT_FORMAT='[{severity}]: {message}'
export RCUTILS_COLORIZED_OUTPUT=1
export RCUTILS_LOGGING_USE_STDOUT=0
export RCUTILS_LOGGING_BUFFERED_STREAM=1

alias cw='cd ~/ros2_ws'
alias cs='cd ~/ros2_ws/src'
alias ccd='colcon_cd'

alias cb='cd ~/ros2_ws && colcon build --symlink-install'
alias cbs='colcon build --symlink-install'
alias cbp='colcon build --symlink-install --packages-select'
alias cbu='colcon build --symlink-install --packages-up-to'
alias ct='colcon test'
alias ctp='colcon test --packages-select'
alias ctr='colcon test-result'

alias tl='ros2 topic list'
alias te='ros2 topic echo'
alias nl='ros2 node list'

alias killgazebo='killall -9 gazebo & killall -9 gzserver & killall -9 gzclient'

alias af='ament_flake8'
alias ac='ament_cpplint'

alias testpub='ros2 run demo_nodes_cpp talker'
alias testsub='ros2 run demo_nodes_cpp listener'
alias testpubimg='ros2 run image_tools cam2image'
alias testsubimg='ros2 run image_tools showimage'

alias di='rosdep install --from-paths src -y --ignore-src --os=ubuntu:jammy'

# export ROS_DOMAIN_ID=7
```

<br>

### 1-2. Add to ~/.bashrc for bash

```bash
source /opt/ros/humble/setup.bash
source ~/ros2_ws/install/local_setup.bash
# source ~/uros_ws/install/local_setup.bash

source /usr/share/colcon_argcomplete/hook/colcon-argcomplete.bash
source /usr/share/vcstool-completion/vcs.bash
source /usr/share/colcon_cd/function/colcon_cd.sh
export _colcon_cd_root=~/ros2_ws

# argcomplete for ros2 & colcon
eval "$(register-python-argcomplete3 ros2)"
eval "$(register-python-argcomplete3 colcon)"

# export ROS_NAMESPACE=robot1

export RMW_IMPLEMENTATION=rmw_fastrtps_cpp
# export RMW_IMPLEMENTATION=rmw_connext_cpp
# export RMW_IMPLEMENTATION=rmw_cyclonedds_cpp
# export RMW_IMPLEMENTATION=rmw_gurumdds_cpp

# export RCUTILS_CONSOLE_OUTPUT_FORMAT='[{severity} {time}] [{name}]: {message} ({function_name}() at {file_name}:{line_number})'
export RCUTILS_CONSOLE_OUTPUT_FORMAT='[{severity}]: {message}'
export RCUTILS_COLORIZED_OUTPUT=1
export RCUTILS_LOGGING_USE_STDOUT=0
export RCUTILS_LOGGING_BUFFERED_STREAM=1

alias cw='cd ~/ros2_ws'
alias cs='cd ~/ros2_ws/src'
alias ccd='colcon_cd'

alias cb='cd ~/ros2_ws && colcon build --symlink-install'
alias cbs='colcon build --symlink-install'
alias cbp='colcon build --symlink-install --packages-select'
alias cbu='colcon build --symlink-install --packages-up-to'
alias ct='colcon test'
alias ctp='colcon test --packages-select'
alias ctr='colcon test-result'

alias tl='ros2 topic list'
alias te='ros2 topic echo'
alias nl='ros2 node list'

alias killgazebo='killall -9 gazebo & killall -9 gzserver & killall -9 gzclient'

alias af='ament_flake8'
alias ac='ament_cpplint'

alias testpub='ros2 run demo_nodes_cpp talker'
alias testsub='ros2 run demo_nodes_cpp listener'
alias testpubimg='ros2 run image_tools cam2image'
alias testsubimg='ros2 run image_tools showimage'

alias di='rosdep install --from-paths src -y --ignore-src --os=ubuntu:jammy'

# export ROS_DOMAIN_ID=7
```

<br>

나는 기본 쉘로 Zsh을 사용하기 때문에 .zshrc 파일의 맨 하단에 주석을 추가하고 [관련 코드](#1-1-add-to-zshrc-for-zsh)를 붙여 넣었다. 첨부한 이미지는 코드 추가 전/후의 .zshrc 파일이다.

<br>

![zshrc before](/assets/img/2025-03-25/zshrc-before.png)|![zshrc after](/assets/img/2025-03-25/zshrc-after.png)

<br>

새 터미널을 열어 설정 파일이 잘 수정되었는지 확인한다. 터미널을 열면 경고 메시지가 표시되는데 이는 아직 워크스페이스가 구성되지 않았기 때문이다. 워크스페이스가 구성되기 전까지 해당 경고 메시지는 터미널을 새로 열 때마다 보이며 워크스페이스가 구성되면 사라진다.

<br>

확인을 위해 처음과 같이 `ros2` 명령어를 실행한다. 더 이상 명령어를 찾을 수 없다는 에러 메시지는 표시되지 않는다.

<br>

![After add zshrc](/assets/img/2025-03-25/after-add-1.png)|![After add zshrc](/assets/img/2025-03-25/after-add-2.png)

<br>

---

## 2. Example Program

<br>

ROS 2가 정상적으로 동작하는지 확인하기 위해 예제 프로그램을 실행한다. 두 터미널을 열어 한 터미널에서는 토픽을 구독하고(subscribe) 다른 터미널에서는 토픽을 발행한다(publish).

<br>

> **Tips!** Terminator 좌우 분할 단축키: Ctrl + Shift + E

<br>

```bash
# One terminal
testsub

# Another terminal
testpub
```

<br>

`testsub` 명령어를 실행한 뒤 `testpub` 명령어를 실행하면, 즉시 두 터미널에서 토픽의 발행과 구독이 정상적으로 이루어지는 것을 확인할 수 있다.

<br>

![Example program](/assets/img/2025-03-25/example-program.png)

<br>

---

## 3. Create Workspace

<br>

새 터미널을 열어 ros2_ws 디렉토리와 그 안에 src 디렉토리를 만든다.

<br>

```bash
mkdir -p ros2_ws/src
```

<br>

![Make workspace](/assets/img/2025-03-25/mkdir-ros2_ws.png)

<br>

ros2_ws 디렉토리로 이동한 뒤 ROS 2 기본 빌드 툴인 `colcon`을 사용해 워크스페이스를 빌드한다.

<br>

```bash
cd ros2_ws
colcon build --symlink-install
```

<br>

![colcon build](/assets/img/2025-03-25/colcon-build.png)

<br>

ros2_ws 디렉토리를 확인해 보면, src 디렉토리 외에 build, install, log 디렉토리가 추가로 생성된 것을 확인할 수 있다. 여기까지 문제없이 진행되었다면 워크스페이스가 정상적으로 구성된 것이다.

<br>

![colcon build ls](/assets/img/2025-03-25/colcon-build-ls.png)

<br>

이제부터는 새로 터미널을 열 때 경고 메시지가 보이지 않는다. 워크스페이스 빌드 후 install 디렉토리가 생성되었고 그 안에 있는 local_setup.zsh 파일(또는 local_setup.bash 파일)을 정상적으로 source 할 수 있기 때문이다.

<br>

```bash
# The second line of 'Add to ~/.zshrc'
source ~/ros2_ws/install/local_setup.zsh

# The second line of 'Add to ~/.bashrc'
source ~/ros2_ws/install/local_setup.bash
```

<br>

![ros2_ws > install](/assets/img/2025-03-25/ros2_ws-install.png)

&nbsp;

---