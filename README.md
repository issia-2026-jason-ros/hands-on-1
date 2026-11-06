# Hands-on activity

# 1. Introduction

This activity refers to programming an agent to be embedded in a vehicle whose sensors and actuators are accessible through ROS topics. These topics are specified in the tables below.

#### Actuators Topics
| Topic name | Data type | Function | Possible parameter values |
|---|---|---|---|
| `/arduino/motor/command` | `std_msgs/msg/String` | Controls the robot's movement direction | `forward`, `backward`, `left`, `right`, `stop` |
| `/arduino/motor/speed/command` | `std_msgs/msg/String` | Sets the motor speed level | `high`, `default`,`low`|
| `/arduino/buzzer/command` | `std_msgs/msg/String` | Controls the buzzer | `on`, `off`, `high`, `low` |
| `/arduino/headlight/command` | `std_msgs/msg/String` | Controls the front headlight | `on`, `off`, `high`, `low` |
| `/arduino/turn_signal/command` | `std_msgs/msg/String` | Controls the turn signals and hazard lights | `alert`, `right`, `left`, `off` |
| `/arduino/brake_light/command` | `std_msgs/msg/Bool` | Turns the brake light on or off to indicate that the brakes are engaged | `true`, `false` |

#### Sensors Topics
| Topic name | Data type | Function | Possible parameter values |
|---|---|---|---|
| `/arduino/motor/status` | `std_msgs/msg/String` | Reports the motor's current state (perception) | `stopped`, `running`, `turningRight`, `turningLeft`, `backward` |
| `/arduino/motor/speed/status` | `std_msgs/msg/String` | Reports the motor's current speed level (perception) | `default`, `high`, `low` |
| `/arduino/buzzer/status` | `std_msgs/msg/String` | Reports the buzzer's current state (perception) | `off`, `on`, `high`, `low` |
| `/arduino/headlight/status` | `std_msgs/msg/String` | Reports the headlight's current state (perception) | `off`, `on`, `high`, `low` |
| `/arduino/turn_signal/status` | `std_msgs/msg/String` | Reports the turn signal's current state (perception) | `alert`, `right`, `left`, `off` |
| `/arduino/brake_light/status` | `std_msgs/msg/Bool` | Reports whether the brake light is currently on (perception) | `on`, `off` |
| `/arduino/light_sensor/reading` | `std_msgs/msg/Float32` | Ambient light level read by the LDR sensor | scale: `1.0`<br>offset: `0.0`<br>minimum: `0.0`<br>maximum: `1023.0` |
| `/arduino/ultrasonic_sensor/distance` | `std_msgs/msg/Float32` | Distance to an object, measured by the ultrasonic sensor | scale: `1.0`<br>offset: `0.0`<br>minimum: `0.0` |
| `/arduino/line_sensor/left` | `std_msgs/msg/Float32` | Value read by the left line sensor | scale: `1.0`<br>offset: `0.0`<br>minimum: `0.0`<br>maximum: `1023.0` |
| `/arduino/line_sensor/right` | `std_msgs/msg/Float32` | Value read by the right line sensor | scale: `1.0`<br>offset: `0.0`<br>minimum: `0.0`<br>maximum: `1023.0` |

