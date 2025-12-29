TF is the transform library that is used to keep track of coordinate frames. it maintains the relationship between coordinate frames in a tree structure *buffered in time* and lets user transform points, vectors and more. between any two coordinate frames at any time

RViz does a good job of giving visualisation of relative positions of the parts of the robot.
All parts are connected to the base_link which acts as the root. Think of it like a root node in a tree, if it's moved all its connected nodes will also move. This applies to any node in the TF tree.

to visualise the tree, `ros-<distro>-tf2-tools` can be used
to look at all the current tf being published, the command is
`ros2 run tf2_tools view_frames -o <any_name>`
now this will create a pdf which will contain all the current tf which is being published 

Everything that comes to movement and positioning is done by the TF library in ROS

URDF (Unified Robot Description Format)
Its a file that describes the parts of the robot and how they are connected to each other, it follows the xml format
basic format
```
<robot name="">
	<material name="">
		<color rgba="0 0 0 0" />
	</material>
	
	<link name="base_link">
		<visual>
			<geometry>
				<shape size="0 0 0" />
			</geometry>
			<origin xyz="0 0 0.1" rpy="0 0 0" />
			<material name="blue" />
		</visual>
	</link>

	<joint name="" type="">
		<parent link="base_link" />
		<child link="second_link" />
		<origin xyz="0 0 0" rpy="0 0 0" />
	</joint>
</robot>
```

More syntax and formats is in the [urdf xml documentation](https://wiki.ros.org/urdf/XML/link)

What happens in the background is
`robot_state_publisher` which is a node, is publishing the tf information from the given urdf file. (urdf file is given by us, this node is pre-built in the ros2 appication which is required to publish urdf information to tf)

if we list the parameters of the node `robot_state_publisher` from [[Nodes]] we can see that `robot_description` is under it. If we look inside the file, its actually the urdf content

What does robot_state_publisher do?
it takes urdf (robot geometry) and the joint states (angles/position of the movable joints) and publishes it to the topics
package: `joint_state_publisher` node: `robot_state_publisher`
the `joint_state_publisher_gui` gives us the ui for the slider for movable objects in the tf

Writing the launch file is a huge task by itself. The way files are managed is different for both xml and python for the launch file.