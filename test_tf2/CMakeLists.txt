cmake_minimum_required(VERSION 3.5)

project(test_tf2)

find_package(ament_cmake REQUIRED)

if(NOT BUILD_TESTING)
  ament_package()
  return()
endif()

# Default to C++14
if(NOT CMAKE_CXX_STANDARD)
  set(CMAKE_CXX_STANDARD 14)
endif()

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic -Wnon-virtual-dtor -Woverloaded-virtual)
endif()

find_package(ament_cmake_gtest REQUIRED)
find_package(builtin_interfaces REQUIRED)
find_package(geometry_msgs REQUIRED)
find_package(launch_testing_ament_cmake REQUIRED)
find_package(rclcpp REQUIRED)
find_package(tf2 REQUIRED)
# TODO (ahcorde): activate when tf2_bullet is merged
# find_package(tf2_bullet REQUIRED)
find_package(tf2_geometry_msgs REQUIRED)
find_package(tf2_kdl REQUIRED)
find_package(tf2_ros REQUIRED)

ament_find_gtest()

# TODO (ahcorde): activate when tf2_bullet is merged
# find_package(PkgConfig REQUIRED)
# pkg_check_modules(bullet REQUIRED bullet)
# include_directories(include ${bullet_INCLUDE_DIRS})

ament_add_gtest(buffer_core_test test/buffer_core_test.cpp)
if(TARGET buffer_core_test)
  ament_target_dependencies(buffer_core_test
    builtin_interfaces
    geometry_msgs
    rclcpp
    tf2
    tf2_geometry_msgs
    tf2_ros
  )
endif()

ament_add_gtest(test_message_filter test/test_message_filter.cpp)
if(TARGET test_message_filter)
  ament_target_dependencies(test_message_filter
    builtin_interfaces
    geometry_msgs
    rclcpp
    tf2
    tf2_ros
  )
endif()

# TODO (ahcorde): activate when tf2_bullet is merged
# ament_add_gtest(test_convert test/test_convert.cpp)
# if(TARGET test_convert)
#   ament_target_dependencies(test_convert
#     tf2
#     tf2_bullet
#   )
# endif()

ament_add_gtest(test_utils test/test_utils.cpp)
if(TARGET test_utils)
  ament_target_dependencies(test_utils
    geometry_msgs
    tf2
    tf2_geometry_msgs
    tf2_kdl
  )
endif()

# TODO (ahcorde): activate when tf2_bullet is merged
# add_executable(test_buffer_server test/test_buffer_server.cpp)
# if(TARGET test_buffer_server)
#   ament_target_dependencies(test_buffer_server
#     rclcpp
#     tf2_bullet
#     tf2_ros
#   )
# endif()

# TODO (ahcorde): activate when tf2_bullet is merged
# add_executable(test_buffer_client test/test_buffer_client.cpp)
# if(TARGET test_buffer_client)
#   ament_target_dependencies(test_buffer_client
#     rclcpp
#     tf2_bullet
#     tf2_kdl
#     tf2_ros
#   )
#   target_link_libraries(test_buffer_client ${GTEST_LIBRARIES})
#   add_launch_test(test/buffer_client_tester.launch.py)
# endif()

add_executable(test_static_publisher test/test_static_publisher.cpp)
if(TARGET test_static_publisher)
  ament_target_dependencies(test_static_publisher
    geometry_msgs
    rclcpp
    tf2
    tf2_ros
  )
  target_include_directories(test_static_publisher PRIVATE ${GTEST_INCLUDE_DIRS})
  target_link_libraries(test_static_publisher ${GTEST_LIBRARIES})
  add_launch_test(test/static_publisher.launch.py)
endif()

# TODO (ahcorde): activate when tf2_bullet is merged
# ament_add_gtest(test_tf2_bullet test/test_tf2_bullet.cpp)
# if(TARGET test_tf2_bullet)
#   ament_target_dependencies(test_tf2_bullet
#       rclcpp
#       tf2_bullet
#       tf2_ros
#   )
# endif()
#
# add_launch_test(test/test_buffer_client.launch.py)
# add_rostest(${CMAKE_CURRENT_SOURCE_DIR}/test/test_tf2_bullet.launch)

# install executables
install(TARGETS
  # test_buffer_client
  # test_buffer_server
  test_static_publisher
  DESTINATION lib/${PROJECT_NAME}
)
install(PROGRAMS
  test/test_buffer_client.py
  DESTINATION lib/${PROJECT_NAME}
)

ament_package()
