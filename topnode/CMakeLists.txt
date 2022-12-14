cmake_minimum_required(VERSION 3.8)
project(topnode)

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()

find_package(ament_cmake REQUIRED)
find_package(rclcpp REQUIRED)
find_package(rclcpp_components REQUIRED)
find_package(topnode_interfaces REQUIRED)

include_directories(include)

add_library(resource_monitor_node SHARED src/resource_monitor_node.cpp)
target_compile_definitions(resource_monitor_node PRIVATE "TOPNODE_DLL")
ament_target_dependencies(resource_monitor_node rclcpp rclcpp_components topnode_interfaces)

add_executable(resource_monitor src/topnode.cpp)
target_link_libraries(resource_monitor resource_monitor_node)
ament_target_dependencies(resource_monitor rclcpp)

install(TARGETS
  resource_monitor_node
  ARCHIVE DESTINATION lib
  LIBRARY DESTINATION lib
  RUNTIME DESTINATION bin)

install(TARGETS
  resource_monitor
  DESTINATION lib/${PROJECT_NAME})

if(BUILD_TESTING)
  find_package(ament_lint_auto REQUIRED)
  ament_lint_auto_find_test_dependencies()
endif()

ament_package()
