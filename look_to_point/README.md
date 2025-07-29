# TIAGo face tracking tutorials (Noetic)

This package contains 2 nodes for head action control:

* _look_to_point_ is required in the [Head action](https://wiki.ros.org/Robots/TIAGo/Tutorials/motions/head_action) tutorial
* _look_to_face_ permits to combine the [Head action](https://wiki.ros.org/Robots/TIAGo/Tutorials/motions/head_action) tutorial and  the [FaceDetection](https://wiki.ros.org/Robots/TIAGo/Tutorials/FaceDetection) tutorial to automatically track a face with the head action.


## Automatic face tracking

### Concept

The [Head action](https://wiki.ros.org/Robots/TIAGo/Tutorials/motions/head_action) tutorial expects the user to click a point in the image so that the head of TIAGo looks at the clicked direction.

The [FaceDetection](https://wiki.ros.org/Robots/TIAGo/Tutorials/FaceDetection) tutorial provides a debug image to visualize the detected face, but also outputs a face detection message containing the coordinates of the center of the detected face if any.

The _look_to_face_ node connects to the raw tiago camera image as did the _look_to_point_, contains the same click to look at code, but also subscribes to the face detection message outputed by the _pal_face_detector_opencv_ of the FaceDetection tutorial, to provide live coordinates of new faces to track.

### Starting up

1. Start a Simulation world with TIAGo and some people

        roslaunch tiago_gazebo tiago_gazebo.launch public_sim:=true end_effector:=pal-gripper world:=simple_office_with_people

2. Start the face detection node

        roslaunch pal_face_detector_opencv detector.launch image:=/xtion/rgb/image_raw

3. Start the look_to_face node

        rosrun look_to_point look_to_face 

4. Start RQT GUI  as visualization and control tool using the provided perspective

        rosrun rqt_gui rqt_gui --perspective-file `(rospack find look_to_point)`/config/look_to_face_viz_steer.perspective

5. Use the Robot steering plugin to move the robot towards a perso. As soon as a face is detected, the central image will show a drawn frame around the face, and the robot head will move to look at it, even if the robot base moves away or the torso is moved up/down (using the joint_trajectory_controller_gui).

**Note** If no face is detected, the clicking on the color image in the OpenCV view window is still possible to look at the pointed direction, but face tracking has priority.