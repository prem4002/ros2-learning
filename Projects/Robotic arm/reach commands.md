#### dummy
```
ros2 launch reach_ros setup.launch.py robot_description_file:=/workspaces/arm_ws/src/dummy_description/urdf/dummy.urdf.xacro


ros2 launch reach_ros start.launch.py \
robot_description_file:=/workspaces/arm_ws/src/dummy_description/urdf/dummy.urdf.xacro \
robot_description_semantic_file:=/workspaces/arm_ws/src/dummy_moveit_config/config/dummy.srdf \
robot_description_kinematics_file:=/workspaces/arm_ws/src/dummy_moveit_config/config/kinematics.yaml \
robot_description_joint_limits_file:=/workspaces/arm_ws/src/dummy_moveit_config/config/joint_limits.yaml \
config_file:=/workspaces/arm_ws/config.yaml \
config_name:=config \
results_dir:=results
```

#### mogi
```
ros2 launch reach_ros setup.launch.py robot_description_file:=/home/prem/Workspaces/pfizer_arm_ws/src/pfizer/urdf/mogi_arm.xacro

ros2 launch reach_ros start.launch.py \
robot_description_file:=/home/prem/Workspaces/pfizer_arm_ws/src/pfizer/urdf/mogi_arm.xacro \
robot_description_semantic_file:=/home/prem/Downloads/ws/src/pfizer_moveit_config/config/mogi_arm.srdf \
robot_description_kinematics_file:=/home/prem/Downloads/ws/src/pfizer_moveit_config/config/kinematics.yaml \
robot_description_joint_limits_file:=/home/prem/Downloads/ws/src/pfizer_moveit_config/config/joint_limits.yaml \
config_file:=/home/prem/Workspaces/pfizer_arm_ws/config.yaml \
config_name:=new \
results_dir:=results
```

ur3
```
ros2 launch reach_ros setup.launch.py robot_description_file:=/workspaces/ur_ws/src/ur_description/urdf/robot.urdf

ros2 launch reach_ros start.launch.py \
robot_description_file:=/workspaces/ur_ws/src/ur_description/urdf/robot.urdf \
robot_description_semantic_file:=/workspaces/ur_ws/src/ur_description/config/ur3.srdf \
robot_description_kinematics_file:=/workspaces/ur_ws/src/ur_driver/ur_moveit_config/config/kinematics.yaml \
robot_description_joint_limits_file:=/workspaces/ur_ws/src/ur_driver/ur_moveit_config/config/joint_limits.yaml \
config_file:=/workspaces/ur_ws/src/config.yaml \
config_name:=ex \
results_dir:=results
```


robot should enter the container at an angle


arm_one
```
ros2 launch reach_ros setup.launch.py robot_description_file:=/home/prem/Workspaces/arm_ws/src/arm_one_description/urdf/arm_one.urdf.xacro

ros2 launch reach_ros start.launch.py \
robot_description_file:=/home/prem/Workspaces/arm_ws/src/arm_one_description/urdf/arm_one.urdf.xacro \
robot_description_semantic_file:=/home/prem/Workspaces/arm_ws/src/arm_one_moveit_config/config/arm_one.srdf \
robot_description_kinematics_file:=/home/prem/Workspaces/arm_ws/src/arm_one_moveit_config/config/kinematics.yaml \
robot_description_joint_limits_file:=/home/prem/Workspaces/arm_ws/src/arm_one_moveit_config/config/joint_limits.yaml \
config_file:=/home/prem/Workspaces/arm_ws/config.yaml \
config_name:=check_ \
results_dir:=results
```

dummy with prismatic
```
ros2 launch reach_ros setup.launch.py robot_description_file:=/workspaces/arm_ws/src/dummy_description/urdf/dummy.urdf.xacro

ros2 launch reach_ros start.launch.py \
robot_description_file:=/workspaces/arm_ws/src/dummy_description/urdf/dummy.urdf.xacro \
robot_description_semantic_file:=/workspaces/arm_ws/src/prismatic_moveit_config/config/dummy.srdf \
robot_description_kinematics_file:=/workspaces/arm_ws/src/prismatic_moveit_config/config/kinematics.yaml \
robot_description_joint_limits_file:=/workspaces/arm_ws/src/prismatic_moveit_config/config/joint_limits.yaml \
config_file:=/workspaces/arm_ws/config.yaml \
config_name:=result \
results_dir:=results
```


```
git clone https://github.com/moveit/moveit2.git -b main
for repo in moveit2/moveit2.repos $(f="moveit2/moveit2_jazzy.repos"; test -r $f && echo $f); do vcs import < "$repo"; done
rosdep install -r --from-paths . --ignore-src --rosdistro jazzy -y
```


3dof
```
ros2 launch reach_ros setup.launch.py robot_description_file:=/workspaces/arm_ws/src/dummy_description/urdf/dummy.urdf.xacro

ros2 launch reach_ros start.launch.py \
robot_description_file:=/workspaces/arm_ws/src/dummy_description/urdf/dummy.urdf.xacro \
robot_description_semantic_file:=/workspaces/arm_ws/src/3dof_moveit_config/dummy.srdf \
robot_description_kinematics_file:=/workspaces/arm_ws/src/3dof_moveit_config/kinematics.yaml \
robot_description_joint_limits_file:=/workspaces/arm_ws/src/3dof_moveit_config/joint_limits.yaml \
config_file:=/workspaces/arm_ws/config.yaml \
config_name:=config_prismatic_dummy \
results_dir:=results
```