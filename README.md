# Control Baxter robot with robonomics

Sample of how it works is available on [YouTube.][db1]

## Requirements:

 - ROS Melodic + Gazebo (installation manual [here][db2])  
 - extra packages:
```sh
sudo apt-get install ros-melodic-gazebo-ros-control ros-melodic-effort-controllers ros-melodic-joint-state-controller
```
- IPFS 0.4.22 (download from [here][db3] and install)
- ipfshttpclient:
```sh
pip install ipfshttpclient
```
 - Robonomics node (binary file) (download latest [release][db4] here)
 - IPFS browser extension (not necessary)

## 1. Download simulation and controller packages
Download packages:
```sh
mkdir -p robot_ws/src
cd robot_ws/src
git clone https://github.com/nakata5321/Baxter_simulation_controller.git
mv Baxter_simulation_controller/* ./
rm -rf Baxter_simulation_controller/
cd ..
catkin build
```
Dont forget to add source command:
```sh
echo "source /home/$USER/robot_ws/devel/setup.bash" >> ~/.bashrc
```

## 2. Start simulation
Let's start gazebo world and put our baxter in it:
```sh
roslaunch gazebo_ros empty_world.launch
```
![empty world][im1]

In new terminal:
```sh
rosrun gazebo_ros spawn_model -file `rospack find baxter_description`/urdf/baxter.urdf -urdf -z 1 -model baxter
```
You can put some models in front of our baxter. It will be more intresting.
![baxter][im2]

## 3.Manage accounts in DAPP

Since we are testing, let us create a local robonomics network with robonomics binary file. Go to folder with robonomics file and run:
```sh
./robonomics --dev --rpc-cors all
```
![robonomics][im3]

Don't forget to remove `db` folder after every launch:
```sh
rm -rf /home/$USER/.local/share/robonomics/chains/dev/db
```

Go to [https://parachain.robonomics.network][db5] and switch to local node
![local node][im4]

Go to Accounts and create __Baxter__ and __Employer__ accounts (__Robot__ is not necessary)

__Important!__ Copy each account's key and address (to copy address click on account's icon). Transfer some money (units) to these accounts:

![create account][im5]
![accounts][im6]

Add Baxter's secret key and adress to `configuration.txt` in `robot_ws/src/robot_controller/src/

## 4.Start simulation

In new terminal run:
```sh
ifps init #you only need to do this once
ipfs daemon
```
Open separate terminal and start *controller package*:
```sh
rosrun robot_controller robot_control.py
```

[db1]: <https://youtu.be/AeufQmaNRWk>
[db2]: <http://wiki.ros.org/melodic/Installation>
[db3]: <https://dist.ipfs.io/go-ipfs/v0.4.22/go-ipfs_v0.4.22_linux-386.tar.gz>
[db4]: <https://github.com/airalab/robonomics/releases>
[im1]: <https://github.com/nakata5321/media/blob/master/empty_world.png>
[im2]: <https://github.com/nakata5321/media/blob/master/baxter_simulation.png>
[im3]: <https://github.com/nakata5321/media/blob/master/robonomics.png>
[db5]: <https://parachain.robonomics.network>
[im4]: <https://github.com/nakata5321/media/blob/master/local_node.png>
[im5]: <https://github.com/nakata5321/media/blob/master/create_account.png>
[im6]: <https://github.com/nakata5321/media/blob/master/accounts.png>
