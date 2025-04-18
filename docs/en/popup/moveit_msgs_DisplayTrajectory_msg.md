---
layout: popup
---

- File : `moveit_msgs/DisplayTrajectory.msg`

- Compact Message Definition

  ```c
  string model_id
  moveit_msgs/RobotTrajectory[] trajectory
  moveit_msgs/RobotState trajectory_start
  ```

- Description

  - `string model_id`  
  The model id for which this path has been generated  

  - `RobotTrajectory[] trajectory`   
  The representation of the path contains position values for all the joints that are moving along the path; a sequence of trajectories may be specified  

  - `RobotState trajectory_start`  
  The robot state is used to obtain positions for all/some of the joints of the robot.
  It is used by the path display node to determine the positions of the joints that are not specified in the joint path message above.
  If the robot state message contains joint position information for joints that are also mentioned in the joint path message, the positions in the joint path message will overwrite the positions specified in the robot state message.  
