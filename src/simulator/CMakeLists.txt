cmake_minimum_required(VERSION 3.8)
project(simulator)

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()

# find dependencies
find_package(ament_cmake REQUIRED)
find_package(rclcpp REQUIRED)
find_package(rosgraph_msgs REQUIRED)
find_package(geometry_msgs REQUIRED)
find_package(nav2_msgs REQUIRED)
find_package(rclcpp_action REQUIRED)
find_package(tf2_ros REQUIRED)
find_package(tf2_geometry_msgs REQUIRED)

set(dependencies
  rclcpp
  rosgraph_msgs
  geometry_msgs
  nav2_msgs
  rclcpp_action
  tf2_ros
  tf2_geometry_msgs
)

add_executable(clock_node src/clock_node.cpp)
target_include_directories(clock_node PUBLIC
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
  $<INSTALL_INTERFACE:include>)
target_compile_features(clock_node PUBLIC c_std_99 cxx_std_17)  # Require C99 and C++17
ament_target_dependencies(
  clock_node
  ${dependencies}
)

add_executable(telecontrol src/telecontrol.cpp)
target_include_directories(telecontrol PUBLIC
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
  $<INSTALL_INTERFACE:include>)
target_compile_features(telecontrol PUBLIC c_std_99 cxx_std_17)  # Require C99 and C++17
ament_target_dependencies(
  telecontrol
  ${dependencies}
)

add_executable(planner_bridge src/planner_bridge.cpp)
target_include_directories(planner_bridge PUBLIC
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
  $<INSTALL_INTERFACE:include>)
target_compile_features(planner_bridge PUBLIC c_std_99 cxx_std_17)  # Require C99 and C++17
ament_target_dependencies(
  planner_bridge
  ${dependencies}
)

add_executable(transform src/transform.cpp)
target_include_directories(transform PUBLIC
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
  $<INSTALL_INTERFACE:include>)
target_compile_features(transform PUBLIC c_std_99 cxx_std_17)  # Require C99 and C++17
ament_target_dependencies(
  transform
  ${dependencies}
)

install(TARGETS clock_node telecontrol planner_bridge transform
  DESTINATION lib/${PROJECT_NAME})

install(
    DIRECTORY maps launch rviz
    DESTINATION share/${PROJECT_NAME}
)

if(BUILD_TESTING)
  find_package(ament_lint_auto REQUIRED)
  # the following line skips the linter which checks for copyrights
  # uncomment the line when a copyright and license is not present in all source files
  #set(ament_cmake_copyright_FOUND TRUE)
  # the following line skips cpplint (only works in a git repo)
  # uncomment the line when this package is not in a git repo
  #set(ament_cmake_cpplint_FOUND TRUE)
  ament_lint_auto_find_test_dependencies()
endif()

ament_package()
