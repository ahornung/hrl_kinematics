cmake_minimum_required(VERSION 2.8)
project(hrl_kinematics)

set(PACKAGE_DEPENDENCIES
  roscpp
  kdl_parser
  tf_conversions
  geometric_shapes
  sensor_msgs
  robot_state_publisher
  visualization_msgs
  urdf
)  
  

find_package(catkin REQUIRED COMPONENTS ${PACKAGE_DEPENDENCIES})

find_package(PCL REQUIRED QUIET COMPONENTS common surface)


include_directories(
  include
  ${catkin_INCLUDE_DIRS}
  ${PCL_INCLUDE_DIRS}
)

catkin_package(
  INCLUDE_DIRS include
  LIBRARIES ${PROJECT_NAME}
  CATKIN_DEPENDS ${PACKAGE_DEPENDENCIES}
  DEPENDS PCL
)

add_library(${PROJECT_NAME} src/TestStability.cpp src/Kinematics.cpp)
target_link_libraries(${PROJECT_NAME} ${catkin_LIBRARIES} ${PCL_LIBRARIES})

add_executable(test_stability src/test_stability.cpp)
target_link_libraries(test_stability ${PROJECT_NAME})



# install targets:
install(TARGETS ${PROJECT_NAME}
  test_stability
  ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)

install(DIRECTORY include/${PROJECT_NAME}/
  DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION}
  FILES_MATCHING PATTERN "*.h"
  PATTERN ".svn" EXCLUDE
)