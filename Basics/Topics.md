`ros2 topic list`
Can see how topics are communicating using `rqt_graph`

Run these to inspect how frames are actually being published:
- `ros2 topic echo /tf --once` — shows dynamic TFs.
- `ros2 topic echo /tf_static --once` — shows static TFs.
- `ros2 run tf2_ros tf2_echo base_link tool_link` — tries to echo transform from `base_link` to `tool_link` (replace `tool_link` with any downstream frame).
- `ros2 run tf2_tools view_frames` — creates a frame graph PDF (if you have the package installed).