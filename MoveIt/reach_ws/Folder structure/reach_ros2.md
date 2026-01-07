### `src/ik/moveit_ik_solver.cpp`
--- 
basicially it defines if a pose is reachable if moveIt can find an IK solution near a given seed that is collision-free in the presence of the PLY collision mesh.

the ply mesh is introduced in this file
when loading - exactly one file is loaded `MoveItIKSolver::COLLISION_OBJECT_NAME = "reach_object";`
this comes from one mesh file under `config.yaml` - `collision_mesh_filename
`
`scene_->processCollisionObjectMsg(obj)`
any object in the ply file will be considered under collision checking during IK checking. Whatever object is inside will become a solid obstacle

It starts from the constructor
```cpp
MoveItIKSolver::MoveItIKSolver(moveit::core::RobotModelConstPtr model, const std::string& planning_group,
                               double dist_threshold)
  : model_(model), jmg_(model_->getJointModelGroup(planning_group)), distance_threshold_(dist_threshold){}
```
There are two main things that happen here
- joint model group  which loads robot kinematics (joints that moveit will use and IK solver) `jmg_ = model_->getJointModelGroup(planning_group)
this uses the planning group which is defined in the srdf. All the robot links should be in one group
- Create planning scene and publish it to rviz
```cpp
scene_.reset(new planning_scene::PlanningScene(model_));
scene_pub_ = node->create_publisher<PlanningScene>(...)
```
this scene is the one which checks for collision
	publishing this keeps ROS tools in sync
	pushes the scene to rviz

now IK is solved using `solveIK()`
results are stored from creating robot state - its a snapshot of joint values for each IK attempt
this is the one which says if solutions can be done or not

``` cpp
state.setJointGroupPositions(jmg_, seed_subset);
state.update();
```
this applies the seed(current position)
- IK starts near this seed
- prevents branch switching - ?

- Now to check if the solutions are valid with collisions with thte robot itself and with the mesh and
- to calculate how much distance there is to collision with a set threshold
```cpp
bool MoveItIKSolver::isIKSolutionValid(...)
// this is called during the IK solving
state->setJointGroupPositions(jmg, ik_solution);
state->update();
// just like the above code but for ik_solutions instead of seed's neighbours
scene_->isStateColliding(...)
scene_->distanceToCollision(...) < distance_threshold_

// final decision
return (!colliding && !too_close);
```

### `src/evaluation
--- 
the codes in this section is used for evaluation and score assigning
- it takes a joint configuation, computes a scalar score and rank poses
- refine solutions during optimize phase
##### `distance_penalty_moveit.cpp`
it scores reachable poses based on how far the arm is from colliding with the mesh
Again, all parameters for the threshold are from the `config.yaml` file

It measures the distance first between any robot link and any obstacle nearby. Then it normalizes the distance between 0 and 1 to give it a score

```cpp
double dist = scene_->distanceToCollision(state, ...);
clipped_distance = min(|dist / dist_threshold|, 1.0)

// penalty score is controlled
return pow(clipped_distance, exponent);
```
the scores are updated during the optimisation phase if it has found a better solution to a point from nearby seeds
##### `joint_penalty_moveit.`

```cpp
std::map<std::string, double> pose //joint config input
std::tie(joints_min_, joints_max_) = getJointLimits(); //joint limits input
//these are from the urdf, srdf and joint_limits.yaml files
```
It scores reachable poses based on how close the robot's joints are to their limits
this code calculates how comfortable it is to attain a certain posture(joints configuration)



##### `manipulability_moveit.cpp`
