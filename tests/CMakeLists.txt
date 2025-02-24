if(NOT DEFINED USE_ROS2)
  find_package(GTest REQUIRED)
  include(GoogleTest)
  # Prevents discovery failure before install
  # No effect in CMake < 3.18
  set(CMAKE_GTEST_DISCOVER_TESTS_DISCOVERY_MODE PRE_TEST)
  function(add_ForceColl_test NAME)
    add_executable(${NAME} src/${NAME}.cpp)
    target_link_libraries(${NAME} PUBLIC GTest::gtest ForceColl)
    gtest_discover_tests(${NAME})
  endfunction()
else()
  find_package(ament_cmake_gtest REQUIRED)
  function(add_ForceColl_test NAME)
    ament_add_gtest(${NAME} src/${NAME}.cpp)
    target_link_libraries(${NAME} ForceColl)
  endfunction()
endif()

set(ForceColl_gtest_list
  TestContact
  TestWrenchDistribution
  )

foreach(NAME IN LISTS ForceColl_gtest_list)
  add_ForceColl_test(${NAME})
endforeach()
