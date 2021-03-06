cmake_minimum_required(VERSION 2.8.3)
project(udom_sensor_model)

find_package(catkin REQUIRED
  COMPONENTS
    rospy
    roslint
)

catkin_python_setup()

catkin_package(
  CATKIN_DEPENDS
    rospy
    std_msgs
    geometry_msgs
    udom_common_msgs
    udom_perception_msgs
)

install(PROGRAMS
  scripts/force_sensor_analytical_model
  scripts/force_sensor_model
  scripts/tactile_sensor_model
  scripts/wrench_filter
  DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)

### TESTS
roslint_python()

if(CATKIN_ENABLE_TESTING)
  find_package(roslaunch REQUIRED)
  find_package(rostest REQUIRED)

  roslaunch_add_file_check(launch)
  add_rostest(launch/force_sensor_analytical_model.test)
  add_rostest(launch/force_sensor_model.test)
  add_rostest(launch/tactile_sensor_model.test)
  add_rostest(launch/wrench_filter.test)
  catkin_add_nosetests(test/tactile_sensor_model_unittest.py)
  catkin_add_nosetests(test/identification_utils_unittest.py)
  catkin_add_nosetests(test/tactile_utils_unittest.py)
endif()
