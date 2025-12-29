Nodes
Each node in ROS should be responsible for a single, modular purpose. Each node can send or recieve data from other nodes via topics, services, actions or parameters.

Nodes are often a complex combination of publishers, subscribers, service servers, service clients, action servers, and action clients, all at the same time.

`ros2 node list` will show you the names of all running nodes. This is especially useful when you want to interact with a node, or when you have a system running many nodes and need to keep track of them.

To run a node, must give the informatin to the `console_scripts` under the setup.py file where the format is:
`executable_name = package_name.file_name:function_name`
the node name inside the file is different than the executable name in setup.py

to run a node, we have to use
 `ros2 run <package_name> <executable_name>`
to know more about currently running nodes
`ros2 node list`

running the same node with a different name
`ros2 run <package_name> <executable_name> --ros-args -r __node:=<new_node_name>`

#### rqt_graph
command `rqt_graph` can be used to visualise the running nodes and how they're communicating
This is a very important command to understand how the robot is made

to see the parameters that a node is using we can use
`ros2 param list <node>`
and to list the content of the parameters
`ros2 param get <node> <parameter_name>`