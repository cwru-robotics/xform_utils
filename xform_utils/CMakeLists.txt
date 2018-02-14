cmake_minimum_required(VERSION 2.8.3)
project(xform_utils)

find_package(Eigen3)
# this is depreciated but still necessary for now.
if(NOT EIGEN3_FOUND)
  # Fallback to cmake_modules
  find_package(cmake_modules REQUIRED)
  find_package(Eigen REQUIRED)
  set(Eigen3_INCLUDE_DIRS ${EIGEN_INCLUDE_DIR})
  set(Eigen3_DEP Eigen)
  # message("Found Eigen ${EIGEN_INCLUDE_DIR}")
  # Possibly map additional variables to the EIGEN3_ prefix.
else()
  set(Eigen3_INCLUDE_DIRS ${EIGEN3_INCLUDE_DIR})
  set(Eigen3_DEP Eigen3)
  # message("Found Eigen3 ${EIGEN3_INCLUDE_DIR}")
endif()
add_definitions(${EIGEN_DEFINITIONS})

find_package(catkin REQUIRED COMPONENTS tf std_msgs geometry_msgs)

include_directories(
  include
  ${catkin_INCLUDE_DIRS}
  ${catkin_INCLUDES}
  ${Eigen3_INCLUDE_DIRS}
)

catkin_package(
	DEPENDS ${Eigen3_DEP}
	LIBRARIES xform_utils
	CATKIN_DEPENDS roscpp
	INCLUDE_DIRS include ${EIGEN3_INCLUDE_DIRS}
)

add_library(xform_utils src/xform_utils.cpp)
target_link_libraries(xform_utils
  ${catkin_LIBRARIES}
)
