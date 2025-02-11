cmake_minimum_required(VERSION 3.5)
project(cv_bridge)

find_package(ament_cmake_ros REQUIRED)

# Default to C++14
if(NOT CMAKE_CXX_STANDARD)
  set(CMAKE_CXX_STANDARD 14)
endif()

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra)
endif()

find_package(Boost REQUIRED)
find_package(sensor_msgs REQUIRED)
find_package(OpenCV 4 REQUIRED
  COMPONENTS
    opencv_core
    opencv_imgproc
    opencv_imgcodecs
  CONFIG
)

include_directories(include ${Boost_INCLUDE_DIRS})

add_subdirectory(src)

# cv_bridge_lib_dir is passed as APPEND_LIBRARY_DIRS for each ament_add_gtest call so
# the project library that they link against is on the library path.
# This is especially important on Windows.
# This is overwritten each loop, but which one it points to doesn't really matter.
set(cv_bridge_lib_dir "$<TARGET_FILE_DIR:${PROJECT_NAME}>")

if(BUILD_TESTING)
  add_subdirectory(test)
endif()

ament_export_dependencies(OpenCV)

ament_export_include_directories(include)
ament_export_libraries(${PROJECT_NAME})

# install the include folder
install(
  DIRECTORY include/${PROJECT_NAME}/
  DESTINATION include/${PROJECT_NAME}
)

ament_package(
  CONFIG_EXTRAS "cmake/cv_bridge-extras.cmake.in"
)
