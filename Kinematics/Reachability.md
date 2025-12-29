Heatmap
It's not possible to compute a continuous reachability directly field directly. Discrete points must be sampled first becayse the true reachability function is not analytically known

	|reachability function : math function
To compute reachability continuously I would need a math equation f(quaternion) -> reachability score. This function does not exist closed form.

There are many factors which reachability depends on
- robot geometry
- self collisions
- joint limits
- constraints
- singularities
- obstacles
these factors create a complex, not smooth and discontinuous reachable region in the space, which is why a function does not exist.
