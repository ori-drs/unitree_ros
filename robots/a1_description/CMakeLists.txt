cmake_minimum_required(VERSION 3.0.2)
project(a1_description)

find_package(catkin REQUIRED COMPONENTS xacro)

catkin_package(CATKIN_DEPENDS xacro)

xacro_add_files(xacro/robot.xacro OUTPUT "a1.urdf" TARGET a1 REMAP DEBUG:=false INSTALL DESTINATION urdf)

install(DIRECTORY config launch meshes urdf xacro
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}
)
