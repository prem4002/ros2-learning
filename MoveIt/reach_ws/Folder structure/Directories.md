```
reach_ws
├── boost_plugin_loader
├── reach
│   ├── include
│   │   └── reach
│   │   ├── interfaces
│   │   ├── plugins
│   │   ├── plugin_utils.h
│   │   ├── reach_study_comparison.h
│   │   ├── reach_study.h
│   │   ├── reach_visualizer .h
│   │   ├── types.h
│   │   └── utils.h
├── reach_ros2
│   └── src
│       ├── display
│       │   └── ros_display.cpp
│       ├── evaluation
│       │   ├── distance_penalty_moveit.cpp
│       │   ├── joint_penalty_moveit.cpp
│       │   └── manipulability_moveit.cpp
│       ├── ik
│       │   └── moveit_ik_solver.cpp
│       ├── plugins.cpp
│       ├── python
│       │   ├── CMakeLists.txt
│       │   └── python_bindings.cpp
│       ├── reach_study_node.cpp
│       ├── target_pose_generator
│       │   └── transformed_point_cloud_target_pose_generator.cpp
│       └── utils.cpp
└── ros_industrial_cmake_boilerplate
```

reach_study.h
This is the entire lifecycle of the file
    load (optional) → reuse past data
    run → do reachability once
    optimize → re\fine / analyze connectivity
    save → persist results

First Iteration - Run phase
`ReachStudy::run()`
is the one which does the binary reachability test, whether the bot can reach the point from initial position
For each pose
- Try IK once
- using params.seed_state
- if IK succeeds -> reached = true, if failed reached = false and pose is marked unreachable

for each point IK solution is found using 
	`evaluateIK(tgt_frame, params_.seed_state, ik_solver_, evaluator_)`
This return the solution along with the score

Next Iterations - Optimization phase
Initial run -> from home position (initial pos), after this the optimization phase starts where the robot is found wether it can reach nearby points or not. These nearby points are found using
```
reachNeighborsDirect(*active_result, msg,
                     ik_solver_, evaluator_,
                     params_.radius, search_tree_);
```
It takes the hyperparameter `params_.radius`, tries for IK neighbours.
Seed is the current pose's joint solution.
For a **reachable pose**:

- Find nearby poses within `params_.radius`
- Try IK for neighbors
- Seed = current pose’s joint solution
- If neighbor is better → replace it

```
reachNeighborsRecursive(active_result, *it,
                        ik_solver_, evaluator_,
                        params_.radius, result,
                        search_tree_);
```
This is the one which runs recursively
Starting from one reachable pose
- Find neighbors within radius
- Solve IK using current solution as seed
-  If reachable → recurse
-  Stop when no new neighbors can be reached

This builds a **connected component in joint space**.

```
neighbor_count += result.reached_pts.size() - 1;
total_joint_distance += result.joint_distance;
```
- this distance calulates how far the robot can move without branch switching
A collision free - path is not guarenteed, this is not planning this code only analyses how many points are reachable.

an IK branch is a discrete event
- usually what happens when traveling to nearby points is this:
	- Point A → solution `{q₁}`
	- Nearby point B → solution `{q₁ + small d}`
	- but when point C is nearby by unreachable from `{q1 + q2 + small d}`, it has to reposition to start seed and go there, this is called a transition zone/IK branch switch and the pose for C is marked unreachable from B

Thats why during the initial run `evaluateIK(pose, initial_seed_state, ...)` and then during the optimization phaze connectivity is calculated using `evaluateIK(neighbor_pose, current_solution_as_seed, ...)`

Reach avoids branch switching because it causes:
- branch switching causes violent joint motion
- planners try to avoid it
- controllers hate itm IK solvers try to avoid it

This branch switch will be done by path planners during movement