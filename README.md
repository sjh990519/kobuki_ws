# kobuki_ws

### OS  : Ubuntu 20.04 LTS, Raspbian buster
### Ros : Noetic Ninjemys
### Robot : Kobuki (yujinrobot), Raspberry Pi 4 (Model B)
### Sensor : 360 Laser Distance Sensor LDS-01 (LIDAR) - ROBOTIS
###&nbsp;&nbsp;&nbsp;&nbsp; Kinect Sensor
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












