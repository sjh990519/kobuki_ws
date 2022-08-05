# 개인 공부용

## 참고 : https://cafe.naver.com/openrt/6258

<br><br><br>

# kobuki_ws

### OS  : Ubuntu 20.04 LTS, Raspbian buster
### Ros : Noetic Ninjemys
### Robot : Kobuki (yujinrobot), Raspberry Pi 4 (Model B)
### Sensor : 360 Laser Distance Sensor LDS-01 (LIDAR) - ROBOTIS, Kinect Sensor


---
<br><br>



## SLAM을 위한 ROS 패키지

* 모든 패키지를 거북이에 설치하고 진행하도록 하였다. 패키지의 설치, 각 노드, 런치파일의 실행은 모두 거북이 본체와 연결된 랩톱에서 실행했다.



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

## SLAM 실행

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



### 2) 거북이 노드 실행
  * 데스크탑
```
$ roscore
```
  *  라즈베리파이
```
$ roslaunch kobuki_node minimal.launch --screen
```


### 3) kobuki_slam 실행
  *  데스크탑
  *  kobuki_slam 패키지는 단순히 런치 파일 하나로만 구성 원래는  LRF의 드라이버인 urg_node 노드, 좌표 변환을 위한 tf 를 활용한 kobuki_tf 노드, 맵 작성을 위해 slam_gmapping 노드를 포함하여 총 3개의 노드가 함께 실행 된다.

<br>
  *  하지만 이 패키지의 경우 센서가 다르기 때문에 kobuki_slam launch 파일에 urg_node 를  hls_lfcd_lds_driver로 수정한다.















