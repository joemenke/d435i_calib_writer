# ubuntu 12.04 LTS cmake version 2.8.7
# ubuntu 14.04 LTS cmake version 2.8.12.2
# ubuntu 16.04 LTS cmake version 3.5.1
cmake_minimum_required(VERSION 2.8.3)

project(imusync)
set(CMAKE_CXX_STANDARD 11)

# Save the command line compile commands in the build output
set(CMAKE_EXPORT_COMPILE_COMMANDS 1)
# View the makefile commands during build
SET(CMAKE_MODULE_PATH "${PROJECT_SOURCE_DIR}/cmake_modules")

set(CMAKE_BUILD_TYPE "release")

include(CheckCXXCompilerFlag)
CHECK_CXX_COMPILER_FLAG("-std=c++11" COMPILER_SUPPORTS_CXX11)
CHECK_CXX_COMPILER_FLAG("-std=c++0x" COMPILER_SUPPORTS_CXX0X)
if(COMPILER_SUPPORTS_CXX11)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")
elseif(COMPILER_SUPPORTS_CXX0X)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++0x")
else()
    message(STATUS "The compiler ${CMAKE_CXX_COMPILER} has no C++11 support. Please use a different C++ compiler.")
endif()

find_package(OpenCV REQUIRED)
IF(OpenCV_FOUND)
   MESSAGE(STATUS "Found OpenCV: ${OpenCV_INCLUDE_DIRS}")
ENDIF(OpenCV_FOUND)

find_package(realsense2 REQUIRED)
IF(realsense2_FOUND)
   MESSAGE(STATUS "Found RealSense: ${realsense2_INCLUDE_DIRS}")
ENDIF(realsense2_FOUND)

find_package(Eigen REQUIRED)
IF(Eigen_FOUND)
   MESSAGE(STATUS "Found Eigen: ${EIGEN_INCLUDE_DIRS}")
ENDIF(Eigen_FOUND)
#SET( EIGEN_INCLUDE_DIRS "$ENV{EIGEN_INCLUDE_DIR}" )

set(
DEPENDENCIES 
${realsense2_LIBRARY} 
${OpenCV_LIBRARIES}
${EIGEN_LIBRARIES}
)

include_directories(
  ${realsense2_INCLUDE_DIRS}
  ${OpenCV_INCLUDE_DIRS}
  ${EIGEN_INCLUDE_DIRS}
)

set(
  SOURCES
)

add_executable(main src/main.cpp ${SOURCES})
target_link_libraries(main ${DEPENDENCIES})


install(
    TARGETS
    main

    RUNTIME DESTINATION
    ${CMAKE_BINARY_DIR}
)


