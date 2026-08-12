# ROS-2-TurtleSim-Square-Motion-Controller
A practical ROS 2 project that demonstrates robot motion control in TurtleSim using Python, ROS 2 publishers, timed callbacks, and geometry_msgs/Twist velocity commands. 


# ROS 2 TurtleSim Square Motion Controller 🐢🤖

> A practical ROS 2 robotics project that demonstrates motion control in TurtleSim using Python, `rclpy`, timed callbacks, publishers, and `geometry_msgs/Twist` velocity commands.

![ROS 2 TurtleSim Final Output](assets/03-final-output.png)

---

## 📌 Overview

**ROS 2 TurtleSim Square Motion Controller** is a hands-on robotics project built with **ROS 2 Humble** and **Python**.

The project demonstrates how a ROS 2 node can control a simulated robot by publishing velocity commands to the `/turtle1/cmd_vel` topic.

The controller alternates between:

- Moving the turtle forward.
- Rotating the turtle.
- Repeating the movement sequence to create a square-like trajectory.

The main goal of the project is to build a practical understanding of how **Python code, ROS 2 nodes, publishers, topics, and velocity messages work together inside a robotic system**.

---

## 🎯 Project Objectives

This project was developed to practice and demonstrate the following ROS 2 concepts:

- Creating a ROS 2 Python node using `rclpy`
- Creating a ROS 2 publisher
- Publishing `geometry_msgs/Twist` messages
- Controlling linear and angular velocity
- Using ROS 2 timers and callbacks
- Communicating through ROS 2 topics
- Running ROS 2 applications from the Linux terminal
- Using TurtleSim as a robotics simulation environment
- Connecting Python control logic to simulated robot movement
- Understanding the basic architecture of ROS 2 robotic applications

---

## 🧠 How the System Works

The project follows a simple ROS 2 communication pipeline:

```text
                 Python ROS 2 Node
                        │
                        ▼
                Timer Callback
                        │
              ┌─────────┴─────────┐
              │                   │
              ▼                   ▼
        Move Forward            Turn
        linear.x = 2.0       angular.z = 1.57
              │                   │
              └─────────┬─────────┘
                        ▼
               /turtle1/cmd_vel
                        │
                        ▼
                    TurtleSim
                        │
                        ▼
               Simulated Movement
```

The Python node publishes a new `Twist` message every second.

The controller uses a simple step counter to alternate between forward movement and rotation.

### Forward Movement

```python
msg.linear.x = 2.0
msg.angular.z = 0.0
```

The turtle moves forward without rotating.

### Rotation

```python
msg.linear.x = 0.0
msg.angular.z = 1.57
```

The turtle stops its linear movement and rotates at approximately **1.57 rad/s**, which is approximately **90 degrees per second**.

---

## 🛠️ Technologies & Tools

| Technology | Purpose |
|---|---|
| **ROS 2 Humble** | Robotics middleware and communication |
| **Python 3** | Motion-control programming |
| **rclpy** | ROS 2 Python client library |
| **TurtleSim** | Robot simulation and visualization |
| **geometry_msgs/Twist** | Velocity command message |
| **Linux Terminal** | ROS 2 environment and execution |

---

## 📂 Project Structure

```text
ROS2-TurtleSim-Square-Controller/
│
├── turtle_square.py
│
├── assets/
│   ├── 01-development-environment.png
│   ├── 02-ros2-process.png
│   ├── 03-final-output.png
│   ├── final-process.gif
│   └── ros2-final-process.mp4
│
└── README.md
```

---

## 💻 Main Control Node

The main control program is implemented in:

```text
turtle_square.py
```

```python
#!/usr/bin/env python3

import rclpy
from rclpy.node import Node
from geometry_msgs.msg import Twist


class TurtleSquare(Node):

    def __init__(self):
        super().__init__('turtle_square')

        self.publisher_ = self.create_publisher(
            Twist,
            '/turtle1/cmd_vel',
            10
        )

        self.timer = self.create_timer(
            1.0,
            self.timer_callback
        )

        self.step = 0

    def timer_callback(self):
        msg = Twist()

        if self.step % 2 == 0:
            msg.linear.x = 2.0
            msg.angular.z = 0.0
            self.get_logger().info('Moving Forward...')

        else:
            msg.linear.x = 0.0
            msg.angular.z = 1.57
            self.get_logger().info('Turning...')

        self.publisher_.publish(msg)
        self.step += 1


def main(args=None):
    rclpy.init(args=args)

    node = TurtleSquare()

    rclpy.spin(node)

    node.destroy_node()
    rclpy.shutdown()


if __name__ == '__main__':
    main()
```

---

## 🚀 Running the Project

### 1. Source the ROS 2 Environment

Open a terminal and run:

```bash
source /opt/ros/humble/setup.bash
```

---

### 2. Start TurtleSim

Run:

```bash
ros2 run turtlesim turtlesim_node
```

This launches the TurtleSim simulation window.

---

### 3. Navigate to the Project Directory

```bash
cd ~/turtle_scripts
```

---

### 4. Source ROS 2

```bash
source /opt/ros/humble/setup.bash
```

---

### 5. Make the Python Script Executable

```bash
chmod +x turtle_square.py
```

---

### 6. Run the Controller

```bash
python3 turtle_square.py
```

The terminal should display messages similar to:

