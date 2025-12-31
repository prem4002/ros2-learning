`` I use a docker container to code ros in arch linux
Official [ROS Docker Wiki](https://hub.docker.com/_/ros/)
OSRF (open source robotics foundation) has created some pre-made ros images.
#### Creating a container
---
Official ROS 2 Docker Images
ROS 2 images are hosted on Docker Hub under `osrf/ros`

to pull the necessary ros image:
`sudo docker pull osrf/ros:jazzy-desktop-full`
Tags are like
- `osrf/ros:jazzy-desktop` → full desktop install (RViz, tools, etc.)
- `osrf/ros:jazzy-ros-core` → minimal, just core communication libraries
- `osrf/ros:jazzy-perception` → includes perception libraries

### **ROS Needs Access to Your Host**
Unlike generic apps, ROS nodes interact with:
- **Networking** (topics/services across containers or machines)
- **X11 GUI / Wayland** (RViz, Gazebo, rqt, etc.)
- **Hardware** (USB, serial devices, GPU, cameras, LiDAR, etc.)

So running containers will often need extra flags:
- `--net=host` (share host networking, easier for ROS discovery)
- `-e DISPLAY=$DISPLAY -v /tmp/.X11-unix:/tmp/.X11-unix` (for GUI apps)
- `--device=/dev/ttyUSB0` (for serial devices like microcontrollers)
- `--gpus all` (if you want GPU acceleration inside the container).

### **Persistent Workspace**
If you’re developing, don’t just build inside the container. Instead:
- Mount your ROS workspace from host → container:
- -v ~/ros2_ws:/ros2_ws
That way source code stays on your host, and can be re-used across containers.

To start a container
Simple way:
`docker run -it --rm osrf/ros:jazzy-desktop`
-rm removes the container after closing it

`sudo docker run -it osrf/ros:jazzy-desktop-full`

## Persistent Containers
- Start a container once, and instead of `--rm`, you **keep it around**:
	`docker run -it --name ros2_jazzy_dev osrf/ros:jazzy-desktop`
- resume later:
	`docker start -ai ros2_jazzy_dev`

For ros, since we need display access for apps like rviz, we start the container like so. It also contains stuff to mount/bind local folder to the container
```
xhost +local:root

docker run -it -rm \
  --privileged \
  -e WAYLAND_DISPLAY=$WAYLAND_DISPLAY \
  -e XDG_RUNTIME_DIR=$XDG_RUNTIME_DIR \
  -v $XDG_RUNTIME_DIR/$WAYLAND_DISPLAY:/tmp/$WAYLAND_DISPLAY \
  \
  -e DISPLAY=$DISPLAY \
  -v /tmp/.X11-unix:/tmp/.X11-unix:rw \
  \
  -v /run/dbus/system_bus_socket:/run/dbus/system_bus_socket \
  -e "QT_X11_NO_MITSHM=1" \
  \
  --gpus all \
  --device /dev/dri \
  \
  -v "$HOME/.Xauthority:/root/.Xauthority:rw" \
  --device /dev/video0 \
  --net=host \
  \
  -v "<host_path>:<container_path>" \
  \
  --name <name> \
  -h <host_name>
  osrf/ros:<distro>-desktop
```

- `--device /dev/dri` → passes your GPU’s Direct Rendering Interface into the container (needed for OpenGL).
- Works for Intel and AMD.
- For NVIDIA, you’d instead use `--gpus all` with the NVIDIA container toolkit.

for permissions
- `--user $(id -u):$(id -g)` → runs container as your UID/GID
- Files created inside container are owned by **your host user**
- Lock icon disappears forever
- You can build, create URDFs, etc., without touching permissions again

to use nvidia-GPU for docker, you need to install AUR nvidia-container-toolkit, and then add it to config
`sudo nvidia-ctk runtime configure --runtime=docker`

## to open another terminal in the same container
`docker exec -it <container_name> bash`

for intel GPU
```
docker run -it --rm \
  --net=host \
  -e DISPLAY \
  -e QT_QPA_PLATFORM=xcb \
  -v /tmp/.X11-unix:/tmp/.X11-unix \
  --device /dev/dri \
  -v ~/Workspaces:/workspaces \
  jazzy-moveit
```

for NVIDIA-GPU
```
docker run -it --rm \
  --net=host \
  --gpus all \
  -e DISPLAY \
  -e QT_QPA_PLATFORM=xcb \
  -v /tmp/.X11-unix:/tmp/.X11-unix \
  -v ~/Workspaces:/workspaces \
  ros2-jazzy-dev
```

