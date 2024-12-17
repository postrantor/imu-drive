cmake_minimum_required(VERSION 3.5)
project(fdilink_ahrs)

# Default to C99
if(NOT CMAKE_C_STANDARD)
  set(CMAKE_C_STANDARD 99)
endif()

# Default to C++14
if(NOT CMAKE_CXX_STANDARD)
  set(CMAKE_CXX_STANDARD 14)
endif()

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()

find_package(Eigen3 REQUIRED)
set(Eigen3_INCLUDE_DIRS ${EIGEN3_INCLUDE_DIR})

# find dependencies
find_package(ament_cmake REQUIRED)
find_package(rclcpp REQUIRED)
find_package(rclpy REQUIRED)
find_package(tf2 REQUIRED)
find_package(tf2_ros REQUIRED)
# interfaces
find_package(sensor_msgs REQUIRED)
find_package(std_msgs REQUIRED)
find_package(nav_msgs REQUIRED)
find_package(geometry_msgs REQUIRED)
find_package(tf2_geometry_msgs REQUIRED)
# serial port
find_package(serial REQUIRED)

include_directories(include)

add_executable(ahrs_driver_node
  src/ahrs_driver.cpp
  src/crc_table.cpp
)
ament_target_dependencies(ahrs_driver_node
  serial
  rclcpp
  rclpy
  tf2_ros
  tf2
  std_msgs
  sensor_msgs
  nav_msgs
  geometry_msgs
)

add_executable(imu_tf_node
  src/imu_tf.cpp)
ament_target_dependencies(imu_tf_node
  rclcpp
  rclpy
  std_msgs
  sensor_msgs
  serial
  tf2_ros
  tf2
  tf2_geometry_msgs
)

install(
  TARGETS
    ahrs_driver_node
    imu_tf_node
  DESTINATION lib/${PROJECT_NAME}
)

install(
  DIRECTORY launch
  DESTINATION share/${PROJECT_NAME}
)

ament_package()
