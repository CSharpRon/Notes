---
title: "Day 2 Talks"
date: 2020-07-07
keywords:
  - Drone
  - ROS2
  - Robotics
…

# Jetman
_Julien Cecoeur, R&D Engineer @ Flybotix_

## What is Jetman?
![874a5abb1394319fbd3d009b6323ee1e.png](874a5abb1394319fbd3d009b6323ee1e.png)

- Wearable wing powered by four jet engines with two goals:
    - "Fly like a bird and have fun"

- World's first manned VTOL with jet propulsion

- Currently, Jetman is flown by first having the wearer increase its altitude from a helicopter, then the flyer drops and flies, and a parachute is deployed for landing

- Next step: VTOL (eliminate the helicopter) -> VTOL (eliminate the helicopter and the parachute)

- What's needed for those next steps?
    - Safety, Power (180 Kg weight for the wings plus the flier plus the fuel)
    - Control (there are no flaps on the wing. Instead, the shoulders twist to go left/right)
        - Attitude control is managed by thrust control which the pilot controls by hand. In this implementation, yaw cannot be manipulated. To get around this, the Jetman implementation team introduced a new control called the **Thrust Vector**. This vector allows the thrust to rotate according to the pilot's orientation. If they bend forward, the thrust vector will match the pilot body axis.

- Pixhawk is used onboard the VTOL

# Bringing micro-ROS to PX4-based flying systems
_Jaime Martin-Losa, CEO @ eProsima_
_Nuno Marques, Founder and Lead Software Engineer @ Drone / Solutions_

## eProsima
- Experts on middleware, focused on DDS and ROS2
- ROS2 TSC Members: Key ROS2 Contributors
- eProsima Fast #DDS: (Data Distribution Service Implementation)
    - Adopted by ROS2
- eProsima Micro XRCE-DDS:
    - DDS for eXtreme Resource Constrained Environments: Microcontrollers
    - Base of Micro-ROS

## DDS
- Uses the concept of Global data space:
    - In this space we define topics of data, and the publishers publish samples of these topics.
    - DDS distributes these samples to all the subscribers of those topics. Any node can be a publisher or a subscriber