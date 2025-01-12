cmake_minimum_required(VERSION 3.0.2)
project(gnss_converter)

if(NOT CMAKE_BUILD_TYPE)
  set(CMAKE_BUILD_TYPE Release)
endif()

set(CMAKE_CXX_FLAGS_RELEASE "-Ofast")

## Compile as C++11, supported in ROS Kinetic and newer
# add_compile_options(-std=c++11)

## Find catkin macros and libraries
## if COMPONENTS list like find_package(catkin REQUIRED COMPONENTS xyz)
## is used, also find other catkin packages
find_package(catkin REQUIRED COMPONENTS
  roscpp
  std_msgs
  inertiallabs_msgs
  geometry_msgs
  message_filters
  tf
)

find_package(OpenCV REQUIRED)

find_package(Eigen3 REQUIRED)
set(EIGEN_PACKAGE EIGEN3)
if(NOT EIGEN3_FOUND)
  find_package(cmake_modules REQUIRED)
  find_package(Eigen REQUIRED)
  set(EIGEN3_INCLUDE_DIRS ${EIGEN_INCLUDE_DIRS})
  set(EIGEN3_LIBRARIES ${EIGEN_LIBRARIES})
  set(EIGEN_PACKAGE Eigen)
endif()

## System dependencies are found with CMake's conventions
# find_package(Boost REQUIRED COMPONENTS system)

catkin_package(
#  INCLUDE_DIRS include
#  LIBRARIES gnss_converter
#  CATKIN_DEPENDS roscpp std_msgs
#  DEPENDS system_lib
)

###########
## Build ##
###########

## Specify additional locations of header files
## Your package locations should be listed before other locations
include_directories(
  include
  ${catkin_INCLUDE_DIRS}
  ${EIGEN3_INCLUDE_DIRS}
  ${OpenCV_INCLUDE_DIRS}
)

add_executable(gnss_converter
  include/gnss_converter/gnss_converter.h
  include/gnss_converter/LLH2UTM.h
  include/gnss_converter/quaternion_euler.h
  src/gnss_converter.cpp
  src/LLH2UTM.cpp
  src/quaternion_euler.cpp
)
add_dependencies(gnss_converter ${${PROJECT_NAME}_EXPORTED_TARGETS} ${catkin_EXPORTED_TARGETS})
target_link_libraries(gnss_converter
  ${catkin_LIBRARIES}
  ${EIGEN3_LIBRARIES}
  ${OpenCV_LIBS}
)

add_executable(gnss_pose_pub
  include/gnss_converter/LLH2UTM.h
  include/gnss_converter/quaternion_euler.h
  src/gnss_pose_pub.cpp
  src/LLH2UTM.cpp
  src/quaternion_euler.cpp
)
add_dependencies(gnss_pose_pub ${${PROJECT_NAME}_EXPORTED_TARGETS} ${catkin_EXPORTED_TARGETS})
target_link_libraries(gnss_pose_pub
  ${catkin_LIBRARIES}
  ${EIGEN3_LIBRARIES}
  ${OpenCV_LIBS}
)

add_executable(ntrip_client
  src/ntrip_client.cpp
)
add_dependencies(ntrip_client ${${PROJECT_NAME}_EXPORTED_TARGETS} ${catkin_EXPORTED_TARGETS})
target_link_libraries(ntrip_client
  ${catkin_LIBRARIES}
  ${EIGEN3_LIBRARIES}
  ${OpenCV_LIBS}
)

add_executable(position_pub
  include/gnss_converter/LLH2UTM.h
  include/gnss_converter/quaternion_euler.h
  src/position_pub.cpp
  src/LLH2UTM.cpp
  src/quaternion_euler.cpp
)
add_dependencies(position_pub ${${PROJECT_NAME}_EXPORTED_TARGETS} ${catkin_EXPORTED_TARGETS})
target_link_libraries(position_pub
  ${catkin_LIBRARIES}
  ${EIGEN3_LIBRARIES}
  ${OpenCV_LIBS}
)
