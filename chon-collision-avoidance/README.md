## Running

1. Connect to the vehicle's companion computer via SSH:
- Host name: `<VEHICLE NAME>.bot.chon.group`
- User name: root
- Password: root

In Unix-based systems, use `ssh root@<VEHICLE NAME>.bot.chon.group`.   


2. Clone this repository:
   `git clone https://github.com/issia-2026-jason-ros/hands-on-1.git`


3. Access the application directory:
   `cd hands-on-1/chon-collision-avoidance`
  

4. Launch the ROS infrastructure (Rosbride, ROS node):
   `./setup.sh`

   
5. Launch the JaCaMo application:
   `jacamo obstacle_avoidance_vehicle.jcm`

