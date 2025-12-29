Command to launch the moveit setup assistant `ros2 launch moveit_setup_assistant setup_assitant.launch.py` 

If the files are not installed, get them from [here](https://moveit.ai/install-moveit2/binary/)
#### Config files
-  `initial_positions.yaml`
	contains the starting point of the robot for all the joint positions present
-  `joint_limits.yaml`
	limits should be set for the speed and acceleration of the joint's movement, this is for 
-  `kinematics.yaml`
	It performs the movement of the robot using the other files as inpuT
	Uses the kinematics plugin/library
-  `moveit_controllers.yaml`
	This is the program which controls the movement (gives instructions to the robot)
-  `moveit.rviz`
	Required for rviz visualisation. Its the config file to tell rviz to open the application with a preset.
-  `robot.ros2_control.xacro`
	used to insert ros2_control plugin and transform definitions into robot URDF. needed for hardware. mock components will be used for visualisation/simulation
-  `robot.srdf`
	this will contain the example points that needs to be used for planning and contains collision points
-  `robot.urdf.xacro`
	This uses the main urdf file for the robot, it also adds the `robot.ros2_control.xacro` to bring them together.  So the urdf is ultimately the mixture of og urdf and ros2_control tags.
-  `ros2_controller`
	Tells where the movement should be published to. Contains the broadcaster (to a topic)

#### Launch files
-  `demo.launch.py`
	base launch file to test if move it is working or not
	