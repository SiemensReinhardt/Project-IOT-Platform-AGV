# AGV具体操作

所有常用命令放在`.bash_aliases`中，可按需添加/使用


## `.bashrc`文件设置
```bash
export ROS_MASTER_URI=http://10.20.240.247:11311 # 固定，小车的ip地址
export ROS_HOSTNAME=10.20.240.223 # 连Edge网，根据本地ip修改
export SVGA_VGPU10=0
```


## 远程连接小车
首先保证虚拟机网络连接和小车连接的WiFi一致（**SUP_Edge**），同时保证`.bashrc`中`ROS_HOSTNAME`和虚拟机桥接网络ip地址一致  
使用`ssh`远程连接，加上`-Y`参数，密码为: dongguan
```bash
ssh -Y wheeltec@192.168.0.100
```


## 键盘控制
- 远程连接小车
- 先启动初始化节点
```bash
roslaunch turn_on_wheeltec_robot turn_on_wheeltec_robot.launch
```
- 再启动键盘控制节点
```bash
roslaunch wheeltec_robot_rc keyboard_teleop.launch
```
- 具体控制：
  - 方向向前：i(前轮左转) o(无转向) p(前轮右转) 
  - 方向向后：m(前轮左转) ,(无转向) .(前轮右转)


## 绘制地图
- 远程连接小车
- 启动激光建图
```bash
roslaunch turn_on_wheeltec_robot mapping.launch 
```
- 可以用键盘控制（同上）或者APP遥控
  - APP位置  
  `WHEELTEC ROS机器人资料\大车ROS机器人附送资料_2021.11.04\8.软件与驱动\蓝牙APP（用于测试遥控）`
- 查看建图效果
```
rviz # 在本地终端查看
```


## 使用2d导航功能
- 远程连接小车
- 启动导航节点
```bash
roslaunch turn_on_wheeltec_robot navigation.launch
```
- 在本地终端运行rviz：
```bash
rviz # 在本地终端查看
```
- 使用rviz的`2DNavGoal`功能，手动设置终点坐标位置以及方向


## 备份
备份某个版本的SD卡  
### Windows
可使用Win32DiskImager  
工具可在以下路径找到：
`WHEELTEC ROS机器人资料\大车ROS机器人附送资料_2021.11.04\8.软件与驱动\树莓派镜像烧录软件\WIN32Disk`
- 首先创建空白`.img`文件
- 打开Win32DiskImager
- 插入SD卡，选择对应盘符
- 烧录到对应文件：点击read
- 还原镜像到SD卡：点击write

### Linux (Ubuntu)
#### 将SD卡备份为`.img`镜像
- 将SD卡mount，在ubuntu系统里用`df -h`查看存储卡设备号（如：`/dev/sdx`）
- 使用`backup_system.sh`脚本，将`TODO`中分区情况按上一步结果修改
- 运行脚本

#### 还原备份镜像到SD卡
```bash
sudo dd if=backup.img of=/dev/sdx status=progress bs=1MiB
```
- `of`参数为存储卡设备号  

参考：[树莓派系统镜像一键备份脚本, 最小化镜像保存](https://neucrack.com/p/107)