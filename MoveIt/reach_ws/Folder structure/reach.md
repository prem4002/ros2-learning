## `reach_study.cpp`

It's the high level code which performs the entire lifecycle
    load (optional) → reuse past data
    run → do reachability once
    optimize → re\fine / analyze connectivity
    save → persist results
    
---
It uses these codes to calculate:
```cpp
ReachStudy::ReachStudy(IKSolver::ConstPtr ik_solver, Evaluator::ConstPtr evaluator,
                       TargetPoseGenerator::ConstPtr target_generator, Display::ConstPtr display, Logger::Ptr logger,
                       Parameters params)
  : params_(std::move(params))
  , ik_solver_(std::move(ik_solver))
  , evaluator_(std::move(evaluator))
  , display_(std::move(display))
  , logger_(std::move(logger))
  , target_poses_(target_generator->generate())
  , search_tree_(createSearchTree(target_poses_))
{
  checkSeedState();
}
```
Check seed state is to validate the starting config for the IK

First Iteration - Run phase
`ReachStudy::run()` is the one which does the binary reachability test, whether the bot can reach the point from initial position
```code
FOR every target pose:
   Try IK
   If success:
       Score the solution
       Store it
   Else:
       Mark unreachable
```

for each point IK solution is found using 
	`evaluateIK(tgt_frame, params_.seed_state, ik_solver_, evaluator_)`
it returns the IK joints solution along with the score.
- Parallel execution with openMP
- Thread-safe result writes
- Deterministic indexing (pose i → result i)

Next Iterations - Optimization phase
conceptually it does
```code
REPEAT:
   Randomize order
   For each reachable pose:
       Try neighboring poses
       Keep better-scoring solutions
UNTIL improvement is small
```
Initial run -> from home position (initial pos), after this the optimization phase starts where the robot is found wether it can reach nearby points or not. These nearby points are found using
```cpp
reachNeighborsDirect(*active_result, msg,
                     ik_solver_, evaluator_,
                     params_.radius, search_tree_);
```
It takes the hyperparameter `params_.radius`, tries for IK neighbours.
Seed is the current pose's joint solution.
For a reachable pose:
- Find nearby poses within `params_.radius`
euclidean distance is calculated with `params._radius`
- Try IK for neighbors
- Seed = current pose’s joint solution
- If neighbor is better → replace it

```cpp
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
this is used to build a connected component in the target space

```cpp
neighbor_count += result.reached_pts.size() - 1;
//reached_pts -> which nearby points are reachable
total_joint_distance += result.joint_distance;
//joint_distance -> how different the joint solutions are
```
- this distance calulates how far the robot can move without branch switching
- If many neighbors are reachable → good continuity
- If joint distance is small → smooth motion feasibility
A collision free - path is not guarenteed, this is not planning this code only analyses how many points are reachable

IK branches can be found with the joint distance. sometimes when theres a huge difference in distances it might be because the robot might've shfted positions to reach the nearby point
an IK branch is a discrete event
- usually what happens when traveling to nearby points is this:
	- Point A → solution `{q₁}`
	- Nearby point B → solution `{q₁ + small d}`
	- but when point C is nearby by unreachable from `{q1 + q2 + small d}`, it has to reposition to start seed and go there, this is called a transition zone/IK branch switch and the pose for C is marked unreachable from B

during the initial run `evaluateIK(pose, initial_seed_state, ...)` and then during the optimization phase connectivity is calculated using `evaluateIK(neighbor_pose, current_solution_as_seed, ...)` where old score is replaced with new scores and path

One should be careful and not confuse the results with reachability analysis with simulation because the results are not the same from every seed state. If a point has good score it might bebecause of nearby seeds states. So results from reachability cannot be directly used in simulation, but can be used in such a way that when simulation is not happening properly to a point instead of concluding that robot cannot reach there, from the results of reachability we can find that the arm should take another path to reach that pose efficiently so its stil reachable

Reach avoids branch switching because it causes:
- branch switching causes violent joint motion
- planners try to avoid it
- controllers hate itm IK solvers try to avoid it

This branch switch will be done by path planners during movement

---
