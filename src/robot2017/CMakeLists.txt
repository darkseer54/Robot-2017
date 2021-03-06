cmake_minimum_required(VERSION 2.8.3)
project(robot2017)

find_package(catkin REQUIRED COMPONENTS
  bldc_uart_beaglebone
  roscpp
  teleop_msg
)

catkin_package(
#  INCLUDE_DIRS include
#  LIBRARIES drivetest
  CATKIN_DEPENDS bldc_uart_beaglebone roscpp teleop_msg
#  DEPENDS system_lib
)

###########
## Build ##
###########

include_directories(
  ${catkin_INCLUDE_DIRS}
)

add_executable(drive_node src/drive_node.cpp src/motors.cpp src/teleop_exec.cpp)
add_executable(test_controller src/test_controller.cpp)

add_dependencies(drive_node robot2017_generate_messages_cpp teleop_msg_generate_messages_cpp)
add_dependencies(test_controller robot2017_generate_messages_cpp teleop_msg_generate_messages_cpp)

target_link_libraries(drive_node
  ${catkin_LIBRARIES}
)

target_link_libraries(test_controller
  ${catkin_LIBRARIES}
)

#############
## Install ##
#############

install(DIRECTORY launch/
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}/launch
  FILES_MATCHING PATTERN "*.launch"
)