<!--
| Topic name | Data type | Function | Possible parameter values |
|---|---|---|---|
| `/arduino/motor/command` | `std_msgs/msg/String` | Controls the robot's movement direction | `forward`, `backward`, `left`, `right`, `stop` |
| `/arduino/motor/status` | `std_msgs/msg/String` | Reports the motor's current state (perception) | `stopped`, `running`, `turningRight`, `turningLeft`, `backward` |
| `/arduino/motor/speed/command` | `std_msgs/msg/String` | Sets the motor speed level | `high`, `default`,`low`|
| `/arduino/motor/speed/status` | `std_msgs/msg/String` | Reports the motor's current speed level (perception) | `default`, `high`, `low` |
| `/arduino/buzzer/command` | `std_msgs/msg/String` | Controls the buzzer | `on`, `off`, `high`, `low` |
| `/arduino/buzzer/status` | `std_msgs/msg/String` | Reports the buzzer's current state (perception) | `off`, `on`, `high`, `low` |
| `/arduino/headlight/command` | `std_msgs/msg/String` | Controls the front headlight | `on`, `off`, `high`, `low` |
| `/arduino/headlight/status` | `std_msgs/msg/String` | Reports the headlight's current state (perception) | `off`, `on`, `high`, `low` |
| `/arduino/turn_signal/command` | `std_msgs/msg/String` | Controls the turn signals and hazard lights | `alert`, `right`, `left`, `off` |
| `/arduino/turn_signal/status` | `std_msgs/msg/String` | Reports the turn signal's current state (perception) | `alert`, `right`, `left`, `off` |
| `/arduino/brake_light/command` | `std_msgs/msg/Bool` | Turns the brake light on or off to indicate that the brakes are engaged | `true`, `false` |
| `/arduino/brake_light/status` | `std_msgs/msg/Bool` | Reports whether the brake light is currently on (perception) | `on`, `off` |
| `/arduino/light_sensor/reading` | `std_msgs/msg/Float32` | Ambient light level read by the LDR sensor | scale: `1.0`<br>offset: `0.0`<br>minimum: `0.0`<br>maximum: `1023.0` |
| `/arduino/ultrasonic_sensor/distance` | `std_msgs/msg/Float32` | Distance to an object, measured by the ultrasonic sensor | scale: `1.0`<br>offset: `0.0`<br>minimum: `0.0` |
| `/arduino/line_sensor/left` | `std_msgs/msg/Float32` | Value read by the left line sensor | scale: `1.0`<br>offset: `0.0`<br>minimum: `0.0`<br>maximum: `1023.0` |
| `/arduino/line_sensor/right` | `std_msgs/msg/Float32` | Value read by the right line sensor | scale: `1.0`<br>offset: `0.0`<br>minimum: `0.0`<br>maximum: `1023.0` |
-->


# Exercises

## Part 1 - Testing sensors and actuators
To both check whether the hardware is fine and also to become familiar with the available ROS resources, use terminal commands to inspect sensor values and actuator status and to control the actuators.

First of all, connect with the vehicle companion computer via ssh:
- Host name: `<VEHICLE NAME>.bot.chon.group`
- User name: root
- Password: root

For inspecting sensor values and actuator status:
```
ros2 topic echo <TOPIC NAME>
```

For controlling the actuators:
```perception_action
ros2 topic pub -1 <TOPIC NAME> <TOPIC TYPE> "data: <VALUE>"
```

Notes: 
- for boolean topics, VALUE is `true` or `false`.
- for string topics, VALUE must be enclosed in single quotes.

## Part 2 - Implementing a  collision avoidance vehicle

### Preliminaries

First of all, clone the repository at `https://github.com/issia-2026-jason-ros/<VEHICLE NAME>`. 

This repository contains a JaCaMo application composed of an agent that has the goal `walk` to move itself around the environment. However, this agent does not have any plan to satisfy its goal. In this exercise, you will code the agent to do that. Use the available ROS topics can be connected to the perceptions and actions of the agent. Use the available ROS topics to connect the cognitive portion of the agent with the hardware.

### Exercise 2.1
Make the vehicle to move forward and stop when an obstacle is detected.

### Exercise 2.2
Make the vehicle to move forward and tune its speed according to the distance from the obstacle:
- High speed when the obstacle is in a safe distance;
- Default speed when the obstacle is in an average distance;
- Low speed when an obstacle gets close;
- Stop when the obstacle is in a critical distance.

In addition, blink the brake light for a little while (e.g. 500 milliseconds) when the vehicle slows down.


### Exercise 2.3
Make the vehicle move around the environment avoiding obstacles. Define your own navigation strategy.
Observe the following requirements:
- blink the brake light for a little while (e.g. 500 milliseconds) when the vehicle slows down;
- blink the turn signal when the vehicle is turning left/right.

