# widowx_turret

This ROS package is intended to work with the [WidowX](https://www.roscomponents.com/en/pan-tilts/132-widowx-mx-28-robot-turret-kit.html#/assembled-no) and [ScorpionX](https://www.roscomponents.com/en/pan-tilts/134-scorpionx-mx-64-robot-turret-kit.html#/assembled-no) turrets

* widowx_turret_controller : Controller based on arbotix_python driver to control de turret
* widowx_turret_description : Description of the turret

## Installation and configuration

### Setting up the Arbotix-M board

In order to work with ROS it is necessary to upload the firmware into the Arbotix-M board.

* Download Arduino ide from https://downloads.arduino.cc/arduino-1.0.6-linux64.tgz
  * wget https://downloads.arduino.cc/arduino-1.0.6-linux64.tgz
  
* Extract it into a folder.
* Download the firmware archives from https://github.com/trossenrobotics/arbotix/archive/master.zip
  * wget https://github.com/trossenrobotics/arbotix/archive/master.zip
* Extract it into a folder like ~/Documents/Arduino
* Run arduino from the folder you extracted it previously
  * cd ~/Downloads/arduino-1.0.6
  * ./arduino
* Once Arduino IDE is running, change the Sketchbook folder location to /Documents/Arduino/arbotixmaster or the one you extracted it previously.
  * File->Preferences->Sketchbook Location
  * Tools->Board->Arbotix
  * Tools->Serial Port->/dev/ttyUSBX
  * File->Sketchbook->Arbotix Sketches ->ros
  * Verify + Upload
* The Arbotix is ready to work with ROS!!

### Downloading the package

clone the repo into your workspace and compile it.
```
git clone https://github.com/RobotnikAutomation/widowx_turret.git
```
### Creating the udev rule for the device

In the widowx_turret_controller/config folder there's the file 60-widowx_turret.rules. You have to copy it into the /etc/udev/rules.d folder.

```
sudo cp 60-widowx_turret.rules /etc/udev/rules.d
```

You have to set the attribute ATTRS{serial} with the current serial number of the ftdi device

```
udevadm info -a -n /dev/ttyUSB0 | grep serial 
```
Once modified you have to reload and restart the udev daemon

```
sudo service udev reload
sudo service udev restart
sudo udevadm trigger
```

### Running the controller

```
roslaunch widowx_turret_controller widowx_turret_controller.launch
```

### Commanding the controller 

```
rostopic pub /servo_pan_joincommand std_msgs/Float64 "data: 0.5" 
rostopic pub /servo_tilt_joincommand std_msgs/Float64 "data: 0.5" 
```

### Visualizing the state

Load the description and run the state publisher

```
roslaunch widowx_turret_description load_description.launch
```

Open the RVIZ tool and add the plugins you need to visualize the arm

```
rosrun rviz rviz
```

