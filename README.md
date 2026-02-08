# ROS 2 Turtlesim – Catch Them All 🐢🎯

This repository contains a ROS 2 project developed as part of a hands-on ROS 2 course.  
The objective of this project is to control a master turtle in **Turtlesim** that automatically hunts and catches other turtles spawned dynamically on the screen.

This project demonstrates core ROS 2 concepts such as **nodes, topics, services, custom interfaces, parameters, and launch files**.

---

## 📌 Project Description

The system consists of three main nodes working together:

1. **turtlesim_node** (provided by the standard `turtlesim` package)
2. **turtle_controller** – controls the master turtle (`turtle1`)
3. **turtle_spawner** – spawns, tracks, and removes turtles

The master turtle moves autonomously using a simple **P-controller** and catches other turtles one by one.

---

## 🧩 System Architecture

### Nodes

### 🐢 turtlesim_node
- Provides the simulation environment
- Advertises `/spawn` and `/kill` services
- Publishes turtle pose data

### 🐢 turtle_controller
- Controls the master turtle (`turtle1`)
- Subscribes to:
  - `/turtle1/pose`
  - `/alive_turtles`
- Publishes to:
  - `/turtle1/cmd_vel`
- Uses a proportional controller to move toward a target turtle
- Calls `/catch_turtle` service when a turtle is caught

### 🐢 turtle_spawner
- Spawns turtles at random positions
- Keeps track of all alive turtles
- Publishes:
  - `/alive_turtles`
- Provides:
  - `/catch_turtle` service
- Internally calls:
  - `/spawn`
  - `/kill`

---

## 🔁 Custom Interfaces

Defined in the `my_robot_interfaces` package.

### Messages

**Turtle.msg**
string name  
float32 x  
float32 y  

**TurtleArray.msg**
Turtle[] turtles

### Service

**CatchTurtle.srv**
string name  
---  
bool success

---

## ⚙️ Parameters

### turtle_controller
- `catch_closest_turtle_first` (bool)  
  - If enabled, the controller selects the closest turtle as the target

### turtle_spawner
- `spawn_frequency` (float)  
  - Rate at which new turtles are spawned
- `turtle_name_prefix` (string)  
  - Prefix used when naming spawned turtles

---

## 🚀 Launch System

A launch file and YAML parameter file are provided in the `my_robot_bringup` package.  
The launch file starts:

- `turtlesim_node`
- `turtle_controller`
- `turtle_spawner`

with configurable parameters.

---

## 🛠️ Implementation Steps

1. Created `turtle_controller` and subscribed to `/turtle1/pose`
2. Implemented a control loop using a P-controller
3. Created `turtle_spawner` to spawn turtles using `/spawn` service
4. Maintained and published an array of alive turtles
5. Implemented `/catch_turtle` service to remove turtles dynamically
6. Enhanced logic to catch the closest turtle
7. Added parameters, YAML configuration, and launch files

---

## ▶️ How to Run the Project

cd ~/ros2_ws  
colcon build  
source install/setup.bash  
ros2 launch my_robot_bringup turtlesim_catch_them_all.launch.py

---

## 👤 Author

Rajesh Thangaraj