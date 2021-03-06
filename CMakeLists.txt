cmake_minimum_required(VERSION 2.8.3)
project(rosfalcon)
include(CheckCXXCompilerFlag)
CHECK_CXX_COMPILER_FLAG("-std=c++11" COMPILER_SUPPORTS_CXX11)
if(COMPILER_SUPPORTS_CXX11)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")
else()
        message(STATUS "The compiler ${CMAKE_CXX_COMPILER} has no C++11 support. Please use a different C++ compiler.")
endif()

find_package(catkin REQUIRED COMPONENTS
 roscpp
 joy
 message_generation
)

find_package(Boost REQUIRED)
include_directories(${BOOST_INCLUDE_DIRECTORIES})

add_message_files(
 FILES
 falconForces.msg
 falconPos.msg
 falconSetPoint.msg
)

#uncomment if you have defined messages
generate_messages(
 DEPENDENCIES
 std_msgs
)

catkin_package(

)

include_directories(
  ${catkin_INCLUDE_DIRS}
)

#set the default path for built executables to the "bin" directory
#set(EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/bin)
#set the default path for built libraries to the "lib" directory
#set(LIBRARY_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/lib)


#uncomment if you have defined services
#rosbuild_gensrv()

#common commands for building c++ executables and libraries
#rosbuild_add_library(${PROJECT_NAME} src/example.cpp)
add_executable(joystick src/joystick.cpp)
add_executable(gripper src/gripper.cpp)
add_executable(falcon_cs_pid src/falcon_cs_pid.cpp)
add_executable(falcon_js_pid src/falcon_js_pid.cpp)
add_executable(falcon_node src/main.cpp)
target_link_libraries(joystick nifalcon_cli_base nifalcon usb-1.0)
target_link_libraries(falcon_cs_pid nifalcon_cli_base nifalcon usb-1.0)
target_link_libraries(falcon_js_pid nifalcon_cli_base nifalcon usb-1.0)
target_link_libraries(falcon_node nifalcon_cli_base nifalcon usb-1.0)
#rosbuild_add_boost_directories()
#rosbuild_link_boost(${PROJECT_NAME} thread)
#target_link_libraries(libnifalcon_cli_base ${PROJECT_NAME})
target_link_libraries(joystick ${catkin_LIBRARIES})
target_link_libraries(gripper ${catkin_LIBRARIES})
target_link_libraries(falcon_node ${catkin_LIBRARIES})

target_link_libraries(falcon_cs_pid ${catkin_LIBRARIES})
target_link_libraries(falcon_js_pid ${catkin_LIBRARIES})
