noetic && kobuki
================

<div>
 <img src = "https://user-images.githubusercontent.com/94280596/183295070-e0a2fa99-c87f-473c-95df-bd71389167af.png" width="60%" align='left'>
 <img src = "https://user-images.githubusercontent.com/94280596/183295113-872722db-8f0b-473d-8a0e-dcfab79536d2.png" height="60%" align='left'>
</div>   





<br><br><br>
# kobuki_ws

### OS  : Ubuntu 20.04 LTS, Raspbian buster
### Ros : Noetic Ninjemys
### Robot : Kobuki (yujinrobot), Raspberry Pi 4 (Model B)
### Sensor : 360 Laser Distance Sensor LDS-01 (LIDAR) - ROBOTIS, Kinect Sensor


---
<br><br>

## 1. SLAM을 위한 ROS 패키지

* 모든 패키지를 * 모든 패키지를 Desktop && Rapsberry Pi 에 설치하고 진행하였다.


  * kobuki package는 yujinrobot git에서 clone
  * slam_gmapping package는 git에 noetic 브렌치에 있는 것을 clone

```
$ sudo apt-get install ros-noetic-gmapping
```

<br>

```
$ sudo apt-get install ros-noetic-navigation
```

<br><br>

### 1) hls_lfcd_lds_driver
```
$ git clone -b noetic-devel https://github.com/ROBOTIS-GIT/hls_lfcd_lds_driver
```

<br><br>

### 2) Kinect
```
$ sudo apt-get install ros-noetic-openni-camera ros-noetic-openni-launch
```

---
<br><br>
## 2. Desktop && Raspverry Pi (SSH Connect)
* 조건 : 서로 같은 네트워크를 사용해야한다.
* 필자의 경우 데스트톱을 유선랜 연결이 아닌 WiFi 동글을 데스크탑에 착용하여 공유기의 무선랜을 같이 사용


### 1. Desktop
```
$ sudo apt-get install chrony
```
```
$ sudo ntpdate -q ntp.ubuntu.com
```
(만약 ntpdate not found 일 경우)
```
$ sudo apt-get install nptdate
```

#### 1) ROS 환경 설정 
```
$ gedit ~/.bashrc
```

##### ~/.bashrc 에 추가 
```
source /opt/ros/noetic/setup.bash
source ~/kobuki_ws/devel/setup.bash

#  Set ROS Network
export ROS_HOSTNAME=데스크탑_IP
export ROS_MASTER_URI=http://데스크탑_IP:11311
```

#### 2) ROS 환경 설정 변경 확인
```
$ source ~/.bashrc
```
```
$ export |grep ROS
```

#### 3) SSH install
```
$ sudo apt-get install ssh
```

---
---

### 2. Raspberry Pi

#### 1) SSH install
```
$ sudo apt-get install ssh
```

#### 2) 서로 다른 PC간의 통신에서 ROS Time 오차 줄이기
```
$ sudo apt-get install ssh
```
```
$ sudo apt-get install chrony
```
```
$ sudo ntpdate -q ntp.ubuntu.com
```
(만약 ntpdate not found 일 경우)
```
$ sudo apt-get install nptdate
```

#### 3) ROS 환경 설정
```
$ nano ~/.bashrc
```

##### ~/.bashrc 에 추가 
```
source /opt/ros/noetic/setup.bash
source ~/kobuki_ws/devel/setup.bash

#  Set ROS Network
export ROS_HOSTNAME=라즈베리파이_IP
export ROS_MASTER_URI=http://데스크탑_IP:11311
```

#### 4) ROS 환경 설정 변경 확인
```
$ source ~/.bashrc
```
```
$ export |grep ROS
```










<br><br>
## 3. SLAM 실행

### 1) 소스다운 및 검파일
```
$ cd ~/kobuki_ws/src
```

```
$ git clone https://github.com/oroca/rosbook_kobuki.git
```

