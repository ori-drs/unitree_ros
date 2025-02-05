cmake_minimum_required(VERSION 3.0.2)
project(unitree_legged_real)

set(CMAKE_CXX_STANDARD 14)
set(CMAKE_BUILD_TYPE "RelWithDebInfo")
set(CMAKE_CXX_FLAGS "-O3")

find_package(catkin REQUIRED COMPONENTS
    roscpp
    unitree_legged_msgs
)

find_package(Eigen3 REQUIRED)

find_package(aliengo_sdk CONFIG)
find_package(unitree_legged_sdk REQUIRED)

catkin_package()

include_directories(
    include
    ${catkin_INCLUDE_DIRS}
)

if(${aliengo_sdk_FOUND})  #  $ENV{UNITREE_SDK_VERSION} STREQUAL "3_1"
    include_directories($ENV{ALIENGO_SDK_PATH}/include)
    link_directories($ENV{ALIENGO_SDK_PATH}/lib)
    string(CONCAT LEGGED_SDK_NAME libaliengo_comm.so)
    set(EXTRA_LIBS ${LEGGED_SDK_NAME} lcm)

    add_definitions(-DSDK3_1)

#     add_executable(lcm_server_3_1 $ENV{ALIENGO_SDK_PATH}/examples/lcm_server.cpp)
#     target_link_libraries(lcm_server_3_1 ${EXTRA_LIBS} ${catkin_LIBRARIES})
#     add_dependencies(lcm_server_3_1 ${${PROJECT_NAME}_EXPORTED_TARGETS} ${catkin_EXPORTED_TARGETS})

elseif(${unitree_legged_sdk_FOUND})  #  $ENV{UNITREE_SDK_VERSION} STREQUAL "3_2"
    include_directories($ENV{UNITREE_LEGGED_SDK_PATH}/include)
    link_directories($ENV{UNITREE_LEGGED_SDK_PATH}/lib)
    string(CONCAT LEGGED_SDK_NAME libunitree_legged_sdk_$ENV{UNITREE_PLATFORM}.so)
    set(EXTRA_LIBS ${LEGGED_SDK_NAME} lcm)

    add_definitions(-DSDK3_2)

#     add_executable(lcm_server_3_2 $ENV{UNITREE_LEGGED_SDK_PATH}/examples/lcm_server.cpp)
#     target_link_libraries(lcm_server_3_2 ${EXTRA_LIBS} ${catkin_LIBRARIES})
#     add_dependencies(lcm_server_3_2 ${${PROJECT_NAME}_EXPORTED_TARGETS} ${catkin_EXPORTED_TARGETS})
endif()

add_executable(position_lcm src/exe/position_mode.cpp)
target_link_libraries(position_lcm ${EXTRA_LIBS} ${catkin_LIBRARIES})
add_dependencies(position_lcm ${${PROJECT_NAME}_EXPORTED_TARGETS} ${catkin_EXPORTED_TARGETS})

add_executable(velocity_lcm src/exe/velocity_mode.cpp)
target_link_libraries(velocity_lcm ${EXTRA_LIBS} ${catkin_LIBRARIES})
add_dependencies(velocity_lcm ${${PROJECT_NAME}_EXPORTED_TARGETS} ${catkin_EXPORTED_TARGETS})

add_executable(torque_lcm src/exe/torque_mode.cpp)
target_link_libraries(torque_lcm ${EXTRA_LIBS} ${catkin_LIBRARIES})
add_dependencies(torque_lcm ${${PROJECT_NAME}_EXPORTED_TARGETS} ${catkin_EXPORTED_TARGETS})

add_executable(walk_lcm src/exe/walk_mode.cpp)
target_link_libraries(walk_lcm ${EXTRA_LIBS} ${catkin_LIBRARIES})
add_dependencies(walk_lcm ${${PROJECT_NAME}_EXPORTED_TARGETS} ${catkin_EXPORTED_TARGETS})
