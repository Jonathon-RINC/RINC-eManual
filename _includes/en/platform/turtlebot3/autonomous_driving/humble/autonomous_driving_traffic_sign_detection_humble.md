## [Traffic Sign Detection](#traffic-sign-detection)

Traffic sign detection allows the TurtleBot3 to recognize and respond to traffic signs while driving autonomously.

<iframe width="640" height="360" src="https://www.youtube.com/embed/DhSZo3dGW6A" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>


This feature uses the `SIFT` (Scale-Invariant Feature Transform) algorithm, which detects key feature points in an image and compares them to a stored reference image for recognition. Signs with more distinct edges tend to yield better recognition results.

This section explains how to capture and store traffic sign images, configure detection parameters, and run the detection process in the Gazebo simulation.

**NOTE**: More and better defined edges in the traffic sign increase recognition results from the SIFT algorithm.  
Please refer to [this SIFT documentation](https://docs.opencv.org/master/da/df5/tutorial_py_sift_intro.html) for additional information.
{: .notice}

**Launching Traffic Sign Detection in Simulation**

Start the Autorace Gazebo simulation to set up the environment:
```bash
$ ros2 launch turtlebot3_gazebo turtlebot3_autorace_2020.launch
```

Then, control the TurtleBot3 manually using the keyboard to navigate the vehicle toward traffic signs:
```bash
$ ros2 run turtlebot3_teleop teleop_keyboard
```
Position the robot so that traffic signs are clearly visible in the camera feed.
<br><br>

**Capturing and Storing Traffic Sign Images**

To ensure accurate recognition, the system requires pre-captured traffic sign images as reference data. While the repository provides default images, recognition accuracy may vary depending on conditions. **If the SIFT algorithm does not perform well with the provided images, capturing and using your own traffic sign images can improve recognition results**.

1. Open `rqt`, then navigate to Plugins > Visualization > Image View.
2. Create a new image view window and select the topic: `/camera/image_compensated` to display the camera feed.
3. Position the TurtleBot3 so that traffic signs are clearly visible in the camera view.
4. Capture images of each traffic sign and crop any unnecessary background, focusing only on the sign itself.
5. For the best performance, use the original traffic signs from the track whenever possible.

Save the images in the turtlebot3_autorace_detect package **/turtlebot3_autorace/turtlebot3_autorace_detect/image/**.

Ensure that the file names match those used in the source code, as the system references these names:
- The `construction.png`, `intersection.png`, `left.png`, `right.png`, `parking.png`, `stop.png` and `tunnel.png` file names are used by default.


If recognition performance is inconsistent with the default images, manually captured traffic sign images may enhance accuracy and improve overall detection reliability.
<br><br>

**Running Traffic Sign Detection**

Before launching the detection node, run the camera calibration processes to ensure the camera feed is properly aligned:
```bash
$ ros2 launch turtlebot3_autorace_camera intrinsic_camera_calibration.launch.py
$ ros2 launch turtlebot3_autorace_camera extrinsic_camera_calibration.launch.py
```

Then, launch the traffic sign detection node, specifying the mission type:<br>
A specific mission for the ***mission*** argument must be selected from the following options:
- `intersection`, `construction`, `parking`, `level_crossing`, `tunnel`
```bash
$ ros2 launch turtlebot3_autorace_detect detect_sign.launch.py mission:=SELECT_MISSION
```
This command starts the detection process and allows TurtleBot3 to recognize and respond to the selected traffic sign.
    
    **NOTE**: Replace the `SELECT_MISSION` keyword with one of the available options above.
    {: .notice}
<br>

**Visualizing Detection Results**

To check the detected traffic signs, open `rqt`, then navigate to: **Plugins > Visualization > Image View**  

Create a new image view window and select the topic:  `/detect/image_traffic_sign/compressed`

This will display the result of traffic sign detection in real-time. The detected traffic sign will be overlaid on the screen based on the mission assigned.

Below are examples of successfully detected traffic signs for different missions:

<p align="center">
  <img src="/assets/images/platform/turtlebot3/autonomous_driving/noetic_detect_intersection.png" width="450"/>
  <img src="/assets/images/platform/turtlebot3/autonomous_driving/noetic_detect_left.png" width="450"/>
  <img src="/assets/images/platform/turtlebot3/autonomous_driving/noetic_detect_right.png" style="width: 450px; height: 350px;"/>
</p>
<p align="center">
  <em>Detecting Intersection, Left, and Right signs (`mission:=intersection`)</em>
</p>

<p align="center">
  <img src="/assets/images/platform/turtlebot3/autonomous_driving/noetic_detect_construction.png" width="500"/>
  <img src="/assets/images/platform/turtlebot3/autonomous_driving/noetic_detect_parking.png" width="500"/>
</p>
<p align="center">
  <em>Detecting Construction, and Parking signs (`mission:=construction`, `mission:=parking`)</em>
</p>

<p align="center">
  <img src="/assets/images/platform/turtlebot3/autonomous_driving/noetic_detect_level_crossing.png" width="500"/>
  <img src="/assets/images/platform/turtlebot3/autonomous_driving/noetic_detect_tunnel.png" width="500"/>
</p>
<p align="center">
  <em>Detecting the Tunnel, and Level Crossing signs (`mission:=level_crossing`, `mission:=tunnel`)</em>
</p>
