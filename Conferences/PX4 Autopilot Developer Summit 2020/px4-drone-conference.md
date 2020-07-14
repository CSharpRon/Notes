---
title: "Day 1 Talks"
date: 2020-07-06
keywords:
  - Drone
  - ROS2
  - Robotics
…

# Embedded Machine Learning in Practice
_Alessandro Grande, IoT Ecosystem Manager @ arm_

Benefits of ML on Embedded Devices:
- Security and Privacy
- Power and Cost
- Reliability
    - In a real-time situation, you want to ensure that your actions are happening "right away" and you do not want to make decisions that are delayed from what you are doing.

What is ML Being used for in Endpoint Devices:
- Vibration and Motion (Any 'signal')
    - Predictive maintenance, sensor fusion, accelerometer, pressure, lidar, etc

Arm Cortex-M Microcontroller:
- #microcontroller that is used for ML or neural networks on embedded devices
- Works together with #TensorFlow lite (an effort to make TensorFlow work on ARM processors)

Turbulence Recognition:
In the demo, Alessandro showed how the Cortex-M took in data from the accelerometer to determine if a particular motion is an UpDown motion or turbulence (shaking)
- It's running on TensorFlow Lite Micro

# Using PX4 for Educational Satellites: CanSats
_Abhas Maskey, PhD Student @ Kyushu Institute of Technology_

- Abhas helped build the first satellite (that is in orbit) for Nepal - Nepali Sat 1

What are CubeSats:
- A satellite standard for #nanosatellites
- Single watt POWER systems (stackable)
- Weighs about 1kg
- One can fit in your hand (about the size of a Rubix cube)

CubeSats slowly follow the trends of drone technology:
- Repurpose Drone OS to CubeSat OS (PX4)
- Repurpose Software In-Loop Simulations (Gazebo)
- Access to Drone Hardware Supply Chain

What are CanSats:
- Non-flight satellites, primarily designed for educational purposes
- Tool to prepare students and faculty for future CubeSat projects 
![aeeed79b233aa302562b5383e54c505f.png](aeeed79b233aa302562b5383e54c505f.png) 

![c4dadf95fa10062d63c7864744c5991b.png](c4dadf95fa10062d63c7864744c5991b.png)
