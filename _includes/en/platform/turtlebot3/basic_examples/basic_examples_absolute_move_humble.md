{% capture notice_04 %}
**NOTE**:

- This example was formerly called *Position Control*, now renamed to *Absolute Move*.
- Although it works similarly to **Relative Move** — moving the robot based on user-input `(x, y)` and `theta` — the key difference lies in the **coordinate system**:

  - **Relative Move**: inputs are **relative** to the robot’s current pose  
  - **Absolute Move**: inputs are **absolute** coordinates in the global odometry frame  
*So make sure to choose the appropriate example depending on the type of control you need.*  

{% endcapture %}
<div class="notice--info">{{ notice_04 | markdownify }}</div>
This example explains how to use the TurtleBot3 Absolute Move feature, which enables precise navigation to an absolute coordinate in the odometry frame. The system uses odometry data and quaternion-based orientation calculations to move the robot to a target position and heading.  

<iframe width="640" height="360" src="https://www.youtube.com/embed/FrJTGl4bPfc" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>  

**Understanding the Overall Flow**

To successfully move TurtleBot3 to a desired absolute position, several key components must be launched. The interaction between these components enables precise position tracking and movement control.

- **TurtleBot3 Absolute Move Node (`turtlebot3_absolute_move node`)**
    - Subscribes to odometry `/odom` to track the current position and orientation of the robot.
    - Receives user input for absolute target coordinates `x, y` and heading.
    - Computes motion commands `cmd_vel` to navigate the robot to the desired position.
- **TurtleBot3 Node (`turtlebot3_node`)**
    - Subscribes to `cmd_vel` to execute movement based on position control commands.
    - These components work together to allow precise movement and heading alignment, ensuring that TurtleBot3 accurately reaches its target destination.



### [**Running the Absolute Move System**](#running-the-absolute-move-system)

**Step 1: Preparing TurtleBot3 (Odom Reset)**  
Before running the absolute move system, follow these steps to initialize the odometry frame `odom` properly:
1. Place TurtleBot3 on a stable surface in the desired starting location.
2. Ensure the robot is facing the correct direction.
3. Turn on the power, allowing the system to reset the odometry frame `odom = (0,0,0)` at the starting position.  
If you do not reset the odometry properly, the robot may navigate incorrectly due to incorrect initial position data.

**Step 2: Launch TurtleBot3 Bringup**  
Open a new terminal on the remote PC (e.g., your laptop) and connect to the Raspberry Pi via SSH:  
  **[Remote PC]**  
  ```bash
  $ ssh ubuntu@{IP_ADDRESS_OF_RASPBERRY_PI}
  ```  

Once connected, launch the essential TurtleBot3 bringup package:  
**[TurtleBot3 SBC]**  
  ```bash
  $ export TURTLEBOT3_MODEL=burger
  $ ros2 launch turtlebot3_bringup robot.launch.py
  ```

Wait until the bringup process finishes and the TurtleBot3 is ready before proceeding.

**Step 3: Start the Absolute Move Node**  
Open a new terminal and run:  
  **[Remote PC]**  
  ```bash
  $ ros2 run turtlebot3_example turtlebot3_absolute_move
  ```
The node will initialize and prompt you to enter the target coordinates and heading.

**Step 4: Enter Target Coordinates**  
When prompted, enter the absolute position `x, y` and heading (in degrees). Example:

```bash
TurtleBot3 Absolute Move
----------------------------------------------
Enter absolute coordinates in odometry frame
goal x: absolute x position (unit: m)
goal y: absolute y position (unit: m)
goal heading: absolute orientation (range: -180 ~ 180, unit: deg)
----------------------------------------------
[INFO] [1741151360.648697875] [turtlebot3_absolute_move]: Ready to receive goal inputs.
goal x (absolute): 1.5
goal y (absolute): 2.0
goal heading (absolute, degrees): 90
```

This will move the robot to `1.5, 2.0` and rotate it to face `90°`.

**How It Works**  
- **1. Absolute Move**  
    The system calculates the distance between the current position and the goal.
    The robot moves toward the goal while adjusting its angle to minimize deviation.
    If the distance to the goal is under 0.05m, the robot stops linear motion and transitions to heading alignment.

- **2. Heading Control**  
    Once the robot reaches the target `x, y`, it calculates the heading error (difference between current and goal heading).
    The robot rotates until the heading error is minimized (below 1.0°).
    Upon successful alignment, it stops and waits for the next goal input.