```text
[turtle_square]: Moving Forward...
[turtle_square]: Turning...
[turtle_square]: Moving Forward...
[turtle_square]: Turning...
```

At the same time, TurtleSim receives the published velocity commands and moves according to the programmed logic.

---

## 🖥️ Development Process

### 01 — Development Environment

The project was developed in a Linux-based ROS 2 environment using Python and the ROS 2 Humble distribution.

![Development Environment](assets/01-development-environment.png)

---

### 02 — ROS 2 Process

The ROS 2 TurtleSim node and the Python controller are executed through the terminal.

The controller publishes movement commands to:

```text
/turtle1/cmd_vel
```

![ROS 2 Process](assets/02-ros2-process.png)

---

### 03 — Final Output

The final simulation demonstrates the programmed turtle movement and the resulting trajectory inside TurtleSim.

![Final Output](assets/03-final-output.png)

---

## 🎥 Final Demonstration

The following GIF provides a quick preview of the final ROS 2 TurtleSim process directly inside GitHub:

[![ROS 2 TurtleSim Final Process](assets/final-process.gif)](assets/ros2-final-process.mp4)

### Full Demonstration

**[▶ Open the full MP4 demonstration](assets/ros2-final-process.mp4)**

> The GIF is included for quick visual preview inside the README, while the original MP4 is provided for the complete demonstration.

---

## 🔍 ROS 2 Concepts Demonstrated

### 1. ROS 2 Node

The project defines a custom ROS 2 node:

```python
class TurtleSquare(Node):
```

The node is responsible for generating and publishing the movement commands.

---

### 2. Publisher

The controller creates a publisher using:

```python
self.create_publisher(
    Twist,
    '/turtle1/cmd_vel',
    10
)
```

This allows the Python node to send velocity commands to TurtleSim.

---

### 3. ROS 2 Topic

The main communication topic is:

```text
/turtle1/cmd_vel
```

This topic receives velocity commands that control the TurtleSim robot.

---

### 4. Twist Message

The project uses:

```python
from geometry_msgs.msg import Twist
```

A `Twist` message provides linear and angular velocity components.

The project mainly uses:

```text
linear.x
```

for forward movement and:

```text
angular.z
```

for rotation.

---

### 5. Timer Callback

The controller uses a ROS 2 timer:

```python
self.create_timer(1.0, self.timer_callback)
```

This causes the callback function to execute every second.

The callback then decides whether the turtle should move forward or rotate.

---

## ⚙️ Motion Logic

The controller uses a simple alternating state:

```text
Step 0 → Forward
Step 1 → Turn
Step 2 → Forward
Step 3 → Turn
Step 4 → Forward
Step 5 → Turn
...
```

The decision is controlled by:

```python
if self.step % 2 == 0:
```

Even steps result in forward movement, while odd steps result in rotation.

This provides a simple introduction to programmed robot motion using ROS 2.

---

## 📊 Key Parameters

| Parameter | Value | Description |
|---|---:|---|
| Timer period | `1.0 s` | Callback execution interval |
| Linear velocity | `2.0` | Forward movement speed |
| Angular velocity | `1.57 rad/s` | Approx. 90°/s rotation |
| ROS 2 topic | `/turtle1/cmd_vel` | Velocity command topic |
| Message type | `Twist` | Robot velocity command |

---

## 📈 Possible Improvements

The current implementation intentionally focuses on the fundamentals of ROS 2 communication and motion control.

Several improvements could make the controller more precise and scalable:

- Implement a proper finite-state machine for the four sides of the square.
- Stop automatically after completing one square.
- Add configurable square dimensions.
- Add ROS 2 parameters for speed and turning angle.
- Use elapsed time to control the exact distance traveled.
- Subscribe to `/turtle1/pose` for feedback.
- Implement closed-loop motion control.
- Add a ROS 2 service to start and stop the movement.
- Convert the script into a complete ROS 2 Python package.
- Add a ROS 2 launch file.
- Add automated testing.
- Separate configuration from the control logic.

These improvements would move the project from a simple open-loop demonstration toward a more robust robotics control system.

---

## 🧩 What I Learned

Through this project, I practiced the fundamental connection between software and robotics:

```text
Python
   ↓
ROS 2 Node
   ↓
Publisher
   ↓
Twist Message
   ↓
ROS 2 Topic
   ↓
TurtleSim
   ↓
Robot Motion
```

The project provided practical experience with ROS 2 nodes, publishers, topics, messages, timers, callbacks, and simulated robot control.

---

## 🏁 Conclusion

**ROS 2 TurtleSim Square Motion Controller** is a compact robotics project designed to demonstrate the fundamentals of ROS 2 motion control using Python.

Although the controller uses a simple open-loop approach, it provides a strong foundation for progressing toward more advanced robotics concepts such as:

- Sensor integration
- Robot localization
- Feedback control
- Autonomous navigation
- ROS 2 services and actions
- Odometry-based control
- Real robotic platforms

The project represents a practical step from programming logic toward real robotic software development.

---

## 👤 Author

**Anas Sami Al-Harthi**

---

## ⭐ Project Summary

**ROS 2 TurtleSim Square Motion Controller** demonstrates how a Python program can communicate with a simulated robot through ROS 2 and control its movement using velocity commands.

It combines **Python, ROS 2, TurtleSim, publishers, topics, timers, and `Twist` messages** into a practical robotics application.

---
