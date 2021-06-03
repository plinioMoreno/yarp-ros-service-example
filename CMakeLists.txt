# Copyright (C) 2006-2021 Istituto Italiano di Tecnologia (IIT)
# All rights reserved.
#
# This software may be modified and distributed under the terms of the
# BSD-3-Clause license. See the accompanying LICENSE file for details.

cmake_minimum_required(VERSION 3.12)
project(yarp_ros_example)

find_package(YARP 3.3.103 COMPONENTS os dev math rosmsg idl_tools REQUIRED)

yarp_idl_to_dir(INPUT_FILES roscpp_tutorials/TwoInts.srv OUTPUT_DIR ${CMAKE_BINARY_DIR}/msg 	    	SOURCES_VAR TwoInts_SOURCES
                HEADERS_VAR TwoInts_HEADERS
                INCLUDE_DIRS_VAR TwoInts_INCLUDES
                ROSMSG_WITH_ROS)

yarp_idl_to_dir(INPUT_FILES rospy_tutorials/HeaderString.msg OUTPUT_DIR ${CMAKE_BINARY_DIR}/msg 	    	SOURCES_VAR HeaderString_SOURCES
                HEADERS_VAR HeaderString_HEADERS
                INCLUDE_DIRS_VAR HeaderString_INCLUDES
                ROSMSG_WITH_ROS)

include_directories(${CMAKE_BINARY_DIR}/msg)
add_executable(add_int_server_v1b)
target_sources(add_int_server_v1b PRIVATE add_int_server_v1b.cpp
                                          ${TwoInts_SOURCES}
                                          ${TwoInts_HEADERS})
target_include_directories(add_int_server_v1b PRIVATE ${TwoInts_INCLUDES})
target_link_libraries(add_int_server_v1b PRIVATE YARP::YARP_os
                                                 YARP::YARP_init
                                                 YARP::YARP_rosmsg)

add_executable(add_int_client_v1b)
target_sources(add_int_client_v1b PRIVATE add_int_client_v1b.cpp
                                          ${TwoInts_SOURCES}
                                          ${TwoInts_HEADERS})
target_include_directories(add_int_client_v1b PRIVATE ${TwoInts_INCLUDES})
target_link_libraries(add_int_client_v1b PRIVATE YARP::YARP_os
                                                 YARP::YARP_init
                                                 YARP::YARP_rosmsg)

add_executable(listener_header)
target_include_directories(listener_header PRIVATE ${HeaderString_INCLUDES})
target_sources(listener_header PRIVATE listener_header.cpp ${HeaderString_SOURCES}
                                          ${HeaderString_HEADERS})
target_link_libraries(listener_header PRIVATE YARP::YARP_os
                                       YARP::YARP_init
                                       YARP::YARP_rosmsg)

add_executable(talker_header)
target_include_directories(talker_header PRIVATE ${HeaderString_INCLUDES})
target_sources(talker_header PRIVATE talker_header.cpp ${HeaderString_SOURCES}
                                          ${HeaderString_HEADERS})
target_link_libraries(talker_header PRIVATE YARP::YARP_os
                                     YARP::YARP_init
                                     YARP::YARP_rosmsg)
