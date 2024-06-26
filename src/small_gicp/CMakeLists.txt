cmake_minimum_required(VERSION 3.8)
project(small_gicp)

## Use C++14
set(CMAKE_CXX_STANDARD 14)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

## By adding -Wall and -Werror, the compiler does not ignore warnings anymore,
## enforcing cleaner code.
add_definitions(-Wall -Werror)

## Export compile commands for clangd
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

#######################
## Find dependencies ##
#######################

find_package(ament_cmake_auto REQUIRED)
ament_auto_find_build_dependencies()

find_package(OpenMP) 
if (OPENMP_FOUND) 
set (CMAKE_C_FLAGS "${CMAKE_C_FLAGS} ${OpenMP_C_FLAGS}") 
set (CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${OpenMP_CXX_FLAGS}") 
set (CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} ${OpenMP_EXE_LINKER_FLAGS}") 
endif()

###########
## Build ##
###########

ament_auto_add_library(${PROJECT_NAME} SHARED
  src/small_gicp.cpp
)

rclcpp_components_register_node(${PROJECT_NAME}
  PLUGIN small_gicp::SmallGicpNode
  EXECUTABLE ${PROJECT_NAME}_node
)

#############
## Testing ##
#############

if(BUILD_TESTING)
  find_package(ament_lint_auto REQUIRED)
  list(APPEND AMENT_LINT_AUTO_EXCLUDE
    ament_cmake_copyright
    ament_cmake_uncrustify
    ament_cmake_cpplint
  )
  ament_lint_auto_find_test_dependencies()

  # find_package(ament_cmake_gtest)
  # set(TEST_NAME test_kalman_filter)
  # ament_add_gtest(${TEST_NAME} test/${TEST_NAME}.cpp)
  # target_link_libraries(${TEST_NAME} ${PROJECT_NAME})
endif()

#############
## Install ##
#############

ament_auto_package(
  INSTALL_TO_SHARE
  launch
)