cmake_minimum_required(VERSION 3.10)

set(PROJECT_NAME force_control_collection)
set(PROJECT_GENERATED_HEADERS_SKIP_DEPRECATED ON)
set(PROJECT_GENERATED_HEADERS_SKIP_CONFIG ON)
set(PROJECT_GENERATED_HEADERS_SKIP_WARNING ON)
set(PROJECT_URL https://github.com/isri-aist/ForceControlCollection)
set(PROJECT_DESCRIPTION "")
set(CMAKE_CXX_STANDARD 17)
set(PROJECT_USE_CMAKE_EXPORT TRUE)
set(CXX_DISABLE_WERROR ON)
option(INSTALL_DOCUMENTATION "Generate and install the documentation" OFF)

include(cmake/base.cmake)
project(force_control_collection LANGUAGES CXX)

# mc_rtc
add_project_dependency(mc_rtc REQUIRED)

if(NOT DEFINED USE_ROS2)
  set(DOXYGEN_HTML_OUTPUT html)
  find_package(ament_cmake REQUIRED)
  find_package(rclcpp REQUIRED)
  find_package(qp_solver_collection REQUIRED)

  # Eigen
  find_package(Eigen3 REQUIRED)
  include_directories(${EIGEN3_INCLUDE_DIR})


  include_directories(include ${ament_INCLUDE_DIRS})
  link_directories(${ament_LIBRARY_DIRS})

  ament_package()
else()
  set(DOXYGEN_HTML_OUTPUT doxygen-html)
  set(BUILD_TESTING OFF)
  option(BUILD_SHARED_LIBS "Build libraries as shared as opposed to static" ON)
  add_project_dependency(qp_solver_collection REQUIRED)
endif()

add_subdirectory(src)

if(BUILD_TESTING)
  add_subdirectory(tests)
endif()

if(INSTALL_DOCUMENTATION)
  add_subdirectory(doc)
endif()
