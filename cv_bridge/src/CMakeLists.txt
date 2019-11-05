# add library
add_library(${PROJECT_NAME} cv_bridge.cpp rgb_colors.cpp)
ament_target_dependencies(${PROJECT_NAME}
  "OpenCV"
  "sensor_msgs"
)
target_link_libraries(${PROJECT_NAME} ${Boost_LIBRARIES} ${OpenCV_LIBRARIES})

install(TARGETS ${PROJECT_NAME} DESTINATION lib)
