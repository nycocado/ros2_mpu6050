cmake_minimum_required(VERSION 3.8)
project(mpu6050_driver)

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()

# find dependencies
find_package(ament_cmake REQUIRED)
find_package(rclcpp REQUIRED)
find_package(sensor_msgs REQUIRED)

include_directories(include)

add_executable(mpu6050_node src/mpu6050.cpp src/mpu6050_hal.cpp src/mpu6050_node.cpp)
ament_target_dependencies(mpu6050_node rclcpp sensor_msgs)
target_link_libraries(mpu6050_node i2c)

add_executable(mpu6050_calibrate src/mpu6050.cpp src/mpu6050_hal.cpp src/mpu6050_calibrate.cpp)
ament_target_dependencies(mpu6050_calibrate rclcpp sensor_msgs)
target_link_libraries(mpu6050_calibrate i2c)

install(TARGETS
  mpu6050_node
  mpu6050_calibrate
  DESTINATION lib/${PROJECT_NAME}
)

install(
  DIRECTORY config launch
  DESTINATION share/${PROJECT_NAME}
)

ament_package()
