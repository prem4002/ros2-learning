
```
cd turtlesim_ws/
colcon build

source install/setup.bash
```

![[Pasted image 20241001110239.png]]

There are two things that need's to be done:

- First, draw **four circles** with centers at **(2.0,2.0)**, **(2.0,8.0)**, **(8.0,8.0)**, and **(8.0,2.0)**, each having a diameter of **2.0**, using the turtle to represent the propellers.
    
- Next, draw the drone frame, which is a **square** with its center at **(5.0,5.0)**. The vertices of the square are at **(3.0,5.0)**, **(5.0,7.0)**, **(7.0,5.0)** and **(5.0,3.0)**.
    
- Then draw the **lines** to complete the frame as shown in the picture.
    
- At the end, turtle/drone should come at the center of square i.e **(5.0, 5.0)**.

### [Submission Instructions ](https://portal.e-yantra.org/themeBook/wd/Task_1/task_1a.html#submission-instructions-)

- **Step 1:**
    - To start the turtlesim, navigate to the workspace folder
        `cd ~/turtlesim_ws`
    - Source your workspace
        `source install/setup.bash`
    - Run the turtlesim node
        `ros2 run turtlesim turtlesim_node`
        
- **Step 2:**
    - Start the bag recording using,
        `ros2 bag record -o task_1a --all` 
        
- **Step 3:**
    - Run the Python script
        `ros2 run wd_task_1a task_1a_<team_id>`
        
- **Step 4:**
    - Once your run is complete, stop the bag recording using `Ctrl + C`.
    - Take a screenshot of the entire screen.
    - Now zip all these 4 files and name the zip file as `WD_<team_id>_task_1a.zip`.Make sure you follow this structure of your zip file.
        This should be the structure of zip file  
        |__WD_1234_task_1a.zip  
        … |__task_1a_0.db3  
        … |__metadata.yaml  
        … |__WD_1234_task_1a.py/cpp  
        … |__WD_1234_task_1a.png  
        
- **Step 5:** Submit a zip file on the portal in the place of Task 1A and check your score.