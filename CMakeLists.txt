cmake_minimum_required(VERSION 3.5)

project(laser_geometry)

find_package(ament_cmake REQUIRED)

find_package(angles REQUIRED)
find_package(rclcpp REQUIRED)
find_package(sensor_msgs REQUIRED)
find_package(tf2 REQUIRED)
find_package(ecl_eigen REQUIRED)

# TODO(dhood): enable python support once ported to ROS 2
# catkin_python_setup()

#
#catkin_package(
#  INCLUDE_DIRS include
#  LIBRARIES laser_geometry
#  DEPENDS Boost Eigen
#)
#

add_definitions(-DROS2)

include_directories(include
    ${angles_INCLUDE_DIRS}
    ${rclcpp_INCLUDE_DIRS}
    ${sensor_msgs_INCLUDE_DIRS}
    ${tf2_INCLUDE_DIRS}
    ${tf2_eigen_INCLUDE_DIRS}
    $ENV{BOOST_ROOT}
)

add_library(laser_geometry src/laser_geometry.cpp)
target_link_libraries(laser_geometry 
    ${angles_LIBRARIES}
    ${rclcpp_LIBRARIES}
    ${sensor_msgs_LIBRARIES}
    ${tf2_LIBRARIES}
    ${tf2_eigen_LIBRARIES}
)

ament_export_include_directories(include)
ament_export_libraries(laser_geometry)
ament_package()

install(TARGETS laser_geometry
  ARCHIVE DESTINATION lib
  LIBRARY DESTINATION lib)
install(DIRECTORY include/laser_geometry/
  DESTINATION include/${PROJECT_NAME}/
  FILES_MATCHING PATTERN "*.h")
