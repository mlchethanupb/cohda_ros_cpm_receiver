cmake_minimum_required(VERSION 2.8.3)
project(cohda_ros_cpm_receiver)

## Find catkin macros and libraries
find_package(catkin REQUIRED COMPONENTS
  roscpp
  std_msgs
  etsi_its_msgs
  # Add other necessary dependencies
)

# Find FlatBuffers package
find_package(Flatbuffers REQUIRED)

# Generate code from FlatBuffers schema
set(FLATBUFFERS_SOURCES_DIR ${CMAKE_CURRENT_SOURCE_DIR}/flatbuffers)
set(FLATBUFFERS_SCHEMA ${FLATBUFFERS_SOURCES_DIR}/all_interface.fbs)

# Add the generated FlatBuffers code as a library
set(FLATBUFFERS_GENERATED_FILES
  ${FLATBUFFERS_SOURCES_DIR}/all_interface_generated.h
)

add_library(etsi_flatbuffers_lib_r ${FLATBUFFERS_GENERATED_FILES})
set_target_properties(etsi_flatbuffers_lib_r PROPERTIES LINKER_LANGUAGE CXX)
target_link_libraries(etsi_flatbuffers_lib_r PUBLIC ${Flatbuffers_LIBRARIES})

## Declare a catkin package
catkin_package(
  CATKIN_DEPENDS 
  roscpp 
  std_msgs
  etsi_its_msgs # Add other necessary dependencies
)

## Build the node
include_directories(
  include
  ${catkin_INCLUDE_DIRS}
  ${Flatbuffers_INCLUDE_DIRS}

)

add_executable(cohda_ros_cpm_receiver src/cpm_receiver.cpp)
target_link_libraries(  cohda_ros_cpm_receiver 
                        ${catkin_LIBRARIES}
                        etsi_flatbuffers_lib_r )