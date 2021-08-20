cmake_minimum_required(VERSION 3.0.2)
project(unitree_legged_msgs)

find_package(catkin REQUIRED COMPONENTS
  message_generation
  std_msgs
  geometry_msgs
  sensor_msgs
)

add_message_files(
  FILES
  MotorCmd.msg
  MotorState.msg
  Cartesian.msg
  IMU.msg
  LED.msg
  LowCmd.msg
  LowState.msg
  HighCmd.msg
  HighState.msg
  WirelessRemote.msg
)

generate_messages(
  DEPENDENCIES
  std_msgs
  geometry_msgs
  sensor_msgs
)

catkin_package(
  CATKIN_DEPENDS
  message_runtime
  std_msgs
  geometry_msgs
  sensor_msgs 
)
