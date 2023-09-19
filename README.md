# Street Driver Workshop

## Setup

IP Address into `env.txt`

## Running

Run the base containers controling the motors and camera of the robot

```bash
docker-compose --env-file env.txt up -d ros_master
docker-compose --env-file env.txt up -d motor_control camera
```

## Run the Street Driver code

In another terminal, run:

```bash
export ROBOT_IP=10.0.0.5
export ROS_IP=10.0.0.11

docker run -it --rm \
    -e ROS_MASTER_URI=http://${ROBOT_IP}:11311 \
    -e ROS_IP=${ROS_IP} \
    --network="host" \
    --mount type=bind,source="$(pwd)",target=/root/catkin_ws/src/ \
    ghcr.io/roboticamed/mardan_noetic:latest@sha256:3b26d5e831b2d24bae3150b490db6913f329947c9e11d93a513d5deb7da6888d \
    /usr/bin/tmux
```

For AMD64: `sha256:07b6696f07c9b913c70c8b04cb26c5c40ca05e4b31556b7de3c4e8930f909b90`.

Inside the tmux session, run:

```bash
cd /root/catkin_ws
catkin_make
source devel/setup.bash

roslaunch street_driver street_driver.launch
```

## Teleop

```bash
source /opt/ros/noetic/setup.bash
rosrun teleop_twist_keyboard teleop_twist_keyboard.py cmd_vel:=motors/motor_twist
```