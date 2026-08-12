# ROS 2 TurtleSim Square Motion Controller 🐢🤖

> A practical ROS 2 project that demonstrates robot motion control in
> **TurtleSim** using Python, ROS 2 publishers, timed callbacks, and
> `geometry_msgs/Twist` velocity commands. The turtle alternates between
> forward motion and 90°-style rotation to produce a square-like
> trajectory.

![ROS 2 TurtleSim Final Output](assets/03-final-output.png)

## 📌 Project Overview

This project was built as a hands-on introduction to **ROS 2 robot
control** and the publisher-based communication model.

The Python node creates a ROS 2 publisher connected to:

``` text
/turtle1/cmd_vel
```

and continuously publishes `Twist` messages to control the TurtleSim
robot's:

-   **Linear velocity** → forward movement
-   **Angular velocity** → rotation
-   **Timer callback** → periodic movement commands

The result is a simple but clear example of how a ROS 2 node can
translate programmed motion logic into visible robot behavior.

------------------------------------------------------------------------

## 🎯 Objectives

The project was designed to practice the following ROS 2 concepts:

-   Creating a ROS 2 Python node with `rclpy`
-   Creating and using a publisher
-   Publishing `geometry_msgs/Twist` messages
-   Controlling linear and angular velocity
-   Using ROS 2 timers and callbacks
-   Running and testing a ROS 2 node from the terminal
-   Visualizing robot motion through TurtleSim
-   Understanding the relationship between a Python control node and a
    ROS 2 topic

------------------------------------------------------------------------

## 🧠 How It Works

The control logic is intentionally simple:

``` text
ROS 2 Node
    │
    ▼
Timer Callback
    │
    ├── Even Step ──► Move Forward
    │                  linear.x = 2.0
    │
    └── Odd Step ───► Turn
                       angular.z = 1.57
    │
    ▼
/turtle1/cmd_vel
    │
    ▼
TurtleSim
```

Every second, the timer callback creates a new `Twist` message.

### Forward Motion

``` python
msg.linear.x = 2.0
msg.angular.z = 0.0
```

The turtle moves forward without rotating.

### Rotation

``` python
msg.linear.x = 0.0
msg.angular.z = 1.57
```

The turtle stops its linear motion and rotates approximately at **π/2
radians per second** (90°/s).

The `step` counter alternates between these two states.

------------------------------------------------------------------------

## 🛠️ Technologies & Tools

  Technology                Purpose
  ------------------------- -----------------------------------------
  **ROS 2 Humble**          Robotics middleware and communication
  **Python 3**              Motion-control node
  **rclpy**                 ROS 2 Python client library
  **TurtleSim**             Robot simulation and visualization
  **geometry_msgs/Twist**   Velocity command message
  **Linux Terminal**        Build, environment setup, and execution

------------------------------------------------------------------------

## 📂 Project Structure

``` text
.
├── turtle_square.py
├── assets/
│   ├── 01-development-environment.png
│   ├── 02-ros2-process.png
│   ├── 03-final-output.png
│   ├── final-process.gif
│   └── ros2-final-process.mp4
└── README.md
```

------------------------------------------------------------------------

## 💻 The Control Node

The main Python file is `turtle_square.py`.

``` python
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

------------------------------------------------------------------------

## 🚀 Running the Project

### 1. Start ROS 2

Open a terminal and source the ROS 2 Humble environment:

``` bash
source /opt/ros/humble/setup.bash
```

### 2. Launch TurtleSim

In a terminal:

``` bash
ros2 run turtlesim turtlesim_node
```

This starts the TurtleSim simulation window.

### 3. Open the Project Directory

``` bash
cd ~/turtle_scripts
```

### 4. Source ROS 2 Again

``` bash
source /opt/ros/humble/setup.bash
```

### 5. Make the Python Script Executable

``` bash
chmod +x turtle_square.py
```

### 6. Run the Controller

``` bash
python3 turtle_square.py
```

You should see ROS 2 log messages similar to:

``` text
[turtle_square]: Moving Forward...
[turtle_square]: Turning...
[turtle_square]: Moving Forward...
[turtle_square]: Turning...
```

At the same time, TurtleSim responds to the published velocity commands.

------------------------------------------------------------------------

## 🖥️ Development Process

### ROS 2 Environment & Code

The project was developed and executed in a Linux-based ROS 2
environment.

![Development Environment](assets/01-development-environment.png)

### Running ROS 2 and TurtleSim

The second stage demonstrates the ROS 2 node running alongside TurtleSim
and publishing movement commands.

![ROS 2 Process](assets/02-ros2-process.png)

### Final Output

The final simulation shows the turtle following the programmed movement
pattern.

![Final Output](assets/03-final-output.png)

------------------------------------------------------------------------

## 🎥 Final Demonstration

### Live Process Preview

[![ROS 2 TurtleSim Final
Process](assets/final-process.gif)](assets/ros2-final-process.mp4)

**[▶ Open the full MP4 demonstration](assets/ros2-final-process.mp4)**

> The GIF is included so the motion can be previewed directly from the
> GitHub README. The original MP4 is also included for the complete
> demonstration.

------------------------------------------------------------------------

## 🔍 ROS 2 Concepts Demonstrated

### Node

The `TurtleSquare` class inherits from:

``` python
Node
```

This makes the Python class a ROS 2 node.

### Publisher

The node publishes `Twist` messages to:

``` text
/turtle1/cmd_vel
```

using:

``` python
self.create_publisher(Twist, '/turtle1/cmd_vel', 10)
```

### Timer

A ROS 2 timer calls the movement function every second:

``` python
self.create_timer(1.0, self.timer_callback)
```

### Twist Message

`geometry_msgs/Twist` provides the velocity components used to control
the simulated robot:

``` text
linear.x  → forward/backward velocity
angular.z → rotational velocity
```

------------------------------------------------------------------------

## 📈 Possible Improvements

This project intentionally keeps the control logic simple. A natural
next step would be to make the motion more precise and scalable.

Possible improvements include:

-   Implement a proper finite-state machine for the four sides of the
    square
-   Stop automatically after completing one square
-   Add configurable side length and turning angle
-   Use elapsed time or odometry instead of fixed timing
-   Add a ROS 2 service or action to start/stop the motion
-   Add parameters for speed and square size
-   Subscribe to `/turtle1/pose` for closed-loop control
-   Convert the script into a complete ROS 2 Python package
-   Add launch files and configurable ROS 2 parameters

These improvements would move the project from a basic open-loop
demonstration toward a more robust robotics control implementation.

------------------------------------------------------------------------

## 📚 What This Project Demonstrates

This project demonstrates practical understanding of:

**Python → ROS 2 Node → Publisher → Twist → `/turtle1/cmd_vel` →
TurtleSim**

Rather than only writing Python code, the project connects software
logic to a simulated robotic system through ROS 2 communication.

------------------------------------------------------------------------

## 👤 Author

**Anas Sami Al-Harthi**

------------------------------------------------------------------------

## ⭐ Project Summary

**ROS 2 TurtleSim Square Motion Controller** is a compact robotics
project focused on learning the fundamentals of ROS 2 communication and
motion control. It uses Python and `rclpy` to publish velocity commands
to TurtleSim, providing a practical foundation for more advanced ROS 2
and robotics applications.
