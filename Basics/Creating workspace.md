Always remember to source your ros env or add it to bash
`~/.bashrc`
Every ros workspace also have to be added to the bashrc

if `colcon build` doesn't work then we have to install `ros-dev-tools`
if you want to install just colcon then install `python3-colcon-common-extensions`

use `mkdir -p workspace_name/src` to create a new workspace to start

inside source, we can start creating nodes/packages where our actual codes go so we can start coding
Next up come back inside workspace and do colcon build to initialise,

We can create a package and use it for each fun0

#### Building the workspace
the command to build the whole package is
`colcon build`
for a specfic package we can add the argument
`colcon build --packages-select <package name>`
`--symlink-install` can be added if you want autoupdate code to see outputs (only works with python because cpp has to compile the code every time)
#### To create a new package
#### For python
`ros2 pkg create my_pkg --my_py_pkg --build-type ament_python`

Template code
Python package
```
#!/usr/bin/env python3
import rclpy
from rclpy.node import Node

class NodeName(Node): # MODIFY NAME
	def __init__(self):
	    super().__init__("node_name") # MODIFY NAME

def main(args=None):
	rclpy.init(args=args)
    node = NodeName() # MODIFY NAME
	rclpy.spin(node)
    rclpy.shutdown()

if __name__ == "__main__":
	main()
```

#### For cpp
`ros2 pkg create my_cpp_pkg --build-type ament_cmake`

Template code
cpp package
```
#include "rclcpp/rclcpp.hpp"

class MyCustomNode : public rclcpp::Node // MODIFY NAME
{
public:
	MyCustomNode() : Node("node_name") // MODIFY NAME
	{
	}

private:
};

int main(int argc, char **argv)
{
	rclcpp::init(argc, argv);
	auto node = std::make_shared<MyCustomNode>(); // MODIFY NAME
	rclcpp::spin(node);
	rclcpp::shutdown();
	return 0;
}
```

dependencies can be added using, to provide them from the creation
`--dependencies`
To add more dependencies, we have to access package.xml

There will be a folder with the same name of the parent folder of the package