```
$ cd ~/kobuki_ws && catkin_make
```

<br><br>
### 2) 거북이 노드 실행
  * [ Desktop ]
```
$ roscore
```
  *  [ Raspberry Pi ]
```
$ roslaunch kobuki_node minimal.launch --screen
```
#### 2-1) roslaunch kobuki_node minimal.launch --screen [ ERROR ]
```
[ERROR] [1358185944.885286960]: Kobuki : malformed sub-payload detected. 
or
[ERROR] [1358185986.942876038]: Kobuki : Timed out while waiting for serial data stream [/mobile_base].
```
1. kobuki_firmware upgrade 필요
2. USB Serial Port latency_timer update 필요

<br><br>
### 3) kobuki_slam 실행

  *  kobuki_slam 패키지는 단순히 런치 파일 하나로만 구성 원래는  LRF의 드라이버인 urg_node 노드, 좌표 변환을 위한 tf 를 활용한 kobuki_tf 노드, 맵 작성을 위해 slam_gmapping 노드를 포함하여 총 3개의 노드가 함께 실행 된다. <br><br>


  *  하지만 이 패키지의 경우 센서를 LDS-01 lidar 센서를 사용하므로  kobuki_slam.launch 파일에 urg_node 를  hls_lfcd_lds_driver로 수정한다.

##### 심볼릭 링크에 센서 권한을 설정하지 않았다면 아래와 같이 센서 권한을 바꿔야 한다.
##### 필자는 라이다의 심볼릭이 ttyLiDAR
*  [ Raspberry Pi ]
```
$ sudo chmod a+rw /dev/ttyLiDAR
```

*  [ Desktop ]
```
$ roslaunch kobuki_slam kobuki_slam.launch
```

<br><br>
### 4) Rviz 실행
* SLAM 도중 결과를 눈으로 확인 할 수 있도록 ROS 의 시각화툴인 Rviz를 구동한다.
* 구동 시 아래와 같이 옵션을 붙여주면 디스플레이 플러그인들이 처음부터 추가되어 매우 편리하다.
```
$ rosrun rviz rviz -d 'rospack find kobuki_slam' /rviz/kobuki_slam.rviz
```

#### 4-1) rosrun rviz rviz -d 'rospack find kobuki_slam' /rviz/kobuki_slam.rviz [ ERROR ]
```
[ WARN] [1632008194.010794315, 950.815000000]: MessageFilter [target=odom ]: Dropped 100.00% of messages so far. 
Please turn the [ros.gmapping.message_notifier] rosconsole logger to DEBUG for more information.
```
1. 위와 같이 Rviz 실행 시 라이다 센서 및 로봇 정보가 오지 않을 때 
```
$ rosrun rqt_logger_level rqt_logger_level
```
2. 위의 명령어 실행 후 넘어오지 않는 정보들을 Info -> Debug 로 변환 해준 후 Rviz를 다시 실행 한다.



<br><br>
### 5) 로봇 조종
* 유저가 직접 로봇을 조정하며 SLAM 작업을 한다.
* 로봇의 속도를 너무 급하게 바꾸거나 너무 빠른 속도로 전/후진 회전 하지 않도록 한다.
* 계측할 환경의 세세하게 로봇이 돌아다니면서 스캔을 해야 정확하게 스캔이 가능하다.

<br>

*  [ Desktop ]
```
$ roslaunch kobuki_keyop keyop.launch
```


<br><br>
### 6) Topic Data 저장
* 로봇을 조종하면서 SLAM 작업을 진행하는데, 이떄 kobuki & kobuki_slam 패키지에서 발행하는 /scan, /tf 토픽을 scan_data 이라는 파일명의 .bag 파일로 저장한다.
* 추후 이 파일을 가지고 맵을 만들 수도 있으며, 맵핑작업시에 실험 당시의 /scan과 /tf 토픽 을 재현할 수 있다.
















