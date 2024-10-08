cmake_minimum_required(VERSION 2.8.3)
project(pure_pursuit_controller)

find_package(catkin REQUIRED COMPONENTS
  roscpp
  std_msgs
  nav_msgs
  visualization_msgs
  tf
)

catkin_package(
  INCLUDE_DIRS include
  LIBRARIES ${PROJECT_NAME}
  CATKIN_DEPENDS roscpp std_msgs nav_msgs visualization_msgs tf
)

catkin_add_gtest(target
  test/test_pure_pursuit.cpp
  test/test_state_machine.cpp
)

include_directories(
  include
  ${catkin_INCLUDE_DIRS}
)

add_executable(pure_pursuit_controller_node src/main.cpp src/pure_pursuit_controller.cpp src/frame_id_remapper.cpp src/path_tracking_planner.cpp src/state_machine.cpp)

add_dependencies(${PROJECT_NAME}_node ${${PROJECT_NAME}_EXPORTED_TARGETS} ${catkin_EXPORTED_TARGETS})

target_link_libraries(${PROJECT_NAME}_node
  ${catkin_LIBRARIES}
)

# Enable testing
if(CATKIN_ENABLE_TESTING)
  find_package(rostest REQUIRED)
  add_subdirectory(test)
endif()