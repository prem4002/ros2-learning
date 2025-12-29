This folder is complete about the ros2 package
For this we mainly use two launch files after setup.

main setup:
## Installation
First, clone the repository into a `colcon` workspace
``` bash
cd ~/reach_ws/src
git clone https://github.com/ros-industrial/reach_ros2.git
cd ..
```

Install the dependencies
``` bash
vcs import src < src/reach_ros2/dependencies.repos
rosdep install --from-paths src --ignore-src -r -y
```

Build the repository
```
colcon build --symlink-install
```

#### Usage
After this, to use the tool the following steps to run a reach study with a robot using the ROS1 infrastructure and plugins.
1. Create any files describing your robot system required by the REACH plugins (e.g., URDF, SRDF, kinematics file, joint limits file, etc.)
2. Create a mesh model of the workpiece
    > Note: the origin of this model should align with the kinematic base frame of the robot
3. Create a point cloud of the target points on the workpiece
    - This point cloud can be generated using a command line tool from PCL 1.8:
```
pcl_mesh_sampling <workpiece_mesh>.ply <output_cloud>.pcd -n_samples <number of samples> -leaf_size <leaf_size> -write_normals true
```
4. Create a configuration YAML file defining the parameters of the reach study and the configuration of the interface plugins (see [this demo example](demo/config/reach_study.yaml))
5. Run the setup launch file
```
ros2 launch reach_ros setup.launch robot_description_file:=<path_to_URDF>
```
6. Run the reach study analysis
```
ros2 launch reach_ros start.launch \
robot_description_file:=<path_to_URDF> \
robot_description_semantic_file:=<path_to_SRDF> \
robot_description_kinematics_file:=<path_to_kinematics.yaml> \
robot_description_joint_limits_file:=<path_to_joint_limits.yaml> \
config_file:=<config_file.yaml> \
config_name:=<arbitrary_config> \
results_dir:=<arbitrary_results_directory>
```


