# TurtleBot3 Obstacle Avoidance Using ROS2 with Docker-Compose

## **Project Overview**
This project demonstrates the implementation of an **Obstacle Avoidance Algorithm** for TurtleBot3 using **ROS2 Humble** in a **Dockerized environment**. It leverages **LIDAR sensor data** for dynamic obstacle detection and navigation within the **Gazebo simulation environment**.

This guide provides **step-by-step instructions** for setting up the project using **Docker Compose**, bypassing the need for **Rocker**.

---

## **Table of Contents**
- [Requirements](https://github.com/HasibuzzamanFUAS/docker-obstacle-avoidance-turtlebot3-ros2?tab=readme-ov-file#requirements)
- [Dependencies](https://github.com/HasibuzzamanFUAS/docker-obstacle-avoidance-turtlebot3-ros2?tab=readme-ov-file#dependencies)
- [Installation Steps](https://github.com/HasibuzzamanFUAS/docker-obstacle-avoidance-turtlebot3-ros2?tab=readme-ov-file#installation-steps)
- [Running the Simulation](https://github.com/HasibuzzamanFUAS/docker-obstacle-avoidance-turtlebot3-ros2?tab=readme-ov-file#running-the-simulation)
- [Teleoperation Control](https://github.com/HasibuzzamanFUAS/docker-obstacle-avoidance-turtlebot3-ros2?tab=readme-ov-file#teleoperation-control)
- [Sensor Verification](https://github.com/HasibuzzamanFUAS/docker-obstacle-avoidance-turtlebot3-ros2?tab=readme-ov-file#teleoperation-control)
- [Conclusion](https://github.com/HasibuzzamanFUAS/docker-obstacle-avoidance-turtlebot3-ros2?tab=readme-ov-file#teleoperation-control)
- [References](https://github.com/HasibuzzamanFUAS/docker-obstacle-avoidance-turtlebot3-ros2?tab=readme-ov-file#teleoperation-control)

---

## **Requirements**
- **Operating System**: Windows 10/11 with **Docker Desktop** installed.
- **X Server for Windows**: [VcXsrv](https://sourceforge.net/projects/vcxsrv/) for GUI support.
- **Docker Desktop** with **WSL2 Backend** enabled.

---

## **Dependencies**
- **Docker Compose**: For container orchestration.
- **ROS2 Humble**: Pre-installed in the Docker image.
- **Gazebo**: For robot simulation.
- **TurtleBot3 Packages**: Pre-installed for obstacle avoidance and teleoperation.

---

## **Installation Steps**

### **Step 1: Clone the Repository**
```bash
git clone https://github.com/HasibuzzamanFUAS/docker-obstacle-avoidance-turtlebot3-ros2.git
cd docker-obstacle-avoidance-turtlebot3-ros2
```

### **Step 2: Build Docker Image**
```bash
docker build . -t turtlebot3_container
```
Verify the image:
```bash
docker image ls
```
Ensure the image **`turtlebot3_container`** appears in the list.

### **Step 3: Configure Docker Compose**
Create a file named **`docker-compose.yaml`** in the project directory with the following content:
```yaml

services:
  turtlebot3:
    image: turtlebot3_container
    privileged: true
    network_mode: "host"
    environment:
      - DISPLAY=host.docker.internal:0
      - QT_X11_NO_MITSHM=1
    volumes:
      - /tmp/.X11-unix:/tmp/.X11-unix
    stdin_open: true
    tty: true
    command: /bin/bash
```

### **Step 4: Setup Display Environment**
- Install **VcXsrv** and start it with:
  - **Multiple Windows** → **Start no client** → **Disable Access Control**.
- Set **DISPLAY** environment variable:
```bash
set DISPLAY=host.docker.internal:0
```

### **Step 5: Start Docker Compose**
```bash
docker-compose up -d

# to see container runnig list
docker ps
```
Attach to the running container:
```bash
docker exec -it <container_name> bash
```
Replace `<container_name>` with the name of your running container (e.g., `turtlebot3_container`).

### **Step 6: Source ROS2 Workspace**
Inside the container:
```bash
source install/setup.bash
```

---

## **Running the Simulation**

### **Step 1: Run Obstacle Avoidance Algorithm**
```bash
ros2 launch obstacle_avoidance_tb3 launch.py
```
### **Step 2: Launch Gazebo Simulation**
```bash
ros2 launch turtlebot3_gazebo turtlebot3_dqn_stage2.launch.py gui:=false
```
> Use `ros2 launch turtlebot3_gazebo turtlebot3_dqn_stage2.launch.py gui:=false` to run Gazebo in **headless mode** if GUI issues persist.
---

## **Teleoperation Control**
1. Open another terminal in the container:
```bash
docker exec -it <container_name> bash
```
2. Run the teleoperation script:
```bash
ros2 run obstacle_avoidance_tb3 turtlebot_teleop.py
```
3. Control the robot using the following keys:
Key Bindings and Actions

**Key	Action**

W	--------> Move Forward

X	-------> Move Backward

A	-------> Rotate Left

D	-------> Rotate Right

S	-------> Stop all motion (Emergency Halt)

Space	Stop all motion (Emergency Halt)

**CTRL + C	Exit the program**

---

## **Sensor Verification**

```bash
#Check Active Topics
ros2 topic list
```

```bash
# Monitor LIDAR Sensor Data
ros2 topic echo /scan
```


```bash
# Verify Velocity Commands
ros2 topic echo /cmd_vel
```

---
## **Result**

![image](https://github.com/user-attachments/assets/6ef69230-3b5a-4125-860f-059a1488eb04)


## **Conclusion**
This project successfully demonstrates how to implement an **Obstacle Avoidance Algorithm** for TurtleBot3 using **ROS2 Humble** and **Docker Compose**. By leveraging **Gazebo simulation**, the robot dynamically avoids obstacles using **LIDAR sensor data**.
Key features include:
- **Headless mode support** for lightweight execution.
- **Teleoperation functionality** for manual control.
- **Dynamic decision-making** using real-time sensor data.

Future extensions can include:
- **Path planning algorithms** with start and goal nodes.
- Deployment on **physical TurtleBot3 hardware**.
  
## References
1. Gazebo Simulation Tutorials - https://emanual.robotis.com/docs/en/platform/turtlebot3/simulation/
2. Python3 rocker - https://github.com/osrf/rocker
3. Turtlebot3 teleop - https://github.com/ROBOTIS-GIT/turtlebot3/tree/humble-devel/turtlebot3_teleop
4. Linux Implementation: https://github.com/vinay06vinay/Turtlebot3-Obstacle-Avoidance-ROS2

> For any questions or contributions, feel free to raise an issue or pull request in the repository!

---

