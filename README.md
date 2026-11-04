# Hands-on activity #1

In this activity, we are going to program an agent to be embedded in a two-wheeled vehicle whose sensors and actuators are controlled through ROS topics.
```
*** colocar aqui os tópicos disponíveis***
Especificar: nome do tópico, tipo de dado do tópico, função do tópico (ex. acionar motor, indicar status do motor), possíveis valores de parâmetros (no caso de tópicos em que o agente escreve para produzir uma atuação)


```

## Exercise 1 - Testing sensors and actuators
To both check whether the hardware is fine and also to become familiar with the available ROS resources, use terminal commands to inspect sensor values and actuator status and to control the actuators.

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

## Exercise 2 - Implementing a  collision avoidance vehicle
Use the available resources to implement a vehicle that freely runs in the environment. The only requirement is to avoid obstacles. 

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

