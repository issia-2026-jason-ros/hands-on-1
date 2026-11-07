# Hands-on activity

# 1. Introduction

This activity involves programming an agent to be embedded in a vehicle equipped with the following sensors and actuators:

##### Available actuators:
- Motor;
- Buzzer;
- Headlight;
- Turn signals;
- Brake light.

##### Available sensors:
- Motor status sensor;
- Motor speed sensor;
- Buzzer status;
- Headlight status;
- Turn signal status;
- Brake light status;
- Light sensor;
- Distance sensor;
- Line detector.

The sensors and actuators are accessible through ROS topics, as specified in the tables below.

#### Actuators Topics
| Topic name | Data type | Function | Possible parameter values |
|---|---|---|---|
| `/arduino/motor/command` | `std_msgs/msg/String` | Controls the vehicle's movement direction | `forward`, `backward`, `left`, `right`, `stop` |
| `/arduino/motor/speed/command` | `std_msgs/msg/String` | Sets the motor speed | `high`, `default`,`low`|
| `/arduino/buzzer/command` | `std_msgs/msg/String` | Controls the buzzer | `on`, `off`, `high`, `low` |
| `/arduino/headlight/command` | `std_msgs/msg/String` | Controls the headlight | `on`, `off`, `high`, `low` |
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

## Part 1 - Using ROS to check sensors and actuators
To check whether the hardware is working properly and become familiar with the available ROS resources, use terminal commands to inspect sensor readings and actuator status, as well as to control the actuators. 

First, connect to the vehicle's companion computer via SSH:
- Host name: `<VEHICLE NAME>.bot.chon.group`
- User name: root
- Password: root

In Unix-based systems, use `ssh root@<VEHICLE NAME>.bot.chon.group`.   

To inspect sensor readings and actuator status, use:
```
ros2 topic echo <TOPIC NAME>
```

To control the actuators, use:
```perception_action
ros2 topic pub -1 <TOPIC NAME> <TOPIC TYPE> "{data: <VALUE>}"
```

Notes: 
- for boolean topics, VALUE must be either `true` or `false`.
- for string topics, VALUE must be enclosed in single quotes.

## Part 2 - Implementing a collision-avoidance vehicle

###  Preliminaries

#### Getting the base source code
First of all, clone the repository from `https://github.com/issia-2026-jason-ros/<VEHICLE NAME>`. 

This repository contains a JaCaMo application with an agent whose goal is to `walk` around the environment. However, the agent does not have any plans to achieve this goal. In this exercise, you will implement the plans required to achieve it. Use the available ROS topics to connect the agent's perceptions and actions to the corresponding hardware resources.

The vehicle companion computer has a copy of this repository in the folder `~/<VEHICLE NAME>`.

#### Running the agent
To run a new version of the agent, you must transfer the changes to the vehicle's companion computer by following these steps:
1. Commit and push the changes to the repository.
2. In the vehicle's companion computer:
   - access the application folder:  `cd ~/<VEHICLE NAME>`.
   - update the files: `git pull`.
   - run the application: `./gradlew run`.
   


### Exercise 2.1 - Testing the development environment
This exercise aims to get familiar with the development environment. 

In its initial version, the agent only prints the message `I do not know how to satisfy this goal.`.  Modify the agent so that it also prints `Hello world`. The expected output is:
```
[chonbot_agent] Hello world.
[chonbot_agent] I do not know how to satisfy this goal.
```
Modify the agent's code, push the changes to the repository, access the vehicle's companion computer via SSH, pull the changes, run the updated version of the agent, and observe the output.

### Exercise 2.2 - Preventing collisions
Make the vehicle move forward and stop when an obstacle is detected.

### Exercise 2.3 - Implementing adaptive speed control
Make the vehicle move forward and adjust its speed according to the distance to the detected obstacle:
- High speed when the obstacle is at a safe distance;
- Default speed when the obstacle is at a moderate distance;
- Low speed when the obstacle is nearby;
- Stop when the obstacle is at a critical distance.

In addition, briefly blink the brake light for a little while (e.g. for 500 milliseconds) whenever the vehicle reduces its speed.


### Exercise 2.4 - Implementing obstacle avoidance
Make the vehicle navigate the environment while avoiding obstacles. Define your own navigation strategy.
Observe the following requirements:
- Blink the brake light whenever the vehicle reduces its speed;
- Blink the corresponding turn signal whenever the vehicle turns left or right.

