There are two main launch files which play here
one is the `setup.launch.py` and the other one is `start.launch.py`.

`setup.launch.py` is just to get the urdf of the bot about to be tested
This uses 4 different arguments out of which only one is necessary - the first one where we input the urdf.
```
parameters = [
{'name': 'robot_description_file', 'description': 'Path to the URDF/xacro file', 'default': PathJoinSubstitution([FindPackageShare('reach_ros'), 'demo', 'model', 'reach_study.xacro'])},
{'name': 'robot_description_semantic_file', 'description': 'Path to the SRDF file', 'default': PathJoinSubstitution([FindPackageShare('reach_ros'), 'demo', 'model', 'reach_study.srdf'])},
{'name': 'use_rviz', 'description': 'Flag indicating whether Rviz should be launchd', 'default': 'True'},
{'name': 'rviz_config', 'description': 'Reach study Rviz configuration', 'default': PathJoinSubstitution([FindPackageShare('reach_ros'), 'launch', 'reach_study_config.rviz'])},
]
```