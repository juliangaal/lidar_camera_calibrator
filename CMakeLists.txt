cmake_minimum_required(VERSION 3.0.2)
project(lidar_camera_calibrator)
set(CMAKE_BUILD_TYPE "release") 
set(CMAKE_CXX_STANDARD 14)

find_package(catkin REQUIRED COMPONENTS 
        roscpp sensor_msgs cv_bridge pcl_ros pcl_conversions std_msgs
)
find_package(OpenCV REQUIRED)
find_package(PCL REQUIRED)
find_package(catkin)
add_definitions(${PCL_DEFINITIONS})

if (catkin_FOUND)
  catkin_package(
          INCLUDE_DIRS include
          DEPENDS PCL
  )
  
  include_directories(
          include
          ${catkin_INCLUDE_DIRS}
          ${PCL_INCLUDE_DIRS}
  )
  
  link_directories(
          include
          ${PCL_LIBRARY_DIRS}
  )
  add_library(get_sync_data include/get_sync_data/get_sync_data.cpp)
  add_executable(get_sync_data_node ./src/get_sync_data_node.cpp )
  target_link_libraries(get_sync_data_node 
          get_sync_data       
          ${OpenCV_LIBS}
          ${PCL_LIBRARIES}
          ${catkin_LIBRARIES}
  )
  
  add_subdirectory(src/calibrator)
else()
  add_subdirectory(src/calibrator)
endif()
